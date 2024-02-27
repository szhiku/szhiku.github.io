---
title: Zabbix学习路线及重难点摘录
slug: zabbix-learning-route-and-heavy-difficulties-excerpt-zovbcn
url: /post/zabbix-learning-route-and-heavy-difficulties-excerpt-zovbcn.html
date: '2024-02-17 23:18:11'
lastmod: '2024-02-28 00:12:26'
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

# Zabbix学习路线及重难点摘录

* zabbix相关书籍

  * Zabbix企业级分布式监控系统 (吴兆松) (Z-Library)
  * 深入理解Zabbix监控系统 (鲍光亚) (Z-Library)
* roadmap

  * 本文根据参考书籍目录得出roadmap并摘录要点为新手作指引，未完善的内容可自行百度学习
  * 参考书籍：Zabbix企业级分布式监控系统
  * 第1章 开篇——监控系统简介

    * ### 1.1 监控系统的功能概述（什么是监控系统？）

      监控系统，一是监测，二是控制，监测以达到控制的效果，在计算机领域，可以将其分为5种监控类型。

      * 应用性能监控（Application Performance Monitoring）。
      * 业务交易监控（Business Transaction Monitoring）。
      * 网络性能监控（Network Monitoring）。
      * 操作系统监控（System Monitoring）。
      * 网络站点监控（Website Monitoring）。

      大规模的监控环境，被监控节点多且监控类型多产生的数据和网络连接开销非常大，数据采集方式除了使用主动采集模式，还需要使用代理架构分摊服务器端的性能开销。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272055466.png)​

      通常监控系统会选用以下 几种数据存储方式。 

      （1）本地存储。使用本地磁盘，基于文件的方式存储。 

      （2）使用时序数据库进行数据存储，如古老的环状数据库 （Round Robin Database, RRD）等。近年来，随着时序数据技术的不断发展，出现了比较成熟的时序数据库，如OpenTSDB（底层存储基于 HBase）、Graphite、InfluxDB、Prometheus等，与直接使用文件的存储方式相比，这些时序数据库更加高效。  
      （ 3 ） 使 用 数 据 库 管 理 系 统 （ Database Management System, DBMS）进行数据存储，如常见的MySQL、Oracle、SQL Server等。使用 这种数据库来存储监控数据，当数据量达到一定规模时，其读/写效率 均会显著下降，数据库的压力比较大，通常优化方案思路有3种，一是 减少数据的存储量；二是优化数据库本身，调整配置参数，优化运行 环境；三是使用分布式数据库和数据库集群技术方案。  
      （4）使用NoSQL数据库进行数据存储。NoSQL相对于DBMS这种传统 的数据库有着一些天然的优势，单机的QPS通常较高。但NoSQL本身并 不是为监控系统设计的，在数据结构存储方面存在一些缺陷  
      （5）使用列存储数据库进行数据存储。列存储数据库由于其设计 之初专为大数据而有所考虑，故无须担心其存储容量，底层均有良好 的解决方案。但由于其部署、运维均较为复杂，故一般监控系统也不 会常采用这种技术作为数据库存储。这方面的数据库代表为HBase。  
      （6）使用全文搜索引擎数据库进行监控数据存储。这方面的代表 是Elasticsearch，其作为监控数据库存储监控数据具有天然的优势， 支持集群、分布式部署、容灾，并且集群能够提供较高的性能。目前 采用全文搜索引擎数据库进行监控数据存储，典型的代表是ELK套件，在Zabbix 4.0中可以选用 Elasticsearch作为数据库存储。

      监控系统的重要功能是根据设定的阈值进行告警，将不同级别的告警分成不同的梯度发送给不同的告警接收人。告警模块需要支持故障的有效汇报和集中汇报，尽量避免出现“告警风暴”，防止同一时间大量发送重复、类 似的告警，即告警功能支持对告警内容进行分析和自动处理，防止误报、漏报及抖动。

      事后还需要对告警信息进行统计分析，以方便衡量系统的稳定性、可用性。通常使用SLA服务质量指标来衡量。
    * ### 为什么要使用监控

      1.对系统不间断实时监控  
       2.实时反馈系统当前状态  
       3.保证服务可靠性安全性  
       4.保证业务持续稳定运行
    * ## 如果去到一家新公司，如何入手监控（怎么实现监控系统？新入职）

      1.硬件监控 路由器、交换机、防火墙  
       2.系统监控 CPU、内存、磁盘、网络、进程、 TCP  
       3.服务监控 nginx、 php、 tomcat、 redis、 memcache、 mysql  
       4.WEB 监控 请求时间、响应时间、加载时间、  
       5.日志监控 ELk（收集、存储、分析、展示） 日志易  
       6.安全监控 Firewalld、 WAF(Nginx+lua)、安全宝、牛盾云、安全狗  
       7.网络监控 smokeping 多机房  
       8.业务监控 活动引入多少流量、产生多少注册量、带来多大价值
    * ### 单机时代如何监控，比如我们需要监控磁盘的使用率

      CPU 监控命令: w、 top、 htop、 glances

      ```css
      %Cpu(s): 0.3 us, 0.3 sy, 0.0 ni, 99.3 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
      us 用户态: 跟用户的操作有关 35%
      sy 系统态: 跟内核的处理有关 60%
      id CPU 空闲:
      ```
      内存监控命令: free

      ```cpp
      [root@m01 ~]# free -h
                    total        used        free      shared  buff/cache   available
      Mem:           977M        105M        724M        6.6M        148M        729M
      Swap:          1.0G          0B        1.0G
      ```
      磁盘监控命令: df、 iotop

      ```undefined
      Device: tps kB_read/s kB_wrtn/s kB_read kB_wrtn
      sda 0.80 25.32 33.36 221034 291193
      设备名 每秒传输次数 每秒读大小 每秒写大小 读的总大小 写的总大小
      ```
      1.如何查看磁盘使用率 df -h  
       2.监控磁盘的那些指标 block、 inode  
       3.如何获取具体的信息 df -h | awk '{if(NR>1) print $6,$5}'  
       4.获取的数值到达多少报警 80%

      网络监控命令: ifconfig、 route、 glances、 iftop、 nethogs、 netstat

      ```php
      单位换算
      Mbps 100Mbps/8
      MB 12MB
      iftop 中间的<= =>这两个左右箭头，表示的是流量的方向。
      TX：发送流量、 RX：接收流量、 TOTAL：总流量
      #查看 TCP11 中状态
      netstat -an|grep ESTABLISHED
      netstat -rn # 查看路由信息
      netstat -lntup
      ```
      ###### 需求: 每隔 1 分钟监控一次内存,当你的可用内存低于 100m,发邮件报警,要求显示剩余内存 1.怎么获取内存可用的值 free -m|awk '/^Mem/{print $NF}' 2.获取到内存可用的值如何和设定的阈值进行比较 3.比较如果大于 100m 则不处理，如果小于 100 则报警 4.如何每隔 1 分钟执行一次

      ```bash
      [root@ZabbixServer ~]# cat free.sh
      #!/usr/bin/bash
      HostName=$(hostname)_$(hostname -i)
      Date=$(date +%F)
      while true;do
      Free=$(free -m|awk '/^Mem/{print $NF}')
      if [ $Free -le 100 ];then
      echo "$Date: $HostName Mem Is < ${Free}MB"
      fi
      sleep 5
      done
      ```
    * ### 1.2 监控系统的实现原理（怎么实现监控系统？原理）

      * 一个监控系统的组成大体可以分为两部分：数据采集部分（客户 端，Agent）和数据存储分析告警展示部分（服务器端，Server）
      * 监控系统数据采集可以分为两种：专用客 户端采集和公用协议采集（SNMP、IPMI、SSH、Telnet等）
      * 监控系统数据采集的工作模式可以分为被动模式（从服务器端到 客户端采集数据，对应的英文单词是pull）和主动模式（客户端主动 上报数据到服务器端，对应的英文单词是push）两种。一般来说，被动模式对监控控制端服务器的开销较大，适合小规 模的监控环境；主动模式对监控控制端服务器的开销较小，适合大规 模的监控环境。
    * ### 1.3 监控系统的开源产品（怎么实现监控系统？现有产品）

      在监控软件中，开源的解决方案有流量监控（MRTG、Cacti、 SmokePing 等 ） 、 性能告警 （ Nagios 、 Zabbix 、 Zenoss Core 、 Ganglia 、 Netdata 等 ） 、 基于时 序据库存储数据的监控 （Graphite、OpenTSDB、InfluxDB、Prometheus、OpenFalcon等）、 基于全文搜索引擎数据库存储数据的监控（如ELK套件）可供选择。上述各监控产品具有一些共同特征，例如 采集数据、分析展示、告警以及简单的故障自动处理等，最终都能达 到对IT系统服务可用性的完全展示。需要说明的是，当前的时序数据库侧重于监控数据的存储，其采集需要借助其他工具来实现。时序监控产品的设计理念较为先进，具有很多创新功能

      流行的监控工具

      1.Zabbix  
      2.Lepus(天兔)数据库监控系统  
      3.Open-Falcon 小米  
      4.Prometheus(普罗米修斯， Docker、 K8s)
  * 第2章 Zabbix简介/

    * 2.3 Zabbix是一个什么样的产品（Zabbix是什么？简介）

      * Zabbix是一个企业级的高度集成的开源监控软件，提供了分布式 监控解决方案，可以用来监控设备、服务等的可用性和性能。其产品不分企业版和社区版，是一个真正的源码开放产品，用户可以自由下载并使用该软件。
    * 2.4 为何选择Zabbix作为监控系统（为什么用Zabbix？）

      可自由修改发布(GPL2.0)，搭建环境、安装和配置简单，完全支持Linux、UNIX、Windows、AIX、BSD和 Solaris，C语言编写占用非常少性能速度非常好，开源社区支持良好，超强扩展能力只有想不 到的事情，没有办不到的事情

    * 2.1 Zabbix的用户群体都有谁（Zabbix谁来用？）

      * Zabbix自身的定位是中型企业和大型企业
      * 单个服务器节点可以支持10万台 设备的监控，其每秒可以处理5万次请求（NVPS在有历史数据和触发器 的情况下为5万次，而在无历史数据和触发器的情况下则可以达到单机 30万次）。
    * 2.2 使用Zabbix需要具备什么基础（Zabbix怎么用？所需基础）

      * 入门阶段：以前从未接触过任何监控系统，也不熟悉Linux操 作系统。在这个阶段，能够熟练地掌握Zabbix的安装和基本配 置即可。
      * 中级阶段：具备Linux基础，熟悉LAMP和LNMP环境搭建、MySQL 数据库、Shell脚本，以及具有简单的英文阅读能力，主要难点 在于触发器、数据库调优和API的使用。在这个阶段，读者可以 将Zabbix与其他系统进行集成对接。
      * 高级阶段：熟悉PHP语言或C语言，具备二次开发能力，能够修改源码，可以对Zabbix从代码级别进行优化和扩展。在这个阶段，读者一般都能熟练地掌握Zabbix的各个功能，已经从使用阶段到了源码级别的研究阶段，因此主要是对编程能力的要求。
    * 2.5 该选用Zabbix的哪个版本（Zabbix怎么用？版本选择）

      到目前2024年，4.0在中国应用广泛，向5.0过渡，6.0很少用
    * 2.6 Zabbix的架构是什么样的（Zabbix怎么用？架构）

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402190240492.png)​

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402271725253.png)​
    * 2.7 Zabbix的功能特性都有哪些（Zabbix是什么？功能特性）

      * 1.数据收集 · 可用性、性能检测。 · 支持Agent、SNMP（包括Trapping和Polling）、IPMI、JMX、 SSH、Telnet等。 · 自定义检测。 · 自定义收集数据的频率。 · 客户端/代理端/服务器端模式。
      *  2.灵活的触发器 可以定义非常灵活的告警阈值和与多种告警相关联的条件。
      * 3.高度可定制的告警 · 发送通知，可定制包括告警级别、动作升级、收件人和媒体类 型。 · 通知可以使用全局宏变量和自定义变量。 · 自动处理功能包括远程命令的自动调用和执行。
      * 4.实时的绘图功能 监控项将数据实时绘制在图形上。
      *  5.Web监控能力 Zabbix可以模拟浏览器请求访问一个网站，并检查返回值和响应时间。
      *  6.多种可视化展示 · 可以自定义监控的展示图，将多种监控数据集中展示到一张图 上。 · 网络拓扑图。 · 自定义的Screens和Slide shows可以将多种图形集中展示。 · 报表功能。 · 资源使用情况的监控展示。
      * 7.历史数据的存储 · 将数据存储在数据库中。 · 历史数据的存放周期可配置。 · 定期删除过期的历史数据。
      * 8.配置非常容易 配置比较简单，只需要以下两步即可。 （1）添加设备。 （2）应用模板即可完成监控。
      * 9.使用模板 · 模板可以分组。 · 模板具有可继承性。
      * 10.网络发现 · 支持自动发现网络设备和服务器（可以通过配置自动发现服务 规则实现）。 · Agent自动注册。 · 支持用自动发现（Low Level Discovery）实现动态监控项的 批量监控（支持自定义），内置的自动发现包括文件系统、网 络接口、SNMP OID，可定制自动发现。
      * 11.快速的访问接口 · Web页面基于PHP。 · 远程访问。 · 日志审计。
      * 12.API功能 应用API功能可以方便地与其他系统结合，包括手机客户端的使 用。
      * 13.系统权限 · 不同的用户展示监控的资源不同。 · 用户身份认证。
      * 14.程序特性 服务器端Zabbix-Server和采集端Zabbix-Agent使用C语言编写， 其性能非常高，内存开销非常小。 15.大型环境的支持 利用Zabbix-Proxy方式可轻松构建远程监
  * 第3章 安装与部署

    * 3.1 安装环境概述

      最小化的安装环境，磁盘I/O性能、数据库性能是系统良好运行的关键因素

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272055735.png)​

      所需磁盘空间：数据库写入的一个关键 指标是NVPS（New Values Per Second），NVPS值是指每秒处理的平均数据量，通过这个值可以计算出数据 存储所需的空间大小。原理为，每条数据都占用大约50B的存储空间， 因此NVPS×每条数据的平均大小=历史数据所需的空间大小。计算公式：历史数据所需的空间大小=天数x每秒处理的数据量x1天24小时x1小时3600sx50B

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272122907.png)​

      操作系统要求：

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272122110.png)​

      软件环境：

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272122355.png)​

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272123989.png)​

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272123112.png)​
    * 3.2 Zabbix-Server服务器端的安装

      * [zabbix服务端一键安装初始配置脚本](/post/the-zabbix-service-side-is-one-click-to-install-the-initial-configuration-script-9alkk.html)
      * Zabbix-Server安装配置步骤详解
    * 3.3 Zabbix-Agent客户端的安装

      * [zabbix客户端一键安装脚本](/post/zabbix-client-one-click-installation-script-z5pc5h.html)
    * 3.4 SNMP监控配置
    * 3.5 在Windows中安装Zabbix-Agent
    * 3.6 在其他平台安装Zabbix-Agent
    * 3.7 Zabbix-Get的使用、3.8 Zabbix相关术语（命令）
    * 3.9 Zabbix-Server对数据的存储

      * 3.9.1 监控数据的存储
      * 3.9.2 MySQL表分区实例
    * 3.10 高可用和安全
    * 3.11 Zabbix数据库备份

      * Zabbix数据库备份脚本
    * 3.12 升级Zabbix

      * 3.12.1 同版本升级的方法
      * 3.12.2 跨版本升级的方法
      * 3.12.3 数据库自动升级的原理
      * 3.12.4 升级失败的处理案例
    * 用docker快速部署zabbix
  * 第4章 快速配置和使用

    * 4.1 配置流程

      其完整的配置流程可以 简单描述为： Host groups（主机组）→Hosts（主机）、图形（模板链接）→Applications（监控 项组）→Items（监控项、模板链接）→Triggers（触发器、模板链接）→Problems（故 障）→Actions（处理动作）→User groups（用户组）→Users（用 户）→Medias（告警方式）→Action log（日志审计）。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402272123609.png)​
    * 4.2 添加主机组

      * 4.2.1 如何划分主机组

        在Zabbix的软 件设计规则中，已规定主机、模板必须属于一个分组。 对同一属性的主机或者模板应归类到相同组，相关原则如下：

        *  以地理位置纬度进行划分。
        * 以业务为单位划分组。
        * 以机器用途进行划分。
        *  以系统版本进行划分。
        * 以应用程序来划分组。
        * 其他方式等。
      * 支持对主机组进行层级分组
    * 4.3 添加模板
    * 4.4 添加主机
    * 4.5 配置图形
    * 4.6 配置大屏
    * 4.7 配置幻灯片
    * 4.8 配置地图（拓扑图）
    * 4.9 使用（IT）服务
    * 4.10 使用报表
    * 4.11 资产管理
    * 4.12 图形共享
    * 4.13 全局搜索
    * 4.14 最新数据
    * 4.15 故障
    * 4.16 数据的导入/导出

      * Zabbix支持将所有的配置导出为标准格式的XML文件，同样，也支 持导入标准格式的XML配置文件。
    * 4.17 用户权限

      * 用户类型主要有3类，分别为超级管 理员、管理员和普通用户。还有一类特殊用户，即匿名用户guest。
    * 4.18 调试模式

      * Zabbix的调试模式（Debug mode）主要用于前后端的调试，如果 想知道对某个页面的查询使用了哪些SQL语句，则可以打开调试模式
    * 4.19 与LDAP对接

      * Zabbix本身具有认证体系，还支持与LDAP集成来实现身份的统一 认证。
    * 4.20 维护模式

      * 在某些场合中，我们不需要告警，例如业务的正常维护，所以此 时维护时间功能特别有用。
    * 4.21 故障确认

      * 故障确认（Problem acknowledgement）功能用于人为干预故障， 当某个故障发生时，我们无法立即解决，但是已经知晓了该故障，即 先响应后彻底消除。为方便故障处理流程的转动，我们就可以使用这 一功能。
    * 4.22 批量更新
  * 第5章 处理监控指标数据

    * 5.1 添加新的监控项（Items）
    * 5.2 监控指标的自定义
    * 5.3 Zabbix内置的监控方式

      Zabbix 4.0支持的监控方式如下：

      * Zabbix agent check

        * Agent用于从Zabbix-Agent采集数据，其工作方式分为被动模式和主动模式两种

          ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006501.png)​
      * SNMP agent check
      * SNMP trap
      *  IPMI check
      * Simple check 

        * Simple check监控方式是Zabbix-Server主动发起的对外请求，所 以请求的源地址是Zabbix-Server服务器的IP地址，目标地址为远程IP 地址，这样我们就可以不用安装任何Agent，即可对一些常见的服务协 议进行监控了

          ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005049.png)​
      * VMware monitoring
      * Log file monitoring
      * Calculated item
      * Zabbix internal check
      * SSH check
      * Telnet check
      * External check
      * Aggregate check
      * Trapper item
      * JMX monitoring
      * ODBC check
      * Dependent items
      * HTTP check
    * 5.3.3 日志监控方式

      Zabbix-Agent支持对日志文件的监控，可以对日志的关键字进行 监控，然后告警。日志监控支持普通的日志文件，也支持日志轮询、 切割的文件。当日志中出现特殊的字符串（例如，警告、报错等字符 串）时，可以发送通知给用户。为了使日志监控能够正常使用，必须 满足以下条件： · 

      * Zabbix-Agent必须运行，且工作方式为主动模式。 ·
      * 日志的Item必须设置，必须指定文件名。 ·
      * Zabbix-Agent有读取日志的权限，如日志位于/home/用户名/ 文件中，则会因为权限的问题而导致无法读取到文件。

      注意：Zabbix日志监控必须工作于主动模式下，在Web前端配置的主机名 （ 宏 为 {HOST.HOST} ） 必须和Zabbix-Agent端 zabbix_agentd.conf中的Hostname值是一致的，并且这个Hostname值具有唯一性；否则，在主动模式下是无法正常采集到数据的。
    * 5.3.4 计算型监控方式

      计算型（Calculated）监控方式是在Zabbix-Server端对历史数据 进行统计分析的一个功能，例如，可以求主机总的磁盘容量、网络流 量等。经过计算后的指标值，如配置了该Item的Trigger，则同样可以 触发告警阈值进行告警。
    * 5.3.5 聚合型监控方式

      聚合型（Aggregate）监控方式是指将已经存储在数据库中的监控 指标数值进行二次统计运算，从而形成新的监控指标。利用这个功 能，我们很容易做到对一组已有的监控指标进行再次统计求值，如计 算一组或多组服务器的CPU平均iowait、计算一组或多组服务器 的/data分区总的容量。
    * 5.3.6 内部检测监控方式

      内部检测用于监控Zabbix自身的性能数据，可以监控ZabbixServer和Zabbix-Proxy，选择监控方式为“Zabbix internal”，添加 相应的Item key，即可完成监控。
    * 5.3.7 SSH监控方式

      在默认情况下，Zabbix-Server并不知道我们使用哪个SSH密钥来 连接服务器，因此需要指定SSH密钥的位置。由于使用RPM包安装的 Zabbix-Server，其用户家目录在/var/lib/zabbix目录下面，因此我 们将SSH密钥目录设置为/var/lib/zabbix/.ssh
    * 5.3.8 Telnet监控方式

      Telnet监控方式，Zabbix-Server使用Telnet协议来获取数据。为 什么要支持这么古老的方式呢？ 因为在某些场景下，有些设备只提供 了Telnet方式来获取数据，所以这个时候Telnet监控方式就有用武之地了。
    * 5.4 监控项指标数据的预处理

      * 5.4.1 预处理概述 

        预处理（Preprocessing），指在Zabbix-Server接收到监控项数 据之后，在入库之前再次处理的过程，这意味着可以从客户端采集任 意格式的数据，大大提高了数据处理的灵活性。例如，我们可以从客 户端采集JSON格式的数据，然后通过预处理功能进行值的分割，将符 合规则的数据入库。采用预处理功能可以批量获取数据，如图5-43所 示，左侧是通过预处理功能，一次可以获取多个指标数据；右侧是以 一问一答方式轮询获取数据的
      * 5.4.2 预处理的运行流程

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005230.png)​
      * 5.4.3 预处理的数据类型
    * 5.5 配置宏（变量）

      * 5.5.1 全局宏
      * 5.5.2 模板宏
      * 5.5.3 主机宏
      * 5.5.4 监控项宏
    * 5.6 配置值映射

      值映射（Value mapping）功能是指将监控指标取到的数据替换为 文本进行展示，如将数值1替换为available，则表示在监控指标取到 数值1时，在界面中将显示为available。
  * 第6章 精通告警配置

    * 6.1 告警流程

      Host groups（设备组）→Hosts（设备）→Applications（监控 项组）→Items（监控项）→Triggers（触发器）→Actions（告警动 作）→Medias（告警方式）→User groups（用户组）→Users（用 户）。

      Zabbix告警的配置步骤如下： （1）配置Trigger。 （2）配置用户。 （3）配置告警介质。 （4）配置Action。
    * 6.2 告警触发器（Trigger）的配置
    * 6.3 告警处理的配置

      * 6.3.1 如何发送告警

        客户端定期采集需要监控的指 标，然后发送给Zabbix-Server服务器，Zabbix-Server对数据进行存 储和分析，如果指标的数据值满足告警表达式规则，则会触发一个告 警，但告警并不会主动通知给用户，在Zabbix-Web界面中我们可以看 到故障告警的显示。如果此故障不仅要在Zabbix-Web界面中展示，同 时也要发送给用户，这时就需要在Zabbix-Web界面的Action页面中对 告警信息进行规则匹配，并将其发送给不同的用户。

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005459.png)​
      * 6.3.2 Action功能概述

        Action中文翻译为“动作”，如果从中文字义来理解它，则确实 很难理解。其实可以这么理解，Action就是指发生了什么事件，根据 一定的条件采取什么措施，可以为发送消息，也可以为执行命令。通 过这样的解释，相信读者对Action的理解就不会很难了。 Action 的 事 件 来 源 有 4 种 ， 包 括 ： Triggers （ 触 发 器 ） 、 Discovery（网络自动发现）、Auto registration（Agent主动注册） 和Internal（内部事件）
    * 6.4 邮件告警配置
    * 6.5 自定义脚本告警

      6.5.1 自定义脚本告警的原理 自定义脚本在/etc/zabbix/zabbix_server.conf中，配置语句如 下：

      ```bash
      AlertScriptsPath=/etc/zabbix/alertscripts
      ```
      Zabbix-Server在调用脚本时会传递参数给脚本，在Zabbix 3.0以 下版本中，默认传递3个参数，即收件人、主题和内容；在Zabbix 3.0 以上版本中，则可以自定义多个参数传递给脚本
    * 6.6 邮件告警脚本的配置
    * 6.7 告警升级机制

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005581.png)​

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005668.png)​
    * 6.8 触发器标签配置
    * 6.9 手动关闭告警
    * 6.10 如何取消告警发送
    * 6.11 如何删除故障信息
    * 6.12 告警聚合

      告警聚合则基于告警对告警数据 进行分析。这种功能属于监控系统的高级功能，在大规模环境中，这 种功能是不可缺少的，其重要性比监控系统本身更重要。比如一天人 均收到几百个告警，但如果都是同一类型，那么接收告警的人员会对 告警产生免疫效果，时间一长，对告警消息处理的灵敏度就会下降。 因此在设置告警时，可以通过触发器函数本身来调整触发器依赖的配 置，最终的解决办法还是告警聚合系统在发挥作用。 在Zabbix 4.0中，告警聚合的原理是通过对每个事件打标签，然 后匹配标签来关闭告警。
    * 6.13 告警配置故障排查
  * 第7章 探究告警触发器

    * 7.1 Trigger函数的意义
    * 7.2 Trigger函数的分类
    * 7.3 Trigger函数——比较与查找
    * 7.4 Trigger函数——计算
    * 7.5 Trigger函数——时间
    * 7.6 Trigger函数——日志
    * 7.7 Trigger函数——字符串匹配
    * 7.8 Trigger函数——趋势预测
    * 7.9 参考资料
  * 第8章 剖析监控方式

    * 8.1 Zabbix支持的监控方式

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280005201.png)​
    * 8.2 Zabbix监控方式的逻辑

      Zabbix支持多种监控方式，其获取数据的方式可以分为三类：一 是Zabbix-Server主动对外请求数据；二是外部主动发送数据给 Zabbix-Server；三是Zabbix-Server内部进行数据计算，对已有数据 进行重新计算分析，如图8-4所示。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006674.png)​
    * 8.3 Zabbix-Agent的工作模式

      * Zabbix-Agent的工作模式可以分为主动模式（Active）和被动模 式（Passive），下文会详细剖析各自的工作原理。 主动模式是指Zabbix-Agent将采集到的数据主动推送给ZabbixServer，其行为是Zabbix-Agent向Zabbix-Server主动发起的数据连接 过 程 ， Zabbix-Server 不 必 等 待 Zabbix-Agent 的 数 据 采 集 行 为 ， Zabbix-Agent能够一次批量发送多条数据给Zabbix-Server，属于一对 多的响应模式，对Zabbix-Server的性能开销较少，适合大规模环境使 用。 被动模式是指Zabbix-Server向Zabbix-Agent请求数据，ZabbixAgent被动接受数据请求后进行回应，属于一对一的响应模式。比如有 100个监控项，Zabbix-Server需要向Zabbix-Agent请求100次，同时， Zabbix-Agent在响应Zabbix-Server时，对监控项数据采集也需要消耗 时间，此时，Zabbix-Server只能耗着时间安静地等待Zabbix-Agent。 相对于主动模式的高效，被动模式从时间开销和发送数据量上都处于 劣势，对Zabbix-Server的性能开销较大，适合小规模环境使用。 主动模式和被动模式在同一个Zabbix-Agent上，是可以共存的， 比如让一部分监控项处于被动模式，另一部分处于主动模式。处于主 动模式的监控项，由Zabbix-Agent周期性地采集数据传输给ZabbixServer；处于被动模式的监控项，则由Zabbix-Server周期性地从 Zabbix-Agent获取数据。
      * 被动模式的工作流程 被动模式的工作流程总结如下： （1）Zabbix-Server打开一个TCP连接。 （2）Zabbix-Server发送一个key为agent.ping\n的请求。 （3）Zabbix-Agent接受这个请求，然后响应数据＜HEADER＞＜ DATALEN＞。 （4）Zabbix-Server对接收到的数据进行处理。 （5）关闭TCP连接。
      * Zabbix-Agent主动向Zabbix-Server发送请求的工作流程 在主动模式中，Zabbix-Agent在启动时就会向Zabbix-Server发送 请求，以获取需要主动监控的监控项。这部分的运行流程总结如下： （1）Zabbix-Agent向Zabbix-Server建立一个TCP连接。 （2）Zabbix-Agent请求需要检测的数据列表。 （ 3 ） Zabbix-Server 响 应 Zabbix-Agent ， 发 送 一 个 Item 列 表 （Item key、Delay）。 （4）Zabbix-Agent响应请求。 （5）完成本次会话后关闭TCP连接。 （6）Zabbix-Agent开始周期性地采集数据
      * Zabbix-Agent发送数据给Zabbix-Server的工作流程 当Zabbix-Agent将监控项数据采集完成之后，会将数据发送给 Zabbix-Server。这部分的运行流程总结如下： （1）Zabbix-Agent向Zabbix-Server建立一个TCP连接。 （2）Zabbix-Agent将数据发送给Zabbix-Server，其发送周期等 于Item的更新周期。 （3）Zabbix-Server处理Zabbix-Agent发送的数据。 （4）关闭TCP连接。
    * 8.4 Zabbix-Trapper（zabbix sender）监控方式

      Zabbix-Trapper 监 控 方 式 可 以 一 次 批 量 发 送 数 据 给 ZabbixServer，与主动模式不同，Zabbix-Trapper可以让用户控制数据的发 送，而不用Zabbix-Agent进程控制，这意味着可以使用Linux定时任 务 ， 或 者 借 助 其 他 程 序 调 用 Zabbix-Trapper 发 送 数 据 给 ZabbixServer。由于可以一次批量发送更多的数据（为防止数据过大导致内 存溢出，在Zabbix 4.0中Zabbix协议明确限制单次传输数据的大小最 多为128MB），因此这种方式的性能比Zabbix-Agent主动模式会更好。 在 Zabbix-Trapper 工 作 模 式 中 ， Zabbix 发 送 数 据 的 程 序 是 zabbix_sender。
    * 8.5 SNMP监控方式

      使用SNMP可以监控路由器、交换机、打印机、UPS或者其他开启 SNMP的设备，如果要支持SNMP监控方式，需要Zabbix-Server在编译源 码时带上--with-net-snmp参数。

      * 8.5.1 SNMP协议概述
      * 8.5.2 SNMP协议的工作方式
      * 8.5.3 SNMP协议的工作原理
    * 8.6 SNMPTraps监控方式

      * 8.6.1 SNMPTraps的概念

        SNMPTraps是SNMP的被管理端向SNMP管理端主动发送数据的过程， 管理端使用UDP 162端口接收被管理端发送的数据，这个功能通常用于 故障推送、信息发送等场景中，以便管理端能够在被管理端发生事件 时及时接收到相关信息。
      * 8.6.2 SNMPTraps的工作原理

        在Zabbix-Server服务器中，SNMPTraps使用snmptrapd进程接收 SNMP被管理端发送过来的数据，然后使用snmptt对接收到的数据进行 处理，写入日志文件中，Zabbix-Server中的snmptrap处理进程对日志 进行处理，符合Zabbix的数据格式要求，后续对数据进行阈值判断、 告警等处理。

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006326.png)​
    * 8.7 IPMI监控方式

      * 8.7.1 IPMI的概念

        IPMI（Intelligent Platform Management Interface）即智能平 台管理接口，原本是Intel架构中企业系统的周边设备所采用的一种工 业标准，后来成为业界通用的标准。 用户可以利用IPMI监视服务器的物理特征，如温度、电压、电扇 工作状态、电源供应以及机箱入侵等。
      * 8.7.2 IPMI的特性

        IPMI独立于CPU BIOS和OS自行运行，允许管理者在缺少操作系 统、系统管理软件或受监控的系统关机但接通电源的情况下仍能远端 管理服务器硬件。IPMI也能在操作系统启动后活动，与系统管理功能 一并使用时还能提供加强功能，IPMI只定义架构和接口格式成为标 准，具体操作时可能会有所不同。
    * 8.8 JMX监控方式

      JMX（Java Management Extensions，Java管理扩展）是Java平台 上为应用程序、设备、系统等植入管理功能的框架。JMX可以跨越一系 列异构操作系统平台、系统体系结构和网络传输协议，灵活地开发无 缝集成的系统、网络和服务管理应用。 JMX可以获取Java应用程序的性能数据，因此，我们可以直接通过 JMX协议对Java应用程序内部进行深入的监控

      * 8.8.1 JMX在Zabbix中的运行流程

        在Zabbix中，对JMX监控数据的获取由专门的代理程序来完成，即 由Zabbix-Java-Gateway负责数据的采集。Zabbix-Java-Gateway采集 Java应用程序（已开启JMX协议）的性能数据，然后把采集到的数据发 送给Zabbix-Server

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006226.png)​
    * 8.9 HTTP agent监控方式

      HTTP是互联网中应用最广泛的一种网络协议，主要有HTTP和HTTPS 两种，在HTTP agent监控方式中，是由Zabbix-Server主动对外发起请 求的，然后目标URL响应请求。除支持Zabbix-Server主动发起请求 外，HTTP agent还支持Zabbix-Porxy主动对目标URL发起请求（即Item 位于Proxy模式的主机中），不需要Agent即可采集到数据。
    * 8.10 Web监控方式

      Web监控方式是用来监控Web程序的，可以监控Web程序的下载速 度、返回码及响应时间，还支持将一组连续的Web动作作为一个整体进 行监控。

      * 8.10.1 Web监控的原理

        Web监控即对HTTP服务的监控，模拟用户访问网站，对状态码、返 回字符串等特定的数据进行比较和监控，从而判断网站Web服务的可用 性。很多时候，我们可以用脚本、程序来进行自定义监控，如Linux下 的命令程序curl、wget，其他语言提供的http库等都可以帮助我们完 成这一需求。
    * 8.11 Dependent item监控方式
    * 8.12 ODBC监控方式
    * 8.13 其他监控方式
    * 8.14 命令执行的监控方式

      * 8.14.1 system.run 

        system.run是Zabbix-Agent的一个key，在默认情况下这个key是 不 能 使 用 的 。 若 要 使 用 这 个 key ， 需 要 在 /etc/zabbix/zabbix_agentd.conf 中 将 EnableRemoteCommands=0 修 改为EnableRemoteCommands=1（如图8-73所示）。注意，如果没有远 程执行命令需要这个key，那么为了安全考虑，可以将其关闭，在关闭 后，Action中的远程命令也将无法使用；如果将其开启，则务必保证 Zabbix-Server和Zabbix-Agent之间的通信安全，防止被黑客劫持而造 成损失。
  * 第9章 分布式监控与自动化

    * 9.1 Zabbix-Proxy分布式监控

      Zabbix是一个分布式的监控系统，即采用一个中心点、多个分节 点的部署模式，适合于跨机房、跨地域的网络环境。 Zabbix-Proxy的典型工作环境如下： · 监控远程区域，如异地区域。 · 监控网络非直连的区域，如处于NAT模式下的设备。 · 分担Zabbix-Server服务器的负载，在Zabbix-Proxy中预先处 理数据。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006970.png)​

      Zabbix-Proxy支持的监控功能和Zabbix-Server差不多，不同的 是，对于需要在数据库层面计算的监控方式，Zabbix-Proxy不支持， 其他方式均支持，详细支持情况如表9-1所示。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280006814.png)​

      Zabbix-Proxy和Zabbix-Server之间的数据通信，需要一个TCP端 口连接，为保证数据传输的安全，可以在网络防火墙层面设置允许 Zabbix-Proxy和Zabbix-Server（默认10051端口）进行通信。 Zabbix-Proxy将采集到的数据传送给Zabbix-Server之前保存在本 地数据库中。这样的功能设计保证了不会因为Zabbix-Proxy临时与 Zabbix-Server中断而造成监控数据丢失。Zabbix-Proxy配置文件中的 ProxyLocalBuffer和ProxyOfflineBuffer参数控制数据在本地保存的 时间。
    * 9.2 监控的自动化功能

      首先，Zabbix提供了网络自动发现功能，该功能可以基于FTP、 SSH 、 Web 、 LDAP 、 POP3 、 IMAP 、 SMTP 、 TCP 、 SNMP 、 Telnet 、 zabbix_agent等，主动扫描网络中的协议和服务，当它们存在时，即 认为主机和设备存在，表示该IP地址存活，而是否添加到监控，则由 Action来决定。在Zabbix中，网络自动发现和自动注册都具有以上提 到的功能。 其次，Zabbix提供了对多变的监控项自动发现监控的功能。比 如，服务器有两块网卡，再增加两块网卡，那么新增加的两块网卡如 何做到自动监控？再比如磁盘分区、硬盘设备等，它们存在不确定的 因素，一台服务器可能只有一块硬盘，也可能有多块，那么如何做到 自动监控？如上问题，都可以用Zabbix的LLD功能轻松解决。即对于监 控项中具有相同的属性，但存在部分变量配置不同的情况，完成自动 添加监控项。 基于Zabbix的这两个功能，我们可以实现： · 自动添加主机、模板，自动分组，自动添加监控项、触发器 等。 · 自动添加监控项中有规律的“变量”。 Zabbix监控的自动化功能如图9-4所示。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007907.png)​
    * 9.3 网络自动发现

      Zabbix的网络自动发现是一个非常强大的功能，利用该功能可以 完成以下工作。 · 快速发现并添加主机。 · 进行简单的管理。 · 随着环境的改变而快速搭建监控系统。 网络自动发现基于以下信息： · IP地址段。 · 基于服务的FTP、SSH、Web、POP3、IMAP、TCP等。 · 从Zabbix-Agent接收到的信息。 · 从SNMP agent接收到的信息。 网络自动发现功能不能做到的事情是发现网络拓扑图。 网络自动发现的两个工作流程是：Discovery和Action。
    * 9.4 主动方式的自动注册功能

      主动方式的自动注册（Active agent auto-registration）功能 主要用于Agent主动且自动向Server注册,即Agent处于主动模式，主动 向Zabbix-Server发送数据进行注册，如图9-8所示。与网络自动发现 具有同样的功能，都能够实现将设备自动添加到监控系统中。但是主 动方式的功能更适合于在特定的环境中使用，当一个条件未知（这里 的未知条件包括Agent端的IP地址段、Agent端的操作系统版本等信 息）时，仍然可以实现自动添加监控。它特别适合于当前云环境下的 监控，在云环境中IP地址分配、操作系统版本等都可能随机，该功能 可以很好地解决类似的问题。
    * 9.5 监控项自动发现功能

      在Zabbix中，由于监控指标都是基于模板进行的，因此会存在模 板的通用性问题。比如OS层面的监控，一台服务器具有多块网卡、多 个分区、多块磁盘、多个进程端口等需要监控，因为Zabbix提供了 LLD（Low Level Discovery）功能，可以实现一个监控项原型生成多 个监控项的场景
    * 9.6 使用自动化工具SaltStack批量部署Zabbix

      在实际的生产环境中，会对Zabbix进行大规模的部署、运维和管 理，此时一套集中的配置管理工具是必需的。自动化部署软件包、管 理配置文件等开源的工具有Chef、Puppet、SaltStack、Ansible等， 本节介绍SaltStack的安装与配置，其他工具的安装与配置与之类似。

      * 9.6.1 使用SaltStack配置管理Zabbix

        SaltStack是一个管理配置工具，其作用是为系统管理人员提供标 准 化 的 配 置 管 理 和 命 令 执 行 ， 架 构 为 通 用 的 Client/Server （ Master/Minion ， 服 务 器 / 客 户 端 ） 或 Client/Proxy/Server（Master/Sync-dic/Minion，服务器/代理/客户 端），通信采用证书认证方式，开发语言为Python，提供API，二次开 发 较 容 易 ， 可 以 运 行 在 多 种 平 台 上 ， 其 官 方 网 址 为 http://www.saltstack.com。 注意：一般采用自动化运维工具进行的配置管理，其软件都应该 采用标准化安装，如RPM包安装，需要有软件仓库。因此， 前提是将Zabbix放在指定的软件仓库中，定制RPM包，相关 内容可参考第15章。
  * 第10章 监控功能案例

    * 10.1 监控TCP连接状态
    * 10.2 监控Nginx
    * 10.3 监控PHP-FPM
    * 10.4 监控MySQL
    * 10.5 监控物理服务器
    * 10.6 监控物理机磁盘
    * 10.7 监控Cisco路由器
    * 10.8 监控VMware
    * 10.9 监控RabbitMQ
    * 10.10 监控Elasticsearch
    * 10.11 监控Kafka
    * 10.12 监控Redis
    * 10.13 监控Oracle数据库
    * 10.14 监控W ebLogic
    * 10.15 监控SQL Server
    * 10.16 监控HTTPS证书过期
  * 第11章 监控数据可视化

    * 11.1 Grafana
    * 11.2 Graphtrees
    * 11.3 谷歌浏览器告警插件
    * 11.4 Mac App的使用
    * 11.5 手机App的使用
    * 11.6 导出实时监控数据
    * 11.7 网络拓扑自动发现
    * 11.8 监控数据可视化的意义
    * 11.9 总结
  * 第12章 监控性能优化

    * 12.1 Zabbix性能优化概述

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007972.png)​

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007737.png)​

      Zabbix性能低下的一些表现如下： · Zabbix 队 列 中 有 太 多 被 延 迟 的 Item ， 可 以 通 过 Administration→Queue查看。 · Zabbix绘图中经常出现断图，一些Item没有数据。 · 带有nodata()函数的触发器出现告警。 · 前端页面无响应，或者响应很慢。 解决方案如下： · 不使用默认模板，定制自己的模板。 · 调优数据库，使用分布式数据解决方案，使用最新稳定版本， 如MySQL 5.6/5.7/8.0，一般来说，版本越新，其性能越好。 · 优化架构，如使用分布式架构，各服务器功能独立。 · 调优Item、Trigger。 · 更换更好的硬件，如硬盘采用PCI-E SSD。
    * 12.2 Zabbix性能优化依据

      Zabbix-Server的缓存监控情况如图12-3所示

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007221.png)​

      监 控 指 标 是 剩 余 量 ， 如 果 剩 余 量 非 常 小 ， 则 可 以 调 大 zabbix_server.conf中的缓存参数，直到这里的相关指标数据的剩余 量大于0为止。 Zabbix-Server性能的监控指标有以下两个： · 每秒处理数据的数量。 · 等待的队列数量。 监控Zabbix-Server内部的进程运行情况，如图12-4所示。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007081.png)​

      查看等待的队列，如图12-5所示。

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280007115.png)​

      等待的队列越多，说明Zabbix-Server性能越差。

      衡量Zabbix-Server服务处理性能的一个指标是每秒新处理的数据 个数，英文缩写是NVPS。在Zabbix 4.0中，从zabbix_server进程端口 中获取NVPS，查看前端代码
    * 12.3 Zabbix配置文件参数的优化

      在Zabbix-Server中，调整配置文件中的参数，可以设置的内容如 下： · 进程的数量。 · 缓存的大小。 · 超时时间。
    * 12.4 Zabbix架构的优化

      Zabbix架构有以下两种模式： · Server/Agent模式。 · Serve/Proxy/Agent模式。 通过采用分布式模式，可以大大降低服务器的负担，从而大大提 升单台服务器的性能。
    * 12.5 Item的工作模式及Trigger的优化

      Zabbix中的Item默认工作于被动模式下，但可以通过设置主动模 式来提升服务器的性能。 Item除可以采用主动模式外，还可以采用Trapper工作模式，通过 zabbix_sender程序发送数据，此种模式也具有很高的性能（请参考本 书8.4节）。 Trigger中正则表达式函数last()、nodata()的处理速度是最快 的，min()、max()、avg()的处理速度是最慢的，尽量使用处理速度快 的函数，并根据实际情况来权衡使用哪个函数。 在Trigger的配置中，需要注意一些表达式，如果配置失误，则很 可能会由于一个函数的逻辑错误而导致数据库查询较慢。
    * 12.6 Zabbix数据库的优化

      Zabbix数据库的优化分为以下几个部分。 · 对数据库本身的优化。采用更高性能的数据库版本，如选择 Percona（在最新版本的性能对比中，和MySQL官方版本相比， 性能相差不是特别的大），选用最新版本的MySQL。 · 对数据库本身的参数进行调优配置。 · 对Zabbix数据库结构进行优化，采用诸如表分区的方案，表分 区在删除一个区间的数据时速度非常快，如删除200GB的数据， 也只需要1分钟。因此Zabbix数据库结构优化一般就采用表分区 的方式，方法是对history. *、trends.* 等表进行分表操作，从 而降低Zabbix数据库的压力。具体实施方法，请读者参考第3章 的内容。 · 采用数据库中间件的方式。 在进行分表操作时，我们是直接关闭Housekeeper的，因为MySQL 执行DELETE（删除）操作非常低效，而采用表分区删除却非常高效。 zabbix_server.conf中的LogSlowQueries参数设置是关于慢查询 的，比如将超过1000ms的查询记录到日志中，就将其值设置为1000。 开启这个参数可以对慢查询继续记录，方便调优配置。 更 多 MySQL 参 数 的 配 置 ， 读 者 可 以 参 考 https://github.com/zabbix-book/MySQL_conf，此处省略相关配置， 因为本书的主体不是MySQL配置的优化。
    * 12.7 Zabbix运行硬件的优化

      通常，在优化软件无效的情况下，更换成更好的物理硬件会有非 常明显的效果，如选用RAID10、SSD固态硬盘、多核CPU、大容量内存 等硬件配置，如果使用云主机，则更换为高配即可。
    * 12.8 Zabbix压力测试

      12.8.1 压力测试原理 

      Zabbix-Server对Zabbix-Agent的监控项取值时间间隔，最小值限 制为1s（但是对于zabbix_sender，则可以取小于1s的值存储时间间 隔）。因此，我们通过创建大量的监控项，让监控项1s更新一个数 据，对同一个Zabbix-Agent频繁地大量获取数据，即可模拟出真实的 压力情况。
    * 12.9 Zabbix-Server内部实现原理

      * Zabbix-Server的整体流程

        在Zabbix内部的进程中，进程和缓存之间是相互关联的。或许读 者会有这样的疑问：为什么Zabbix使用关系型数据库存储配置数据， 但速度却不慢，而自己写程序直接读取关系型数据库速度却不够理 想？为什么Zabbix没有使用RabbitMQ这样专业的消息队列？为什么 Zabbix没有使用第三方的NoSQL缓存？这是因为Zabbix具有良好的软件 架构设计，在内部通过高性能的算法实现了队列、缓存等技术。当 然，只有一层缓存是不够的，还需要通过分层的缓存设计，让不同层 级的缓存解决不同的问题，最终让程序尽量少地访问关系型数据库。 在Zabbix中，缓存有以下几类。 （1）配置缓存：将主机组、主机、模板、全局宏、主机宏、监控 项、触发器表达式、Action的条件和动作、告警聚合条件和动作等的 数据同步到内存中。 （2）历史数据缓存：将采集到的监控数据，经过预处理进程处理 后进行缓存。 （3）趋势数据缓存：将趋势数据存储到内存中。 （4）指标数据缓存：数据归档，将历史数据归档为趋势数据时， 从指标数据缓存中读取；触发告警事件，对需要告警的监控指标进行 判断，如涉及历史数据的告警指标判断，会从指标数据缓存中读取数 据，而不是从数据库中直接读取的。 以上缓存和其他进程中的流程与数据走向，以及如何通信，我们 可以通过图12-14来了解。看懂这个图后，我们对配置参数的优化思路 会更加明晰。

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008578.png)​
      * Zabbix-Server采集器的工作流程

        我们学习了Zabbix监控数据的采集方式，但读者 是否意识到一个问题，这样的采集功能是如何高效工作的？配置数据 又这么复杂，会不会很低效？答案就在本节中。 采集进程在读取采集任务时，首先从配置缓存中获取数据，只有 在需要更新主机可用性状态，或者处理LLD数据时，才会与数据库交互 写入数据，而绝大多数时候是直接读写缓存的，从而提高了处理性 能。如图12-15所示为Zabbix-Server数据采集的流程，展示了Zabbix 内部的采集器是如何在缓存和数据库之间进行交互的。 我们看到，对于每种数据采集器所采集到的数据，当需要预处理 时都会通过预处理进程进行处理，流程如图12-16所示。

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008988.png)​

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008413.png)​
      * 12.9.3 Zabbix-Proxy工作流程

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008275.png)​
      * 12.9.4 Zabbix-Server告警的工作流程

        Zabbix-Server告警支持多进程，并且可以自定义脚本，其高效的 告警离不开内部缓存的支持，工作流程如图12-18所示

        ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008777.png)​
    * 12.10 Zabbix-Server配置参数

      ​![image](https://raw.githubusercontent.com/szhiku/hugoNext/main/content/pic/202402280008459.png)​
    * 12.11 Zabbix-Server性能优化总结

      其思路无非就是本身配置的优化、 数据库的优化，在做了这一系列优化后，其效果会比较明显，对于中 小规模的环境，Zabbix能够运行良好；但对于超大规模的环境，可能 就会出现另一种局面了。由于Zabbix本身的一些限制，其无法横向扩 展，无法使用第三方消息队列，也不支持集群。因此，在超大规模的 环境中其存在天然的缺陷，尤其是在飞速发展的互联网环境中，加之 本身的监控方式具有限制（虽然足够良好，但仍无非满足某些场合的 需求，如把Zabbix当日志采集、APM数据采集等功能使用时），因此要 取得非常理想的优化效果是非常困难的。只能从代码级别进行二次开 发优化，改造成适合超大规模环境的架构，但其改造成本不低。
  * 第13章 Zabbix API的使用

    * 13.1 Zabbix API简介
    * 13.2 JSON-RPC
    * 13.3 Zabbix API的使用流程
    * 13.4 第三方Zabbix API模块
    * 13.5 编写命令行管理工具zbx-tool
  * 第14章 安装与部署的扩展

    * 14.1 源码安装Zabbix-Server
    * 14.2 源码安装Zabbix-Agent
    * 14.3 定制安装包
    * 14.4 使用RPMbuild定制RPM包
    * 14.5 使用Elasticsearch作为数据库
  * 第15章 分布式监控项目实践

    * 15.1 监控系统项目概述

      在大规模的监控系统中，需要考虑以下因素。 第一，分布式架构是首要考虑因素。要求系统架构具备分布式的 设计，原则是将中心节点压力分散在各边缘节点上，使其尽可能监控 更多的设备。 第二，数据存储扩展的问题。节点数量增加到一定规模后，给监 控数据的存储带来了十分严峻的挑战，数据存储扩展的问题是整个监 控系统能否正常工作的前提条件。 第三，高可用性和健壮性、稳定可靠的系统架构、冗余的灾备， 是大型监控系统必备条件。 第四，提供API的能力，易于与第三方集成。在大型环境中，一个 孤立的监控系统会给其他业务系统造成很大的麻烦，通常需要花费更 多的精力进行改造，使其为其他系统提供所需的数据。 第五，具备自动化功能。自动化是解放繁重的体力劳动最有效的 方式，未来的运维一定更智能、更偏向于业务，以业务为核心，而不 是仅仅解决系统的底层问题。 以上观点适用于任何监控系统，而不仅仅是Zabbix。
    * 15.2 监控系统项目的背景

      在IT运维管理中，日常的工作可以分为三类：一是基础环境搭 建；二是维护与更新；三是监控告警与调整优化。在维护与更新的工 作中，可以采用手动或自动化工具来实现，均可以满足不同企业的需 求。对于配置统一、基础环境一致的环境，通常会采用统一的配置管 理工具，如Puppet、SaltStack、Ansible、Chef等。而有些环境，配 置变更不频繁，其没法统一管理，如一年半载可能才发生一次变更， 而相同规模的设备不多，在这种情况下，配置管理工具就无法发挥它 的优势了。但是对于监控告警系统，无论是互联网公司还是传统公 司，都是必不可少的，因为IT设备并不是100%可靠的，就算是100%可 靠的设备，也会有人为因素造成服务不可用。因此，集中的监控告警 平台建设，是每个公司IT部门都十分关心的问题，即使是不懂IT技术 的公司成员，也知道监控告警系统是必需的。 在了解了IT监控系统的重要性后，我们来看看具体的监控需求。 一般来说，监控系统的构建可以分为如下几个层面。 （1）基础架构设施的监控，包括硬件服务器、存储、网络、虚拟 化等。 （2）应用存活与基本性能的监控，包括数据库、Web应用、中间 件、消息队列、缓存等。 （3）代码内部的工作状态监控，这属于监控的细分领域，需要采 用其他技术来实现，如采用开源的APM监控工具PinPoint、Zipkin、 CAT、OpenTraceing等，这种层面的监控，需要软件工程师在编写业务 代码时，采用业务监控指标的埋点，进行整个业务调用链的监控，一 般需要软件架构师来规划设计，项目的整体推动实现过程通常较难。 （4）日志流监控，监控应用设备和应用软件的日志，并从日志中 提取相关的指标进行分析，其实现方式有开源的套件ELK等。 在Zabbix能够支持的监控范围中，上面提到的（1）（2）（4）部 分的功能被支持。所以，在构建监控系统之前，必须明白监控的具体 需求范围，以免范围扩大，从而造成需求蔓延和监控系统使用效果满 意度的下降。 在通常意义的监控建设项目中，一般的需求有：建立基于网络、 存储、虚拟化、应用、数据库系统、缓存、消息队列等的监控预警体 系，设备和服务的宕机、恢复均需要实时反馈给运维团队。监控目标 触发预警后可以设置重复的轮询作业确认问题是否存在。设置合理的 预警级别，根据预警的重要程度、紧急程度、问题处理程度，预警反 馈给不同级别的人员。 在监控系统的构建过程中，监控系统并不是孤立存在的，它往往 需要与其他系统打通，形成闭环，让工作的各个环节进行流通。因 此，企业在建立监控系统的过程中，往往是按照标准产品，加上个性 化定制来实现的。 在IT运维管理框架中，往往从逻辑结构上划分为五个平台和一个 中心配置库（简称“五台一库”），分别是集中监控平台、流程管理 平台、数据展现平台、自动化操作平台、历史数据分析平台和配置管 理数据库（CMDB）。它们的功能如下。 （1）监控平台：构建整个IT监控架构，实现集中事件管理，并为 面向业务的监控管理打下基础。 （2）流程管理平台：整合并标准化运维的日常工作，将日常的工 作规范化，并透明化。 （3）数据展现平台：建设统一报表平台和统一门户平台，将有效 增强数据利用和展示效果。 （4）自动化操作平台：完成对整个IT操作的集中管控和自动化。 （5）历史数据分析平台：集中存放历史数据，提供后期统一分析 及规划。 （6）配置管理数据库：记录完整的、准确的IT环境中各组件的信 息和彼此间的关联关系，作为唯一、可信的数据源，为周边系统提供 支撑数据。 因此，在这种背景环境下，我们需要重新审视企业中的IT监控系 统项目。无论是高速发展的互联网企业，还是平稳发展的传统企业， 抑或是金融保险企业，其构建监控系统的目的都一样，都是通过监控 系统发现故障与问题，并及时通知相关负责人，一线工程师能够根据 故障报告快速处理，并对处理的过程和结果进行整理分析，形成知识 积累，对于可以预见的常规故障，制定出应急方案。
    * 15.3 监控系统项目的步骤
    * 15.4 监控系统项目的规划——工作计划
    * 15.5 监控系统项目的启动——需求调研
    * 15.6 监控系统架构的设计——架构设计图
    * 15.7 监控系统项目的推进——软硬件环境配置
    * 15.8 监控系统项目的实施——安装与部署
    * 15.9 监控功能的实现——配置与定制开发
    * 15.10 监控系统与其他系统的集成
    * 15.11 监控系统项目的总结
  * 第16章 后记——探究监控系统

    * 16.1 监控系统的使用场景
    * 16.2 如何设置监控指标
    * 16.3 如何度量设置告警指标
    * 16.4 如何发送告警与处理告警风暴
    * 16.5 告警轮班机制
    * 16.6 DevOps与监控
    * 16.7 ITIL与监控
