---
title: flask几个库
date: 2019-04-12 15:56:50
tags: python
---

### `Flask-sse`
---

#### SSE 

Server-Sent Events 让服务器向客户端流式发送文本消息, 如服务器生生成的实时通知或者更新.

SSE 提供的是一个高效, 跨浏览器的 XHR 流实现, 消息交付只使用一个长 HTTP 连接. 

浏览器会帮我们管理连接, 解析消息, 从而让我们只关注业务逻辑.

SSE 事件流是以流式 HTTP 响应形式交付的: 客户端发起常规 HTTP 请求, 服务器以自定义的 “text/event-stream” 内容类型响应, 然后交付 UTF-8 编码的事件数据.

- 事件载荷就是一个或多个相邻 data 字段的值
- 事件可以带 ID 和 event 表示事件类型
- 事件边界用换行符标识

SSE 连接本质上是 HTTP 流式响应, 因此响应时可以压缩的, 就跟压缩其他 HTTP 响应一样, 而且是动态压缩.

除了自动解析事件数据, SSE 还内置支持断线重连, 以及恢复客户端因断线而丢失的消息. 默认情况下, 如果连接中断, 浏览器会自动重新连接, SSE 规范建议间隔时间为 2~3 s, 这也是大多数浏览器采用的默认值. 

#### 特点 与局限
1. 特点
  - 通过一个长连接低延迟交付
  - 高效的浏览器消息解析, 不会出现无限缓冲
  - 自动跟踪最后看到的消息及自动重新连接.
  - 消息通知在客户端以 DOM 事件形式呈现.

2. 局限
  - 只能从服务器向客户端发送数据, 不能满足需要请求流的场景.
  - 事件流协议设计为只能传输 UTF-8 数据, 即使可以传输二进制数据, 效率也不高.
  - SSE 在服务端和客户端都比较容易实现, 但网络中间设备如 代理, 防火墙等不支持 SSE, 因此, 中间设备可能会缓冲事件流数据, 导致额外延迟, 甚至彻底毁掉 SSE 链接, 可以考虑通过 TLS 发送 SSE 事件流.

以上转自 [Blog](https://www.pyfdtic.com/2018/03/16/flaskExt--flask-sse/)

#### Flask-SSE

- `pip install flask_sse` 下载 ， `from flask_sse import sse`使用
- [官方实例](https://flask-sse.readthedocs.io/en/latest/quickstart.html)
- flask-sse 用法简单，使用前需安装Redis，因为sse使用了redis的发布订阅。
- `app.register_blueprint(sse, url_prefix='/stream')` 已经确定了 stream，前端需要他进行收消息。
- 实例中的路由hello 模拟了发消息，发消息可以配置不同的type。
- sse 也有一些高级配置，channel通道的处理。源码较简单，看下就知道了。

查看sse源码其实就是对redis的发布订阅的封装，也可以自己实现这个功能。同样的浏览器new 事件 `/stream`

		app = flask.Flask(__name__)
		app.secret_key = 'asdf'
		red = redis.StrictRedis()


		def event_stream():
		    pubsub = red.pubsub()
		    pubsub.subscribe('chat')
		    # TODO: handle client disconnection.
		    for message in pubsub.listen():
		        print message
		        yield 'data: %s\n\n' % message['data']

		@app.route('/stream')
		def stream():
		    return flask.Response(event_stream(),
		                          mimetype="text/event-stream")

至于前端怎么写的，我就不关心了！

### `Flask-CORS`
---

	由于固有的安全机制，js的跨域请求是无法被服务器成功响应的。
	现在前后端分离日益成为web开发主流方式的大趋势下，后台逐渐趋向指提供API服务，为各客户端提供数据及相关操作，
	而网站的开发全部交给前端搞定，网站和API服务很少部署在同一台服务器上并使用相同的端口，
	js的跨域请求时普遍存在的，开发RESTful API时，通常都要考虑到CORS功能的实现，以便js能正常使用API

开发Flask RESTAPI 时使用Flask-CORS可以解决跨域的问题。

安装 pip install flask-cors

用法：

		from flask import Flask
		from flask_cors import CORS

		app = Flask(__name__)
		CORS(app)

		@app.route("/")
		def helloWorld():
		  return "Hello, cross-origin-world!"

对指定的资源增加CORS： `cors = CORS(app, resources={r"/api/*": {"origins": "*"}})`
应用于Blueprint: `main = Blueprint('main', __name__)   CORS(main)`

高级用法见[官方文档](https://flask-cors.readthedocs.io/en/latest/api.html#extension)
