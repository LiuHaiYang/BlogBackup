---
title: 转-Nginx
date: 2019-04-02 20:22:24
tags: linux
---

### 简述

Nginx 是一款面向性能设计的HTTP服务器，能反向代理HTTP，HTTPS 和邮件相关（SMTP，POP3，IMAP）的协议链接。
并且提供了负载均衡以及HTTP缓存。他的设计充分使用异步事件模型，削减上下文调度的开销，提高服务器并发能力。采用了模块化设计，提供丰富的第三方模块。

Nginx 标签：【异步】【事件】【模块化】【高性能】【高并发】【反向代理】【负载均衡】

#### linux安装： 下载安装包，进入目录编译安装 `./configure` 之前需安装相关依赖。`make  make install`

#### nginx测试

运行下面命令会出现两个结果，一般情况nginx会安装在/usr/local/nginx目录中

`cd /usr/local/nginx/sbin/`
`./nginx -t`

	# nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
	# nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful

设置全局Nginx命令，添加path后使生效。

#### Mac 安装：

Mac OSX 安装特别简单，首先你需要安装 Brew， 通过 brew 快速安装 nginx `brew install nginx`

注意默认端口不是 8080 查看确认端口是否被占用

### 开机自启动
#### 开机自启动方法一：

编辑 `vi /lib/systemd/system/nginx.service` 文件，没有创建一个 `touch nginx.service` 然后将如下内容根据具体情况进行修改后，添加到`nginx.service`文件中：

	[Unit]
	Description=nginx
	After=network.target remote-fs.target nss-lookup.target

	[Service]

	Type=forking
	PIDFile=/var/run/nginx.pid
	ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
	ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
	ExecReload=/bin/kill -s HUP $MAINPID
	ExecStop=/bin/kill -s QUIT $MAINPID
	PrivateTmp=true

	[Install]
	WantedBy=multi-user.target

- [Unit]:服务的说明
- Description:描述服务
- After:描述服务类别
- [Service]服务运行参数的设置
- Type=forking是后台运行的形式
- ExecStart为服务的具体运行命令
- ExecReload为重启命令
- ExecStop为停止命令
- PrivateTmp=True表示给服务分配独立的临时空间

注意：[Service]的启动、重启、停止命令全部要求使用绝对路径。

[Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3。

保存退出。

设置开机启动，使配置生效：

	# 启动nginx服务
	systemctl start nginx.service
	# 停止开机自启动
	systemctl disable nginx.service
	# 查看服务当前状态
	systemctl status nginx.service
	# 查看所有已启动的服务
	systemctl list-units --type=service
	# 重新启动服务
	systemctl restart nginx.service
	# 设置开机自启动
	systemctl enable nginx.service
	# 输出下面内容表示成功了
	Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.

	systemctl is-enabled servicename.service # 查询服务是否开机启动
	systemctl enable *.service # 开机运行服务
	systemctl disable *.service # 取消开机运行
	systemctl start *.service # 启动服务
	systemctl stop *.service # 停止服务
	systemctl restart *.service # 重启服务
	systemctl reload *.service # 重新加载服务配置文件
	systemctl status *.service # 查询服务运行状态
	systemctl --failed # 显示启动失败的服务
	注：*代表某个服务的名字，如http的服务名为httpd


#### 开机自启动方法二：

	vi /etc/rc.local

	# 在 rc.local 文件中，添加下面这条命令
	/usr/local/nginx/sbin/nginx start
	如果开机后发现自启动脚本没有执行，你要去确认一下rc.local这个文件的访问权限是否是可执行的，因为rc.local默认是不可执行的。修改rc.local访问权限，增加可执行权限：

	# /etc/rc.local是/etc/rc.d/rc.local的软连接，
	chmod +x /etc/rc.d/rc.local
	官方脚本 ed Hat NGINX Init Script `https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/`

### 服务管理

	# 启动
	/usr/local/nginx/sbin/nginx

	# 重启
	/usr/local/nginx/sbin/nginx -s reload

	# 关闭进程
	/usr/local/nginx/sbin/nginx -s stop

	# 平滑关闭nginx
	/usr/local/nginx/sbin/nginx -s quit

	# 查看nginx的安装状态，
	/usr/local/nginx/sbin/nginx -V 
	关闭防火墙，或者添加防火墙规则就可以测试了

	service iptables stop

#### 或者编辑配置文件：

	vi /etc/sysconfig/iptables
	添加这样一条开放80端口的规则后保存：

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
	重启服务即可:

	service iptables restart
	# 命令进行查看目前nat
	iptables -t nat -L

#### 重启服务防火墙报错解决

	service iptables restart
	# Redirecting to /bin/systemctl restart  iptables.service
	# Failed to restart iptables.service: Unit iptables.service failed to load: No such file or directory.
	在CentOS 7或RHEL 7或Fedora中防火墙由 firewalld 来管理，当然你可以还原传统的管理方式。或则使用新的命令进行管理。 假如采用传统请执行一下命令：

	# 传统命令
	systemctl stop firewalld
	systemctl mask firewalld
	# 安装命令
	yum install iptables-services

	systemctl enable iptables 
	service iptables restart

### nginx卸载

	如果通过yum安装，使用下面命令安装。

	yum remove nginx
	编译安装，删除/usr/local/nginx目录即可 如果配置了自启动脚本，也需要删除。


### 配置

在Centos 默认配置文件在 `/usr/local/nginx-1.5.1/conf/nginx.conf `我们要在这里配置一些文件。nginx.conf是主配置文件，由若干个部分组成，每个大括号{}表示一个部分。每一行指令都由分号结束;，标志着一行的结束。

### 配置文件

nginx 的配置系统由一个主配置文件和其他一些辅助的配置文件构成。这些配置文件均是纯文本文件，全部位于 nginx 安装目录下的 conf 目录下。

指令由 nginx 的各个模块提供，不同的模块会提供不同的指令来实现配置。 指令除了 Key-Value 的形式，还有作用域指令。

nginx.conf 中的配置信息，根据其逻辑上的意义，对它们进行了分类，也就是分成了多个作用域，或者称之为配置指令上下文。不同的作用域含有一个或者多个配置项。

### 反向代理

反向代理是一个Web服务器，它接受客户端的连接请求，然后将请求转发给上游服务器，并将从服务器得到的结果返回给连接的客户端。下面简单的反向代理的例子：

	server {  
	  listen       80;                                                        
	  server_name  localhost;                                              
	  client_max_body_size 1024M;  # 允许客户端请求的最大单文件字节数

	  location / {
	    proxy_pass                         http://localhost:8080;
	    proxy_set_header Host              $host:$server_port;
	    proxy_set_header X-Forwarded-For   $remote_addr; # HTTP的请求端真实的IP
	    proxy_set_header X-Forwarded-Proto $scheme;      # 为了正确地识别实际用户发出的协议是 http 还是 https
	  }
	}

复杂的配置: gitlab.com.conf。

	server {
	    #侦听的80端口
	    listen       80;
	    server_name  git.example.cn;
	    location / {
	        proxy_pass   http://localhost:3000;
	        #以下是一些反向代理的配置可删除
	        proxy_redirect             off;
	        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
	        proxy_set_header           Host $host;
	        client_max_body_size       10m; #允许客户端请求的最大单文件字节数
	        client_body_buffer_size    128k; #缓冲区代理缓冲用户端请求的最大字节数
	        proxy_connect_timeout      300; #nginx跟后端服务器连接超时时间(代理连接超时)
	        proxy_send_timeout         300; #后端服务器数据回传时间(代理发送超时)
	        proxy_read_timeout         300; #连接成功后，后端服务器响应时间(代理接收超时)
	        proxy_buffer_size          4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
	        proxy_buffers              4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
	        proxy_busy_buffers_size    64k; #高负荷下缓冲大小（proxy_buffers*2）
	    }
	}


### 负载均衡

upstream指令启用一个新的配置区段，在该区段定义一组上游服务器。这些服务器可能被设置不同的权重，也可能出于对服务器进行维护，标记为down。

	upstream gitlab {
	    ip_hash;
	    # upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
	    server 192.168.122.11:8081 ;
	    server 127.0.0.1:82 weight=3;
	    server 127.0.0.1:83 weight=3 down;
	    server 127.0.0.1:84 weight=3; max_fails=3  fail_timeout=20s;
	    server 127.0.0.1:85 weight=4;;
	    keepalive 32;
	}

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

upstream模块能够使用3种负载均衡算法：轮询、IP哈希、最少连接数。

- 轮询： 默认情况下使用轮询算法，不需要配置指令来激活它，它是基于在队列中谁是下一个的原理确保访问均匀地分布到每个上游服务器；
- IP哈希： 通过ip_hash指令来激活，Nginx通过IPv4地址的前3个字节或者整个IPv6地址作为哈希键来实现，同一个IP地址总是能被映射到同一个上游服务器；
- 最少连接数： 通过least_conn指令来激活，该算法通过选择一个活跃数最少的上游服务器进行连接。如果上游服务器处理能力不同，可以通过给server配置weight权重来说明，该算法将考虑到不同服务器的加权最少连接数。
- RR ： 简单配置 ，这里我配置了2台服务器，当然实际上是一台，只是端口不一样而已，而8081的服务器是不存在的，也就是说访问不到，但是我们访问 http://localhost 的时候，也不会有问题，会默认跳转到http://localhost:8080具体是因为Nginx会自动判断服务器的状态，如果服务器处于不能访问（服务器挂了），就不会跳转到这台服务器，所以也避免了一台服务器挂了影响使用的情况，由于Nginx默认是RR策略，所以我们不需要其他更多的设置

	upstream test {
	    server localhost:8080;
	    server localhost:8081;
	}
	server {
	    listen       81;
	    server_name  localhost;
	    client_max_body_size 1024M;
	 
	    location / {
	        proxy_pass http://test;
	        proxy_set_header Host $host:$server_port;
	    }
	}
负载均衡的核心代码为

	upstream test {
	    server localhost:8080;
	    server localhost:8081;
	}

#### 权重

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。 例如

	upstream test {
	    server localhost:8080 weight=9;
	    server localhost:8081 weight=1;
	}
	那么10次一般只会有1次会访问到8081，而有9次会访问到8080

#### ip_hash

上面的2种方式都有一个问题，那就是下一个请求来的时候请求可能分发到另外一个服务器，当我们的程序不是无状态的时候（采用了session保存数据），这时候就有一个很大的很问题了，比如把登录信息保存到了session中，那么跳转到另外一台服务器的时候就需要重新登录了，所以很多时候我们需要一个客户只访问一个服务器，那么就需要用iphash了，iphash的每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

#### fair

这是个第三方模块，按后端服务器的响应时间来分配请求，响应时间短的优先分配。

	upstream backend {
	    fair;
	    server localhost:8080;
	    server localhost:8081;
	}

#### url_hash

这是个第三方模块，按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。 在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法

	upstream backend {
	    hash $request_uri;
	    hash_method crc32;
	    server localhost:8080;
	    server localhost:8081;
	}

以上5种负载均衡各自适用不同情况下使用，所以可以根据实际情况选择使用哪种策略模式，不过fair和url_hash需要安装第三方模块才能使用

### server指令可选参数：

- weight：设置一个服务器的访问权重，数值越高，收到的请求也越多；
- fail_timeout：在这个指定的时间内服务器必须提供响应，如果在这个时间内没有收到响应，那么服务器将会被标记为down状态；
- max_fails：设置在fail_timeout时间之内尝试对一个服务器连接的最大次数，如果超过这个次数，那么服务器将会被标记为down;
- down：标记一个服务器不再接受任何请求；
- backup：一旦其他服务器宕机，那么有该标记的机器将会接收请求。
- keepalive指令：

Nginx服务器将会为每一个worker进行保持同上游服务器的连接。

### 屏蔽ip

在nginx的配置文件nginx.conf中加入如下配置，可以放到http, server, location, limit_except语句块，需要注意相对路径，本例当中nginx.conf，blocksip.conf在同一个目录中。

	include blockip.conf;
	在blockip.conf里面输入内容，如：

	deny 165.91.122.67;

	deny IP;   # 屏蔽单个ip访问
	allow IP;  # 允许单个ip访问
	deny all;  # 屏蔽所有ip访问
	allow all; # 允许所有ip访问
	deny 123.0.0.0/8   # 屏蔽整个段即从123.0.0.1到123.255.255.254访问的命令
	deny 124.45.0.0/16 # 屏蔽IP段即从123.45.0.1到123.45.255.254访问的命令
	deny 123.45.6.0/24 # 屏蔽IP段即从123.45.6.1到123.45.6.254访问的命令

	# 如果你想实现这样的应用，除了几个IP外，其他全部拒绝
	allow 1.1.1.1; 
	allow 1.1.1.2;
	deny all; 

### 第三方模块安装方法
`./configure --prefix=/你的安装目录  --add-module=/第三方模块目录`

### 重定向

- permanent 永久性重定向。请求日志中的状态码为301
- redirect 临时重定向。请求日志中的状态码为302
- 重定向整个网站

	server {
	    server_name old-site.com
	    return 301 $scheme://new-site.com$request_uri;
	}

- 重定向单页

	server {
	    location = /oldpage.html {
	        return 301 http://example.org/newpage.html;
	    }
	}

- 重定向整个子路径

	location /old-site {
	    rewrite ^/old-site/(.*) http://example.org/new-site/$1 permanent;
	}


### 性能

内容缓存
允许浏览器基本上永久地缓存静态内容。 Nginx将为您设置Expires和Cache-Control头信息。

	location /static {
	    root /data;
	    expires max;
	}

如果要求浏览器永远不会缓存响应（例如用于跟踪请求），请使用-1。

	location = /empty.gif {
	    empty_gif;
	    expires -1;
	}

### 打开文件缓存

	open_file_cache max=1000 inactive=20s;
	open_file_cache_valid 30s;
	open_file_cache_min_uses 2;
	open_file_cache_errors on;

### 监控

使用ngxtop实时解析nginx访问日志，并且将处理结果输出到终端，功能类似于系统命令top。所有示例都读取nginx配置文件的访问日志位置和格式。如果要指定访问日志文件和/或日志格式，请使用-f和-a选项。

注意：在nginx配置中/usr/local/nginx/conf/nginx.conf日志文件必须是绝对路径。

	# 安装 ngxtop
	pip install ngxtop

	# 实时状态
	ngxtop
	# 状态为404的前10个请求的路径：
	ngxtop top request_path --filter 'status == 404'

	# 发送总字节数最多的前10个请求
	ngxtop --order-by 'avg(bytes_sent) * count'

	# 排名前十位的IP，例如，谁攻击你最多
	ngxtop --group-by remote_addr

	# 打印具有4xx或5xx状态的请求，以及status和http referer
	ngxtop -i 'status >= 400' print request status http_referer

	# 由200个请求路径响应发送的平均正文字节以'foo'开始：
	ngxtop avg bytes_sent --filter 'status == 200 and request_path.startswith("foo")'

	# 使用“common”日志格式从远程机器分析apache访问日志
	ssh remote tail -f /var/log/apache2/access.log | ngxtop -f common

### 屏蔽.git等文件

	location ~ (.git|.gitattributes|.gitignore|.svn) {
	    deny all;
	}



