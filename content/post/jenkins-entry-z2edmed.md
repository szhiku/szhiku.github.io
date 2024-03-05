---
title: Jenkins入门
slug: jenkins-entry-z2edmed
url: /post/jenkins-entry-z2edmed.html
date: '2024-03-05 12:32:45'
lastmod: '2024-03-05 12:34:21'
toc: true
tags:
  - 运维
  - Linux
  - Jenkins
categories:
  - 技术
keywords: 运维,Linux,Jenkins
isCJKLanguage: true
---

# Jenkins入门

# 第0章 Jenkins介绍

```css
1.jenkins是一个开源的持续集成工具，由java语言开发
2.jenkins是一个调度平台，拥有众多的插件，绝大部分功能都是由插件来完成的
```

# 第1章 Jenkins安装

## 1.官方网站

```cpp
https://www.jenkins.io/zh/
https://www.jenkins.io/zh/doc/
```

## 2.安装部署

　　清华源直接下载rpm包安装即可,下载地址如下：

```ruby
https://mirrors.tuna.tsinghua.edu.cn/jenkins/redhat/
```

　　安装命令如下：

```css
rpm -ivh jdk-8u181-linux-x64.rpm 
rpm -ivh jenkins-2.176.1-1.1.noarch.rpm 
```

## 3.目录文件说明

```csharp
[root@jenkins ~]# rpm -ql jenkins 
/etc/init.d/jenkins                         #启动文件
/etc/logrotate.d/jenkins                #日志切割脚本
/etc/sysconfig/jenkins                  #配置文件
/usr/lib/jenkins                                #安装目录
/usr/lib/jenkins/jenkins.war        #安装包
/usr/sbin/rcjenkins                     
/var/cache/jenkins                      
/var/lib/jenkins                                #数据目录
/var/log/jenkins                                #日志目录
```

## 4.配置使用root账户运行

```bash
vim /etc/sysconfig/jenkins
JENKINS_USER="root"
```

## 5.启动jenkins

```undefined
systemctl start jenkins
```

## 6.解锁Jenkins

　　​![](//upload-images.jianshu.io/upload_images/14248468-f8377d8d5e65bca3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 7.修改admin密码

　　​![](//upload-images.jianshu.io/upload_images/14248468-029c4923840f031f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 8.使用清华源作为插件地址

　　清华源地址：

```ruby
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

　　配置步骤：

　　​![](//upload-images.jianshu.io/upload_images/14248468-8d1e28f31d72f26d.png?imageMogr2/auto-orient/strip|imageView2/2/w/640/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-06f7ea12c181ebd7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-93f127f665a19e60.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-d7e4774e660ac4c6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 9.使用离线安装插件

　　我们可以将插件提前下好，然后只需要解压到jenkins对应的目录即可

```csharp
tar zxf jenkins_plugins.tar.gz -C /var/lib/jenkins/
ll /var/lib/jenkins/plugins/
```

　　重启jenkins：

```undefined
systemctl restart jenkins
```

# 第2章 构建自由风格的项目

## 1.创建新任务

　　​![](//upload-images.jianshu.io/upload_images/14248468-ed796fd4c6731efd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-16d0fb9d088a29dc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-04f8873f53b2706a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 2.添加构建步骤

　　​![](//upload-images.jianshu.io/upload_images/14248468-bc678db89fcde269.png?imageMogr2/auto-orient/strip|imageView2/2/w/678/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-068ac56f2e216e3c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 3.点击立即构建

　　​![](//upload-images.jianshu.io/upload_images/14248468-419a3587730ecb19.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 4.查看控制台输出

　　​![](//upload-images.jianshu.io/upload_images/14248468-ffb395226b5d7609.png?imageMogr2/auto-orient/strip|imageView2/2/w/594/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-e5071f2a2003d5ec.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

# 第3章 发布gitlab中的静态项目

## 1.gitlab导入工程

　　这是一个h5小游戏的项目，项目地址：

```cpp
https://gitee.com/skips/game.git
```

　　使用gitlab直接导入项目：

　　​![](//upload-images.jianshu.io/upload_images/14248468-440b7c4b826702b3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-7578543bc750c1e9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-55a56065bbdd8af0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-38ed2676c80fe7e3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-468ae64cb8f93e3f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 2.在jenkins中关联gitlab的h5game项目

### 2.1 创建新项目

　　​![](//upload-images.jianshu.io/upload_images/14248468-cc6f8da51bb49d59.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-1ccf61fff7ea7545.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

### 2.2 填写仓库地址

　　选择源码管理，然后填写gitlab仓库信息，但是我们发现报错了，因为jenkins没有拉取gitlab项目的权限。

　　​![](//upload-images.jianshu.io/upload_images/14248468-7fba0e0ea61640cb.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 3.配置jenkins访问gitlab的权限

### 3.1 部署公钥解释和步骤

　　解释

```css
1.如果我们想让jenkins从gitlab上拉取代码，那么需要将jenkins的公钥信息放在gitlab上。
2.gitlab针对这种情况有一个专门的功能，叫做部署部署公钥。
3.部署公钥的作用是不需要创建虚拟用户和组，直接在需要拉取的项目里关联部署公钥即可。
```

　　步骤

```css
1.获取jenkins公钥信息
2.将jenkins公钥信息填写到gitlab的部署公钥里
3.由项目管理员操作，在需要jenkins拉取的项目里关联部署公钥
4.jenkins配置私钥凭证，部署项目时关联凭证
```

### 3.2 获取jenkins服务器的SSH公钥信息

```ruby
[root@jenkins-201 ~]# cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCg8+DQFOjR+gl1Xw83CIyGJ50vI4DBeTaMRFdu5+5pT/IMnYq1iS7/lRS6JxXLYvVeNMDUfDxA1sOL70okyA3npjASXgJPGE1FsbpqzWjsN0TAGoZkR1VWuP9Yn0CrH7dA4lhZQfUUVjvqzFBZK8N9iZMzIu6KOiSY/aD4Ol59vbDS4kO0rTG1DYQNnjZzMPNlIiJ+0EVkfuYRwABRFA8fmL+6btqZqhjGY29EHuIfzIMTDTysrtCTGxQn2ql1zwjReGiNXzmFncwvyy92DAuMbnOQiE1YNn72wThy2oWSHsCwKdIvcNHqY2xBvFnkZ9Ltga7PgR33kbJ7Gl8tjiZF root@jenkins-201
```

### 3.3 gitlab添加部署公钥

　　​![](//upload-images.jianshu.io/upload_images/14248468-8245335176b870e6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-911c35751356101b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-a15072f418867524.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

### 3.4 gitlab项目关联部署公钥

　　​![](//upload-images.jianshu.io/upload_images/14248468-923c0626e97cac54.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-269984bc18f7b63c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-295d87972e0f87d0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

### 3.5 jenkins配置私钥凭证

　　​![](//upload-images.jianshu.io/upload_images/14248468-be5e1d77e73b1f78.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-5ed3036f123ac2d8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-f2583b5486ae0c9d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

### 3.6 测试获取代码

　　​![](//upload-images.jianshu.io/upload_images/14248468-4714f458d20d2ea1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-505097d0d0014a40.png?imageMogr2/auto-orient/strip|imageView2/2/w/690/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-d14c12d0768d5a49.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　查看拉取的代码：

```csharp
[root@jenkins-201 ~]# ll /var/lib/jenkins/workspace/h5game
总用量 16
drwxr-xr-x 4 jenkins jenkins   47 8月   6 09:37 game
-rw-r--r-- 1 jenkins jenkins 9349 8月   6 09:37 LICENSE
-rw-r--r-- 1 jenkins jenkins  937 8月   6 09:37 README.md
```

## 4.编写部署脚本

```bash
#创建目录
mkdir -p /scripts/jenkins/

#编写脚本
cat > /scripts/jenkins/deploy.sh << 'EOF'
#!/bin/bash

PATH_CODE=/var/lib/jenkins/workspace/h5game/
PATH_WEB=/usr/share/nginx
TIME=$(date +%Y%m%d-%H%M)
IP=10.0.0.7

#打包代码
cd ${PATH_CODE} 
tar zcf /opt/${TIME}-web.tar.gz ./*

#拷贝打包好的代码发送到web服务器代码目录
ssh ${IP} "mkdir ${PATH_WEB}/${TIME}-web -p"
scp /opt/${TIME}-web.tar.gz ${IP}:${PATH_WEB}/${TIME}-web

#web服务器解压代码
ssh ${IP} "cd ${PATH_WEB}/${TIME}-web && tar xf ${TIME}-web.tar.gz && rm -rf ${TIME}-web.tar.gz"
ssh ${IP} "cd ${PATH_WEB} && rm -rf html && ln -s ${TIME}-web html"
EOF

#添加可执行权限
chmod +x /scripts/jenkins/deploy.sh
```

　　也可以使用jenkins内置的变量来代替自定义变量，查看jenkins内置变量的地址如下：

```cpp
http://10.0.0.201:8080/env-vars.html
```

## 4.jenkins调用构建脚本

　　在构建的位置填写执行shell脚本的命令

　　​![](//upload-images.jianshu.io/upload_images/14248468-4213a8e519fbafe2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　然后立即构建，发现报错了，提示权限不足：

　　​![](//upload-images.jianshu.io/upload_images/14248468-8802e9763ff10980.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　报错原因是因为jenkins是以jenkins用户运行的，所以提示权限不足，我们可以修改jenkins为root用户运行。

```csharp
[root@jenkins-201 ~]# vim /etc/sysconfig/jenkins 
[root@jenkins-201 ~]# grep "USER" /etc/sysconfig/jenkins 
JENKINS_USER="root"
[root@jenkins-201 ~]# systemctl restart jenkins
```

　　重启好之后我们重新构建一下：

　　​![](//upload-images.jianshu.io/upload_images/14248468-0d66ead3b64b327b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　查看一下web服务器的代码目录

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
drwxr-xr-x 3 root root 50 8月   6 10:13 20200806-1013-web
lrwxrwxrwx 1 root root 17 8月   6 10:13 html -> 20200806-1013-web
```

# 第4章 监听gitlab自动触发构建

## 1.jenkins项目里添加构建触发器

　　​![](//upload-images.jianshu.io/upload_images/14248468-728449bb72b3352f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 2.gitlab添加webhook

　　将刚才jenkins里配置的token和URL地址复制进去

　　​![](//upload-images.jianshu.io/upload_images/14248468-c009f8dedb04f9d4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　较新版本的gitlab此时点击添加会提示报错：

　　​![](//upload-images.jianshu.io/upload_images/14248468-d698109232b97afd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　解决方法：进入admin area区域，然后点击setting-->network进行设置

　　​![](//upload-images.jianshu.io/upload_images/14248468-909c35138faab3bd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　正常添加成功之后，会在下方出现测试的选项

　　​![](//upload-images.jianshu.io/upload_images/14248468-e8a0a5b65af14e9d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　选择push事件来测试是否可以正常出发，如果可以，会在页面上方显示200状态码

　　​![](//upload-images.jianshu.io/upload_images/14248468-ac106fe6813583ec.png?imageMogr2/auto-orient/strip|imageView2/2/w/726/format/webp)​

　　此时我们去查看jenkins项目页面，就会发现以及出发了来自gitlab的构建任务

　　​![](//upload-images.jianshu.io/upload_images/14248468-5fee98a1052e320e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

# 第5章 返回构建状态给gitlab

## 1.gitlab生成access token

　　​![](//upload-images.jianshu.io/upload_images/14248468-a764ae4dacddafcc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　点击创建之后会生成一串token,注意及时保存，因为刷新就没有了

　　​![](//upload-images.jianshu.io/upload_images/14248468-7a92c08e19cebc72.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 2.jenkins配置gitlab的token

　　点击jenkins的系统管理-->系统设置，然后找到gitlab选项

　　​![](//upload-images.jianshu.io/upload_images/14248468-a9151d975fb91b50.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　填写gitlab的信息：

　　​![](//upload-images.jianshu.io/upload_images/14248468-82b90e6c5901eadf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　点击添加后返回上一层页面，然后选中刚才添加的gitlab凭证

　　​![](//upload-images.jianshu.io/upload_images/14248468-d1a197f4b240325e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 3.设置项目构建后将结果通知给gitlab

　　​![](//upload-images.jianshu.io/upload_images/14248468-6b6bda4a7153b110.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 4.合并分支然后检查gitlab能否收到消息

　　​![](//upload-images.jianshu.io/upload_images/14248468-c7d923d1edfcc146.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 5.防止重复构建

　　jenkins具有很多内置变量，点击项目-->构建--> 查看 可用的环境变量列表

```cpp
http://10.0.0.201:8080/env-vars.html/
```

　　这里我们使用两个变量，一个是此次提交的commit的hash，另一个是上一次提交成功的commit的hash

　　我们可以在部署脚本里添加一行判断，如果这两个变量一样，那么就不用再次提交了

　　这些变量不需要在脚本里定义，直接引用即可

　　​![](//upload-images.jianshu.io/upload_images/14248468-ae7d6674496200fe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

# 第6章 tag方式发布版本

## 1.给代码打标签

　　首先我们先给代码打上标签，然后提交2个版本

　　v1.0版本：修改代码，然后发布v1.0版本

```bash
git commit -am 'v1.0'
git tag -a v1.0 -m "v1.0 稳定版"
git push -u origin v1.0
git tag
```

　　v2.0版本：修改代码，然后发布v2.0版本

```bash
git commit -am 'v2.0'
git tag -a v2.0 -m "v2.0 稳定版"
git push -u origin v2.0
git tag
```

## 2.gitlab查看标签

　　此时gitlab上可以看到2个标签

　　​![](//upload-images.jianshu.io/upload_images/14248468-8b64cc7a62c08e76.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　点进去之后可以看到具体标签名称

　　​![](//upload-images.jianshu.io/upload_images/14248468-4674a81262ed034b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 3.jenkins配置参数化构建

　　jenkins上我们新建一个参数化构建项目

　　​![](//upload-images.jianshu.io/upload_images/14248468-1443f148892eb0da.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　然后配置git的标签参数:

　　​![](//upload-images.jianshu.io/upload_images/14248468-153e800fcee67e3b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　最后还需要配置一下git仓库地址,注意需要修改拉取的版本的变量为 $git_version

　　​![](//upload-images.jianshu.io/upload_images/14248468-d887d7dad72788ed.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　此时点击项目的build with parameters就会看到对应的版本号：

　　​![](//upload-images.jianshu.io/upload_images/14248468-80a89d94d1158457.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　然后去jenkins工作目录下查看是否拉取了对应版本:

```csharp
/var/lib/jenkins/workspace/my-deploy-rollback
```

## 4.优化部署脚本

```bash
cat >/scripts/jenkins/deploy_rollback.sh<<'EOF'
#!/bin/bash

PATH_CODE=/var/lib/jenkins/workspace/my-deploy-rollback/
PATH_WEB=/usr/share/nginx
IP=10.0.0.7

#打包代码
cd ${PATH_CODE} 
tar zcf /opt/web-${git_version}.tar.gz ./*

#拷贝打包好的代码发送到web服务器代码目录
ssh ${IP} "mkdir ${PATH_WEB}/web-${git_version} -p"
scp /opt/web-${git_version}.tar.gz ${IP}:${PATH_WEB}/web-${git_version}

#web服务器解压代码
ssh ${IP} "cd ${PATH_WEB}/web-${git_version} && tar xf web-${git_version}.tar.gz && rm -rf web-${git_version}.tar.gz"
ssh ${IP} "cd ${PATH_WEB} && rm -rf html && ln -s web-${git_version} html"
EOF
```

## 5.jenkins添加执行脚本动作并测试

　　​![](//upload-images.jianshu.io/upload_images/14248468-550d6e21c5836a80.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 6.测试发布

　　​![](//upload-images.jianshu.io/upload_images/14248468-179825d3e082e54c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　然后去web服务器上查看发现已经发布了

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
lrwxrwxrwx 1 root root  8 8月   6 15:59 html -> web-v2.0
drwxr-xr-x 3 root root 68 8月   6 15:59 web-v2.0
```

# 第7章 tag方式回滚版本

## 1.jenkins配置回滚选项参数

　　在工程配置里添加选项参数:

　　​![](//upload-images.jianshu.io/upload_images/14248468-cb57cd24349fb385.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　在参数化构建里添加2个选项：发布和回滚

　　​![](//upload-images.jianshu.io/upload_images/14248468-635aa525f63635c8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　此时查看构建页面就会发现多了选项卡:

　　​![](//upload-images.jianshu.io/upload_images/14248468-8dbc88c910f9aec2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 2.修改发布脚本增加条件判断

```bash
cat >/scripts/jenkins/deploy_rollback.sh <<'EOF'
#!/bin/bash

PATH_CODE=/var/lib/jenkins/workspace/my-deploy-rollback/
PATH_WEB=/usr/share/nginx
IP=10.0.0.7

#打包代码
code_tar(){
        cd ${PATH_CODE} 
        tar zcf /opt/web-${git_version}.tar.gz ./*
}

#拷贝打包好的代码发送到web服务器代码目录
code_scp(){
        ssh ${IP} "mkdir ${PATH_WEB}/web-${git_version} -p"
        scp /opt/web-${git_version}.tar.gz ${IP}:${PATH_WEB}/web-${git_version}
}

#web服务器解压代码
code_xf(){
        ssh ${IP} "cd ${PATH_WEB}/web-${git_version} && tar xf web-${git_version}.tar.gz && rm -rf web-${git_version}.tar.gz"
}

#创建代码软链接
code_ln(){
        ssh ${IP} "cd ${PATH_WEB} && rm -rf html && ln -s web-${git_version} html"
}

main(){
        code_tar
        code_scp
        code_xf
        code_ln
}

#选择发布还是回滚
if [ "${deploy_env}" == "deploy" ]
then
        ssh ${IP} "ls ${PATH_WEB}/web-${git_version}" >/dev/null 2>&1
        if [ $? == 0 -a ${GIT_COMMIT} == ${GIT_PREVIOUS_SUCCESSFUL_COMMIT} ] 
        then
                echo "web-${git_version} 已部署,不允许重复构建"
                exit
        else 
                main
        fi
elif [ "${deploy_env}" == "rollback" ]
then
        code_ln
fi
EOF
```

## 3.测试回滚功能

　　部署v1.0版本

　　​![](//upload-images.jianshu.io/upload_images/14248468-0fb97bee362054f6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　部署v2.0版本：

　　​![](//upload-images.jianshu.io/upload_images/14248468-7556f99c43166f34.png?imageMogr2/auto-orient/strip|imageView2/2/w/1190/format/webp)​

　　检查web服务器当前的版本

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
lrwxrwxrwx 1 root root  8 8月   6 16:52 html -> web-v2.0
drwxr-xr-x 3 root root 68 8月   6 16:51 web-v1.0
drwxr-xr-x 3 root root 68 8月   6 16:52 web-v2.0
```

　　然后我们选择v1.0版本并且选择回滚操作：

　　​![](//upload-images.jianshu.io/upload_images/14248468-6087d515bd66d430.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　查看控制台显示回滚成功：

　　​![](//upload-images.jianshu.io/upload_images/14248468-19a944d32ff5572e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　在web服务器上查看发现已经回滚成功：

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
lrwxrwxrwx 1 root root  8 8月   6 16:56 html -> web-v1.0
drwxr-xr-x 3 root root 68 8月   6 16:51 web-v1.0
drwxr-xr-x 3 root root 68 8月   6 16:52 web-v2.0
```

## 4.发布新代码并打标签测试

　　修改代码并发布v3.0:

```bash
cd h5game/
echo v3.0 >> index.html
git commit -am 'v3.0'
git tag -a v3.0 -m 'v3.0 稳定版'   
git push -u origin v3.0
git tag
```

　　jenkins查看并发布3.0版本:

　　[图片上传失败...(image-a1e6e7-1598955937011)]

　　web服务器查看发布情况：

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
lrwxrwxrwx 1 root root  8 8月   6 16:58 html -> web-v3.0
drwxr-xr-x 3 root root 68 8月   6 16:51 web-v1.0
drwxr-xr-x 3 root root 68 8月   6 16:52 web-v2.0
drwxr-xr-x 3 root root 68 8月   6 16:58 web-v3.0
```

　　然后工程选择回滚到v2.0版本:

　　​![](//upload-images.jianshu.io/upload_images/14248468-cb002e65ff2e61ff.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　再次在web服务器上查看:

```csharp
[root@web-7 ~]# ll /usr/share/nginx/
总用量 0
lrwxrwxrwx 1 root root  8 8月   6 16:59 html -> web-v2.0
drwxr-xr-x 3 root root 68 8月   6 16:51 web-v1.0
drwxr-xr-x 3 root root 68 8月   6 16:52 web-v2.0
drwxr-xr-x 3 root root 68 8月   6 16:58 web-v3.0
```

# 第8章 打标签工程

## 1.jenkins新建打标签工程

　　​![](//upload-images.jianshu.io/upload_images/14248468-d6949b9013bd7998.png?imageMogr2/auto-orient/strip|imageView2/2/w/1062/format/webp)​

## 2.配置参数化构建

　　​![](//upload-images.jianshu.io/upload_images/14248468-563b942422e1c2fd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 3.配置工程的源码管理

　　​![](//upload-images.jianshu.io/upload_images/14248468-fbd3e7309543f7c0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

## 4.配置构建动作

　　​![](//upload-images.jianshu.io/upload_images/14248468-8b4a07f3c0a49694.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　配置命令：

```bash
git tag -a $git_newversion -m "$git_newversion 稳定版"
git push -u origin $git_newversion
```

## 5.发布新版本并提交到master

```css
echo "v4.0" >> index.html 
git add .
git commit -m "v4.0 稳定版"
git push -u origin v4.0
```

## 6.jenkins使用打标签工程来打标签

　　​![](//upload-images.jianshu.io/upload_images/14248468-f8493f594917ebab.png?imageMogr2/auto-orient/strip|imageView2/2/w/1092/format/webp)​

　　查看控制台输出发现提示没有权限:

　　​![](//upload-images.jianshu.io/upload_images/14248468-a2171db53067508e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　我们需要在gitlab上把deploy key设置允许写入

　　​![](//upload-images.jianshu.io/upload_images/14248468-6c2a9f469057641b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)​

　　​![](//upload-images.jianshu.io/upload_images/14248468-55ed2349bc13109d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)​

　　然后重新构建

```undefined

```

　　返回到deploy-rollback工程查看可以发现已经生成了新标签，然后我们开始构建：

　　​![](//upload-images.jianshu.io/upload_images/14248468-569a33f00f8aa90e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1008/format/webp)​

　　在web服务器上查看可以发现新版本已经发布成功了：

```csharp
[root@web ~]# ll /usr/share/nginx/
总用量 20
lrwxrwxrwx 1 root root    8 5月  13 16:09 html -> web-v4.0
drwxr-xr-x 8 root root 4096 5月  13 16:05 web-
drwxr-xr-x 8 root root 4096 5月  13 15:00 web-v1.0
drwxr-xr-x 8 root root 4096 5月  13 15:00 web-v2.0
drwxr-xr-x 8 root root 4096 5月  13 15:37 web-v3.0
drwxr-xr-x 8 root root 4096 5月  13 16:09 web-v4.0
```

　　作者：被运维耽误的厨子  
链接：https://www.jianshu.com/p/a9d607ec3a05  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
