---
title: java中dao和service
date: 2019-09-19 14:39:25
tags: java
---

![image](https://images.pexels.com/photos/2017299/pexels-photo-2017299.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
#### DAO层
 
	dao层叫数据访问层，全称为 data access object 属于一种比较底层，比较基础的操作，具体到对于某个表，某个实体的层删改查。

#### service层

	service层叫服务层，被称为服务层，肯定是相比之下比较高层次的一层结构，相当于将几种操作封装起来。

#### 至于为什么service层要使用接口来定义有以下几点好处

- 在java中接口是多继承的，而类是单继承的，如果你需要一个类实现多个service，你用接口可以实现，用类定义service就没那么灵活。
- 要提供不同的数据库的服务时，我们只需要面对接口用不同的类实现即可，而不用重复的定义类
- 编程规范问题，接口化的编程是为的就是将实现封装起来，然调用者只关心接口，不关心实现，也就是"高内聚，低耦合"的思想。

![image](https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=c0d4c31a4fa7d933bffdec759d7bfd2b/d009b3de9c82d15803d8a0bf8d0a19d8bc3e4262.jpg)

#### 扩展资料

Java web 是用java技术来解决相关web互联网领域的技术总和。

web包括
- web服务器： servlet jsp
- web客户端： java applet

#### java技术 对web领域的发展注入了强大的动力。