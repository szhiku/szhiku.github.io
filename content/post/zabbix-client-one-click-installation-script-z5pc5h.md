---
title: zabbix客户端一键安装脚本
slug: zabbix-client-one-click-installation-script-z5pc5h
url: /post/zabbix-client-one-click-installation-script-z5pc5h.html
date: '2024-02-20 01:50:17'
lastmod: '2024-02-20 01:54:58'
toc: true
tags:
  - 运维
  - Linux
  - zabbix
categories:
  - 技术
keywords: 运维,Linux,zabbix
isCJKLanguage: true
---

# zabbix客户端一键安装脚本

　　本环境：centos7.9

　　简易安装脚本，`Server=192.168.2.111`​此处需要修改为server的ip地址

```bash
#!/bin/bash
# 关闭SELinux、暂停防火墙
setenforce 0
systemctl stop firewalld

# 安装Zabbix仓库和zabbix-agent zabbix-get
rpm -ivh https://mirrors.tuna.tsinghua.edu.cn/zabbix/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
sed -i 's#repo.zabbix.com#mirrors.tuna.tsinghua.edu.cn/zabbix#g' /etc/yum.repos.d/zabbix.repo
yum install -y zabbix-agent zabbix-get.x86_64 net-tools

# 配置zabbix-agent
cat << 'EOF' > /etc/zabbix/zabbix_agentd.conf
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
Server=192.168.2.111
ServerActive=127.0.0.1
Hostname=Zabbix server
Include=/etc/zabbix/zabbix_agentd.d/*.conf
EOF

# 启动zabbix-agent并检查
systemctl start zabbix-agent.service 
systemctl enable zabbix-agent.service
netstat -lntup | grep 10050

```
