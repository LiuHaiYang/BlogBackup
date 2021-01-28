---
title: maven中optional和scope元素的使用-转
date: 2020-12-05 14:53:23
tags: maven
---

![image](https://images.pexels.com/photos/6042554/pexels-photo-6042554.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

---
在使用maven时对optional元素和scope元素不是很深入，导致发布的jar包或war包非常大、编译速度慢，而且还很容易生产jar冲突等问题


### optional元素

这里以Spring Boot项目中的使用为例，比如我们在项目中经常使用的热部署组件spring-boot-devtools，就可以使用optional元素来进行定义，对应pom文件中配置如下：

	<!--devtools 热部署-->
	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-devtools</artifactId>
	    <optional>true</optional>
	</dependency>

那么，这里的optional元素设置为true表示何意？optional是Maven依赖jar时的一个选项，表示该依赖是可选的，项目之间依赖不传递。不设置optional（默认）或者optional是false，表示传递依赖。

项目依赖时：父项目并未设置optional元素为true，那么便具有依赖传递性。此时，子项目中会直接引入父项目中引入的Junit的jar包。也就是说子项目打包时，jar/war包中会包含junit的jar包。

optional元素为true

当父项目引入junit依赖时，设置optional元素为true。那么，子项目便有了更多的选择。
如果子项目不需要Junit的jar包，那么在其pom文件中不需进行任何处理便可以。如果B项目也需要对应的jar包依赖，可以有两种选择：第一、父项目中对应依赖的optional设置为false或去掉；第二、子项目中直接引入需要的该依赖。

### parent继承的情况

在parent项目中配置统一的依赖版本控制，如下：

	<dependencyManagement>
	    <dependencies>
	        <dependency>
	            <groupId>junit</groupId>
	            <artifactId>junit</artifactId>
	            <version>4.12</version>
	            <optional>true</optional>
	        </dependency>
	    </dependencies>
	</dependencyManagement>

此时，如果子项目需要Junit的jar包，可以直接在项目中引入，这里父项目中的optional配置对子项目并无影响。

综上所述，在Maven项目中，恰当的使用optional配置，可以在很大程度上减少jar包的大小，提升编译和发布速度。


### scope元素

scope元素主要用来控制依赖的使用范围，指定当前包的依赖范围和依赖的传递性，也就是哪些依赖在哪些classpath中可用。常见的可选值有：compile, provided, runtime, test, system等。

- compile（编译）
默认值。compile表示对应依赖会参与当前项目的编译、测试、运行等，是一个比较强的依赖。打包时通常会包含该依赖，部署时会打包到lib目录下。比如：spring-core这些核心的jar包。

	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-web</artifactId>
	</dependency>

- test（测试）
scope为test表示依赖项目仅参与测试环节，在编译、运行、打包时不会使用。最常见的使用就是单元测试类了：

	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.12</version>
	    <scope>test</scope>
	</dependency>

类似单元测试这样的依赖，如果不设置scope为test，很显然它们会被打包、发布，但其实真是环境中并无什么作用。

- runntime（运行时）
runntime仅仅适用于运行和测试环节，在编译环境下不会被使用。比如编译时只需要JDBC API的jar，而只有运行时才需要JDBC驱动实现。

	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>8.0.20</version>
	    <scope>runtime</scope>
	</dependency>

- provided（已提供）
provided适合在编译和测试的环境，和compile功能相似，但provide仅在编译和测试阶段生效，provide不会被打包，也不具有传递性。

比如：上面讲到的spring-boot-devtools、servlet-api等，前者是因为不需要在生产中热部署，后者是因为容器已经提供，不需要重复引入。

	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <scope>provided</scope>
	</dependency>

- system
system范围依赖与provided类似，不过依赖项不会从maven仓库获取，而需要从本地文件系统提供。使用时，一定要配合systemPath属性。不推荐使用，尽量从Maven库中引用依赖。

	<dependency>
	  <groupId>sun.jdk</groupId>
	  <artifactId>tools</artifactId>
	  <version>1.5.0</version>
	  <scope>system</scope>
	  <systemPath>${java.home}/../lib/tools.jar</systemPath>
	</dependency>

### scope依赖的传递性
针对不同参数值，在多个项目中的依赖传递性如下：

![image](https://ask.qcloudimg.com/http-save/yehe-1161110/0tvex1pe38.jpeg?imageView2/2/w/1620)