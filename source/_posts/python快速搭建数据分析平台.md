---
title: python快速搭建数据分析平台superset
tags: 数据分析
abbrlink: 45133
date: 2017-09-04 14:12:51
---

使用 python  快速搭建数据分析平台，这是我在学习搭建平台中可能是是最简单的教程，有一点 python 基础就行，他就是Superset。因为他是开源的，所以借助工具搭建十分的方便。

所有数据分析平台，一般都会有数据载入(csv , mysql  ... )、数据查询、数据统计、数据可视化等功能。但是以往的很多人，都生活在只有sql的数据分析中，简直就是在浪费生命。

## **Superset**

Superset是一款轻量级的BI工具，由Airbnb的数据部门开源。整个项目基于Python框架，它集成了Flask、D3、Pandas、SqlAlchemy等。

在windows环境下部署

一、环境准备

python3.5 、pip安装工具、虚拟环境venv   (为了不和本地的python库 发生冲突，全程都在虚拟环境中操作)

pip install virtualenv

virtualenv env

//等待初始化完成...

//激活：env\Scripts\activate

安装过程需要联网。

二、安装步骤

官方安装说明，比较详细了，在windows环境下安装时有一些坑需要注意。记录一下安装过程。

具体步骤

pip install virtualenv//安装插件

virtualenv venv//创建虚拟环境

venv\Scripts\activate//启动环境

 安装并初始化Superset

首先是官方的步骤

\# 安装Superset

pip install superset

注意，可能会提示no module named fcntl。下载fcntl.py文件放到PYTHONPATH路径即可，我是放到了Python34\Lib目录下。 

\# 创建管理员帐号

fabmanager create-admin --app superset

\# 初始化数据库

superset db upgrade

\# 加载例子

superset load_examples

\# 初始化角色和权限

superset init

\# 启动服务，端口 8088, 使用 -p 更改端口号。

superset runserver

出现的问题：

（1）命令行中，直接运行superset 命令会提示，不是内部或外部命令。

  首先需要进入superset安装目录中运行。

​      Cd   ..\Lib\site-packages\superset\bin

   命令改为

Python superset db upgrade

Python superset load_examples

Python superset init

Python superset runserver

  (2)执行命令Python superset runserver，出错。原因是superset使用*gunicorn**作为应用程序服务器，而**gunicorn**不支持**windows**。命令行中添加**-d**，使用**development web server**运行。最终运行命令为：*

Python superset runserver -d

这时候就可以在浏览器中登录系统查看演示了。

#### 汉化

先别急着使用，因为Superset是英文，我们先把它汉化了。Superset自身支持语言切换。

进入到Superset所在目录文件，按我之前的步骤，应该在anaconda/envs/superset/lib/python3.4/site-packages/superset中，路径视各位情况可能有差异。

在目录下有一个叫config.py的文件，打开它，找到Setup default language这一行，修改变量。

BABEL_DEFAULT_LOCALE调整为zh，这样界面默认为中文。languages字典中zh前面的注释#去掉。保存后退出。

接下来还是在Superset的目录下新创建文件夹，按translations/zh/LC_MESSAGES的路径依次创建三个。Superset官网提供了汉化包，在最大的同性交友网站github上下载，目录为：

> [https://github.com/apache/incubator-superset/blob/master/superset/translations/zh/LC_MESSAGES/messages.mo**](http://link.zhihu.com/?target=https%3A//github.com/apache/incubator-superset/blob/master/superset/translations/zh/LC_MESSAGES/messages.mo)

网址路径有点长，下载后把mo文件放在LC_MESSAGES文件下。清除浏览器的缓存，重新登陆localhost。

搞定！

麻雀虽小，五脏俱全，对于大部分中小型的企业，Superset足以应付数据分析工作。

友情连接：

http://airbnb.io/projects/superset/

https://zhuanlan.zhihu.com/p/28485468?utm_source=qq&utm_medium=social

http://baijiahao.baidu.com/s?id=1575710401053015&wfr=spider&for=pc

http://blog.csdn.net/xiaoqi0531/article/details/53266315

