---
title: springcloudgateway
date: 2021-01-26 20:21:24
tags: 系统
---

[SpringCloud gateway](https://www.cnblogs.com/crazymakercircle/p/11704077.html)

### SpringCloud Gateway 

SpringCloud Gateway 是 Spring Cloud 的一个全新项目，该项目是基于 Spring 5.0，Spring Boot 2.0 和 Project Reactor 等技术开发的网关，它旨在为微服务架构提供一种简单有效的统一的 API 路由管理方式。

为了提升网关的性能，SpringCloud Gateway是基于WebFlux框架实现的，而WebFlux框架底层则使用了高性能的Reactor模式通信框架Netty。

Spring Cloud Gateway 的目标，不仅提供统一的路由方式，并且基于 Filter 链的方式提供了网关基本的功能，例如：安全，监控/指标，和限流。

### SpringCloud官方，特征介绍如下：
- 基于 Spring Framework 5，Project Reactor 和 Spring Boot 2.0
- 集成 Hystrix 断路器
- 集成 Spring Cloud DiscoveryClient
- Predicates 和 Filters 作用于特定路由，易于编写的 Predicates 和 Filters
- 具备一些网关的高级功能：动态路由、限流、路径重写

从以上的特征来说，和Zuul的特征差别不大。SpringCloud Gateway和Zuul主要的区别，还是在底层的通信框架上。


springcloud-gateway是基于netty运行的环境，在servlet容器环境或者把它构建为war包运行的话是不允许的，因此在项目当中没有必要添加spring-boot-starter-web。在gateway当中有三个重要的元素他们分别是:

- Route 是最核心的路由元素，它定义了ID，目标URI ，predicates的集合与filter的集合，如果Predicate聚合返回真，则匹配该路由
- Predicate 基于java8的函数接口Predicate，其输入参数类型ServerWebExchange，其作用就是允许开发人员根据当前的http请求进行规则的匹配，比如说http请求头，请求时间等，匹配的结果将决定执行哪种路由
- Filter为GatewayFilter,它是由特殊的工厂构建，通过Filter可以在下层请求路由前后改变http请求与响应

#### 工作流程

![image](https://cloud.spring.io/spring-cloud-gateway/reference/html/images/spring_cloud_gateway_diagram.png)

Spring Cloud Gateway收到客户端的请求之后，如果本次请求在Gateway Handler Mapping匹配到一个路由，请求将被发送到Gateway Web Handler之中，在这个Handler之中，会将请求发送到一个具体的过滤器链(filter chain)中。过滤器之间用虚线分开是因为过滤器可能会在发送代理请求之前（“pre”）或之后（“post”）执行业务逻辑。


#### 术语
- Filter（过滤器）：和Zuul的过滤器在概念上类似，可以使用它拦截和修改请求，并且对上游的响应，进行二次处理。过滤器为org.springframework.cloud.gateway.filter.GatewayFilter类的实例。
- Route（路由）：网关配置的基本组成模块，和Zuul的路由配置模块类似。一个Route模块由一个 ID，一个目标 URI，一组断言和一组过滤器定义。如果断言为真，则路由匹配，目标URI会被访问。
- Predicate（断言）：这是一个 Java 8 的 Predicate，可以使用它来匹配来自 HTTP 请求的任何内容，例如 headers 或参数。断言的输入类型是一个 ServerWebExchange。
- 地址(Uri)：匹配路由后转发的地址。

Gateway是基于Webflux实现的，它通过扩展HandlerMapping与WebHandler来处理用户的请求，先通过Predicate定位到Router然后在经过FilterChain的过滤处理，最后定位到下层服务。同时官方给我们提供了许多Prdicate与Filter,比如说限流的。从这点来说它的功能比zuul还强大呢，zuul里有的服务发现，断路保护等，Gateway分别通过GlobalFilter与Filter来实现。

### SpringCloud Gateway和架构

Spring在2017年下半年迎来了Webflux，Webflux的出现填补了Spring在响应式编程上的空白，Webflux的响应式编程不仅仅是编程风格的改变，而且对于一系列的著名框架，都提供了响应式访问的开发包，比如Netty、Redis等等。

SpringCloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架。
![image](https://upload-images.jianshu.io/upload_images/19816137-8758f092be21e6f7.gif?imageMogr2/auto-orient/strip)

###  SpringCloud Zuul的IO模型
Springcloud中所集成的Zuul版本，采用的是Tomcat容器，使用的是传统的Servlet IO处理模型。

大家知道，servlet由servlet container进行生命周期管理。container启动时构造servlet对象并调用servlet init()进行初始化；container关闭时调用servlet destory()销毁servlet；container运行时接受请求，并为每个请求分配一个线程（一般从线程池中获取空闲线程）然后调用service()。

弊端：servlet是一个简单的网络IO模型，当请求进入servlet container时，servlet container就会为其绑定一个线程，在并发不高的场景下这种模型是适用的，但是一旦并发上升，线程数量就会上涨，而线程资源代价是昂贵的（上线文切换，内存消耗大）严重影响请求的处理时间。在一些简单的业务场景下，不希望为每个request分配一个线程，只需要1个或几个线程就能应对极大并发的请求，这种业务场景下servlet模型没有优势。
在这里插入图片描述

![image](https://upload-images.jianshu.io/upload_images/19816137-bb466f6b0135bb71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以Springcloud Zuul 是基于servlet之上的一个阻塞式处理模型，即spring实现了处理所有request请求的一个servlet（DispatcherServlet），并由该servlet阻塞式处理处理。所以Springcloud Zuul无法摆脱servlet模型的弊端。虽然Zuul 2.0开始，使用了Netty，并且已经有了大规模Zuul 2.0集群部署的成熟案例，但是，Springcloud官方已经没有集成改版本的计划了。

### Webflux 服务器
Webflux模式替换了旧的Servlet线程模型。用少量的线程处理request和response io操作，这些线程称为Loop线程，而业务交给响应式编程框架处理，响应式编程是非常灵活的，用户可以将业务中阻塞的操作提交到响应式框架的work线程中执行，而不阻塞的操作依然可以在Loop线程中进行处理，大大提高了Loop线程的利用率。

Webflux虽然可以兼容多个底层的通信框架，但是一般情况下，底层使用的还是Netty，毕竟，Netty是目前业界认可的最高性能的通信框架。而Webflux的Loop线程，正好就是著名的Reactor 模式IO处理模型的Reactor线程，如果使用的是高性能的通信框架Netty，这就是Netty的EventLoop线程。


