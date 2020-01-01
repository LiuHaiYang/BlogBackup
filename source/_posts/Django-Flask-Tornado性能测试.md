---
title: 'Django_Flask_Tornado性能测试'
tags: python
abbrlink: 26134
date: 2018-08-20 23:38:01
---
本次测试在windows平台下进行。

在性能测试前先了解一款测力测试软件: ` siege`

一款开源的压力测试工具，可以根据配置对一个WEB站点进行多用户的并发访问，记录每个用户所有请求过程的相应时间，并在一定数量的并发访问下重复进行。

windows版软件下载：` https://download.csdn.net/download/masonwu21/6254773`

使用方法：

` siege -c 200 -r 10 -b example.url `
 
     -c 是并发量，-r 是重复次数, url文件就是一个文本，每行都是一个url，它会从里面随机访问的。
     
#### 测试代码

    #Django
    django-admin startproject django_test
    python manage.py startapp app_test
    python manage.py migrate
    python manage.py runserver
    
---

    #Tornado
    # coding=utf-8

    import tornado.ioloop
    import tornado.web
    
    class MainHandler(tornado.web.RequestHandler):
        def get(self):
            self.write("Hello, world")
    def make_app():
        return tornado.web.Application([
            (r"/", MainHandler),
        ])
    if __name__ == "__main__":
        app = make_app()
        app.listen(8888)
        tornado.ioloop.IOLoop.current().start()

---

    #Flask
    # coding=utf-8
    from flask import Flask
    import logging
    app = Flask(__name__)
    
    @app.route('/')
    def hello_world():
        return 'Hello World!'
    
    if __name__ == '__main__':
        app.run(port=8888, debug=False)


--- 
#### 测试

##### Django

    siege -c 2 -r 50 -b http://127.0.0.1:8000/
    
    Transactions:           100 hits
    Availability:           100.00 %
    Elapsed time:           1.66 secs
    Data transferred:       1.56 MB
    Response time:          0.03 secs
    Transaction rate:       60.17 trans/sec
    Throughput:             0.94 MB/sec
    Concurrency:            1.78
    Successful transactions:100
    Failed transactions:    0
    Longest transaction:    0.24
    Shortest transaction:   0.02

##### Flask

    siege -c 2 -r 50 -b http://127.0.0.1:8888/
             100 hits
          100.00 %
            1.02 secs
            0.00 MB
            0.02 secs
           98.33 trans/sec
            0.00 MB/sec
            1.66
             100
               0
            0.25
            0.01

##### Tornado

    siege -c 2 -r 50 -b http://127.0.0.1:8888/
             100 hits
          100.00 %
            0.93 secs
            0.00 MB
            0.02 secs
          107.76 trans/sec
            0.00 MB/sec
            1.65
             100
               0
            0.38
            0.01

### 可以从测试的指标明显的看出不同框架的性能如何！

---
续
---

### Flask

```
 Name                                                          # reqs      # fails     Avg     Min     Max  |  Median   req/s
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                          14248     0(0.00%)    1145       2    6644  |     860  320.70
--------------------------------------------------------------------------------------------------------------------------------------------
 Total                                                          14248     0(0.00%)                                     320.70

Percentage of the requests completed within given times
 Name                                                           # reqs    50%    66%    75%    80%    90%    95%    98%    99%   100%
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                           14248    860   1600   1900   2100   3000   3400   4100   4600   6600
--------------------------------------------------------------------------------------------------------------------------------------------
 Total   
```

### Tornado

```
  Name                                                          # reqs      # fails     Avg     Min     Max  |  Median   req/s
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                          25537     0(0.00%)     333       1    2016  |     230  531.20
--------------------------------------------------------------------------------------------------------------------------------------------
 Total                                                          25537     0(0.00%)                                     531.20

Percentage of the requests completed within given times
 Name                                                           # reqs    50%    66%    75%    80%    90%    95%    98%    99%   100%
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                           25537    230    340    440    520   1000   1200   1300   1400   2000
--------------------------------------------------------------------------------------------------------------------------------------------
 Total                                                           25537    230    340    440    520   1000   1200   1300   1400   2000

```

### Django

```
 Name                                                          # reqs      # fails     Avg     Min     Max  |  Median   req/s
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                          11662   143(1.21%)    3954       3   24863  |    2900  282.90
--------------------------------------------------------------------------------------------------------------------------------------------
 Total                                                          11662   143(1.23%)                                     282.90

Percentage of the requests completed within given times
 Name                                                           # reqs    50%    66%    75%    80%    90%    95%    98%    99%   100%
--------------------------------------------------------------------------------------------------------------------------------------------
 GET /                                                           11662   2900   6100   7200   7700   8700   9700  17000  24000  25000
--------------------------------------------------------------------------------------------------------------------------------------------
 Total 
```
