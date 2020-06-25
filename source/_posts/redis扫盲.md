---
title: redis扫盲
date: 2020-06-17 20:23:57
tags: 数据库
---

![image](https://images.pexels.com/photos/4507967/pexels-photo-4507967.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)

---

Redis 是一个key-value存储系统，高性能的k-v数据库。
Redis 是一个开源的使用ANSI C语言编写，遵守BSD协议，支持网络，可基于内存，亦可持久化的日志型、key-value数据库，并提供多种语言的API。
它通常被称为数据结构服务器，因为值 value 可以是字符串(String)、哈希(hash)、列表(list)、集合(sets)、和有序集合(sorted sets)等类型。

redis与其他k-v缓存产品有以下三个特点：
- redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- redis不仅仅支持简单的k-v类型的数据，同时提供list，set，zset，hash 等数据结构的存储。
- redis支持数据的备份，即master-slave 模式的数据备份。

redis优势：
- 性能极高-redis读的速度是110000次/s 写的速度是 81000次/s
- 丰富的数据类型，redis支持二进制案例的Strings，Lists，Hashes，sets及Ordered Sets 数据类型操作。
- 原子-redis的所有操作都是原子性的，意思就是要么执行成功要么失败完全不执行，单个操作是原子性的，多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性-redis还支持 publish 和 subscribe，通知，key过期等特性。

redis 与其他k-v存储有什么不同？
- Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
- Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。

redis 配置：
- redis config 命令格式如下： config get xxxx or * (获取所有的配置项)
- 可以通过修改  redis.conf 文件或使用 CONFIG set 命令来修改配置
- 部分配置项：
| 配置项 | 说明|
| :-: | :-: |
|daemonize no	|Redis 默认不是以守护进程的方式运行，可以通过该配置项修改，使用 yes 启用守护进程（Windows 不支持守护线程的配置为 no ）|
|databases 16	|设置数据库的数量，默认数据库为0，可以使用SELECT 命令在连接上指定数据库id|
| `save <seconds> <changes>`Redis 默认配置文件中提供了三个条件：save 900 1 save 300 10save 60 10000 分别表示 900 秒（15 分钟）内有 1 个更改，300 秒（5 分钟）内有 10 个更改以及 60 秒内有 10000 个更改。|指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合|
|`slaveof <masterip> <masterport>`|设置当本机为 slave 服务时，设置 master 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步|
|appendfsync everysec	|指定更新日志条件，共有 3 个可选值： no：表示等操作系统进行数据缓存同步到磁盘（快） always：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全） everysec：表示每秒同步一次（折中，默认值）|
|||
|||
|||
|||