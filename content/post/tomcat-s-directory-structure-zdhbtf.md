---
title: Tomcat 的目录结构
slug: tomcat-s-directory-structure-zdhbtf
url: /post/tomcat-s-directory-structure-zdhbtf.html
date: '2024-03-04 18:05:57'
lastmod: '2024-03-04 18:13:34'
toc: true
tags:
  - 运维
  - Linux
  - Tomcat
keywords: 运维,Linux,Tomcat
isCJKLanguage: true
---

# Tomcat 的目录结构

　　Tomcat9.x 的目录结构参见 表 3-3, 表中的目录都是＜CATALINA_HOME＞ 的子目录。  
​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403041810472.png)​

　　在Linux系统中如下

```html
[root@sweb01 ~]# cd /opt/tomcat/
[root@sweb01 /opt/tomcat]# tree -L 1
├── bin             #用以启动,关闭Tomcat或其他脚本功能的脚本(.bat和.sh)
├── conf            #用以配置Tomcat的XML及DTD文件
├── lib             #存放web应用能访问的JAR包
├── logs            #Catalina和其他web应用程序的日志文件
├── temp            #临时文件
├── webapps         #Web应用程序根目录
└── work            #用以产生有JSP编译出的Servlet的.java和.class文件
```

　　webapps目录

```html
[root@sweb01 /opt/tomcat]# cd webapps/
[root@sweb01 /opt/tomcat/webapps]# ll
总用量 8
drwxr-x--- 14 root root 4096 8月  10 16:37 docs          #tomcat帮助文档
drwxr-x---  6 root root   78 8月  10 16:37 examples      #web应用
drwxr-x---  5 root root   82 8月  10 16:37 host-manager  #管理
drwxr-x---  5 root root   97 8月  10 16:37 manager       #管理
drwxr-x---  3 root root 4096 8月  10 16:37 ROOT          #默认网站根目录
```

　　bin目录

```html
脚本            作用
startup.sh           开启tomcat脚本
shutdown.sh          关闭tomcat脚本
catalina.shtomcat    核心管理脚本,以后jvm优化参数及相关配置,修改tomcat启动参数
```
