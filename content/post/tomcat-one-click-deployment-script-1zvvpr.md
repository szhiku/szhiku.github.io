---
title: Tomcat一键部署脚本
slug: tomcat-one-click-deployment-script-1zvvpr
url: /post/tomcat-one-click-deployment-script-1zvvpr.html
date: '2024-03-04 11:59:14'
lastmod: '2024-03-04 17:59:54'
toc: true
isCJKLanguage: true
---

# Tomcat一键部署脚本

　　环境：centos7、已安装wget、已换阿里源

　　简易脚本，抛砖引玉

#### Tomcat一键部署脚本

```html
#!/bin/bash

# 安装Java
yum install java-1.8.0 -y

# 检查Java是否安装成功
java -version
if [ $? -ne 0 ]; then
    echo "Java安装失败，请检查后重试。"
    exit 1
fi

# 创建软件目录
mkdir -p /data/soft

# 切换到软件目录
cd /data/soft

# 下载Tomcat安装包
wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.tar.gz

# 解压Tomcat安装包到/opt目录
tar zxf apache-tomcat-8.5.99.tar.gz -C /opt/

# 创建软链接
ln -s /opt/apache-tomcat-8.5.99 /opt/tomcat

# 配置环境变量
echo 'export TOMCAT_HOME=/opt/tomcat' >> /etc/profile
source /etc/profile

# 启动Tomcat
/opt/tomcat/bin/startup.sh

# 检查Tomcat是否启动成功
if [ $? -eq 0 ]; then
    echo "Tomcat安装并启动成功。"
else
    echo "Tomcat启动失败，请检查日志。"
    exit 1
fi

# 检查Tomcat服务是否正在运行
ps -ef | grep tomcat

# 显示Tomcat日志
tail -f /opt/tomcat/logs/catalina.out

# 脚本结束
```
