---
title: Linux随记
tags: linux
abbrlink: 45051
date: 2017-06-22 11:02:48
---
## Linux

### 一 、安装软件的步骤：

* 检查是否安装 rpm -qa |grep jdk


* 下载软件包
* 安装依赖

rpm 包， 已经编译之后的应用程序。

rpm 命令：

### 1、安装  rpm文件 

* rpm   -i       -h  显示进度   -v   显示详细的过程    -vv   更详细的过程
* rpm  -ivh     --nodeps:忽略依赖关系   --replacepkgs : 重新安装，替换原有的安装   --force : 强行安装，实现重装或降级

### 2、查询

* rpm -q  package name: 查询指定的包是否安装
* rpm -qa : 查询已经安装的所有的包

### 二、随记

* init 0 关机
* printenv  查看环境变量
* 修改MAC地址  vi   /etc/sysconfig/network-scripts/.....      第二行就是mac  地址     删除 mac   和 uuid 两行     dd  命名删除 一行    删除  rm  -rf /etc/udev/rules.d/70-......     后重启    init  6 重启
* 修改 ip  vi   /etc/sysconfig/network-scripts/.....     修改IP后  service     network   restart
* 网络模式:  NAT   两层路由                  桥接   仅主机  :一层路由



