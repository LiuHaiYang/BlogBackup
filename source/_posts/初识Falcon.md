---
title: 初识Falcon
tags: python
abbrlink: 8043
date: 2018-08-19 01:34:15
---
#### Falcon简介
Falcon 是一个高性能的 Python 框架，用于构建云端 API 和 Web 应用的后端程序。
falcon主要还是针对RESTful服务打造.

#### 设计目标
 - Fast
 - light
 - Flexible
#### 特性
 - 通过URI模板和资源类可直观的了解路由信息
 - 轻松访问请求和响应类来访问header和body信息
 - 通过方便的异常类实现对HTTP错误响应的处理
 - 通过全局、资源和方法钩子实现DRY请求处理
 - 通过WSGI helper和mock实现单元测试
 - 使用Cython可提升20%的速度
 - 支持Python 2.6,Python 2.7,PyPy和Python 3.3/3.4
 - 高性能

使用pip来安装，` pip install falcon`
安装gunicorn，gunicorn主要是后台服务的稳定运行导入falcon模块，然后就编辑自己的纯逻辑代码

示例代码：
    # sample.py
    
    import falcon
    
    class QuoteResource:
        def on_get(self, req, resp):
            """Handles GET requests"""
            quote = {
                'quote': (
                    "I've always been more interested in "
                    "the future than in the past."
                ),
                'author': 'Grace Hopper'
            }
    
            resp.media = quote
    
    api = falcon.API()
    api.add_route('/quote', QuoteResource())
---
    $ pip install falcon
    $ gunicorn sample:api