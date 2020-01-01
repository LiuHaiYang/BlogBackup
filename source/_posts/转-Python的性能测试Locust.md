---
title: 转-Python的性能测试Locust
date: 2018-12-09 18:39:46
tags: python
---

在性能测试时了解到这个测试神器。
![image](https://images.pexels.com/photos/1122530/pexels-photo-1122530.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
Locust（俗称 蝗虫）, 一个轻量级的开源压测工具，用Python编写。

locust是一个易于使用的，分布式的，用户负载测试工具。用于web站点（或其他系统）的负载测试，然后算出系统能够处理多少并发用户。
locust的思想是：在测试期间，一大群"蝗虫"会攻击你的网站，每一个"蝗虫"的行为都是由你自己定义的，同时，可以在一个web界面上实时的监控这群进程。这会帮助你更好的"进行战斗"，在真正的用户进入之前，就找出代码中的瓶颈。

locust完全是事件驱动的，因此它能够在单机支持数以千计的并发用户，相比许多其他的基于事件的应用，locust不使用回调函数。它使用轻量进程---gevent。每一个访问你的网站的locust实际上都在它自己的进程内部运行（准确地说，是greenlet）,也就是我们通常说的协程。这允许你在不使用带回调函数的复杂代码的情形下，使用python写出非常具有表现力的脚本。


有人已总结了Locust与其余性能测试之间的差异。

![image](https://upload-images.jianshu.io/upload_images/618241-f2995940acd64f4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/632/format/webp)

- 得分项：单机并发能力，Python，开源免费
- 掉分项：不支持资源监控，报告异常简单

简单demo实现：

1. 安装
 `$ pip install locustio`
2. 编写一个Locust文件

locustioTest.py

	from locust import HttpLocust, TaskSet, task

	def index(l):
	    l.client.get("/")

	def stats(l):
	    l.client.get("/stats/requests")

	class Tasks(TaskSet):
	    # 列出需要测试的任务形式一
	    tasks = [index, stats]
	    # 列出需要测试的任务形式二 
	    @task
	    def page404(self):
	        self.client.get("/does_not_exist")
	    
	class WebsiteUser(HttpLocust):
	    host = "http://127.0.0.1:8089"
	    min_wait = 2000
	    max_wait = 5000
	    task_set = Tasks

3. 运行
 `$locust -f basic.py`

4. 打开浏览器，输入地址：http://127.0.0.1:8089,开启Locust Web操作页面

 - 第一个输入框：想并发的人数
 - 第二个输入框：虚拟用户初始化的比例
 - 比如上图中的意思就是想测试1000个虚拟用户对系统的压测，刚开始的时候是以10人/秒的速度开始递增到1000人。
 - 点击“”Start Swarming“”后你就可以开始压测你想压测的系统了。
5. 查看执行结果（上一步点击后页面会自动刷新到结果页面，但是需要手动停止）
 - 请求数
 - chart 展示
 - 目前只有每秒请求数，平均响应时间，用户的增长曲线 三个图

![image](https://upload-images.jianshu.io/upload_images/618241-760aae6563de9b55.png?imageMogr2/auto-orient/)

6. 说明

	1. Type：请求类型；
	2. Name：请求路径；
	3. requests：当前请求的数量；
	4. fails：当前请求失败的数量；
	5. Median：中间值，单位毫秒，一般服务器响应时间低于该值，而另一半高于该值；
	6. Average：所有请求的平均响应时间，毫秒；
	7. Min：请求的最小的服务器响应时间，毫秒；
	8. Max：请求的最大服务器响应时间，毫秒；
	9. Content Size：单个请求的大小，单位字节；
	10. reqs/sec：每秒钟请求的个数。

7. 启动参数说明：

	1. --no-web：表示不使用web界面运行测试；
	2. -c：设置虚拟用户总数；
	3. -r：设置每秒启动虚拟用户数；
	4. -n：设置请求总个数；