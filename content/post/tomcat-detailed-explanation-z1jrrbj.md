---
title: Tomcat各端口详解
slug: tomcat-detailed-explanation-z1jrrbj
url: /post/tomcat-detailed-explanation-z1jrrbj.html
date: '2024-03-05 02:06:01'
lastmod: '2024-03-05 02:07:50'
toc: true
tags:
  - 运维
  - Linux
  - Tomcat
keywords: 运维,Linux,Tomcat
isCJKLanguage: true
---

# Tomcat各端口详解

　　从tomcat配置文件中，我们可以看出，在启动tomcat的时候默认启动了3个端口，分别是8080（8443）、8009、8005。

---

### 8080（8443）端口

```xml
<Connector port="80" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

　　这个应该是我们最熟悉的一个，平常开发测试也经常用，该Connector用于<span style="font-weight: bold;" class="bold">监听浏览器发送的请求</span>，设置为80后可以直接使用[http://localhost](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost)访问。

　　http协议，其中`redirectPort`​表示如果发送的是https请求，就将请求发送到8443端口。

　　8443是默认的https监听端口，默认是没有开启的，如果要开启由于tomcat不自带证书所以除了取消注释之外，还需要自己生成证书并指定。

### 8009端口

```xml
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```

　　Nginx、Apache等反向代理tomcat时就可以使用ajp协议反向代理到该端口。  
 虽然我们经常使用http反向代理到8080端口，但由于ajp建立tcp链接后一般长时间保持，从而减少Http反复进行tcp链接和断开的开销，所以反向代理中ajp是比http高效的。

### 8005端口

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

　　tomcat监听的关闭端口，<span style="font-weight: bold;" class="bold">就是说这个端口负责监听关闭tomcat的请求</span>。

　　当执行`shutdown.sh`​关闭tomcat就是链接8005端口执行`SHUTDOWN`​命令；由此，我们直接用telnet向8005端口执行`SHUTDOWN`​来关闭tomcat，这也是比较正统的关闭方式，如果这个端口没被监听，那么sh脚本就是无效的。

---

　　实际上，8005和8009端口并不是必须的，尤其SHUTDOWN虽然默认是监听在127.0.0.1，但是连接到这个端口，发送`SHUTDOWN`​就可以无任何验证的把tomcat关闭掉，有安全隐患的。

　　AJP端口用来与应用服务器交互时候用，比如apache连接tomcat等，开发期间一般也用不着，可以禁止掉。

### 禁用方式：

　　AJP端口，直接注释掉server.xml文件的配置行就可以了。

　　SHUTDOWN端口是写在server参数里面的，直接去掉是不管用的，也是会默认启动，一般在安全设置时候建议把端口修改为其他端口，SHUTDOWN修改为其他复杂的字符串。

　　实际上这个端口是可以直接屏蔽不监听的。设置时候将其port值修改为-1就可以。

```xml
<Server port="-1" shutdown="SHUTDOWN">
```

### server.xml配置文件

```xml
<!-- 属性说明
port:指定一个端口，这个端口负责监听关闭Tomcat的请求
shutdown:向以上端口发送的关闭服务器的命令字符串
-->
<Server port="8005" shutdown="SHUTDOWN">

  <Listener className="org.apache.catalina.core.AprLifecycleListener" />
  <Listener className="org.apache.catalina.mbeans.ServerLifecycleListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.storeconfig.StoreConfigLifecycleListener"/>

  <GlobalNamingResources>
    <Environment name="simpleValue" type="java.lang.Integer" value="30"/>
    <Resource name="UserDatabase" auth="Container"
        type="org.apache.catalina.UserDatabase"
        description="User database that can be updated and saved"
        factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
        pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <Service name="Catalina">
    <!--
      Connector 元素:
      由 Connector 接口定义.<Connector> 元素代表与客户程序实际交互的组件,它负责接收客户请求,以及向客户返回响应结果.

      属性说明:
        port:服务器连接器的端口号,该连接器将在指定端口侦听来自客户端的请求。
        enableLookups:如果为 true，则可以通过调用 request.getRemoteHost() 进行 DNS 查询来得到远程客户端的实际主机名；若为 false 则不进行DNS查询，而是返回其ip地址。
        redirectPort:服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号。
        acceptCount:当所有可以使用的处理请求的线程都被用光时,可以放到处理队列中的请求数,超过这个数的请求将不予处理，而返回Connection refused错误。
        connectionTimeout:等待超时的时间数（以毫秒为单位）。
        maxThreads:设定在监听端口的线程的最大数目,这个值也决定了服务器可以同时响应客户请求的最大数目.默认值为200。
        protocol:必须设定为AJP/1.3协议。
        address:如果服务器有两个以上IP地址,该属性可以设定端口监听的IP地址,默认情况下,端口会监听服务器上所有IP地址。
        minProcessors:服务器启动时创建的处理请求的线程数，每个请求由一个线程负责。
        maxProcessors:最多可以创建的处理请求的线程数。
        minSpareThreads:最小备用线程 。
        maxSpareThreads:最大备用线程。
        debug:日志等级。
        disableUploadTimeout:禁用上传超时,主要用于大数据上传时。
    -->
    <Connector port="8080" maxHttpHeaderSize="8192"
               maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
               enableLookups="false" redirectPort="8443" acceptCount="100"
               connectionTimeout="20000" disableUploadTimeout="true" />


    <!-- 负责和其他 HTTP 服务器建立连接。在把 Tomcat 与其他 HTTP 服务器集成时就需要用到这个连接器。 -->
    <Connector port="8009" 
               enableLookups="false" redirectPort="8443" protocol="AJP/1.3" />


    <!-- 
      每个Service元素只能有一个Engine元素.元素处理在同一个<Service>中所有<Connector>元素接收到的客户请求
      属性说明:
        name:对应$CATALINA_HOME/config/Catalina 中的 Catalina ;
        defaultHost: 对应Host元素中的name属性,也就是和$CATALINA_HOME/config/Catalina/localhost中的localhost，缺省的处理请求的虚拟主机名，它至少与其中的一个Host元素的name属性值是一样的
        debug:日志等级
    -->
    <Engine name="Catalina" defaultHost="localhost">

      <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
             resourceName="UserDatabase"/>
      <!--
        由 Host 接口定义.一个 Engine 元素可以包含多个<Host>元素.
        每个<Host>的元素定义了一个虚拟主机.它包含了一个或多个Web应用.

        属性说明：
          name:在此例中一直被强调为$CATALINA_HOME/config/Catalina/localhost中的localhost虚拟主机名
          debug:是日志的调试等级 
          appBase:默认的应用路径,也就是把应用放在一个目录下,并在autoDeploy为true的情况下,可自动部署应用此路径相对于$CATALINA_HOME/ (web applications的基本目录)
          unpackWARs:设置为true,在Web应用为*.war是,解压此WAR文件. 如果为true,则tomcat会自动将WAR文件解压;否则不解压,直接从WAR文件中运行应用程序.
          autoDeploy:默认为true,表示如果有新的WEB应用放入appBase 并且Tomcat在运行的情况下,自动载入应用
      -->
      <Host name="localhost" appBase="webapps"
         unpackWARs="true" autoDeploy="true"
         xmlValidation="false" xmlNamespaceAware="false">
        <!-- 
          属性说明：
            path:访问的URI,如:http://localhost/是我的应用的根目录,访问此应用将用:http://localhost/demm进行操作,此元素必须，
                表示此web application的URL的前缀，用来匹配一个Context。请求的URL形式为http://localhost:8080/path/*
            docBase:WEB应用的目录,此目录必须符合Java WEB应用的规范，web application的文件存放路径或者是WAR文件存放路径。
            debug:日志等级 
            reloadable:是否在程序有改动时重新载入,设置成true会影响性能,但可自动载入修改后的文件，
                如果为true，则Tomcat将支持热部署，会自动检测web application的/WEB-INF/lib和/WEB-INF/classes目录的变化，
                自动装载新的JSP和Servlet，我们可以在不重起Tomcat的情况下改变web application
        -->
        <Context path="/demm" docBase="E:\\projects\\demm\\WebRoot" debug="0" reloadable="true"></Context>
      </Host>
    </Engine>
  </Service>
</Server>
```

　　作者：大胖孩  
链接：https://www.jianshu.com/p/95d6ac54fc67  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
