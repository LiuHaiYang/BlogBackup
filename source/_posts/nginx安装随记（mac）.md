---
title: nginx安装随记（mac）
date: 2019-01-11 17:19:47
tags: linux
---

### 1. brew

brew又叫Homebrew，是Mac中的一款软件包管理工具，通过brew可以很方便的在Mac中安装软件或者是卸载软件.
一般Mac电脑会默认安装有brew.


### 2. 常用指令如下

brew 搜索软件
brew search nginx

brew 安装软件
brew install nginx

brew 卸载软件
brew uninstall nginx

brew 升级
sudo brew update

查看安装信息(经常用到, 比如查看安装目录等)
sudo brew info nginx

查看已经安装的软件
brew list

安装完以后，可以在终端输出的信息里看到一些配置路径：
	
	/usr/local/etc/nginx/nginx.conf （配置文件路径）
	/usr/local/var/www （服务器默认路径）
	/usr/local/Cellar/nginx/1.6.2 （貌似是安装路径）

### 3. brew安装nginx

安装nginx
可以用brew很方便地安装nginx.
sudo brew install nginx
启动nginx服务
sudo brew services start nginx
利用http://localhost:8080进行访问,

### 4. 操作

查看nginx版本
nginx -v
关闭nginx服务
sudo brew services stop nginx
另外几个比较方便的指令

重新加载nginx
nginx -s reload
停止nginx
nginx -s stop

---

![image](https://images.pexels.com/photos/1769301/pexels-photo-1769301.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)