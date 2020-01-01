---
title: ZooKeeper详解
tags: 大数据
abbrlink: 27538
date: 2017-05-25 11:53:39
---

`ZooKeeper`是一个分布式应用所设计的 开源协调服务 。他可以为用户提供同步、配置管理、分组和命名等服务。

## 简介

### 1、ZooKeeper的设计目标

* (1)简单化: 允许分布式的进程通过共享体系的命名空间来进行协调，这个命名空间的组织 与标准的文件体统非常的相似，它是由一些数据寄存器组成的。只能                用在大型的、分布式的系统中。
* (2)健壮性  :  只要大部分的服务器可用，那么`ZooKeeper`服务就可用。
* (3) 有序性
* (4) 速度优势  : 当读取主要负载尤其快,     当读取工作比写工作更多的时候，它执行的性能会更好。

### 2、数据模型和层次命名空间

`Zookeeper `中的每一个节点是都通过路径来识别的。树形的结构。

### 3、ZooKeeper中的节点和临时的节点

* 每一个节点 还拥有自身的一些信息
* 使用`Znode`来表示所讨论的`ZooKeeper`节点
* `Znode`维护者数据、ACL、时间戳等包含 信息的数据结构

### 4、ZooKeeper的应用

`ZooKeeper`应用于大量的工业程序中。可以用于消息代理的协调和故障恢复服务。  可以管理上千的总联机程序和信息控制系统 ;  还可用于获取服务并恢复错误故障。

## 安装和配置

### 1、集群安装

- 安装的前提是java的环境;

- 在 /etc/profile文件中进行配置

    #Set    ZooKeeper  Enviroment

  export    ZOOKEEPER_HOME=/............

  export    PATH = $$  PATH :                                                    $ZOOKEEPER_HOME/bin:$ZOOKEEPER_HOME/conf

- 在conf文件夹下创建一个zoo.cnf文件，内容如下:

  tickTime=2000

  dataDir = /var/zookeeper

  clientPort=2181

### 2、配置ZooKeeper

- 最低配置

  clientport       dataDir          tickDir

- 高级配置

  dataLogDir     maxClientCnxns   min/maxSession Timeout

- 集群配置  

  initLimit     syncLimit

### 3、运行ZooKeeper

- 单机模式   zkServer.sh   start
- 集群/伪集群  重复命令

### 4、四字命令 （进行交互）

- `conf`   服务配置的详细信息
- `cons`  列出连接到服务器的所有客户端的详细连接/ 会话信息
- `dump`   列出未经处理的会话和临时节点
- `envi`    输出关于服务环境的详细信息
- `reqs`     列出未经处理的请求
- `ruok`    测试服务是否处于正确的状态
- `wchs`    列出服务器watch的详细信息

### 5、ZooKeeper命令行工具

- 成功启动服务后，输入下述的命令，连接 `ZooKeeper`服务:

  zkCli.sh   -server  xx.xx.xx.xx:xxxx

  连接成功后屏幕会输出Welcome ZooKeeper  等信息

## 简单操作

### 1、命令的简单操作步骤

- `ls`  查看 `ZooKeeper`所包含的内容
- 创建新的`Znde`  使用`create /zk myData` 
- `get  data`进行查看
- `set data `对其关联的字符进行设置
- `delete`  对其进行删除
- `sync`  等待要被传送的数据
- `exists`  测试本地是否存在目标节点

### 2、API的简单使用

- 共包含5个包
- 如果要使用`ZooKeeper`服务，必须创建一个实例
- 客户端和ZooKeeper服务建立连接，ZooKeeper系统将此链接回话分配一个ID值，通过发送心跳来维持会话的链接。   客户端可进行调用`API`

## 特性

### 1、数据模型

#### 1、`Znode`  

- 每一个子节点对应这一个`Znode`。维护这一个属性结构，包含数据的版本号，时间戳等的状态信息。

- `Znode`是客户端要访问的主要实体类，其特征:

  watches      数据访问     临时节点   顺时节点

#### 2、ZooKeeper中的时间(有多种记录时间的形式)

- `Zxid`
- 版本号

#### 3、节点属性结构

### 2、会话及状态

- 客户端会通过句柄为`ZooKeeper` 服务建立一个会话，客户端将尝试连接一个服务器，如果成功，他的状态改变为`connected`

### 3、ZooKeeper Watches

- ZooKeeper可以为所有的读操作设置watch 。这些读操作包括: `exists()`  `getChildren()`  `getData()`
- `watch`事件是一次性触发器，并且只有在数据发生改变时，watch时间才能会被发给客户端。
- `ZooKeeper`所管理的`watch`可以分为两类:一类是数据; 另一类是孩子`watch`。`getData()`和`exists`负责设置数据`watch` ,  `getChildren()`负责设置孩子`watch`

### 4、ZooKeeper  ACL

- ZooKeeper使用ACL来对Znode进行访问控制。ACL的实现和UNIX文件访问许可非常相似：他使用许可位来对一个节点的不同操作进行允许或禁止的权限控制。   和标准的UNIX许可不同的是`ZooKeeper`节点有user(文件拥有者)，group,world三种标准模式，并且没有节点所有者的概念。


- 一个ACL和一个ZooKeeper几点相对应，父节点的ACL和孩子节点的ACL是相互独立的。ACL不能被孩子ACL所继承。  两个所用的权限没有任何的关系。

- ZooKeeper  ACL 的使用依赖于验证，支持以下的几种验证模式：

  world  :代表某一个特定的用户

   auth :  代表已经通过验证的用户

  digest : 通过用户名密码进行验证

   ip  : 通过客户端IP 进行验证

### 5、一致性保证

ZooKeeper是一种高性能、可扩展的服务。  读写的速度特别的快，并且读的速度比写的更快，在读的操作的时候，ZooKeeper依然能够为旧的数据提供服务。

提供的一致性保证:

- 顺序一致性
- 原子性
- 单系统镜像
- 可靠性
- 实时性

## Leader选举

所有的服务(可以理解为服务器)中选举出一个Leader， 然后让这个Leader来负责管理集群。当Leader出现故障是，ZooKeeper能够快速的在其他的中选出下一个Leader。

## ZooKeeper锁服务

完全分部的锁是全局同步的。

## 小结

是`Hadoop`集群管理中一个必不可少的模块，他主要用来控制集群中的数据，如管理`Hadoop`集群中`NameNode`，以及`HBase`中的`Master Election` 、 `Server`之间的状态同步。