---
title: Flume基础入门
tags: 大数据
abbrlink: 28973
date: 2017-07-19 11:01:18
---

#### 日志收集系统

flume （日志收集系统）
Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

#### 工作方式

Flume-og采用了多Master的方式。为了保证配置数据的一致性，Flume引入了ZooKeeper，用于保存配置数据，ZooKeeper本身可保证配置数据的一致性和高可用，另外，在配置数据发生变化时，ZooKeeper可以通知Flume Master节点。Flume Master间使用gossip协议同步数据。
Flume-ng最明显的改动就是取消了集中管理配置的 Master 和 Zookeeper，变为一个纯粹的传输工具。Flume-ng另一个主要的不同点是读入数据和写出数据现在由不同的工作线程处理（称为 Runner）。 在 Flume-og 中，读入线程同样做写出工作（除了故障重试）。如果写出慢的话（不是完全失败），它将阻塞 Flume 接收数据的能力。这种异步的设计使读入线程可以顺畅的工作而无需关注下游的任何问题。
对于数据的采集是十分重要的一步，也是不可避免的一步，Flume的架构、Flume的应用(日志采集)
flume是分布式的日志收集系统，它将各个服务器中的数据收集起来并送到指定的地方去，比如说本地，HDFS在整个数据的传输的过程中，流动的是event，即事务保证是在event级别进行的。那么什么是event呢？—–event将传输的数据进行封装，是flume传输数据的基本单位，如果是文本文件，通常是一行记录，event也是事务的基本单位。event从source，流向channel，再到sink，本身为一个字节数组，并可携带headers(头信息)信息。event代表着一个数据的最小完整单元，从外部数据源来，向外部的目的地去。
一个完整的event包括：event headers、event body、event信息(即文本文件中的单行记录)


#### flume架构介绍 

flume之所以这么神奇，是源于它自身的一个设计，这个设计就是agent，agent本身是一个java进程，运行在日志收集节点—所谓日志收集节点就是服务器节点。 
agent里面包含3个核心的组件：source—->channel—–>sink,类似生产者、仓库、消费者的架构。 
source：source组件是专门用来收集数据的，可以处理各种类型、各种格式的日志数据,包括avro、thrift、exec、jms、spooling directory、netcat、sequence generator、syslog、http、legacy、自定义。 
channel：source组件把数据收集来以后，临时存放在channel中，即channel组件在agent中是专门用来存放临时数据的——对采集到的数据进行简单的缓存，可以存放在memory、jdbc、file等等。 
sink：sink组件是用于把数据发送到目的地的组件，目的地包括hdfs、logger、avro、thrift、ipc、file、null、hbase、solr、自定义。 

#### flume的运行机制 

flume的核心就是一个agent，这个agent对外有两个进行交互的地方，一个是接受数据的输入——source，一个是数据的输出sink，sink负责将数据发送到外部指定的目的地。source接收到数据之后，将数据发送给channel，chanel作为一个数据缓冲区会临时存放这些数据，随后sink会将channel中的数据发送到指定的地方—-例如HDFS等，注意：只有在sink将channel中的数据成功发送出去之后，channel才会将临时数据进行删除，这种机制保证了数据传输的可靠性与安全性。 

#### flume的广义用法 

flume之所以这么神奇—-其原因也在于flume可以支持多级flume的agent，即flume可以前后相继，例如sink可以将数据写到下一个agent的source中，这样的话就可以连成串了，可以整体处理了。flume还支持扇入(fan-in)、扇出(fan-out)。所谓扇入就是source可以接受多个输入，所谓扇出就是sink可以将数据输出多个目的地destination中。 

#### flume应用—日志采集 

对于flume的原理其实很容易理解，我们更应该掌握flume的具体使用方法，flume提供了大量内置的Source、Channel和Sink类型。而且不同类型的Source、Channel和Sink可以自由组合—–组合方式基于用户设置的配置文件，非常灵活。比如：Channel可以把事件暂存在内存里，也可以持久化到本地硬盘上。Sink可以把日志写入HDFS, HBase，甚至是另外一个Source等等。下面我将用具体的案例详述flume的具体用法。 