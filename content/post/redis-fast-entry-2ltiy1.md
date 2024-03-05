---
title: Redis快速入门
slug: redis-fast-entry-2ltiy1
url: /post/redis-fast-entry-2ltiy1.html
date: '2024-03-05 05:09:17'
lastmod: '2024-03-05 12:35:57'
toc: true
tags:
  - 运维
  - Linux
  - Redis
keywords: 运维,Linux,Redis
isCJKLanguage: true
---

# Redis快速入门

# 第1章 Redis介绍

## 1.1 Redis是什么

　　Redis是一种基于键值对的NoSQL数据库,与很多键值对数据库不同,redis中的值可以有string,hash,list,set,zset,geo等多种数据结构和算法组成.  
 因为Redis会将所有的数据都放在内存中,所以他的读写性能非常惊人.  
 不仅如此,Redis还可以将内存中的数据利用快照和日志的形式保存到硬盘上  
 Redis还提供了键过期,发布订阅,事务,流水线等附加功能.

## 1.2 Redis重要特性

### 1.速度快

```undefined
Redis所有的数据都存放在内存中
Redis使用C语言实现
Redis使用单线程架构
```

### 2.基于键值对的数据结构服务器

```undefined
5中数据结构:字符串,哈希,列表,集合,有序集合
```

### 3.丰富的功能

```undefined
提供了键过期功能,可以实现缓存
提供了发布订阅功能,可以实现消息系统
提供了pipeline功能,客户端可以将一批命令一次性传到Redis,减少了网络开销
```

### 4.简单稳定

```css
源码很少,3.0版本以后5万行左右.
使用单线程模型法,是的Redis服务端处理模型变得简单.
不依赖操作系统的中的类库
```

### 5.客户端语言多

```undefined
java,PHP,python,C,C++,Nodejs等
```

### 6.持久化

```undefined
RDB和AOF
```

### 7.主从复制

### 8.高可用和分布式

```undefined
哨兵
集群
```

## 1.3 Redis应用场景

### 1.缓存-键过期时间

```undefined
缓存session会话
缓存用户信息,找不到再去mysql查,查到然后回写到redis
```

### 2.排行榜-列表&有序集合

```undefined
热度排名排行榜
发布时间排行榜
```

### 3.计数器应用-天然支持计数器

```undefined
帖子浏览数
视频播放次数
商品浏览数
```

### 4.社交网络-集合

```undefined
踩/赞,粉丝,共同好友/喜好,推送,打标签
```

### 5.消息队列系统-发布订阅

```undefined
配合elk实现日志收集
```

# 第2章 Redis安装部署

## 2.1 目录规划

```bash
### redis下载目录
/data/soft/
### redis安装目录
/opt/redis_cluster/redis_{PORT}/{conf,logs,pid}
### redis数据目录
/data/redis_cluster/redis_{PORT}/redis_{PORT}.rdb
### redis运维脚本
/root/scripts/redis_shell.sh
```

## 2.2 安装命令

### 编辑hosts文件

```kotlin
[root@db01 ~]# tail -3 /etc/hosts
10.0.0.51 db01  
10.0.0.52 db02  
10.0.0.53 db03
mkdir -p /data/soft
mkdir -p /data/redis_cluster/redis_6379
mkdir -p /opt/redis_cluster/redis_6379/{conf,pid,logs}
cd /data/soft/
wget http://download.redis.io/releases/redis-3.2.9.tar.gz
tar zxf redis-3.2.9.tar.gz -C /opt/redis_cluster/
ln -s /opt/redis_cluster/redis-3.2.9/ /opt/redis_cluster/redis
cd /opt/redis_cluster/redis
make && make install
```

## 2.3 配置文件说明

```bash
### 以守护进程模式启动
daemonize yes
### 绑定的主机地址
bind 10.0.0.51
### 监听端口
port 6379
### pid文件和log文件的保存地址
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log
### 设置数据库的数量，默认数据库为0
databases 16
### 指定本地持久化文件的文件名,默认是dump.rdb
dbfilename redis_6379.rdb
### 本地数据库的目录
dir /data/redis_cluster/redis_6379
```

## 2.4 启动关闭服务

### 启动

```csharp
[root@db01 ~]# redis-server /opt/redis_cluster/redis_6379/conf/redis_6379.conf
```

### 关闭

```csharp
[root@db01 ~]# redis-cli -h db01 shutdown
```

# 第3章 Redis基本操作命令

## 3.1 全局命令

　　Redis有5种数据结构,他们是键值对中的值,对于键来说有一些通用的命令.  
 1.查看所有命键

```undefined
Keys *  
```

# 十分危险的命令,线上禁止使用

　　2.查看键的总数

```bash
Dbsize 
# dbsize 命令在计算键总数时不会遍历所有键,而是直接获取Redis内置的键总数变量.
```

　　3.检查键是否存在

```bash
Exists key
# 如果键存在则返回1,不存在则返回0
```

　　4.删除键

```css
Del key [key …]
```

# 通用命令,无论值是什么数据结构类型,del命令都可以将其删除.

　　5.键过期

```bash
Expire key seconds
# Redis支持对键添加过期时间,当超过过期时间后,会自动删除键.
# 通过ttl命令观察键的剩余时间
大于等于0的证书: 键剩余过期时间
-1: 键没设置过期时间
-2: 键不存在
```

　　6.键的数据类型

```dart
Type key
```

## 3.2 字符串

　　Redis并不是简单地key-value存储,实际上他是一个数据结构服务器,支持不同类型的值.  
 Redis Strings  
 这是最简单的Redis类型,如果你只用这种类型,Redis就像一个持久化的memcache服务器(注:memcache的数据仅保存在内存中,服务器重启后,数据将丢失.)  
 操作命令:  
 通常用SET command 和 GET command来设置和获取字符串值  
 练习:

```csharp
db01:6379> set key1 value1
OK
db01:6379> get key1
"value1"
db01:6379> keys *
1) "key1"
```

　　操作命令:  
 INCR命令将字符串值解析成整型.将其加1,最后结果保存为新的字符串,类似命令: INCRBY,DECR,DECRBY  
 练习:

```dart
db01:6379> set key2 100
OK
db01:6379> get key2
"100"
db01:6379> incr key2
(integer) 101
db01:6379> get key2
"101"
db01:6379> incrby key2 10
(integer) 111
db01:6379> get key2
"111"
```

　　操作命令:  
 MSET和MGET可以一次存储或获取多个key对应的值.  
 练习:

```bash
db01:6379> mset key3 v3 key4 v4 key5 v5
OK
db01:6379> mget key3 key4 key5
1) "v3"
2) "v4"
3) "v5"
```

　　操作命令:  
 EXISTS命令返回1或0标识给定key的值是否存在.  
 使用DEL命令可以删除key对应的值,  
 DEL命令返回1或0标识是被删除(值存在)或者没被删除(key对应的值不存在).  
 练习:

```bash
db01:6379> exists key5
(integer) 1
db01:6379> del key5
(integer) 1
db01:6379> exists key5
(integer) 0
db01:6379> del key5
(integer) 0
```

　　操作命令:  
 Type命令可以返回key对应的存储类型  
 练习:

```tsx
db01:6379> set key5 v5
OK
db01:6379> type key5
string
```

　　操作命令:  
 可以对key设置一个超时时间,当这个时间到达后被删除  
 练习:

```css
db01:6379> get key5
"v5"
db01:6379> ttl key5
(integer) -1
db01:6379> expire key5 10
(integer) 1
db01:6379> ttl key5
(integer) 6
db01:6379> ttl key5
(integer) 5
db01:6379> ttl key5
(integer) 2
db01:6379> ttl key5
(integer) -2
db01:6379> ttl key5
(integer) -2
db01:6379> get key5
(nil)
```

　　操作命令:  
 PERSIST命令去除超时时间  
 练习:

```bash
db01:6379> set key5 v5 ex 10
OK
db01:6379> ttl key5
(integer) 8
db01:6379> ttl key5
(integer) 6
db01:6379> persist key5
(integer) 1
db01:6379> ttl key5
(integer) -1
```

## 3.3 列表

　　操作命令:  
 LPUSH命令可向list的左边(头部)添加一个新元素  
 RPUSH命令可向list的右边(尾部)添加一个新元素.  
 最后LRANGE可以从list中取出一定范围的元素  
 练习:

```bash
db01:6379> rpush list1 A
(integer) 1
db01:6379> rpush list1 B
(integer) 2
db01:6379> lpush list1 top1
(integer) 3
db01:6379> lrange list1 0 -1
1) "top1"
2) "A"
3) "B"
db01:6379> lrange list1 1 -1
1) "A"
2) "B"
db01:6379> lrange list1 2 -1
1) "B"
```

　　操作命令:  
 Pop,从list中删除元素并同时返回删除的值,可以在左边或右边操作.  
 练习:

```bash
db01:6379> rpop list1
"B"
db01:6379> lrange list1 0 -1
1) "top1"
2) "A"
db01:6379> lpop list1
"top1"
db01:6379> lrange list1 0 -1
1) "A"
```

　　3.4 哈希  
 操作命令:  
 Hash看起来就像一个’hash’的样子.由键值对组成  
 HMSET指令设置hash中的多个域  
 HGET取回单个域.  
 HMGET取回一系列的值  
 练习:

```bash
db01:6379> hmset user:1000 username zhangya age 27 job it
OK
db01:6379> hget user:1000 username
"zhangya"
db01:6379> hmget user:1000 username age job
1) "zhangya"
2) "27"
3) "it"
db01:6379> hgetall user:1000
1) "username"
2) "zhangya"
3) "age"
4) "27"
5) "job"
6) "it"
db01:6379> hmset user:1000 qq 526195417
OK
db01:6379> hgetall user:1000
1) "username"
2) "zhangya"
3) "age"
4) "27"
5) "job"
6) "it"
7) "qq"
8) "526195417"
```

　　3.5 集合  
 操作命令:  
 集合是字符串的无序排列,  
 SADD指令把新的元素添加到set中  
 练习:

```bash
db01:6379> sadd set1 1 2 3
(integer) 3
db01:6379> smembers set1
1) "1"
2) "2"
3) "3"
```

　　和list类型不同,set集合不允许出现重复的元素

```bash
db01:6379> sadd set1 1 4
(integer) 1
db01:6379> smembers set1
1) "1"
2) "2"
3) "3"
4) "4"
```

　　Srem用来删除指定的值

```bash
db01:6379> srem set1 2 4
(integer) 2
db01:6379> smembers set1
1) "1"
2) "3"
```

　　Sdiff计算集合的差异成员

```bash
db01:6379> sadd set1 1 2 3 4
(integer) 2
db01:6379> sadd set2 1 4 5
(integer) 3
db01:6379> sdiff set1 set2
1) "2"
2) "3"
```

　　Sinter计算集合的交集

```bash
db01:6379> sinter set1 set2
1) "1"
2) "4"
```

　　Sunion计算集合并集

```bash
db01:6379> sunion set1 set2
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
```

# 第4章 Redis持久化

## 4.1 持久化的两种方式介绍

　　RDB 持久化优缺点  
 可以在指定的时间间隔内生成数据集的 时间点快照（point-in-time snapshot）。  
 优点：速度快，适合于用做备份，主从复制也是基于RDB持久化功能实现的。  
 缺点：会有数据丢失  
 rdb持久化核心配置参数：

```bash
vim /data/6379/redis.conf
dir /data/6379
dbfilename dump.rdb
save 900 1      #900秒（15分钟）内有1个更改
save 300 10     #300秒（5分钟）内有10个更改
save 60 10000   #60秒内有10000个更改  
```

　　AOF 持久化(append-only log file)优缺点  
 记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。  
 AOF 文件中的命令全部以 Redis 协议的格式来保存，新命令会被追加到文件的末尾。  
 优点：可以最大程度保证数据不丢  
 缺点：日志记录量级比较大

　　AOF持久化配置

```bash
appendonly yes          #是否打开aof日志功能
appendfsync always      #每1个命令,都立即同步到aof
appendfsync everysec    #每秒写1次
appendfsync no          #写入工作交给操作系统,由操作系统判断缓冲区大小,统一写入到aof.
```

## 4.2 面试题

　　redis 持久化方式有哪些？有什么区别？  
 rdb：基于快照的持久化，速度更快，一般用作备份，主从复制也是依赖于rdb持久化功能  
 aof：以追加的方式记录redis操作日志的文件。可以最大程度的保证redis数据安全，类似于mysql的binlog

# 第5章 Redis安全认证

　　redis默认开启了保护模式，只允许本地回环地址登录并访问数据库。  
 禁止protected-mode  
 protected-mode yes/no （保护模式，是否只允许本地访问）  
 (1)Bind :指定IP进行监听

```bash
[root@db01 ~]# vim /opt/redis_cluster/redis_6379/conf/redis_6379.conf
bind 10.0.0.51  127.0.0.1
```

　　(2)增加requirepass  {password}

```csharp
[root@db01 ~]# vim /opt/redis_cluster/redis_6379/conf/redis_6379.conf
requirepass 123456
```

　　验证方法一：

```css
[root@db01 ~]# redis-cli -a 123456
127.0.0.1:6379> set k1 v1
OK
```

　　验证方法二：

```css
[root@db01 ~]# redis-cli
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set k2 v2
OK
```

# 第6章 Redis主从复制

## 6.1 主从复制介绍

　　主从复制介绍  
 在分布式系统中为了解决单点问题,通常会把数据复制多个副本到其他机器,满足故障恢复和负载均衡等求.  
 Redis也是如此,提供了复制功能.  
 复制功能是高可用Redis的基础,后面的哨兵和集群都是在复制的基础上实现高可用的.

## 6.2 配置命令

### 6.2.1 建立复制

　　每个从节点只能有一个主节点,主节点可以有多个从节点.  
 配置复制的方式有三种:

```ruby
1.在配置文件中加入slaveof {masterHost} {masterPort} 随redis启动生效.
2.在redis-server启动命令后加入—slaveof {masterHost} {masterPort}生效.
3.直接使用命令:slaveof {masterHost} {masterPort}生效.
```

　　查看复制状态信息命令

```undefined
Info replication
```

### 6.2.2 断开复制

　　Slaveof命令不但可以建立复制,还可以在从节点执行slave of no one来断开与主节点复制关系.  
 断开复制主要流程:

```undefined
1.断开与主节点复制关系
2.从节点晋升为主节点
```

　　从节点断开复制后不会抛弃原有数据,只是无法再获取主节点上的数据变化.  
 通过slaveof命令还可以实现切主操作,所谓切主是指把当前从节点对主节点的复制切换到另一个主节点.  
 执行slaveof {newMasterIp} {newMasterPort}命令即可.  
 切主操作流程如下:

```undefined
1.断开与旧主节点的复制关系
2.与新主节点建立复制关系
3.删除从节点当前所有数据
4.对新主节点进行复制操作
```

　　提示: 线上操作一定要小心,因为切主后会清空之前所有的数据.

# 第7章 Redis Sentinel（哨兵）

## 7.1 哨兵介绍

　　Sentinel介绍  
 Redis的主从模式下，主节点一旦发生故障不能提供服务，需要人工干预，将从节点晋升为主节点，同时还需要修改客户端配置。对于很多应用场景这种方式无法接受。  
 Sentinel（哨兵）架构解决了redis主从人工干预的问题。  
 Redis Sentinel是redis的高可用实现方案，实际生产环境中，对提高整个系统可用性非常有帮助的。

## 7.2 哨兵主要功能

　　Redis Sentinel 是一个分布式系统， Redis Sentinel为Redis提供高可用性。可以在没有人为干预的情况下阻止某种类型的故障。  
 Redis 的 Sentinel 系统用于管理多个 Redis 服务器（instance）该系统执行以下三个任务：  
 1.监控（Monitoring）：  
 Sentinel 会不断地定期检查你的主服务器和从服务器是否运作正常。  
 2.提醒（Notification）：  
 当被监控的某个 Redis 服务器出现问题时， Sentinel 可以通过 API 向管理员或者其他应用程序发送通知。  
 3.自动故障迁移（Automatic failover）：  
 当一个主服务器不能正常工作时， Sentinel 会开始一次自动故障迁移操作， 它会将失效主服务器的其中一个从服务器升级为新的主服务器， 并让失效主服务器的其他从服务器改为复制新的主服务器； 当客户端试图连接失效的主服务器时， 集群也会向客户端返回新主服务器的地址， 使得集群可以使用新主服务器代替失效服务器  
 架构图：

## 7.3 目录规划

```css
角色  IP  端口
Master  10.0.0.51   6379
Sentinel-01     26379
Master  10.0.0.52   6379
Sentinel-01     26379
Master  10.0.0.53   6379
Sentinel-01     26379
```

## 7.4 安装配置命令

　　哨兵是基于主从复制，所以需要先部署好主从复制  
 手工操作步骤如下：  
 1.先配置和创建好1台服务器的节点和哨兵  
 2.使用rsync传输到另外2台机器  
 3.修改另外两台机器的IP地址  
 建议使用ansible剧本批量部署

### 7.4.1 db01命令

```kotlin
mkdir -p /data/redis_cluster/redis_26379
mkdir -p /opt/redis_cluster/redis_26379/{conf,pid,logs}
vim /opt/redis_cluster/redis_26379/conf/redis_26379.conf
bind 10.0.0.51
port 26379
daemonize yes
logfile /opt/redis_cluster/redis_26379/logs/redis_26379.log
dir /data/redis_cluster/redis_26379
sentinel monitor mymaster 10.0.0.51 6379 2
sentinel down-after-milliseconds mymaster 3000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 18000
```

### 7.4.2 配置文件解释

```bash
sentinel monitor mymaster 10.0.0.51 6379 2 
#mymaster主节点别名 主节点 ip 和端口，判断主节点失败，两个 sentinel 节点同意
sentinel down-after-milliseconds mymaster 30000 
#选项指定了 Sentinel 认为服务器已经断线所需的毫秒数。
sentinel parallel-syncs mymaster 1 
#向新的主节点发起复制操作的从节点个数，1轮询发起复制
sentinel failover-timeout mymaster 180000 
#故障转移超时时间
```

### 7.4.3 db02/db03命令

　　db01

```undefined
rsync -avz /opt/* db02:/opt/
rsync -avz /opt/* db03:/opt/
```

　　db02

```bash
mkdir -p /data/redis_cluster/redis_26379
cd /opt/redis_cluster/redis
make install
sed -i 's#bind 10.0.0.51#bind 10.0.0.52#g' /opt/redis_cluster/redis_6379/conf/redis_6379.conf
sed -i 's#bind 10.0.0.51#bind 10.0.0.52#g' /opt/redis_cluster/redis_26379/conf/redis_26379.conf
```

　　db03

```bash
mkdir -p /data/redis_cluster/redis_26379
cd /opt/redis_cluster/redis
make install
sed -i 's#bind 10.0.0.51#bind 10.0.0.53#g' /opt/redis_cluster/redis_6379/conf/redis_6379.conf
sed -i 's#bind 10.0.0.51#bind 10.0.0.53#g' /opt/redis_cluster/redis_26379/conf/redis_26379.conf
```

### 7.4.4 配置主从关系

　　db02和db03

```undefined
redis-server /opt/redis_cluster/redis_6379/conf/redis_6379.conf
redis-cli slaveof 10.0.0.51 6379
ps -ef|grep redis
```

### 7.4.5 启动哨兵

　　3台都操作

```undefined
redis-sentinel /opt/redis_cluster/redis_26379/conf/redis_26379.conf
```

## 7.5 配置文件的变化

　　当所有节点启动后,配置文件的内容发生了变化,体现在三个方面:

```undefined
1)Sentinel节点自动发现了从节点,其余Sentinel节点
2)去掉了默认配置,例如parallel-syncs failover-timeout参数
3)添加了配置纪元相关参数
```

　　查看配置文件命令

```csharp
[root@db01 ~]# tail -6 /opt/redis_cluster/redis_26379/conf/redis_26379.conf  
# Generated by CONFIG REWRITE
sentinel known-slave mymaster 10.0.0.52 6379
sentinel known-slave mymaster 10.0.0.53 6379
sentinel known-sentinel mymaster 10.0.0.53 26379 7794fbbb9dfb62f4d2d7f06ddef06bacb62e4c97
sentinel known-sentinel mymaster 10.0.0.52 26379 17bfab23bc53a531571790b9b31558dddeaeca40
sentinel current-epoch 0
```

## 7.6 哨兵常用操作API

　　登陆命令

```xml
[root@db01 ~]# redis-cli -h db01 -p 26379
Sentinel节点是一个特殊的Redis节点,他们有自己专属的API
Info Sentinel
Sentinel masters
Sentinel master <master name>
Sentinel slaves <master name> 
Sentinel sentinels <master name>
Sentinel get-master-addr-by-name <master name>   
Sentinel failover <master name>
Sentinel flushconfig
```

## 7.7 模拟故障转移

　　停掉其中1个节点，然后观察其他节点的日志变化  
 故障转移后配置文件变化  
 Redis Sentinel存在多个从节点时,如果想将指定的从节点晋升为主节点,可以将其他从节点的slavepriority配置为0,但是需要注意failover后,将slave-priority调回原值.

```css
1.查询命令:CONFIG GET slave-priority
2.设置命令:CONFIG SET slave-priority 0
3.主动切换:sentinel failover mymaster
```

　　操作过程：  
 db02/db03操作

```undefined
redis-cli -h db02 -p 6379 CONFIG SET slave-priority 0
redis-cli -h db03 -p 6379 CONFIG SET slave-priority 0
```

　　db01操作

```undefined
redis-cli -h db01 -p 26379 Sentinel failover mymaster
```

# 第8章 Redis Cluster

## 8.1 集群介绍

　　Redis Cluster 是 redis的分布式解决方案，在3.0版本正式推出  
 当遇到单机、内存、并发、流量等瓶颈时，可以采用Cluster架构方案达到负载均衡目的。  
 Redis Cluster之前的分布式方案有两种：  
 1)客户端分区方案，优点分区逻辑可控，缺点是需要自己处理数据路由，高可用和故障转移等。

2. 代理方案，优点是简化客户端分布式逻辑和升级维护便利，缺点加重架构部署和性能消耗。  
    官方提供的 Redis Cluster集群方案，很好的解决了集群方面的问题

## 8.2 数据分布

　　分布式数据库首先要解决把整个数据库集按照分区规则映射到多个节点的问题，即把数据集划分到多个节点上，每个节点负责整体数据的一个子集，需要关注的是数据分片规则，Redis Cluster采用哈希分片规则。

## 8.3 目录规划

```bash
# redis安装目录
/opt/redis_cluster/redis_{PORT}/{conf,logs,pid}
# redis数据目录
/data/redis_cluster/redis_{PORT}/redis_{PORT}.rdb
# redis运维脚本
/root/scripts/redis_shell.sh
```

## 8.4 集群拓扑

　　不太合理的拓扑：

　　合理的拓扑：

## 8.5 手动搭建部署集群

　　思路:  
 1)部署一台服务器上的2个集群节点  
 2)发送完成后修改其他主机的IP地址  
 3)使用ansible批量部署  
 实现命令：  
 db01操作

```jsx
mkdir -p /opt/redis_cluster/redis_{6380,6381}/{conf,logs,pid}
mkdir –p /data/redis_cluster/redis_{6380,6381}
cat >/opt/redis_cluster/redis_6380/conf/redis_6380.conf<<EOF
bind 10.0.0.51
port 6380
daemonize yes
pidfile "/opt/redis_cluster/redis_6380/pid/redis_6380.pid"
logfile "/opt/redis_cluster/redis_6380/logs/redis_6380.log"
dbfilename "redis_6380.rdb"
dir "/data/redis_cluster/redis_6380/"
cluster-enabled yes
cluster-config-file nodes_6380.conf
cluster-node-timeout 15000
EOF
cd /opt/redis_cluster/
cp redis_6380/conf/redis_6380.conf redis_6381/conf/redis_6381.conf
sed -i 's#6380#6381#g' redis_6381/conf/redis_6381.conf 
rsync -avz /opt/redis_cluster/redis_638* db02:/opt/redis_cluster/
rsync -avz /opt/redis_cluster/redis_638* db03:/opt/redis_cluster/
redis-server /opt/redis_cluster/redis_6380/conf/redis_6380.conf
redis-server /opt/redis_cluster/redis_6381/conf/redis_6381.conf
```

　　db02操作：

```rust
find /opt/redis_cluster/redis_638* -type f -name "*.conf"|xargs sed -i "/bind/s#51#52#g"
mkdir –p /data/redis_cluster/redis_{6380,6381}
redis-server /opt/redis_cluster/redis_6380/conf/redis_6380.conf
redis-server /opt/redis_cluster/redis_6381/conf/redis_6381.conf
```

　　db03操作

```rust
find /opt/redis_cluster/redis_638* -type f -name "*.conf"|xargs sed -i "/bind/s#51#53#g"
mkdir –p /data/redis_cluster/redis_{6380,6381}
redis-server /opt/redis_cluster/redis_6380/conf/redis_6380.conf
redis-server /opt/redis_cluster/redis_6381/conf/redis_6381.conf
```

### 8.5.1 手动配置节点发现

　　当把所有节点都启动后查看进程会有cluster的字样

　　但是登录后执行CLUSTER NODES命令会发现只有每个节点自己的ID,目前集群内的节点  
 还没有互相发现,所以搭建redis集群我们第一步要做的就是让集群内的节点互相发现.

　　在执行节点发现命令之前我们先查看一下集群的数据目录会发现有生成集群的配置文件

　　查看后发现只有自己的节点内容,等节点全部发现后会把所发现的节点ID写入这个文件

　　集群模式的Redis除了原有的配置文件之外又加了一份集群配置文件.当集群内节点  
 信息发生变化,如添加节点,节点下线,故障转移等.节点会自动保存集群状态到配置文件.  
 需要注意的是,Redis自动维护集群配置文件,不需要手动修改,防止节点重启时产生错乱.

　　节点发现使用命令: CLUSTER MEET {IP} {PORT}  
 提示:在集群内任意一台机器执行此命令就可以

```css
[root@db01 ~]# sh redis_shell.sh login 6380
10.0.0.51:6380> CLUSTER MEET 10.0.0.51 6381
OK
10.0.0.51:6380> CLUSTER MEET 10.0.0.52 6380
OK
10.0.0.51:6380> CLUSTER MEET 10.0.0.53 6380
OK
10.0.0.51:6380> CLUSTER MEET 10.0.0.52 6381
OK
10.0.0.51:6380> CLUSTER MEET 10.0.0.53 6381
OK
10.0.0.51:6380> CLUSTER NODES
d03cb38d612802aead8f727b1726a3359c241818 10.0.0.51:6380 myself,master - 0 0 4 connected
67c8128df00b2fa304a41bafbadac25a654f196d 10.0.0.51:6381 master - 0 1562059947079 1 connected
a23ec7d444791a0b258ac454ef15cb4d6ab5abd2 10.0.0.53:6381 master - 0 1562059948087 5 connected
e57807d4d35daaaca05f4a9705e844eab15c7ce8 10.0.0.52:6381 master - 0 1562059949098 0 connected
aa9da67a594dfb357195f12ca4c44001804ee470 10.0.0.53:6380 master - 0 1562059945063 3 connected
5cb6895305520e6a0aa4198a6ea5f2c087530b41 10.0.0.52:6380 master - 0 1562059950108 2 connected
```

　　节点都发现完毕后我们再次查看集群配置文件  
 可以看到,发现到的节点的ID也被写入到了集群的配置文件里

### 8.5.2 Redis Cluster 通讯流程

　　在分布式存储中需要提供维护节点元数据信息的机制，所谓元数据是指：节点负责哪些数据，是否出现故障灯状态信息，redis 集群采用 Gossip(流言)协议，Gossip 协议工作原理就是节点彼此不断交换信息，一段时间后所有的节点都会知道集群完整信息，这种方式类似流言传播。  
 通信过程：  
 1)集群中的每一个节点都会单独开辟一个 Tcp 通道，用于节点之间彼此通信，通信端口在基础端口上家10000.  
 2)每个节点在固定周期内通过特定规则选择结构节点发送 ping 消息  
 3)接收到 ping 消息的节点用 pong 消息作为响应。集群中每个节点通过一定规则挑选要通信的节点，每个节点可能知道全部节点，也可能仅知道部分节点，只要这些节点彼此可以正常通信，最终他们会打成一致的状态，当节点出现故障，新节点加入，主从角色变化等，它能够给不断的ping/pong消息，从而达到同步目的。  
 通讯消息类型：  
 Gossip  
 Gossip 协议职责就是信息交换，信息交换的载体就是节点间彼此发送Gossip 消息。  
 常见 Gossip 消息分为：ping、 pong、 meet、 fail 等  
 meet  
 meet 消息：用于通知新节点加入，消息发送者通知接受者加入到当前集群，meet 消息通信正常完成后，接收节点会加入到集群中并进行ping、 pong 消息交换  
 ping  
 ping 消息：集群内交换最频繁的消息，集群内每个节点每秒想多个其他节点发送 ping 消息，用于检测节点是否在线和交换彼此信息。  
 pong  
 Pong 消息：当接收到 ping，meet 消息时，作为相应消息回复给发送方确认消息正常通信，节点也可以向集群内广播自身的 pong 消息来通知整个集群对自身状态进行更新。  
 fail  
 fail 消息：当节点判定集群内另一个节点下线时，回向集群内广播一个fail 消息，其他节点收到 fail 消息之后把对应节点更新为下线状态。  
 通讯示意图：

### 8.5.3 Redis Cluster手动分配槽位

　　虽然节点之间已经互相发现了,但是此时集群还是不可用的状态,因为并没有给节点分配槽位,而且必须是所有的槽位都分配完毕后整个集群才是可用的状态.  
 反之,也就是说只要有一个槽位没有分配,那么整个集群就是不可用的.  
 测试命令：

```css
[root@db01 ~]# sh redis_shell.sh login 6380 
10.0.0.51:6380> set k1 v1
(error) CLUSTERDOWN Hash slot not served

10.0.0.51:6380> CLUSTER INFO
cluster_state:fail
cluster_slots_assigned:0
cluster_slots_ok:0
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:0
cluster_current_epoch:5
cluster_my_epoch:4
cluster_stats_messages_sent:1200
cluster_stats_messages_received:1200
10.0.0.51:6380>
```

　　前面说了,我们虽然有6个节点,但是真正负责数据写入的只有3个节点,其他3个节点只是作为主节点的从节点,也就是说,只需要分配期中三个节点的槽位就可以了  
 分配槽位的方法:  
 分配槽位需要在每个主节点上来配置,此时有2种方法执行:  
 1.分别登录到每个主节点的客户端来执行命令  
 2.在其中一台机器上用redis客户端远程登录到其他机器的主节点上执行命令  
 每个节点执行命令:

```csharp
[root@db01 ~]# redis-cli -h db01 -p 6380 cluster addslots {0..5461}
OK
[root@db01 ~]# redis-cli -h db02 -p 6380 cluster addslots {5462..10922}
OK
[root@db01 ~]# redis-cli -h db03 -p 6380 cluster addslots {10923..16383}
OK
```

　　分配完所有槽位之后我们再查看一下集群的节点状态和集群状态  
 可以看到三个节点都分配了槽位,而且集群的状态是OK的

### 8.5.4 手动配置集群高可用

　　虽然这时候集群是可用的了,但是整个集群只要有一台机器坏掉了,那么整个集群都是不可用的.  
 所以这时候需要用到其他三个节点分别作为现在三个主节点的从节点,以应对集群主节点故障时可以进行自动切换以保证集群持续可用.  
 注意:  
 1.不要让复制节点复制本机器的主节点, 因为如果那样的话机器挂了集群还是不可用状态, 所以复制节点要复制其他服务器的主节点.  
 2.使用redis-trid工具自动分配的时候会出现复制节点和主节点在同一台机器上的情况,需要注意

### 8.5.5 测试集群

　　这一次我们采用在一台机器上使用redis客户端远程操作集群其他节点  
 注意:  
 1.需要执行命令的是每个服务器的从节点  
 2.注意主从的ID不要搞混了.  
 执行命令：

```csharp
[root@db01 ~]# redis-cli -h db01 -p 6381 CLUSTER REPLICATE 5cb6895305520e6a0aa4198a6ea5f2c087530b41
OK
[root@db01 ~]# redis-cli -h db02 -p 6381 CLUSTER REPLICATE aa9da67a594dfb357195f12ca4c44001804ee470
OK
[root@db01 ~]# redis-cli -h db03 -p 6381 CLUSTER REPLICATE d03cb38d612802aead8f727b1726a3359c241818
OK
```

## 8.6 Redis Cluster测试集群

　　我们使用常规插入redis数据的方式往集群里写入数据看看会发生什么

```css
 [root@db01 ~]# redis-cli -h db01 -p 6380 set k1 v1
(error) MOVED 12706 10.0.0.53:6380
```

　　结果提示error, 但是给出了集群另一个节点的地址  
 那么这条数据到底有没有写入呢? 我们登录这两个节点分别查看

```ruby
[root@db01 ~]# redis-cli -h db03 -p 6380 get k1  
(nil)
```

　　结果没有,这是因为使用集群后由于数据被分片了,所以并不是说在那台机器上写入数据就会在哪台机器的节点上写入,集群的数据写入和读取就涉及到了另外一个概念,ASK路由

## 8.7 Redis Cluster ASK路由介绍

　　在集群模式下,Redis接受任何键相关命令时首先会计算键对应的槽,再根据槽找出所对应的节点  
 如果节点是自身,则处理键命令;  
 否则回复MOVED重定向错误,通知客户端请求正确的节点,这个过程称为Mover重定向.

　　知道了ask路由后,我们使用-c选项批量插入一些数据

```bash
[root@db01 ~]# cat input_key.sh 
#!/bin/bash
for i in $(seq 1 1000)
do
    redis-cli -c -h db01 -p 6380 set k_${i} v_${i} && echo "set k_${i} is ok"
done
```

　　写入后我们同样使用-c选项来读取刚才插入的键值,然后查看下redis会不会帮我们路由到正确的节点上

```css
[root@db01 ~]# redis-cli -c -h db01 -p 6380 
db01:6380> get k_1
"v_1"
db01:6380> get k_100
-> Redirected to slot [5541] located at 10.0.0.52:6380
"v_100"
10.0.0.52:6380> get k_1000
-> Redirected to slot [79] located at 10.0.0.51:6380
"v_1000"
10.0.0.51:6380>
```

## 8.8 模拟故障转移

　　至此,我们已经手动的把一个redis高可用的集群部署完毕了, 但是还没有模拟过故障  
 这里我们就模拟故障,停掉期中一台主机的redis节点,然后查看一下集群的变化  
 我们使用暴力的kill -9杀掉 db02上的redis集群节点,然后观察节点状态  
 理想情况应该是db01上的6381从节点升级为主节点

　　在db01上查看集群节点状态

　　虽然我们已经测试了故障切换的功能,但是节点修复后还是需要重新上线  
 所以这里测试节点重新上线后的操作  
 重新启动db02的6380,然后观察日志

　　观察db01上的日志

　　这时假如我们想让修复后的节点重新上线,可以在想变成主库的从库执行CLUSTER FAILOVER命令  
 这里我们在db02的6380上执行

## 8.9 使用工具搭建部署Redis Cluster

　　手动搭建集群便于理解集群创建的流程和细节，不过手动搭建集群需要很多步骤，当集群节点众多时，必然会加大搭建集群的复杂度和运维成本，因此官方提供了 redis-trib.rb的工具方便我们快速搭建集群。  
 redis-trib.rb是采用 Ruby 实现的 redis 集群管理工具，内部通过 Cluster相关命令帮我们简化集群创建、检查、槽迁移和均衡等常见运维操作，使用前要安装 ruby 依赖环境  
 安装命令：

```csharp
yum makecache fast
yum install rubygems
gem sources --remove https://rubygems.org/
gem sources -a http://mirrors.aliyun.com/rubygems/
gem update –system
gem install redis -v 3.3.5
```

　　我们可以停掉所有的节点，然后清空数据，恢复成一个全新的集群，所有机器执行命令

```kotlin
pkill redis
rm -rf /data/redis_cluster/redis_6380/*
rm -rf /data/redis_cluster/redis_6381/*
```

　　全部清空之后启动所有的节点，所有机器执行

```css
sh redis_shell.sh start 6380
sh redis_shell.sh start 6381
```

　　db01执行创建集群命令

```bash
cd /opt/redis_cluster/redis/src/
./redis-trib.rb create --replicas 1 10.0.0.51:6380 10.0.0.52:6380 10.0.0.53:6380 10.0.0.51:6381 10.0.0.52:6381 10.0.0.53:6381
```

　　检查集群完整性

```undefined
./redis-trib.rb check 10.0.0.51:6380
```

## 8.10 工具扩容节点

　　Redis集群的扩容操作可分为以下几个步骤  
 1)准备新节点  
 2)加入集群  
 3)迁移槽和数据  
 扩容流程图：

　　我们在db01上创建2个新节点

```bash
mkdir -p /opt/redis_cluster/redis_{6390,6391}/{conf,logs,pid}
mkdir -p /data/redis_cluster/redis_{6390,6391}
cd /opt/redis_cluster/
cp redis_6380/conf/redis_6380.conf redis_6390/conf/redis_6390.conf
cp redis_6380/conf/redis_6380.conf redis_6391/conf/redis_6391.conf
sed -i 's#6380#6390#g' redis_6390/conf/redis_6390.conf
sed -i 's#6380#6391#g' redis_6391/conf/redis_6391.conf
```

　　启动节点

```css
bash redis_shell.sh start 6390
bash redis_shell.sh start 6391
```

　　发现节点

```css
redis-cli -c -h db01 -p 6380 cluster meet 10.0.0.51 6390
redis-cli -c -h db01 -p 6380 cluster meet 10.0.0.51 6391
```

　　在db01上使用工具扩容

```bash
cd /opt/redis_cluster/redis/src/
./redis-trib.rb reshard 10.0.0.51:6380
```

　　打印出进群每个节点信息后,reshard命令需要确认迁移的槽数量,这里我们输入4096个:  
 How many slots do you want to move (from 1 to 16384)? 4096  
 输入6390的节点ID作为目标节点,也就是要扩容的节点,目标节点只能指定一个  
 What is the receiving node ID? xxxxxxxxx  
 之后输入源节点的ID,这里分别输入每个主节点的6380的ID最后输入done,或者直接输入all  
 Source node #1:all  
 迁移完成后命令会自动退出,这时候我们查看一下集群的状态  
 ./redis-trib.rb rebalance 10.0.0.51:6380

## 8.11 工具收缩节点

　　流程说明  
 1).首先需要确定下线节点是否有负责的槽,  
 如果是,需要把槽迁移到其他节点,保证节点下线后整个集群槽节点映射的完整性.  
 2).当下线节点不再负责槽或者本身是从节点时,  
 就可以通知集群内其他节点忘记下线节点,当所有的节点忘记该节点后可以正常关闭.

　　这里我们准备将刚才新添加的节点下线,也就是6390和6391  
 收缩和扩容迁移的方向相反,6390变为源节点,其他节点变为目标节点,源节点把自己负责的4096个槽均匀的迁移到其他节点上,.  
 由于redis-trib..rb reshard命令只能有一个目标节点,因此需要执行3次reshard命令,分别迁移1365,1365,1366个槽.  
 操作命令：

```bash
cd /opt/redis_cluster/redis/src/
./redis-trib.rb reshard 10.0.0.51:6380
How many slots do you want to move (from 1 to 16384)? 1365
输入6380的id
输入6390的id
done
```

## 8.12 忘记节点

　　由于我们的集群是做了高可用的,所以当主节点下线的时候从节点也会顶上,所以最好我们先下线从节点,然后在下线主节点

```python
cd /opt/redis_cluster/redis/src/
./redis-trib.rb del-node 10.0.0.51:6391 ID
./redis-trib.rb del-node 10.0.0.51:6390 ID
```

# 第9章 Redis集群常用命令

```objectivec
集群(cluster)
CLUSTER INFO 打印集群的信息
CLUSTER NODES 列出集群当前已知的所有节点（node），以及这些节点的相关信息。 
节点(node)
CLUSTER MEET <ip> <port> 将 ip 和 port 所指定的节点添加到集群当中，让它成为集群的一份子。
CLUSTER FORGET <node_id> 从集群中移除 node_id 指定的节点。
CLUSTER REPLICATE <node_id> 将当前节点设置为 node_id 指定的节点的从节点。
CLUSTER SAVECONFIG 将节点的配置文件保存到硬盘里面。 
槽(slot)
CLUSTER ADDSLOTS <slot> [slot ...] 将一个或多个槽（slot）指派（assign）给当前节点。
CLUSTER DELSLOTS <slot> [slot ...] 移除一个或多个槽对当前节点的指派。
CLUSTER FLUSHSLOTS 移除指派给当前节点的所有槽，让当前节点变成一个没有指派任何槽的节点。
CLUSTER SETSLOT <slot> NODE <node_id> 将槽 slot 指派给 node_id 指定的节点，如果槽已经指派给另一个节点，那么先让另一个节点删除该槽>，然后再进行指派。
CLUSTER SETSLOT <slot> MIGRATING <node_id> 将本节点的槽 slot 迁移到 node_id 指定的节点中。
CLUSTER SETSLOT <slot> IMPORTING <node_id> 从 node_id 指定的节点中导入槽 slot 到本节点。
CLUSTER SETSLOT <slot> STABLE 取消对槽 slot 的导入（import）或者迁移（migrate）。 
键 (key)
CLUSTER KEYSLOT <key> 计算键 key 应该被放置在哪个槽上。
CLUSTER COUNTKEYSINSLOT <slot> 返回槽 slot 目前包含的键值对数量。CLUSTER GETKEYSINSLOT <slot> <count> 返回 count 个 slot 槽中的键。
```

# 第10章 Redis运维工具

## 10.1 运维脚本

```bash
[root@db01 ~]# cat redis_shell.sh 
#!/bin/bash

USAG(){
    echo "sh $0 {start|stop|restart|login|ps|tail} PORT"
}
if [ "$#" = 1 ]
then
    REDIS_PORT='6379'
elif 
    [ "$#" = 2 -a -z "$(echo "$2"|sed 's#[0-9]##g')" ]
then
    REDIS_PORT="$2"
else
    USAG
    exit 0
fi

REDIS_IP=$(hostname -I|awk '{print $1}')
PATH_DIR=/opt/redis_cluster/redis_${REDIS_PORT}/
PATH_CONF=/opt/redis_cluster/redis_${REDIS_PORT}/conf/redis_${REDIS_PORT}.conf
PATH_LOG=/opt/redis_cluster/redis_${REDIS_PORT}/logs/redis_${REDIS_PORT}.log

CMD_START(){
    redis-server ${PATH_CONF}
}

CMD_SHUTDOWN(){
    redis-cli -c -h ${REDIS_IP} -p ${REDIS_PORT} shutdown
}

CMD_LOGIN(){
    redis-cli -c -h ${REDIS_IP} -p ${REDIS_PORT}
}

CMD_PS(){
    ps -ef|grep redis
}

CMD_TAIL(){
    tail -f ${PATH_LOG}
}

case $1 in
    start)
        CMD_START
        CMD_PS
        ;;
    stop)
        CMD_SHUTDOWN
        CMD_PS
        ;;
    restart)
        CMD_START
        CMD_SHUTDOWN
        CMD_PS
        ;;
    login)
        CMD_LOGIN
        ;;
    ps)
        CMD_PS
        ;;
    tail)
        CMD_TAIL
        ;;
    *)
        USAG
esac
```

## 10.2 数据导入导出工具

　　需求背景  
 刚切换到redis集群的时候肯定会面临数据导入的问题,所以这里推荐使用redis-migrate-tool工具来导入单节点数据到集群里  
 官方地址：  
[http://www.oschina.net/p/redis-migrate-tool](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.oschina.net%2Fp%2Fredis-migrate-tool)  
 安装工具

```bash
cd /opt/redis_cluster/
git clone https://github.com/vipshop/redis-migrate-tool.git
cd redis-migrate-tool/
autoreconf -fvi
./configure
make && make install 
```

　　创建配置文件

```css
[root@db01 ~]# cat redis_6379_to_6380.conf  
[source]
type: single
servers:
- 10.0.0.51:6379
 
[target]
type: redis cluster
servers:
- 10.0.0.51:6380 
 
[common]
listen: 0.0.0.0:8888
source_safe: true
```

　　生成测试数据

```bash
[root@db01 ~]# cat input_key.sh 
#!/bin/bash
for i in $(seq 1 1000)
do
    redis-cli -c -h db01 -p 6379 set k_${i} v_${i} && echo "set k_${i} is ok"
done
```

　　执行导入命令

```csharp
[root@db01 ~]# redis-migrate-tool -c redis_6379_to_6380.conf 
```

　　数据校验

```csharp
[root@db01 ~]# redis-migrate-tool -c redis_6379_to_6380.conf -C redis_check
```

## 10.3 分析键值大小

　　需求背景  
 redis的内存使用太大键值太多,不知道哪些键值占用的容量比较大,而且在线分析会影响性能.  
 安装工具

```bash
yum install python-pip gcc python-devel 
cd /opt/
git clone https://github.com/sripathikrishnan/redis-rdb-tools
cd redis-rdb-tools
python setup.py install
```

　　使用方法

```bash
cd /data/redis_cluster/redis_6380/
rdb -c memory redis_6380.rdb -f redis_6380.rdb.csv
```

　　分析rdb并导出

```dart
awk -F ',' '{print $4,$2,$3,$1}' redis_6380.rdb.csv |sort  > 6380.txt
```

## 10.4 监控过期键

　　需求背景  
 因为开发重复提交，导致电商网站优惠卷过期时间失效  
 问题分析  
 如果一个键已经设置了过期时间，这时候在set这个键，过期时间就会取消  
 解决思路  
 如何在不影响机器性能的前提下批量获取需要监控键过期时间  
 1.Keys *  查出来匹配的键名。然后循环读取ttl时间  
 2.scan * 范围查询键名。然后循环读取ttl时间  
 Keys  重操作，会影响服务器性能，除非是不提供服务的从节点  
 Scan 负担小，但是需要去多次才能取完，需要写脚本  
 脚本内容：

```bash
cat 01get_key.sh 
#!/bin/bash
key_num=0
> key_name.log
for line in $(cat key_list.txt)
do
    while true
    do
        scan_num=$(redis-cli -h 192.168.47.75 -p 6380 SCAN ${key_num} match ${line}\* count 1000|awk 'NR==1{print $0}')
        key_name=$(redis-cli -h 192.168.47.75 -p 6380 SCAN ${key_num} match ${line}\* count 1000|awk 'NR>1{print $0}')
        echo ${key_name}|xargs -n 1 >> key_name.log
        ((key_num=scan_num))
        if [ ${key_num} == 0 ]
           then
           break
        fi
    done
done
```

# 第11章 故障案例

　　作者：被运维耽误的厨子  
链接：https://www.jianshu.com/p/2f93bb771469  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
