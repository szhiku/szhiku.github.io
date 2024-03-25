---
title: free命令
slug: free-command-sg6k1
url: /post/free-command-sg6k1.html
date: '2024-03-25 13:21:19'
lastmod: '2024-03-25 13:29:11'
toc: true
tags:
  - 运维
categories:
  - 技术
keywords: 运维
isCJKLanguage: true
---

# free命令

```bash
语　　法： free [-bkmotV][-s <间隔秒数>]
补充说明：free 指令会显示内存的使用情况，包括实体内存，虚拟的交换文件内存，共享内存区段，以及系统核心使用的缓冲区等。
参　　数：
-b 　以 Byte 为单位显示内存使用情况。
-k 　以 KB 为单位显示内存使用情况。
-m 　以 MB 为单位显示内存使用情况。
-o 　不显示缓冲区调节列。
-s <间隔秒数> 　持续观察内存使用状况。
-t 　显示内存总和列。
-V 　显示版本信息。
 
常用操作:

free // 以KB为单位，显式系统内存使用情况
free -ml -s 1  //每秒以M为单位，显式系统内存详细使用情况。
free -c 4 -s 2  //为KB为单位，每2秒显式系统内存使用情况，一共显示4次

```

　　​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202403251329256.png)​

　　Mem 行(第二行)是内存的使用情况。  
Swap 行(第三行)是交换空间的使用情况。  
total 列显示系统总的可用物理内存和交换空间大小。  
used 列显示已经被使用的物理内存和交换空间。  
free 列显示还有多少物理内存和交换空间可用使用。  
shared 列显示被共享使用的物理内存大小。  
buff/cache 列显示被 buffer 和 cache 使用的物理内存大小。  
available 列显示还可以被应用程序使用的物理内存大小。  
————————————————

　　                            版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

　　原文链接：https://blog.csdn.net/weixin_42832313/article/details/96484017
