---
title: 初识Tornado
tags: python
abbrlink: 8672
date: 2018-08-19 00:22:24
---
` Tornado是使用Python编写的一个强大的可扩展的Web服务器 `

tornado 框架特点：
 - ##### 完备的web框架：
   与django，Flask框架等一样，Tornado也提供了URL路由映射，request 上下文，基于模板引擎的页面渲染技术等开发web应用 的必备工具。
 - ##### 非阻塞式服务器且速度相当快：
   tornado每秒可以处理数以千计的链接，基于利于非阻塞的方式和对epoll的运用。
 - ##### 高效的网络库：
   其性能可以与Twisted，Gevent等底层python框架相媲美，提供了异步I/O支持。超时时间处理。这使得Tornado除了可以作为web服务器框架，还可以来做爬虫应用，物联网网关，游戏服务器等后台应用。
 - ##### 提供高效的HTTPClient：
   除了服务器端框架，tornado还提供了基于异步框架的HTTP客户端。
 - ##### 提供高效的内部HTTPSever：
   虽然其他python网络框架（Django，Flask）也提供了内部HTTP服务器，但他们由于性能原因只能用于测试环境。而tornado的HTTPserver与tornado异步调用紧密结合，可以直接用于生成环境。
 - ##### 完备的websocket支持：
   websocket是HTML5   的一种新的标准，实现了浏览器和服务器之间的双向 实时通信。

根据以上特点：

` Tornado常被用作大型站点的接口服务框架，而不像Django那样着眼于建立完整的大型网站。除此之外，Tornado还常用于异步及协程编程、身份认证框架、独特的非WSGI部署方式等等 `
 
 
 #### 工作流程图
 ![流程图](https://gss1.bdstatic.com/-vo3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=1829173a99ef76c6c4dff379fc7f969f/faedab64034f78f00ac25fe870310a55b2191ce4.jpg)
 
 #### 同步和异步及yield
 
 `  协程是Tornado中推荐的编程方式，使用协程可以开发出简捷、高效的异步处理代码 `
 
 1) 同步IO 和 异步IO
    - 同步IO：导致请求进程阻塞，直到IO操作完成。在python中，同步IO可以被理解为一个被调用 的函数会阻塞调用函数的执行。
    - 异步IO：不会导致请求进程阻塞。可以理解为一个被调用的IO函数不会阻塞调用函数的执行。
 
   - 代码举例如下：
   
    #同步I/O --- 使用HTTPClinet客户端
    from tornado.httpclient import HTTPClient #tornado的同步HTTP客户端类
    print '访问前'
    def syn_visit():
        http_client = HTTPClient()
        print '访问中...'  
        response = http_client.fetch("http://www.lanou3g.com") #阻塞,直到访问完成
        print response.body
    syn_visit()
    print '访问后'

HTTPClinet是tornado的同步访问HTTP客户端。

上述代码中的synchronous_visit()函数使用了典型的同步I/O操作访问www.baidu.com网站，该函数的执行时间取决于网络速度、服务器响应速度等，只有对网站访问完成并获取响应结果后，才能完成synchronous_visit()整个函数的执行。

   - 异步代码举例如下：
   
    #异步I/O --- 使用AsyncHTTPClient客户端
    from tornado.httpclient import AsyncHTTPClient #tornado的异步HTTP客户端类
    def handle_response(response):
        print response.body  
    print '访问前'
    def asyn_visit():
        http_client = AsyncHTTPClient()
        print '访问中'
        response = http_client.fetch("http://www.lanou3g.com"，callback=handle_response) #不会阻塞进程
    asyn_visit()
    print '访问后' 

AsyncHTTPClient是Tornado的异步访问HTTP客户端。

代码中的asyn_visit()函数中使用AsyncHTTPClient对第三方网站进行异步访问，http_client.fetch()函数会在调用后立刻返回而无须等待实际访问的完成，从而导致asyn_visit()也会立刻执行完成。当访问实际完成后，AsyncHTTPClient会调用callback参数指定的函数。

2) python 关键字 yield
    - 协程是Tornado中进行异步I/O代码开发的方法。
    - 协程使用了Python关键字yield将调用者挂起和恢复执行。所以在学习协程编程之前，首先掌握Python中yield关键字的使用。
    - 迭代器在Python编程中适用范围很广，那么开发者如何定制自己的迭代器呢？答案就是使用yield关键字。
     
     ` 当调用任何定义中包含yield关键字的函数都不会执行该函数，而会或得一个对应函数的迭代器。 `
 
    示例代码：
    
        #自定义迭代器函数
        def demoIterator():
            print "The first call of next()"
            yield 'number 1'
            print "The second call of next()"
            yield 'number 2'
            print "The third call of next()"
            yield 'number3'
        print type(demoIterator())  #<type 'generator'>
        for i in demoIterator():
            print i
    使用for循环会每次调用迭代器的next()函数，将执行迭代器函数，并返回yield的结果作为迭代返回元素。
    
#### 协程编程基础

` 使用Tornado协程可以开发出类似同步代码的异步行为。同时，因为协程本身不使用线程，所以减少了线程上下文切换的开销。`

1) 协程函数的编写
实例代码：

        # -*- coding: utf-8 -*-
        from tornado import gen  #引入协程库gen
        from tornado.httpclient import AsyncHTTPClient #引入框架中的异步客户端
        @gen.coroutine
        def cor_visit():
            http_client = AsyncHTTPClient()
            response = yield http_client.fetch('http://www.lanou3g.com')
            print response.body
上述代码中，仍然使用了异步客户端AsyncHTTPClient进行页面访问，修饰器@gen.coroutine声明其是一个协程函数。由于使用了yield关键字，所以代码中不用再编写回调函数用于处理访问结果，而可以直接在yield语句的后面编写结果处理语句。
2) 协程函数的使用
` 由于tornado协程基于python的yield关键字实现，所以不能像调用一般函数去调用。`

调用协程函数的方式有三种：

(1) 第一种方式：

在本身是协程的函数内调用另一个协程函数，可以通过yield关键字调用。

    @gen.coroutine
    def use_cor():
        print "开始调用其它协程函数"
        yield cor_visit()
        print "结束调用其它协程函数"
(2) 第二种方式：

在IOLoop尚未启动时，通过IOLoop的run_sync()函数。

    from tornado import gen # 引入协程库
    from tornado.ioloop import IOLoop
    from tornado.httpclient import AsyncHTTPClient
    def loop_stop_visit():
        print('开始调用协程')
        IOLoop.current().run_sync(lambda: cor_visit())
        print('结束协程调用')
    loop_stop_visit()
其中，IOLoop 是Tornado的主事件循环对象，Tornado程序通过它监听外部客户端的访问请求，并执行相应的操作，当程序尚未进入IOLoop的runing状态时，可以通过run_sync()函数调用协程函数。run_sync()函数会将阻塞当前函数的执行，直到被调用的协程函数执行完成。

(2) 第三种方式：

在IOLoop已经启动时，通过IOLoop的spawn_callback()函数调用。

    def loop_start_visit():
        IOLoop.current().spawn_callback(coroutine_visit)
    loop_start_visit()    #调用

上述代码中，` spawn_callback()`函数不会等待被调用的协程函数执行完成，所以` spawn_callback()` 之前和之后的语句会被连续执行，而cor_visit本身将会有IOLoop在合适的时机进行调用，并且` spawn_callback ` 函数没有为开发者提供获取协程函数调用函数返回值的方法，所以只能用`  spawn_callback() `调用没有返回值的协程函数。