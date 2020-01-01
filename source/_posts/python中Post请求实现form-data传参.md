---
title: python中Post请求实现form-data传参
tags: python
abbrlink: 35957
date: 2018-09-04 22:10:41
---

昨天在开发测试中因为使用post传参方法错误，不能正确获取返回结果，后来检查沟通后，应该使用form-data方法进行传参，回顾之前的开发，
一直都是偏后端，只是考虑如何接收参数，并不知道该如何去入参。使用Postman中的formdata传参成功返回数据。然后上网查了下资料，并测试成功。

python开发request库中Post请求实现 postman测试软件中的form-data 传参：

1. 简单引用包
  - 引入 `from requests_toolbelt  import MultipartEncoder` 包
  - `m = MultipartEncoder(datas)` `#datas`为初始参数
  - 添加 `headers` `,headers={'Content-Type': m.content_type}`
  - ` requests.post(url,data=m,headers=headers)`
2. 自己拼接，这种方法工作量过大，不建议使用，也易错。 