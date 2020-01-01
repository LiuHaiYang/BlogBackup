---
title: MySQL explain 了解
tags: 数据库
abbrlink: 51728
date: 2017-07-26 09:44:42
---
### explain显示了mysql如何使用索引来处理select语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。

先解析一条sql语句，看出现什么内容：

`EXPLAIN SELECT s.uid,s.username,s.name,f.email,f.mobile,f.phone,f.postalcode,f.address FROM uchome_space AS s,uchome_spacefield AS f WHERE 1 AND s.groupid=0 AND s.uid=f.uid`

![运行结果图](http://img.blog.csdn.net/20131107215222265?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1eGluZWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 1 . id

SELECT识别符。这是SELECT查询序列号。这个不重要,查询序号即为sql语句执行的顺序。

`EXPLAINSELECT *FROM (SELECT* FROMuchome_space LIMIT 10)AS s)`

它的执行结果为:

![执行结果](http://img.blog.csdn.net/20131107215627187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1eGluZWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这个时候看到 id 变化了。

### 2 . select_type

select类型，它有以下几种值

2.1 simple 它表示简单的select,没有union和子查询

2.2 primary 最外面的select,在有子查询的语句中，最外面的select查询就是primary,上图中就是这样

2.3 union union语句的第二个或者说是后面那一个.现执行一条语句，

`explain select  *  from uchome_space limit 10 union select * from uchome_space limit 10,10`

![运行结果](http://img.blog.csdn.net/20131107220609703?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1eGluZWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

第二条语句使用了union

2.4 dependent union    UNION中的第二个或后面的SELECT语句，取决于外面的查询

2.5 union result        UNION的结果,如上面所示

### 3 . table

输出的行所用的表，这个参数显而易见，容易理解

### 4 . type

连接类型，有多个参数

system : 表仅有一行，这是const类型的特列，平时不会出现，这个也可以忽略不计

const  :可以理解为const是最优化的

eq_ref  :mysql使用eq_ref联接来处理uchome_space表。

ref :如果联接不能基于关键字选择单个行的话，则使用ref。

ref_or_null :联接类型如同ref，添加了MySQL专门搜索包含NULL值的行。解决子查询中使用该联接类型的优化。

index_merge:

unique_subquery:

index_subquery:

range:给定范围内的检索，使用一个索引来检查行

index:

ALL:对于每个来自于先前的表的行组合，进行完整的表扫描。如果表是第一个没标记const的表，这通常不好，并且通常在它情况下**很**差。通常可以增加更多的索引而不要使用ALL，使得行能基于前面的表中的常数值或列值被检索出。

### **5 . possible_keys** 提示使用哪个索引会在该表中找到行，不太重要

### **6 .keys** MYSQL使用的索引，简单且重要

### 7 . key_len MYSQL使用的索引长度

### 8 .**ref   **ref列显示使用哪个列或常数与key一起从表中选择行。

### 9 .**rows** 显示MYSQL执行查询的行数，简单且重要，数值越大越不好，说明没有用好索引

### 10 .**Extra**  该列包含MySQL解决查询的详细信息。





## 总结：



1）指定了联接条件时，满足查询条件的记录行数少的表为[驱动表]；
2）未指定联接条件时，行数少的表为[驱动表]（Important!）。

**永远用小结果集驱动大结果集**

**今天学到了一个很重要的一点：当不确定是用哪种类型的join时，让mysql优化器自动去判断，我们只需写`select \* from t1,t2 where t1.field = t2.field`**













