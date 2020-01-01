---
title: Tornado部署
date: 2019-04-16 10:05:02
tags: python
---

- [部署Tornado](#部署Tornado)
  - [Supervisor](#Supervisor)
     - [安装](#安装)
     - [配置](#配置)
     - [启动](#启动)
     - [supervisorctl](#supervisorctl)
  
 - [nginx](#nginx)

---

## 部署Tornado

为了充分利用多核CPU，并且为了减少同步代码中的阻塞影响，在部署Tornado的时候需要开启多个进程(最好为每个CPU核开启一个进程)

因为Tornado 自带的服务器性能很高，所以我们只需要多开启几个Tornado进程。为了对外有统一的接口，并且可以分发用户的请求到不同的Tornado进程上，我们使用Nginx来进行代理。

![image](https://img-blog.csdn.net/20180905111818647?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RpY2hpbWkzMzc1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### Supervisor

为了统一管理Tornado的多个进程，我们可以借助supervisor工具。

#### 安装

> pip install supervisor

#### 配置

运行echo_supervisord_conf命令输出默认的配置项，可以如下操作将默认配置保存到文件中

> echo_supervisord_conf > supervisord.conf

vim 打开编辑supervisord.conf文件，修改

>[include]
>files = relative/directory/*.ini

为

>[include]
>files = /etc/supervisor/*.conf

include选项指明包含的其他配置文件。

将编辑后的supervisord.conf文件复制到/etc/目录下

>sudo cp supervisord.conf /etc/

然后我们在/etc目录下新建子目录supervisor（与配置文件里的选项相同），并在/etc/supervisor/中新建tornado管理的配置文件tornado.conf。

```
[group:tornadoes]
programs=tornado-8000,tornado-8001,tornado-8002,tornado-8003
 
[program:tornado-8000]
command=/home/python/.virtualenvs/tornado_py2/bin/python /home/python/Documents/demo/chat    /server.py --port=8000
directory=/home/python/Documents/demo/chat
user=python
autorestart=true
redirect_stderr=true
stdout_logfile=/home/python/tornado.log
loglevel=info
 
[program:tornado-8001]
command=/home/python/.virtualenvs/tornado_py2/bin/python /home/python/Documents/demo/chat    /server.py --port=8001
directory=/home/python/Documents/demo/chat
user=python
autorestart=true
redirect_stderr=true
stdout_logfile=/home/python/tornado.log
loglevel=info
 
[program:tornado-8002]
command=/home/python/.virtualenvs/tornado_py2/bin/python /home/python/Documents/demo/chat    /server.py --port=8002
directory=/home/python/Documents/demo/chat
user=python
autorestart=true
redirect_stderr=true
stdout_logfile=/home/python/tornado.log
loglevel=info
 
[program:tornado-8003]
command=/home/python/.virtualenvs/tornado_py2/bin/python /home/python/Documents/demo/chat    /server.py --port=8003
directory=/home/python/Documents/demo/chat
user=python
autorestart=true
redirect_stderr=true
stdout_logfile=/home/python/tornado.log
loglevel=info
```

#### 启动

> supervisord -c /etc/supervisord.conf

查看supervisord 是否运行：

> ps aux | grep supervisord

#### supervisorctl

我们可以利用supervisorctl来管理supervisor。

>supervisorctl
>
>status  #查看程序状态
>stop  tornados:*   #关闭 tornado组 程序
>start  tornados:*   #开启 tornado组 程序
>restart  tornados:*   #重启 tornado组 程序
>update  # 重启配置文件修改过的程序

执行status命令时显示如下信息说明Tornado组程序运行正常：

>supervisor> status
>tornadoes:tornado-8000 RUNNING pid 32091, uptime 00:00:02
>tornadoes:tornado-8001 RUNNING pid 32092, uptime 00:00:02
>tornadoes:tornado-8002 RUNNING pid 32093, uptime 00:00:02
>tornadoes:tornado-8003 RUNNING pid 32094, uptime 00:00:02

### nginx

对于使用安装nginx，其配置文件位于/etc/nginx/中，修改default文件如下： 

```
upstream tornadoes {
    server 127.0.0.1:8000;
    server 127.0.0.1:8001;
    server 127.0.0.1:8002;
    server 127.0.0.1:8003;
}
 
upstream websocket {
    server 127.0.0.1:8000;
}
 
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    location /static/ {
        root /home/python/Documents/demo/chat;
        if ($query_string) {
            expires max;
        }
    }
 
    location /chat {
        proxy_pass http://websocket/chat;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
 
    location / {
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;  # 协议 http https
        proxy_pass http://tornadoes;
    }
}
```

操作Nginx

>service nginx start   # 启动
>service nginx stop   # 停止
>service nginx restart   # 重新启动

---

![image](https://images.pexels.com/photos/1433052/pexels-photo-1433052.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
