---
title: Tomcat 的组成结构和主配置文件Server.xml详解
slug: >-
  tomcat-s-composition-structure-and-main-configuration-file-serverxml-detailed-explanation-15kcly
url: >-
  /post/tomcat-s-composition-structure-and-main-configuration-file-serverxml-detailed-explanation-15kcly.html
date: '2024-03-04 18:32:11'
lastmod: '2024-03-04 18:37:22'
toc: true
tags:
  - 运维
  - Linux
  - Tomcat
keywords: 运维,Linux,Tomcat
isCJKLanguage: true
---

# Tomcat 的组成结构和主配置文件Server.xml详解

> 参考：《TOMCAT与JAVA WEB开发技术详解  第3版》  
> https://www.jianshu.com/p/2789af11299f

　　Tomcat 本身由一系列可配置的组件构成，其中核心组件是 Servlet容器组件，它是所有 其他 Tomcat 组件的顶层容器，用＜CATALINA_HOME＞ 表示 Tomcat 的安装根目录。Tomcat 的各个组件可以在/conf7server.xml 文件中进行配 置，每个 Tomcat 组件在 server.xml 文件中对应一种配置元素。以下代码以 XML 的形式展示 了各种 Tomcat 组件之间的关系：

```html
	<Server>
		<Service>
			<Connector />
			<Engine>
				<Host>
					<Context>
					</Context>
				</Host>
			</Engine>
		</Service>
	</Server>
```

　　在以上 XML 代码中，每个元素都代表一种 Tomcat 组件。这些元素可分为四类：  
• 顶层类元素 包括〈Server〉元素和元素，它们位于整个配置文件的顶层。  
• 连接器类元素 为〈Connector〉元素，代表介于客户与服务器之间的通信接口，负责将客户的请求发送 给服务器，并将服务器的响应结果发送给客户。  
• 容器类元素，代表处理客户请求并生成响应结果的组件，有四种容器类元素，分别为〈Engine〉、<Host>、<Context> 和<Cluster>元素。Engine 组件为特定的 Service 组 件处理所有客户请求， Host 组件为特定的虚拟主机处理所有客户请求，Context 组件为特定的 Web 应用处理所有客 户请求。Cluster 组件负责为 Tomcat 集群系统进行会话复制、Context 组件的属性的复制，以 及集群范围内 WAR 文件的发布。本书第 26 章的 26.5 节（Tomat 集群）对配置 Tomcat 集群 系统做了进一步介绍。  
• 嵌套类元素 代表可以嵌入到容器中的组件，如<Valve>元素和<Realm>元素等，这些元素的作用将在 后面的章节做介绍

　　Tomcat 的组成结构是由 自身的实现决定的，与 Servlet 规 范无关。不同的服务器开发商可以用不同的方 式来实现符合 Servlet 规范的 Servlet 容器。

　　下面，再对一些基本的 Tomcat 元素进行介绍。具体属性，可以 参照本书附录 A（server.xml 文件）：  
• 元素 〈Server〉元素代表整个 Servlet 容器组件，它是 Tomcat 的顶层元素。〈Server〉元素中可 包含一个或多个元素。  
• <Service>元素＜Service＞元素中包含一'个＜Engine＞元素，以及一个或多个＜（201111©以01＞元素，这些 ＜Connector＞元素共享同一个＜Engine＞元素  
• ＜Connector＞元素 ＜Connector＞元素代表和客户程序实际交互的组件，它负责接收客户请求，以及向客户 返回响应结果。  
• ＜Engine＞元素 每个＜Service＞元素只能包含一个〈Engine〉元素。＜Engine＞元素处理在同一个＜Service＞ 中所有＜Connector＞元素接收到的客户请求。  
• ＜Host＞元素 一个〈Engine〉元素中可以包含多个＜Host＞元素。每个＜Host＞元素定义了一个虚拟主机， 它可以包含一个或多个 Web 应用。  
• ＜Context＞元素 ＜Context＞元素是使用最频繁的元素。每个＜Context＞元素代表了运行在虚拟主机上的单 个 Web 应用。一个＜Host＞元素中可以包含多个＜Context＞元素。

```bash
engine:核心容器组件,catalina引擎,负责通过connector接受用户请求,并处理请求,将请求转至对应的虚拟主机host
host:类似于httpd中的虚拟主机,一般而言支持基于FQDN的虚拟主机
context:定义一个应用程序,是一个最内层的容器类组件(不能再嵌套).篇日志context的主要目的指定对应的webapp的根目录,类似于httpd的alias,其还能为webapp指定额外的属性,如部署方式等.
connector:接收用户请求,类似于httpd的listen配置监听端口.
service(服务):将connector关联至engine,因此一个service内部可以有多个connector,但只能有一个引擎engine.service内部有两个connector,一个engine.因此一个service内部可以有多个connector.
server:表示一个运行于JVM中的tomcat实例
Valve:阀门,拦截请求并在将其转至对应的webapp前进行某种处理操作,可以用于任何容器中,比如记录日志(access log valve),基于IP做访问控制(remote address filter valve).
logger: 日志记录器,用于记录组件内部的状态信息,可以用于除context外的任何容器中.
realm:可以用于任意容器类的组件中,关联一个用户认证库,实现认证和授权.可以关联的认证库有两种:UserDatabaseRealm,MemoryRealm和JDBCRealm
UserDatabaseRealm:使用JNDI自定义的用户认证库.
MemoryRealm:认证信息定义在tomcat-users.xml中
JDBCRealm:认证信息定义在数据库中,并通过JDBC连接至数据库查找认证用户.
```

　　​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403041836542.png)  
​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403041836744.png)​

```bash
tomcat配置文件注释

<?xml version='1.0' encoding='utf-8'?>
<!--
<Server>元素代表整个容器,是Tomcat实例的顶层元素.由org.apache.catalina.Server接口来定义.它包含一个<Service>元素.并且它不能做为任何元素的子元素.
    port指定Tomcat监听shutdown命令端口.终止服务器运行时,必须在Tomcat服务器所在的机器上发出shutdown命令.该属性是必须的.
    shutdown指定终止Tomcat服务器运行时,发给Tomcat服务器的shutdown监听端口的字符串.该属性必须设置
-->
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
  <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>
  <!--service服务组件-->
  <Service name="Catalina">
    <!--
    connector：接收用户请求，类似于httpd的listen配置监听端口.
        port指定服务器端要创建的端口号，并在这个端口监听来自客户端的请求。
        address：指定连接器监听的地址，默认为所有地址（即0.0.0.0）
        protocol连接器使用的协议，支持HTTP和AJP。AJP（Apache Jserv Protocol）专用于tomcat与apache建立通信的， 在httpd反向代理用户请求至tomcat时使用（可见Nginx反向代理时不可用AJP协议）。
        minProcessors服务器启动时创建的处理请求的线程数
        maxProcessors最大可以创建的处理请求的线程数
        enableLookups如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名，若为false则不进行DNS查询，而是返回其ip地址
        redirectPort指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号
        acceptCount指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理
        connectionTimeout指定超时的时间数(以毫秒为单位)
    -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <!--engine,核心容器组件,catalina引擎,负责通过connector接收用户请求,并处理请求,将请求转至对应的虚拟主机host
        defaultHost指定缺省的处理请求的主机名，它至少与其中的一个host元素的name属性值是一样的
    -->
    <Engine name="Catalina" defaultHost="localhost">
      <!--Realm表示存放用户名，密码及role的数据库-->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>
      <!--
      host表示一个虚拟主机
        name指定主机名
        appBase应用程序基本目录，即存放应用程序的目录.一般为appBase="webapps" ，相对于CATALINA_HOME而言的，也可以写绝对路径。
        unpackWARs如果为true，则tomcat会自动将WAR文件解压，否则不解压，直接从WAR文件中运行应用程序
        autoDeploy：在tomcat启动时，是否自动部署。
        xmlValidation：是否启动xml的校验功能，一般xmlValidation="false"。
        xmlNamespaceAware：检测名称空间，一般xmlNamespaceAware="false"。
      -->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <!--
        Context表示一个web应用程序，通常为WAR文件
            docBase应用程序的路径或者是WAR文件存放的路径,也可以使用相对路径，起始路径为此Context所属Host中appBase定义的路径。
            path表示此web应用程序的url的前缀，这样请求的url为http://localhost:8080/path/****
            reloadable这个属性非常重要，如果为true，则tomcat会自动检测应用程序的/WEB-INF/lib 和/WEB-INF/classes目录的变化，自动装载新的应用程序，可以在不重启tomcat的情况下改变应用程序
        -->
        <Context path="" docBase="" debug=""/>
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
    </Engine>
  </Service>
</Server>

```
