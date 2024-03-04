---
title: Tomcat安全配置、管理规范整理
slug: tomcat-security-configuration-management-specifications-1u324h
url: /post/tomcat-security-configuration-management-specifications-1u324h.html
date: '2024-03-05 01:31:29'
lastmod: '2024-03-05 01:48:07'
toc: true
tags:
  - 运维
  - Linux
  - Tomcat
categories:
  - 技术
keywords: 运维,Linux,Tomcat
isCJKLanguage: true
---

# Tomcat安全配置、管理规范整理

> 参考：  
> Tomcat安全管理规范：https://blog.csdn.net/wuxingge/article/details/116517777  
> tomcat 安全规范(tomcat安全加固和规范)：https://www.jb51.net/article/173899.htm  
> [omcat安全配置规范](https://www.cnblogs.com/klslb/p/9232176.html)
>
> 以及个人从业经验

　　以下是整理后的Tomcat安全管理配置规范，具体实施方法可以百度：

### 第1章：账号管理与认证授权

#### 1.1 账号管理

* <span style="font-weight: bold;" class="bold">共享账号管理</span>：避免不同用户间共享账号，确保每个用户有独立的账号。
* <span style="font-weight: bold;" class="bold">无关账号管理</span>：删除或锁定与设备运行、维护无关的账号。

#### 1.2 口令管理

* <span style="font-weight: bold;" class="bold">密码复杂度</span>：密码长度至少8位，包含数字、小写字母、大写字母和特殊符号中的至少2类。
* <span style="font-weight: bold;" class="bold">密码历史</span>：支持按天配置口令生存期，不长于90天。

#### 1.3 用户权利指派

* 根据用户业务需求，配置其所需的最小权限。

### 第2章：日志配置操作

#### 2.1 日志配置

* <span style="font-weight: bold;" class="bold">审核登录</span>：配置日志功能，记录用户登录信息，包括账号、登录成功与否、登录时间以及远程登录时的IP地址。

### 第3章：IP协议安全配置

#### 3.1 IP协议

* <span style="font-weight: bold;" class="bold">支持加密协议</span>：支持使用HTTPS等加密协议进行远程维护。

### 第4章：设备其他配置操作

#### 4.1 安全管理

* <span style="font-weight: bold;" class="bold">定时登出</span>：支持定时账户自动登出，用户需重新登录。
* <span style="font-weight: bold;" class="bold">更改默认端口</span>：更改Tomcat服务器默认端口，避免使用默认端口。
* <span style="font-weight: bold;" class="bold">错误页面处理</span>：自定义错误页面，避免显示敏感信息。
* <span style="font-weight: bold;" class="bold">目录列表访问限制</span>：禁止Tomcat列出目录内容。

#### 4.2 其他安全措施

* <span style="font-weight: bold;" class="bold">版本安全</span>：升级到最新稳定版，避免跨版本升级。
* <span style="font-weight: bold;" class="bold">服务降权</span>：使用普通用户启动Tomcat，避免使用root用户。
* <span style="font-weight: bold;" class="bold">端口保护</span>：更改管理端口和AJP端口，避免使用默认端口。
* **禁用管理端**：删除默认的`tomcat-users.xml`​文件和`webapps`​目录下的默认文件。
* **隐藏Tomcat版本信息**：修改`ServerInfo.properties`​文件，隐藏版本信息。
* <span style="font-weight: bold;" class="bold">关闭WAR自动部署</span>：关闭Tomcat的WAR包热部署功能。
* **脚本权限回收**：控制`CATALINA_HOME/bin`​目录下的脚本文件权限。
* <span style="font-weight: bold;" class="bold">访问日志格式规范</span>：开启访问日志中的Referer和User-Agent记录。
* <span style="font-weight: bold;" class="bold">访问限制</span>：通过配置限定访问的IP来源。

### 附录：建议配置及标准执行方案

* **配置文件**：提供`server.xml`​和`web.xml`​的配置示例。
* **删除默认文件**：删除`webapps`​和`conf/tomcat-user.xml`​等默认文件。
* **权限设置**：修改`tomcat/bin`​目录下的脚本文件权限，防止未授权的起停操作。

　　请注意，这些规范是基于Tomcat 6.*版本的，不同版本的Tomcat可能需要不同的配置方法。
