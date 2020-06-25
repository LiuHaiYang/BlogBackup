---
title: python多进程部署下下日志安全问题
date: 2020-05-31 21:43:25
tags: python
---

![image](https://images.pexels.com/photos/4328962/pexels-photo-4328962.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

---

python 项目写了好几个了，自己感觉一直不专业，功能肯定没问题的，但是很多工程化的东西，也不知道自己搞得是不是正式或正确。
也就这么一直搞着，很多东西都是看的博客和一些资料。
最近封装了一些简单的接口，调用量还挺高的，对并发也是有很高的要求的，也做了很久了。
最近在日志监控时发现日志输出混乱：
使用的是logging 库中 RotatingFileHandler 和 TimedRotatingFileHandler  handle，本地测试期间都没问题，部署上线后，短期内也是没发现任务问题，但是在日志切割后就发现了问题，每天百万接口的调用量居然成30w了，
跟调用方对过之后发现，问题还是出在了我们这边，然后进行检查，看到日志会输出到多个历史文件中，监控日志监控的是最新的日志文件，所以调用日志有误差。
得出的结论是，日志安全问题，因为线上是用 gunicorn 多进程部署的，我用到的日志库不是进程安全的。

---
一下是查资料到的，大佬的总结：转 [Python日志模块-多进程日志记录](https://juejin.im/post/5e1303026fb9a0482c4ea59f)

当前使用gunicorn的多进程启动程序，多半是多个进程同时写入当个文件造成日志文件丢失。

logging模块是线程安全的，但并不是进程安全的。

logging中RotatingFileHandler类和TimedRotatingFileHandler类分别实现按照日志文件大小和日志文件时间来切分文件，均继承自BaseRotatingHandler类。
BaseRotatingHandler类中实现了文件切分的触发和执行.

分析如上过程，整个步骤是：

当前正在处理的日志文件名为self.baseFilename，该值self.baseFilename = os.path.abspath(filename)是设置的日志文件的绝对路径，假设baseFilename为error.log。
当进行文件回滚的时候，会依次将error.log.i重命名为error.log.i+1。
判断error.log.1是否存在，若存在，则删除，将当前日志文件error.log重命名为error.log.1。
self.stream重新指向新建error.log文件。

当程序启动多进程时，每个进程都会执行doRollover过程，若有多个进程进入临界区，则会导致dfn被删除多次等多种混乱操作。

2.2 多进程日志安全输出到同一文件方案
相应的解决方法：

将日志发送到同一个进程中，由该进程负责输出到文件中（使用Queue和QueueHandler将所有日志事件发送至一个进程中）
对日志输出加锁，每个进程在执行日志输出时先获得锁（用多处理模块中的Lock类来序列化对进程的文件访问）
让所有进程都将日志记录至一个SocketHandler，然后用一个实现了套接字服务器的单独进程一边从套接字中读取一边将日志记录至文件（Python手册中提供）


3.1 使用ConcurrentRotatingFileHandler包

该方法就属于加锁方案。

ConcurrentLogHandler 可以在多进程环境下安全的将日志写入到同一个文件，并且可以在日志文件达到特定大小时，分割日志文件（支持按文件大小分割）。但ConcurrentLogHandler 不支持按时间分割日志文件的方式。
ConcurrentLogHandler 模块使用文件锁定，以便多个进程同时记录到单个文件，而不会破坏日志事件。该模块提供与RotatingFileHandler类似的文件循环方案。
该模块尝试不惜一切代价保存记录，这意味着日志文件将大于指定的最大大小（如果磁盘空间不足，则坚持使用RotatingFileHandler，因为它是严格遵守最大文件大小），如果有多个脚本的实例同时运行并写入同一个日志文件，那么所有脚本都应该使用ConcurrentLogHandler，不应该混合和匹配这这个类。
并发访问通过使用文件锁来处理，该文件锁应确保日志消息不会被丢弃或破坏。这意味着将为写入磁盘的每个日志消息获取并释放文件锁。（在Windows上，您可能还会遇到临时情况，必须为每个日志消息打开和关闭日志文件。）这可能会影响性能。在我的测试中，性能绰绰有余，但是如果您需要大容量或低延迟的解决方案，建议您将其放在其他地方。
ConcurrentRotatingFileLogHandler类是python标准日志处理程序RotatingFileHandler的直接替代。
这个包捆绑了portalocker来处理文件锁定。由于使用了portalocker模块，该模块当前仅支持“nt”和“posix”平台。


---
后续补充

