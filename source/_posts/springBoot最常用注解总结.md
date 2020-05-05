---
title: springBoot最常用注解总结
date: 2020-03-05 22:06:16
tags: springBoot
---

springBoot
===

1. @SpringBootApplication

这是springBoot 最核心的注解，用在SpringBoot 主类上，标识这是一个
springBoot 应用，用来开启Spring Boot的各项能力。
其实这个注解就是 @SpringBootConfiguration  @EnableAutoConfiguration
 @ComponentScan 这三个注解的组合，也可以用这三个注解来代替 @SpringBootApplication 注解。

2. @EnableAutoConfiguration

允许 Spring Boot 自动配置注解，开启这个注解之后，Spring Boot 就能根据当前类路径下的包或者类来配置 Spring Bean。

如：当前类路径下有 Mybatis 这个 JAR 包，MybatisAutoConfiguration 注解就能根据相关参数来配置 Mybatis 的各个 Spring Bean。

3. @Configuration

这是 Spring 3.0 添加的一个注解，用来代替 applicationContext.xml 配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。

4. @SpringBootConfiguration 

这个注解就是 @Configuration 注解的变体，只是用来修饰是 Spring Boot 配置而已，或者可利于 Spring Boot 后续的扩展。

5. @ComponentScan

这是 Spring 3.1 添加的一个注解，用来代替配置文件中的 component-scan 配置，开启>组件扫描，即自动扫描包路径下的 @Component 注解进行注册 bean 实例到 context 中。

6. @Conditional

这是 Spring 4.0 添加的新注解，用来标识一个 Spring Bean 或者 Configuration 配置文件，当满足指定的条件才开启配置。

7. @ConditionalOnBean

组合 @Conditional 注解，当容器中有指定的 Bean 才开启配置。

