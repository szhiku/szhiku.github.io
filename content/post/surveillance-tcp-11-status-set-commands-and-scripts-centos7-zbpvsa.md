---
title: 监控TCP11种状态集命令、脚本（centos7）
slug: surveillance-tcp-11-status-set-commands-and-scripts-centos7-zbpvsa
url: /post/surveillance-tcp-11-status-set-commands-and-scripts-centos7-zbpvsa.html
date: '2024-02-26 21:57:25'
lastmod: '2024-02-26 22:08:12'
toc: true
tags:
  - 运维
  - Linux
  - TCP/IP
categories:
  - 技术
keywords: 运维,Linux,TCP/IP
isCJKLanguage: true
---

# 监控TCP11种状态集命令、脚本（centos7）

　　先决条件：  
用户有足够的权限来执行 `netstat`​ 命令。

　　不是root用户可能需要在命令前加上 `sudo`​。

　　安装了 `netstat`​。

　　命令合集，脚本：

```bash
#!/bin/bash

# 清除旧的输出
clear

# 显示所有TCP连接的状态
echo "所有TCP连接状态："
netstat -ant

# 显示LISTEN状态的连接
echo "处于LISTEN状态的连接："
netstat -an | grep LISTEN

# 显示SYN_SENT状态的连接
echo "处于SYN_SENT状态的连接："
netstat -an | grep SYN_SENT

# 显示SYN_RECEIVED状态的连接
echo "处于SYN_RECEIVED状态的连接："
netstat -an | grep SYN_RECEIVED

# 显示ESTABLISHED状态的连接
echo "处于ESTABLISHED状态的连接："
netstat -an | grep ESTABLISHED

# 显示FIN_WAIT1状态的连接
echo "处于FIN_WAIT1状态的连接："
netstat -an | grep FIN_WAIT1

# 显示FIN_WAIT2状态的连接
echo "处于FIN_WAIT2状态的连接："
netstat -an | grep FIN_WAIT2

# 显示CLOSE_WAIT状态的连接
echo "处于CLOSE_WAIT状态的连接："
netstat -an | grep CLOSE_WAIT

# 显示CLOSING状态的连接
echo "处于CLOSING状态的连接："
netstat -an | grep CLOSING

# 显示LAST_ACK状态的连接
echo "处于LAST_ACK状态的连接："
netstat -an | grep LAST_ACK

# 显示TIME_WAIT状态的连接
echo "处于TIME_WAIT状态的连接："
netstat -an | grep TIME_WAIT

# 显示CLOSED状态的连接
echo "处于CLOSED状态的连接："
netstat -an | grep CLOSED

# 注意：由于netstat的输出可能包含其他信息，上述命令可能需要进一步的过滤来精确匹配状态。
```

　　扩展：

> 以下是TCP的11种主要状态：
>
> 1. <span style="font-weight: bold;" class="bold">LISTEN（监听）</span> ： 服务器处于监听状态，等待客户端的连接请求。
> 2. <span style="font-weight: bold;" class="bold">SYN-SENT（同步已发送）</span> ： 客户端已发送一个连接请求（SYN，同步序列编号），正在等待服务器的响应。
> 3. <span style="font-weight: bold;" class="bold">SYN-RECEIVED（同步已接收）</span> ： 服务器已接收到客户端的连接请求，并发送了一个响应（SYN-ACK，同步和确认），正在等待客户端的最终确认。
> 4. <span style="font-weight: bold;" class="bold">ESTABLISHED（已建立）</span> ： 客户端和服务器之间的连接已成功建立，可以开始数据传输。
> 5. <span style="font-weight: bold;" class="bold">FIN-WAIT-1（终止等待1）</span> ： 发送方（客户端或服务器）已发送一个终止连接的请求（FIN，结束），但还在等待对方的确认。
> 6. <span style="font-weight: bold;" class="bold">FIN-WAIT-2（终止等待2）</span> ： 在FIN-WAIT-1之后，发送方收到了对方的确认，但还在等待对方发送的终止连接请求。
> 7. <span style="font-weight: bold;" class="bold">CLOSE-WAIT（关闭等待）</span> ： 接收方（客户端或服务器）已接收到对方的终止连接请求，但还没有发送自己的终止请求。
> 8. <span style="font-weight: bold;" class="bold">CLOSING（正在关闭）</span> ： 双方都已发送了终止连接的请求，但还没有完全关闭连接。
> 9. <span style="font-weight: bold;" class="bold">LAST-ACK（最后确认）</span> ： 发送方在收到对方的终止请求后，已发送了最终的确认，正在等待对方的连接完全关闭。
> 10. <span style="font-weight: bold;" class="bold">TIME-WAIT（时间等待）</span> ： 在发送方发送了最终的确认之后，会进入TIME-WAIT状态，等待一段时间（通常是2个最大段生命周期MSL），以确保对方能够接收到这个确认。
> 11. <span style="font-weight: bold;" class="bold">CLOSED（已关闭）</span> ： 连接已经完全关闭，没有任何数据传输正在进行。
