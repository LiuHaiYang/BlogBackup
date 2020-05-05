---
title: java版本更新的新特性
date: 2020-03-24 23:19:20
tags: java
---

![image](https://images.pexels.com/photos/3746197/pexels-photo-3746197.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)
---

### jdk 1.8 新特性

Java 8 (又称为 jdk 1.8) 是 Java 语言开发的一个主要版本。 Oracle 公司于 2014 年 3 月 18 日发布 Java 8 ，它支持函数式编程，新的 JavaScript 引擎，新的日期 API，新的Stream API 等。

- 新特性

Java8 新增了非常多的特性，我们主要讨论以下几个：
 - Lambda 表达式 − Lambda 允许把函数作为一个方法的参数（函数作为参数传递到方法中）。
 - 方法引用 − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
 - 默认方法 − 默认方法就是一个在接口里面有了一个实现的方法。
 - 新工具 − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。
 - Stream API −新添加的Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中。
 - Date Time API − 加强对日期与时间的处理。
 - Optional 类 − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。
 - Nashorn, JavaScript 引擎 − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。

### jdk 1.9

Java 9 发布于 2017 年 9 月 22 日，带来了很多新特性，其中最主要的变化是已经实现的模块化系统。接下来我们会详细介绍 Java 9 的新特性。

- Java 9 新特性
 - 模块系统：模块是一个包的容器，Java 9 最大的变化之一是引入了模块系统（Jigsaw 项目）。
 - REPL (JShell)：交互式编程环境。
 - HTTP 2 客户端：HTTP/2标准是HTTP协议的最新版本，新的 HTTPClient API 支持 WebSocket 和 HTTP2 流以及服务器推送特性。
 - 改进的 Javadoc：Javadoc 现在支持在 API 文档中的进行搜索。另外，Javadoc 的输出现在符合兼容 HTML5 标准。
 - 多版本兼容 JAR 包：多版本兼容 JAR 功能能让你创建仅在特定版本的 Java 环境中运行库程序时选择使用的 class 版本。
 - 集合工厂方法：List，Set 和 Map 接口中，新的静态工厂方法可以创建这些集合的不可变实例。
 - 私有接口方法：在接口中使用private私有方法。我们可以使用 private 访问修饰符在接口中编写私有方法。
 - 进程 API: 改进的 API 来控制和管理操作系统进程。引进 java.lang.ProcessHandle 及其嵌套接口 Info 来让开发者逃离时常因为要获取一个本地进程的 PID 而不得不使用本地代码的窘境。
 - 改进的 Stream API：改进的 Stream API 添加了一些便利的方法，使流处理更容易，并使用收集器编写复杂的查询。
 - 改进 try-with-resources：如果你已经有一个资源是 final 或等效于 final 变量,您可以在 try-with-resources 语句中使用该变量，而无需在 try-with-resources 语句中声明一个新变量。
 - 改进的弃用注解 @Deprecated：注解 @Deprecated 可以标记 Java API 状态，可以表示被标记的 API 将会被移除，或者已经破坏。
 - 改进钻石操作符(Diamond Operator) ：匿名类可以使用钻石操作符(Diamond Operator)。
 - 改进 Optional 类：java.util.Optional 添加了很多新的有用方法，Optional 可以直接转为 stream。
 - 多分辨率图像 API：定义多分辨率图像API，开发者可以很容易的操作和展示不同分辨率的图像了。
 - 改进的 CompletableFuture API ： CompletableFuture 类的异步机制可以在 ProcessHandle.onExit 方法退出时执行操作。
 - 轻量级的 JSON API：内置了一个轻量级的JSON API
 - 响应式流（Reactive Streams) API: Java 9中引入了新的响应式流 API 来支持 Java 9 中的响应式编程。

### jdk 1.10

- Java 10 新特性
 - 局部变量的类型推断 var关键字
 - GC改进和内存管理 并行全垃圾回收器 G1
 - 垃圾回收器接口
 - 线程-局部变量管控
 - 合并 JDK 多个代码仓库到一个单独的储存库中
 - 新增API：ByteArrayOutputStream
 - 新增API：List、Map、Set
 - 新增API：java.util.Properties
 - 新增API： Collectors收集器

### jdk 1.11
- Java 11 新特性
 - 本地变量类型推断
 - 字符串加强
 - 集合加强
 - Stream 加强
 - Optional 加强
 - InputStream 加强
 - HTTP Client API
 - 化繁为简，一个命令编译运行源代码

 [Java11新特性](https://blog.csdn.net/itcast_cn/article/details/84991548)，java程序员必看哦！

### jdk 1.12

- Java 12 新特性
 - switch表达式
 	- 新形式的开关标签，类似lamada表达式“ case L ->”形式，表示如果标签匹配，则只执行标签右侧的代码，没有break
	- case后的标签支持多个逗号分隔的标签
	- 标签右侧代码支持表达式，代码块，直接抛异常
	- 更清晰，更安全的switch 表达式，局部变量返回值，语句返回值，个人觉得此处是参考了lamada表达式，可直接返回值然后赋值给接收的变量
	- break语句返回值，不过此处需注意，如果需要break语句返回值，则需每一个case后都有返回值或者抛出一个异常
 示例：
 ```
 	// java12以前
	switch (day) {
	    case MONDAY:
	    case FRIDAY:
	    case SUNDAY:
	        System.out.println(6);
	        break;
	    case TUESDAY:
	        System.out.println(7);
	        break;
	    case THURSDAY:
	    case SATURDAY:
	        System.out.println(8);
	        break;
	    case WEDNESDAY:
	        System.out.println(9);
	        break;
	}
	// java12 ’case L ->‘形式
	switch (day) {
	    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
	    case TUESDAY                -> System.out.println(7);
	    case THURSDAY, SATURDAY     -> System.out.println(8);
	    case WEDNESDAY              -> System.out.println(9);
	}

	// java12以前
	int numLetters;
	switch (day) {
	    case MONDAY:
	    case FRIDAY:
	    case SUNDAY:
	        numLetters = 6;
	        break;
	    case TUESDAY:
	        numLetters = 7;
	        break;
	    case THURSDAY:
	    case SATURDAY:
	        numLetters = 8;
	        break;
	    case WEDNESDAY:
	        numLetters = 9;
	        break;
	    default:
	        throw new IllegalStateException("Wat: " + day);
	}
	// java12 更清晰，更安全的switch表达式
	int numLetters = switch (day) {
	    case MONDAY, FRIDAY, SUNDAY -> 6;
	    case TUESDAY                -> 7;
	    case THURSDAY, SATURDAY     -> 8;
	    case WEDNESDAY              -> 9;
	};
	// java12 break可携带返回值
	int numLetters = switch (day) {
	    case MONDAY, FRIDAY, SUNDAY:
	      break 6;
	    case TUESDAY:
	      break  7;
	    case THURSDAY, SATURDAY:
	      break  8;
	    case WEDNESDAY:
	      break  9;
	    default:
	        System.out.println("default statement...");
	        break 0;
	};
	// java12 break value error
	int i = switch (day) {
	    case MONDAY -> {
	        System.out.println("Monday"); 
	        // ERROR! Block doesn't contain a break with value
	    }
	    default -> 1;
	};
	i = switch (day) {
	    case MONDAY, TUESDAY, WEDNESDAY: 
	        break 0;
	    default: 
	        System.out.println("Second half of the week");
	        // ERROR! Group doesn't contain a break with value
	};

 ```
 - 默认CDS归档 : 在64位平台上使用默认类列表增强JDK构建过程以生成类数据共享（CDS）归档。
 - Shenandoah低暂停时间垃圾收集器
	- 添加一个名为Shenandoah的新垃圾收集（GC）算法，通过与正在运行的Java线程同时进行疏散工作来减少GC暂停时间。使用Shenandoah的暂停时间与堆大小无关，这意味着无论堆是200 MB还是200 GB，您都将具有相同的一致暂停时间。
	- 作为一个实验性的功能，要启用/使用Shenandoah GC，需要以下JVM选项：-XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC。
 - JMH 基准测试 : 在JDK源代码中添加一套基本的微基准测试，使开发人员可以轻松运行现有的微基准测试并创建新的基准测试。
 - JVM常量API : 引入API来模拟关键类文件和运行时工件的名义描述，特别是可从常量池加载的常量。
 - 改进 AArch64 实现
	- Java 12 中将只保留一套 AArch64 实现，删除所有与 arm64 实现相关的代码，只保留 32 位 ARM 端口和 64 位 aarch64 的端口。删除此套实现将允许所有开发人员将目标集中在剩下的这个 64 位 ARM 实现上，消除维护两套端口所需的重复工作。
	- 当前 Java 11 中存在两套 64 位 AArch64 端口，它们主要存在于 src/hotspot/cpu/arm 和 open/src/hotspot/cpu/aarch64 目录中。这两套代码中都实现了 AArch64，Java 12 中将删除目录 open/src/hotspot/cpu/arm 中关于 64-bit 的这套实现，只保留其中有关 32-bit 的实现，余下目录的 open/src/hotspot/cpu/aarch64 代码部分就成了 AArch64 的默认实现。
 - 改善 G1 垃圾收集器，使其能够中止混合集合
 - 增强 G1 垃圾收集器，使其能自动返回未用堆内存给操作系统

 参考 [java 12 新特性](https://www.ibm.com/developerworks/cn/java/the-new-features-of-Java-12/index.html)











