---
title: tomcat管理
slug: tomcat-management-ztwwto
url: /post/tomcat-management-ztwwto.html
date: '2024-03-04 18:26:01'
lastmod: '2024-03-04 18:28:55'
toc: true
isCJKLanguage: true
---

# tomcat管理

　　测试功能,生产环境不要用  
Tomcat管理功能用于对Tomcat自身以及部署在Tomcat的应用管理的web应用,在默认的情况下处于禁止状态的.如果需要开启这个功能,就要配置管理用户,即配置前面说过的tomcat-user.xml  
下面是命令集合:  
修改tomcat-users.xml

```bash
[root@sweb01 ~]# cat /opt/tomcat/conf/tomcat-users.xml    
<?xml version='1.0' encoding='utf-8'?>
<!--
     这是个优雅的注释
-->
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
  <role rolename="admin-gui"/>
  <role rolename="host-gui"/>
  <role rolename="manager-gui"/>
  <user username="tomcat" password="tomcat" roles="admin-gui,host-gui,manager-gui"/>
</tomcat-users>
```

　　修改context.xml文件

```bash
[root@sweb01 ~]# grep "10" /opt/tomcat/webapps/manager/META-INF/context.xml  
         allow="10\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
```

　　重新启动tomcat

```bash
[root@sweb01 ~]# /opt/tomcat/bin/shutdown.sh 
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
[root@sweb01 ~]# /opt/tomcat/bin/startup.sh 
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Using CATALINA_TMPDIR: /opt/tomcat/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /opt/tomcat/bin/bootstrap.jar:/opt/tomcat/bin/tomcat-juli.jar
Tomcat started.
```

　　​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403041829156.png)​

　　作者：被运维耽误的厨子  
链接：https://www.jianshu.com/p/2789af11299f  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
