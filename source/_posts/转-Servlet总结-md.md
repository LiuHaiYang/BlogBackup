---
title: 转_Servlet总结.md
date: 2019-07-01 15:59:32
tags: java
---

#### 总结一

- HttpServlet

> 想要实现一个servlet必须继承这个类，其实一个servlet就是一个java文件，但是这个类必须继承HttpService

	- 生命周期

> Servlet 的生命周期是从创建到毁灭的一个过程，具体的过程如下：
>>  Servlet 通过调用 init() 方法进行初始化
>>  Servlet 通过service() 方法来处理客户端的请求，但是在这一步还是要用到具体的实现的两个方法，分别是doPost() doGet()
>>  Servlet 通过调用destory() 方法终止(结束)
>>  最后 Servlet 是由JVM 的垃圾回收器进行垃圾回收的。

	- 常用的方法

>
>>  init() 初试化方法
>>  doGet(HttpServletRequest request,HttpServletResponse response) 处理get请求的方法
>>  doPost(HttpServletRequest request,HttpServletResponse response) 处理post请求的方法
>>  destroy() 最后销毁
>>  Enumeration<E> getInitParameterNames() 该方法从 servlet 的 ServletConfig 对象获取所有的参数名称

> ServletConfig getServletConfig() 返回一个ServletConfig对象，这个方法在后面讲到ServletConfig类的时候回详细的说到
> ServletContext getServletContext() 返回一个ServletContext对象，这个和ServletConfig类一样重要，在后面会详细讲解

- HttpServletRequest

> 这是servlet容器中用来处理请求的类，并且该对象作为一个参数传给doGet,doPost方法中

	- 常用的方法
```
getParameter(String name) 获取表单中的值，name是input中定义的name值，如果不存在返回null，否则返回的字符串 String[]
getParameterValues(String name) 获取表单中有多个name相同的值，例如多选列表，复选框
Enumeration getParameterNames() 返回所有请求中的参数，返回的是一个枚举对象，可以通过对应的方法进行列出所有的参数

Enumeration getHeaderNames() 获取所有请求头中的参数的名称，返回的是一个枚举对象
String getHeader(String name) 根据请求头中的名称获取对应名称的请求内容

String getContextPath() 获取应用程序的环境路径，就是上一级目录
String getMethod() 返回请求的方式 Get Post
String getQueryString() 返回请求行中的参数部分
StringBuffer getRequestURL() 返回完整的URL
String getRequestURI() 返回请求行中的资源名部分
getRemoteAddr方法返回发出请求的客户机的IP地址。
getRemoteHost方法返回发出请求的客户机的完整主机名。
getRemotePort方法返回客户机所使用的网络端口号。
getLocalAddr方法返回WEB服务器的IP地址。
getLocalName方法返回WEB服务器的主机名。
```

	- 请求转发与包含
> 请求转发相当于一个重定向，但是这个又和重定向不同的是：请求转发是在web容器中进行的，因此浏览器的地址栏并不会改变，但是重定向是要求浏览器重新请求另一个url，因此可以在地址栏清楚的看到地址的变化

> 请求转发使用的是HttpServletRequest中的getRequestDispatcher方法，下面将会详细介绍

- HttpServletResponse

> 这个类是用于对浏览器进行响应的

	- 常用的方法
```
PrintWriter getWriter() 返回一个PrintWriter对象，可以将字符串发送到客户端
addCookie(Cookie cookie) 将指定的cookie添加到响应中，这个是直接添加到set-cookie中，用于存储一些信息


Cookie cookie=new Cookie("age", "22");
cookie.setMaxAge(7*24*60*60); //设置cookie的失效时间(秒为单位）
response.addCookie(cookie);   //添加cookie

sendError(int src) 将指定的错误信息发送到客户端 比如401，302….
sendError(int sec,String message) 发送错误信息的同时，还发送提醒的信息message
sendRedirect(String url) 网页重定向，url是重定向的网址，但是也可以是相对的url
ServletOutputStream getOutputStream() 返回适用于在响应中编写二进制数据的 ServletOutputStream。
```

	- ServletConfig

> 在web.xml中对于每一个Servlet的设置web容器会为其生成一个ServletConfig作为代表对象，你可以从该对象中取得设置在web.xml中的Servlet初始参数

	- 常用方法
```
String getInitParameter(String name) 根据属性的名称获取指定的值

Enumeration getInitParameterNames() 获取该servlet中设置的所有的属性的名称（并不是设置的初始值）

ServletContext getServletContext() 获取ServletContext对象

```

![image](https://images.pexels.com/photos/1574142/pexels-photo-1574142.jpeg?auto=compress&cs=tinysrgb&dpr=1&fit=crop&h=300&w=500)



