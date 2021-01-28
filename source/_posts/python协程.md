---
title: python协程
date: 2019-05-27 12:40:03
tags: python
---

![image](https://images.pexels.com/photos/2363367/pexels-photo-2363367.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
### 实例code:


```
import asyncio
import concurrent.futures
from concurrent.futures import ThreadPoolExecutor
import time

def cpu_bound(i,j):
    for o in range(10):
        asyncio.sleep(1)
        print('a----->>',o)
    print(i,j)
    return sum((i,j))

async def main():

    loop = asyncio.get_event_loop()
    executor = ThreadPoolExecutor(1)

    i,j = 0,1
    res = loop.run_in_executor(executor, cpu_bound, i, j)

    n = 0
    for l in range(10):
        print('m--->',l)
        time.sleep(1)
        n+=l
        if n >20:
            await_res = await res
            print(n,await_res)
            break

if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
    ## 先起一个线程
```

其中用到的是 `run_in_executor` 函数



### 多线程比协程有何优势？

最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换（协程的特点在于是一个线程执行）而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。

第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。

因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

Python对协程的支持还非常有限，用在generator中的yield可以一定程度上实现协程。虽然支持不完全，但已经可以发挥相当大的威力了。

### yield

Python通过yield提供了对协程的基本支持，但是不完全。而第三方的gevent为Python提供了比较完善的协程支持。

gevent是第三方库，通过greenlet实现协程，其基本思想是：

当一个greenlet遇到IO操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO。

由于切换是在IO操作时自动完成，所以gevent需要修改Python自带的一些标准库，这一过程在启动时通过monkey patch完成：

    from gevent import monkey; monkey.patch_socket()
    import gevent

    def f(n):
        for i in range(n):
            print gevent.getcurrent(), i

    g1 = gevent.spawn(f, 5)
    g2 = gevent.spawn(f, 5)
    g3 = gevent.spawn(f, 5)
    g1.join()
    g2.join()
    g3.join()

运行结果：

    <Greenlet at 0x10e49f550: f(5)> 0
    <Greenlet at 0x10e49f550: f(5)> 1
    <Greenlet at 0x10e49f550: f(5)> 2
    <Greenlet at 0x10e49f550: f(5)> 3
    <Greenlet at 0x10e49f550: f(5)> 4
    <Greenlet at 0x10e49f910: f(5)> 0
    <Greenlet at 0x10e49f910: f(5)> 1
    <Greenlet at 0x10e49f910: f(5)> 2
    <Greenlet at 0x10e49f910: f(5)> 3
    <Greenlet at 0x10e49f910: f(5)> 4
    <Greenlet at 0x10e49f4b0: f(5)> 0
    <Greenlet at 0x10e49f4b0: f(5)> 1
    <Greenlet at 0x10e49f4b0: f(5)> 2
    <Greenlet at 0x10e49f4b0: f(5)> 3
    <Greenlet at 0x10e49f4b0: f(5)> 4


可以看到，3个greenlet是依次运行而不是交替运行。

要让greenlet交替运行，可以通过gevent.sleep()交出控制权：

    def f(n):
        for i in range(n):
            print gevent.getcurrent(), i
            gevent.sleep(0)
执行结果：

    <Greenlet at 0x10cd58550: f(5)> 0
    <Greenlet at 0x10cd58910: f(5)> 0
    <Greenlet at 0x10cd584b0: f(5)> 0
    <Greenlet at 0x10cd58550: f(5)> 1
    <Greenlet at 0x10cd584b0: f(5)> 1
    <Greenlet at 0x10cd58910: f(5)> 1
    <Greenlet at 0x10cd58550: f(5)> 2
    <Greenlet at 0x10cd58910: f(5)> 2
    <Greenlet at 0x10cd584b0: f(5)> 2
    <Greenlet at 0x10cd58550: f(5)> 3
    <Greenlet at 0x10cd584b0: f(5)> 3
    <Greenlet at 0x10cd58910: f(5)> 3
    <Greenlet at 0x10cd58550: f(5)> 4
    <Greenlet at 0x10cd58910: f(5)> 4
    <Greenlet at 0x10cd584b0: f(5)> 4

3个greenlet交替运行，

把循环次数改为500000，让它们的运行时间长一点，然后在操作系统的进程管理器中看，线程数只有1个。

当然，实际代码里，我们不会用gevent.sleep()去切换协程，而是在执行到IO操作时，gevent自动切换，代码如下：

    from gevent import monkey; monkey.patch_all()
    import gevent
    import urllib2

    def f(url):
        print('GET: %s' % url)
        resp = urllib2.urlopen(url)
        data = resp.read()
        print('%d bytes received from %s.' % (len(data), url))

    gevent.joinall([
            gevent.spawn(f, 'https://www.python.org/'),
            gevent.spawn(f, 'https://www.yahoo.com/'),
            gevent.spawn(f, 'https://github.com/'),
    ])

运行结果：

    GET: https://www.python.org/
    GET: https://www.yahoo.com/
    GET: https://github.com/
    45661 bytes received from https://www.python.org/.
    14823 bytes received from https://github.com/.
    304034 bytes received from https://www.yahoo.com/.

从结果看，3个网络操作是并发执行的，而且结束顺序不同，但只有一个线程。

[廖雪峰协程](https://www.liaoxuefeng.com/wiki/897692888725344/923057403198272
[协程和任务](https://docs.python.org/zh-cn/3/library/asyncio-task.html)
