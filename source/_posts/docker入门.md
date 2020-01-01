---
title: docker入门
date: 2019-02-21 20:11:52
tags: linux
---

![image](https://images.pexels.com/photos/1803027/pexels-photo-1803027.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
## docker 学习操作记录

#### helloworld
 - 启动容器：

	$ docker run IMAGE [CIMMAND] [ARG...]

 - 启动容器 并输出 helloworld

	$ docker run ubuntu echo 'hello world'

#### 镜像加速
  
	鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：http://hub-mirror.c.163.com。

 - 新版的 Docker 使用 /etc/docker/daemon.json（Linux） 或者 %programdata%\docker\config\daemon.json（Windows） 来配置 Daemon。

    	请在该配置文件中加入（没有该文件的话，请先建一个）：

		{
		  "registry-mirrors": ["http://hub-mirror.c.163.com"]
		}
		或者：https://blog.csdn.net/shenzhen_zsw/article/details/74277518
		进入官网 docker加速器
		http://guide.daocloud.io/dcs/docker-9153151.html
	 
#### 基础命令：
 - 使用bash 启动：

	`$ docker run -i -t ubuntu /bin/bash`

 - 查看 容器：

	`$ docker ps [-a]  [-l ` 不加参数时，返回的是正在运行的容器！

		-a 查看所有的容器
		-l 查看最new创建的容器

 - $ `docker inspect [容器的id] ` 会对容器进行详细的检查！

 - 自定义容器名：`$ docker run --name= 自定义名 -i -t IMAGE /bin/bash`

 - 重新启动已停止的容器：

	`$ docker start [-i] 容器名`
	-i 已交互的方式重启

 - `$ docker rm 容器名/id`  删除已经停止的容器

 - docker 守护式容器
	
		启动后可以 Ctrl+p  / Ctrl+Q 进行退出
		退出了交互式 容器 后台还在运行

 -` $ docker attach 容器名/id`  可以进入后台运行的容器

 - run 命令启动守护式容器：

		`$ docker run -d IMAGE `
		-d 后台启动
		`docker run --dc1 -d ubuntu /bin/bash -c "while true; do echo helloworld; sleep 1; done"`

- `$ docker logs` 返回日志 不指定 返回所有的日志
	
		-f 一直跟踪log 变化并返回
		-t 返回结果加输出时间
		--tail 结尾处数量的指定

 - `docker top` 查看容器内的进程
	
		在运行的容器中启动新的进程：docker exec 
		 -d  -i  -t

 - `docker exec -i -t dc1 /bin/bash`

 - 停止运行中的容器：
 
		 docker stop 容器/id
		 docker kill 容器名/id
 
#### 容器中部署web 网站
 1. 设置的容器的端口映射 
	
	`containerPort  只指定容器的端口`
	
    `run -P -p  `
	
    `docker run -p 80 -i -t ubuntu /bin/bash`
 
 2. 宿主机和容器的端口通知指定
	 
    `hostPort：containerport`
	 
    `docker run -p 8080:80 -i -t ubuntu /bin/bash`
 
 3.`ip::containerPort`

	`ocker run -p 0.0.0.0:80 -i -t ubuntu /bin/bash`
 
 4. `ip:hostPort:containerPort`
	
    	docker run -p 0.0.0.0:8080:80 -i -t ubuntu /bin/bash 
    	docker run -p 80 --name web -i -t ubuntu /bin/bash
    	apt-get install nginx
    	apt-get install vim
	 
 - 操作：
	
    	mkdir -p /var/www/html
    
    	cd /var/www/html
    	vim index.html

		<html>
		<head>nginx in docker</head>
		<body>
		hello, i'm website in docler
		</body>
		</html> 

	`whereis nginx`
	
	配置nginx 修改root文件目录
	到根目录 启动 nginx
	ps -ef 查看容器中的进程 
	Ctrl +q/Q  可后台运行退出！
 
		docker ps 可查看刚才容器 可以看到其端口映射
		docker port web 可查看端口映射
		docker top web 查看容器中进程运行的情况

		docker inspect web  查看该容器的ip地址
 
#### docker镜像 

	镜像：使用联合加载技术实现的层叠的只读文件系统。是容器构建的基石。
	存储位置： /var/lib/docker
	docker info 来查看docker的基本信息
 
#### 镜像的操作：
 
 查看：
 
		docekr images [OPTIONS] [repository]
		-a ,--all=false
		-f ,--filter=[]
		--no-trunc=false
		-q ,--quiet=false

eg:
	`docker image` 列出当前已经在docker已经安装的镜像。

- REPOSITORY 仓库  镜像的集合。一个仓库包含的是一系列关联的镜像
- REGISTRY 仓库 包含了很多 REPOSITORY 
- 不同的镜像是以标签的形式来区分的。REPOSITORY+TAG 就构成了一个完整的镜像名字。对应一个镜像的ID
	
	`eg： ubuntu：14.04  ubuntu：laste`

		docker images --no-trunc 查看完整的image id
		docker images -a 显示所有的镜像 很多没有仓库名 tag 即中间层镜像 docker images  -q 只返回 id 一列

 - docker images 仓库名  返回该名的所有镜像信息

 - 查看镜像的详细信息：

    	docker inspect [OPTIONS] CONTAINER|IMAGE [CONTAINER|IMAGE...]
    	支持镜像的查看，容器的查看。

- 删除镜像
 
		 docker rmi [OPTIONS] IMAGE [IMAGE]
		 -f ,--force=flase  强制删除
		 --no-prune=flase   保留被打标签的父镜像
	 
 eg:
 
		docker rmi ubuntu:14.04 可以跟多个镜像名和tag  or id
		docker rmi id （简短形式or完整形式 都可以）
  
	  当 镜像有多个相同的 镜像标签，删除时只是单纯的删除一个标签，实际上是没有删除镜像。
	  删除所有标签后 会删除此镜像。
	  当用 id 删除时，也会删除所有的标签，删除此镜像。
  
- 删除所有的镜像：
	
    	eg：docker rmi $(docker images -q  ubuntu)  删除仓库中所有的ubuntu镜像

#### 获取和推送镜像

 - 查找镜像:

		1. docker Hub  https://registry.hub.docker.com
		2. docekr search [OPTIONS] team
			--automated=false 显示自动化构建出的 镜像
			--no-trunc=flase  不以截断的形式显示
			-s 星级的筛选

`eg: docker search ubuntu`

- 拉取镜像：

		doker pull [OPTIONS] NAME [:TAG]

		-a, --all=flase 
		eg： docker pull ubuntu：14.04

#### 镜像配置：
	正常会特别的慢。可以使用 -registry-mirror 选项 可以使用国内的docker镜像服务器
	1.修改 /etc/default/docker
	2.添加： DOCKER_OPTS = "registry-mirror=https:mittor_addr"
	https://www.daocloud.io

#### 推送镜像：
	docker push 镜像名
	不会提交整个镜像，只会提交修改的部分。

#### 如何让构建docker的镜像：
	保存对容器的修改，并再次使用。
	自定义镜像的能力
	以软件的形式打包并分发服务及其运行环境

	docker commit 通过容器构建镜像
	docker commit [OPTIONS] container [PEROSITORY[:tag]]
	-a ， --author 镜像的作者
	-m ，--message="" 构建的信息
	-P --pause=true  不暂停该容器进行构建镜像

#### 实践：
	docker run -it -p 80 --name commit_test ubuntu /bin/bash
	apt-get update
	apt-get install nginx
	exit

#### 提交为镜像：
	docker commit -a 'harry' -m "nginx" commit_test （容器名）  dormancypress/commit_test1 (为镜像起一个名字)

	返回一个唯一id，就是新生成的镜像的id
	docker ps


#### docker build 通过Dockerfile 文件构建

1.创建Dockerfile 文件   包含一系列命令的文本文件
	eg：

	#First Dockerfile
	FROM ubuntu：14.04
	MAINUAINER dormancypress "xxxx@qq.com"
	RUN apt-get update
	RUN apt-get install -y nginx
	EXPORT 80

`mkdir -p dockerfile/df_test`

进入后把刚才的内容填写到文件,执行`docker build`


2.使用$docker build 命令

	docker build -t "镜像名字"   dockerfile 的地址


### Docker的cs模式：

客户端c：
1. docker 命令行客户端
2. Remote API  RESTFul 风格API  自己的程序可以与docker 集成 

cs的链接方式： socket 3种：
	
    1. unix:///var/run/docker.sock  默认的链接方式，可修改。
	2. tcp://host:port
	3. fd://socketfd

cs可在同一台机器或不在同一台，远程链接

- 实现socket的链接：
	`nc -U （指明是socket） /var/run/docker.sock ` (输入后就已经连接到socket)

 - 操作：
		GET /info (调用remote API info  则返回相关信息)HTTP/1.1   (协议)
返回数据是json格式、

#### docker 守护进程的配置和操作
	查看docker的状态
	sudo status docker

	使用service 命令操作：
	sudo service docker start
	sudo service docker stop
	sudo service doxker restart

#### docker de 启动及选项：很多
	docker -d [OPTIONS]
	-D, --debug=false 
	-e, --exec-driver="native"
	-g,--graph="var/lib/docker"
	--icc=true
	-l,--log-level="info" 日志级别 
	--label=[]
	-p,--pidfile="/var/lib/docker.pid"  写入id的地址
及：

		docker 服务器相关的选项
		存储相关 
		remoteAPI相关
		驱动相关
		Redistry 仓库链接相关的
		网络相关的 DNS 等

有灵活的设置选项
具体文档查看地址：
		`https://docs.docker.com/reference/commandline/cli`


启动配置文件： `/etc/default/docker`
可以设置docker的启动时的各种选项。

### docker 的远程访问

docker的客户端与守护进程的远程访问。 不在同一台机器。
保证 ClientAPI 和 SercerAPI 版本一致。

#### 客户端修改

修改原有服务器的启动配置文件。 `vim /etc/default/docker` 
两台机器 可添加 ` DOCKER_OPTS = "label name=docker_server_1/2"`

重启`docker` 
`docker info` 查看添加的信息，存在 则客户端信息已经修改好。

#### 服务器端修改

-H docker 守护进程启动选项

守护进程默认配置：

    -H unix:///var/run/docker.sock
    继续修改 DOCKER_OPTS  的值：
    DOCKER_OPTS  后面添加 -H tcp://0.0.0.0:2375

重启 并查看` ps -ef | grep dcoker`

可看到修改后的服务端配置信息,查看ip  

    到另一台机器测试连接 
    curl http://x.x.x.x:2374/info
    客户端查看：-H
    docker -H tcp://10.211.55.5:2375  info

客户端 提供了简化操作， 配置环境变量` DOCKER_HOST`
执行命令` export DOCKER_HOST="tcp://x.x.x.x:2375"`

直接运行 info 测试 返回的就是远程的 信息
若要连本地自己的，只需 ` export DOCKER_HOST =''` 置空 就行，为默认配置

-H 可以指定多个参数值

### Dockerfile

#### 第一个 Dockerfile

	FROM UBUNTU:14.04
	MAINTAINER dormancypress "xxxx@qq.com"
	RUN apt-get update
	RUN apt-get install -y nginx
	EXPOSE 80

#### 指令
- FROM  <image> or <image:tag>  
	- 已存在的镜像
	- 基础镜像
	- 必须是第一条非注释的指令
- MAINTAINER  作者名 及 email地址
- RUN  当前镜像中的命令
	- RUN <command>  (shell 模式)
	- RUN ['EXEC', 'PARAM1'] (EXEC 模式)
	- 分层概念
	- 会在当前镜像的上层创建新的镜像运行命令
	
- EXPOSE  指定运行该镜像的容器使用的端口，

- CMD
- ENTERYPOINT
- ADD
- COPY
- VOLUME
- WORKDIR
- ENV
- USER
- ONBUILD


端口制定后还需要在创建容器时手动的指定 端口

### Docker容器的网络基础

docker0 为容器网络提供网络连接
linux 中的虚拟网桥
OSI 七层模型中的网桥
网桥时七层模型中 数据链路层中的设备
通过mac 地址对网络进行划分，在不同的之间进行通信

linux虚拟网桥的特点：

- 可以设置IP地址
- 相当拥有一个隐藏的虚拟网桥

docker0 的地址划分：

    - ip： 172.17.42.1 子网掩码：255.255.0.0
    - MAC：02:42:ac:11:00:00  到 02:42:ac:11:ff:ff
    - 总共提供了 65534 个地址
    - 对每一个容器提供一个 mac ip


需要网桥管理工具，安装： `apt-get install bridge-utils`
运行： `sudo brctl show `  查看网桥设备

可自定义docker0

- 满足我们对容器网络的一些要求。
- `eg： sudo ifconfig docker0 192.168.200.1 network 255.255.255.0`

自定义虚拟网桥：

- 添加虚拟网桥

    	sudo brctl addbr br0
    	sudo ifconfig br0 192.168.100.1 network 255.255.255.0
	
- 更改 docker 守护进程的启动配置：
	`/etc/default/docker `中添加 `DOCKER_OPTS 值
	-b = br0`

### 容器的互联
- 默认：允许所有容器的互联

容器重启后ip就改变了，解决办法是
` --link`

`docker run --link=[CONTAINER_NAME] [:ALIAS] [IMAGE] [COMMAND]`

`EG: docker run -it --name cct3 --link=cct1:webtest  dormancypress/cct1`


- 拒绝容器间互联

        Docker守护进程的启动选项
        --icc = fasle
        vim /etc/default/docker
        DOCKER_OPTS=' --icc = false'
        重启docker 服务

- 允许特定容器的间的链接

        守护进程的启动选项
    	--icc =false -iptables=true
    		vim /etc/default/docker
    		DOCKER_OPTS=' --icc = false -iptables=true'
    		重启 docekr 服务
    		
    	--link  创建容器时 加了 link 选项才可以被链接。

### docker容器与外部网络的链接

- ip_forward

	--ip_forward =true 它决定系统是否转发流量
	查看： sudo sysctl.ipv4.conf.all.forwaring
	      net.ipv4.conf.all.forwarding = 1
- iptables 
- 允许端口映射访问
- 限制ip访问容器

### docker容器的数据管理

- docker容器的数据卷
- docker容器的卷容器
- docker数据卷的备份与还原

### 容器的数据卷
- 什么是数据卷
	- 数据卷是经过特殊设计的目录，可以绕过联合文件系统（UFS），为一个或多个容器提供访问。
	- 数据卷设计的目的，在于数据的永久化，它完全独立与容器的生存周期，因此，Docker不会在容器删除时删除其挂载的数据卷，也不会存在类似的垃圾收集机制对容器引用的数据卷进行处理。
- 数据卷的架构
	- docker的数据卷是独立于docker的存在，存在于docker的宿主机中，与docker容器的生存周期是分离的
	- docker数据卷是存在于宿主机的文件系统中
	- docker 数据卷可以是 目录也可以是文件
	- docker容器可以利用数据卷的技术与宿主机进行数据共享
	- 同一个数据文件可以支持多个容器的访问共享，实现了容器间的数据共享和交换

### 数据卷（Data Volume）的特点
- 数据卷在容器启动时初始化，如果容器使用的镜像在挂载点包含了数据，这些数据会拷贝到新初始化的数据卷中。
- 数据卷可以在容器之间共享和重用
- 可以对数据卷里的内容直接进行修改
- 数据卷的变化不会影响镜像的更新
- 数据卷会一直存在，即使挂载数据卷的容器已经被删除

### 数据卷的使用
- 为容器添加数据卷 `sudo docker run -v ~/container_data:/data -it ubuntu /bin/bash`
- 为数据卷添加访问权限 `sudo docker run -v ~/datavalume:/data:ro -it ubuntu /bin/bash`  在目录后面加权限
- 使用dockerfile 构建包含数据卷的镜像  Dockerfile指令： VOLUME["/data"]

### 数据卷容器
- 什么是数据卷容器：
	- 命名的容器挂载数据卷，其他容器通过挂载这个容器实现数据共享，挂载数据卷的容器就叫做数据容器卷
- 挂载数据卷容器的方法 `docker 润--volumes-from [CONTAINER NAME]`
- 方便了在不同的容器间共享数据，不需要容器确切的连接到宿主机的准确目录。
- 若数据卷还在被容器使用，他就会一直存在。

### docker数据卷的备份和还原
- 数据备份方法 
`docker run --vilumes-from [container name] -v $(pwd):/backup ubuntu  tar cvf /backup/backup.tar [container data volume]`
- 数据还原方法
`docker run --volumes-from [container name] -v $(pwd):/backup ubuntu tar xvf /backup/backup.tar [container data volume]`

### 容器跨主机访问方法
- 使用网桥实现跨主机容器链接
	- 实现容器网段和主机网段一直，可以实现跨主机链接
	- 配置较简单，不需要依赖第三方软件可实现
	- 需要小心的规划容器哦，主机划分IP地址
	- 需要有网段控制权，在生产环境中不易实现
	- 不容易管理
	- 兼容性不佳
- 使用Open vSwitch 实现跨主机容器链接
	- 高质量，多层虚拟交换机，使用开源Apache2.0 许可协议，由Nicira Networks 开发
	- 实现大规模网络自动化可以通过编程扩展，同时仍然支持标准的管理接口和协议。
	- GRE隧道： 通用路由协议封装
		- 隧道技术（Tunneling） 点对点 在封装 是一种通过使用互联网络的基础设施在网络之间传递数据的方式。使用隧道传递的数据（或负载）可以是不同协议的数据侦或包。隧道协议将其他协议的数据侦或包重新封装然后通过隧道发送。新的针头提供路由信息，以便通过互联网传递封装的负载数据。
- 使用 weave 实现跨主机容器链接
	- weave 简介： 编织，建立一个虚拟的网络，用于将运行在不同主机的docker容器链接起来


### 删除 docker image

- docker中删除images的命令是docker rmi，但有时候执行此命令并不能删除images
- ocker的帮助会发现有两个与删除有关的命令rm和rmi

images和container。其中images很好理解，跟平常使用的虚拟机的镜像一个意思，相当于一个模版，而container则是images运行时的的状态。docker对于运行过的image都保留一个状态（container），可以使用命令docker ps来查看正在运行的container，对于已经退出的container，则可以使用docker ps -a来查看。 如果你退出了一个container而忘记保存其中的数据，你可以使用docker ps -a来找到对应的运行过的container使用docker commit命令将其保存为image然后运行。

由于image被某个container引用（拿来运行），如果不将这个引用的container销毁（删除），那image肯定是不能被删除。

要删除运行过的images必须首先删除它的container。 `dokcer ps -a`

看出 image id 的image被 CONTAINER ID 的container使用着，所以必须首先删除该container

docker rm CONTAINER ID

若错误，可能是在执行，则需要stop ； docker stop CONTAINER ID

docker images

