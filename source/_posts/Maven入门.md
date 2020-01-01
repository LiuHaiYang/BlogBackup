---
title: Maven入门
date: 2019-11-02 13:50:20
tags: waven
---

### 项目构建(maven可以干的事)
- 编码
- 编译
- 测试(Junit)
- 运行
- 打包
- 部署

### maven的好处
- 依赖的管理(就是对jar包的统一管理，可以节省空间)
- 一键构建

### 常用命令
- run 意见启动
- clean 清空编译

- compile  编译(编译主目录的文件)
- test 编译测试Test目录的文件
- package 打包
- install 把项目发布到本地仓库 (运行了 compile，test，package)有一定的顺序
- deploy 发布到私服

maven 的生命周期，每次执行之前的几个命令。
### 三种生命周期。

- clean生命周期
- default 生命周期（compile，test，package，install）
- site 生命周期 site命令 mvn site 出现site文件夹，生成静态页面，项目的说明，

#### 命令和生命周期的关系 不同的生命周期的命令可以同时执行
eg： mvn clean package    先执行 clean 在执行 package（也会执行complie和test）
 
 
### 依赖范围
- complie
- provided 编译时，测试时需要， 运行时不需要，打包时 不需要
- runtime   数据库驱动包 编译时不需要，测试 运行时 需要
- test   编译时不需要，测试时需要，运行时不需要
