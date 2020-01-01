---
title: Django基础
tags: python
abbrlink: 53101
date: 2017-03-21 12:25:16
---
Django 是用Python开发的一个免费开源的Web框架，可以用于快速搭建高性能，优雅的网站！

Django 特点
强大的数据库功能
用python的类继承，几行代码就可以拥有一个丰富，动态的数据库操作接口（API），如果需要你也能执行SQL语句
自带的强大的后台功能
几行简单的代码就让你的网站拥有一个强大的后台，轻松管理你的内容！
优雅的网址
用正则匹配网址，传递到对应函数，随意定义，如你所想！
模板系统
强大，易扩展的模板系统，设计简易，代码，样式分开设计，更容易管理。
缓存系统
与memcached或其它的缓存系统联用，更出色的表现，更快的加载速度。
国际化
完全支持多语言应用，允许你定义翻译的字符，轻松翻译成不同国家的语言。

一览 Django 全貌
urls.py
网址入口，关联到对应的views.py中的一个函数（或者generic类），访问网址就对应一个函数。
views.py
处理用户发出的请求，从urls.py中对应过来, 通过渲染templates中的网页可以将显示内容，比如登陆后的用户名，用户请求的数据，输出到网页。
models.py
与数据库操作相关，存入或读取数据时用到这个，当然用不到数据库的时候 你可以不使用。
forms.py
表单，用户在浏览器上输入数据提交，对数据的验证工作以及输入框的生成等工作，当然你也可以不使用。
templates 文件夹
views.py 中的函数渲染templates中的Html模板，得到动态内容的网页，当然可以用缓存来提高速度。
admin.py
后台，可以用很少量的代码就拥有一个强大的后台。
settings.py
Django 的设置，配置文件，比如 DEBUG 的开关，静态文件的位置等。



虚拟环境

    mkvirtualenv zqxt：创建运行环境zqxt
    workon zqxt: 工作在 zqxt 环境 或 从其它环境切换到 zqxt 环境
    deactivate: 退出终端环境

其它的：

    rmvirtualenv ENV：删除运行环境ENV
    mkproject mic：创建mic项目和运行环境mic
    mktmpenv：创建临时运行环境
    lsvirtualenv: 列出可用的运行环境
    lssitepackages: 列出当前环境安装了的包
创建的环境是独立的，互不干扰，无需sudo权限即可使用 pip 来进行包的管理。


