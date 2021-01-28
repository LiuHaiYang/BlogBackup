---
title: springcloudgateway
date: 2021-01-26 20:21:24
tags: 系统
---

#### springcloud-gateway是基于netty运行的环境，在servlet容器环境或者把它构建为war包运行的话是不允许的，因此在项目当中没有必要添加spring-boot-starter-web。在gateway当中有三个重要的元素他们分别是:

- Route 是最核心的路由元素，它定义了ID，目标URI ，predicates的集合与filter的集合，如果Predicate聚合返回真，则匹配该路由
- Predicate 基于java8的函数接口Predicate，其输入参数类型ServerWebExchange，其作用就是允许开发人员根据当前的http请求进行规则的匹配，比如说http请求头，请求时间等，匹配的结果将决定执行哪种路由
- Filter为GatewayFilter,它是由特殊的工厂构建，通过Filter可以在下层请求路由前后改变http请求与响应

#### 工作流程

![image](https://cloud.spring.io/spring-cloud-gateway/reference/html/images/spring_cloud_gateway_diagram.png)

Spring Cloud Gateway收到客户端的请求之后，如果本次请求在Gateway Handler Mapping匹配到一个路由，请求将被发送到Gateway Web Handler之中，在这个Handler之中，会将请求发送到一个具体的过滤器链(filter chain)中。在上面的图中，Filter那部分被使用虚线分开，是因为这些filter可能在代理请求发送之前执行，这些过滤器称为“pre”类型，也会在代理请求发送之后执行，这些过滤器称为“post”类型。


#### 术语
- 路由(Route)：路由为一组断言与一组过滤器的集合，他是网关的一个基本组件。
- 断言(Predicate)：匹配路由的判断条件，例如Path=/demo，匹配后应用路由。
- 过滤器(Filter)：过滤器可以对请求和返回进行修改，比如增加头信息等。
- 地址(Uri)：匹配路由后转发的地址。



Gateway是基于Webflux实现的，它通过扩展HandlerMapping与WebHandler来处理用户的请求，先通过Predicate定位到Router然后在经过FilterChain的过滤处理，最后定位到下层服务。同时官方给我们提供了许多Prdicate与Filter,比如说限流的。从这点来说它的功能比zuul还强大呢，zuul里有的服务发现，断路保护等，Gateway分别通过GlobalFilter与Filter来实现。

