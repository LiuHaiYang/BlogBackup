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
|`vm-enabled no`|指定是否启用虚拟内存机制，默认值为 no，简单的介绍一下，VM 机制将数据分页存放，由 Redis 将访问量较少的页即冷数据 swap 到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析 Redis 的 VM 机制）|

redis数据类型：
redis支持五种数据类型：String（字符串），hash（哈希），list（列表），set（集合）及zset（sorted set）有序集合。

String（字符串）
string 是redis 最基本的类型，你可以理解成与Memached 一模一样的类型，一个 key 对应一个 value。
string 类型是二进制安全的，意思是 redis 的string 可以包含任何数据，比如jpg图片或者序列化的对象。
string 类型是redis最基本的数据类型，string 类型的值最大能存储512MB。

'''
set key value
get key
'''
Hash(哈希)
redis hash 是一个键值（key value） 对集合。
redis hash 是一个string 类型的 field 和 value的映射表，hash 特别适合用于存储对象。
del key 删除key
HMSET key field1 value1 field2 value2
HGET key field1

实例中我们使用了redis HMSET HGET 命令，HMSET 设置了两个 field => value 对，HGET 获取field对应的value。
每个hash 可以存储 2 ** 32-1键值对（40多亿）

List（列表）

redis 列表是简单的字符串列表，按照插入顺序排序，你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

'''
lpush key value
lrange key 0 10
'''

Set (集合)
redis 的set是string类型的无序集合
集合是通过hash表实现的，所以添加，删除，查找的复杂度都是 O(1).

sadd命令
添加一个string 元素到key对应的set集合中，成功返回1，如果元素已经在集合中返回0.

'''
sadd key value
smembers key
'''

注意：如果实例被添加两次，但根据集合内元素的唯一性，第二次插入元素将被忽略。
集合中最大的成员数为2 ** 32 -1 （最多40多亿个成员）

Zset （sorted set 有序集合）
redis zset 和 set 一样，也是string类型元素的集合，且不允许重复的成员。
不同的每个成员元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序。
zset 的成员是唯一的，但是分数（score）却可以重复。

zadd 命令：添加元素到集合，元素在集合中存在则更新对应score。

'''
zadd runoob 0 redis
zadd runoob 0 rabitmq
ZRANGEBYSCORE runoob 0 1000
'''

---

redis 命令

redis client： redis-cli -h host -p port -a pwd
PING -> PONG

---

redis 键（key）

redis 键命令用于管理redis的键。
'''
set
get
del 如果键被删除成功，命令执行后输出 (integer) 1，否则将输出 (integer) 0
'''

|序号|命令|描述|
|----|----|----|
|1| DEL key |该命令用于在 key 存在时删除 key。|
|2| DUMP key | 序列化给定 key ，并返回被序列化的值。 |
|3 | 	EXISTS key | 检查给定 key 是否存在。|
|4 | EXPIRE key seconds | 为给定 key 设置过期时间，以秒计。|
|5 | KEYS pattern | 查找所有符合给定模式( pattern)的 key 。|
|6 | PERSIST key | 移除 key 的过期时间，key 将持久保持。|
|7| PTTL key | 以毫秒为单位返回 key 的剩余的过期时间。|
|8| RENAME key newkey | 修改 key 的名称 |

---
redis 字符串（String）

'''
SET key value
GET key
GETRANGE key start end  返回 key 中字符串值的子字符
GETSET key value 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。
'''









