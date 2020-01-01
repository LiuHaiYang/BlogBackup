---
title: limit和offset
date: 2019-07-17 14:59:19
tags: 数据库
---

### Mysql limit offset 用法示例:

示例代码：

```
语句1 ： select * from users limit 10,4;  

> limit start, count 返回users 的第 11，12，13，14

语句2 : select * from users limit 4 offset 10

> limit count offset start 返回 users 的第 11，12，13，14
```

### 通过limit和offset 或只通过limit可以实现分页功能

```
 Per_page_count表示每页要显示的条数，
Page_num表示页码

那么 返回第Page_num页，每页条数为 Per_page_coun 的sql语句：

语句3：select * from studnet limit (Page_num-1)*Per_page_coun,Per_page_coun
语句4：select * from student limit Per_page_coun offset (Page_num-1)*Per_page_coun
```

### 区别

```
select * from table limit 2,1;

跳过2条取出1条数据，limit后面是从第2条开始读，读取1条信息，即读取第3条数据

select * from table limit 2 offset 1;
从第1条（不包括）数据开始取出2条数据，limit后面跟的是2条数据，offset后面是从第1条开始读取，即读取第2,3条
```

![image](https://images.pexels.com/photos/2623679/pexels-photo-2623679.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
