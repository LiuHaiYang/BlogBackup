---
title: es比mysql快-转
date: 2020-06-27 18:24:18
tags: 数据库
---

转 [Elasticsearch原理学习--为什么Elasticsearch/Lucene检索可以比MySQL快?](https://www.csdn.net/gather_27/MtTaIgwsOTY2MC1ibG9n.html)

![image](https://images.pexels.com/photos/4450090/pexels-photo-4450090.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)

---

同样都可以对数据构建索引并通过索引查询数据，为什么Lucene 或基于 ElasticSearch会比关系型数据库，比如Mysql搜索性能更优？
两者有什么区别？
各自选型的依据是什么？
它们各自又有什么优势？

本文针对以上问题，基于个人理解及参与网上资料，给出说明。


---

本文将从以下各模块进行阐述：
- 什么是索引？
- Mysql 索引是如何实现的？
- Lucene 索引是如何实现的（包含压缩技术）

## 什么是索引
eg:现实世界中存在很多书籍,当我们想搜索某些内容 ,比如特定的主题，短语，甚至是单个单词,这会是一个灰常耗时的过程,逐页逐词的搜索，猴年马月才能搜索到我们想要的内容,这真的是一个杯具,但是对于很多书籍，有一个非常高效的工具：索引,创建索引需要大量的工作:必须按字母顺序列出单词，并引用它们的页面位置,但是好处是，当我们查找想要的内容时,我们只需要翻到索引所指向的页面,
现实世界中书籍是通过这种方式解决查找问题，那么计算机世界是如何解决查找问题的呢,计算机世界存在有大量的数据,数据库存储了大量文本,如果我们要查找某些内容，通常是非常缓慢的,计算机逐行扫描文本以获得我们要查找的内容,这种方式太耗时了,但是有一个工具可以让这个过程更加高效，没错就是：索引,
对数据库中的文本逐行进行分析并写入索引，这个过程称作indexing,索引按字母顺序组织，方便快速查找,对于写入到索引的每一项，都会附加其在数据库中的原始行，供后续查找使用,现在，当计算机搜索某内容时，它会跳过数据库，直接进入索引显示的匹配项.

让我们来看一个索引建立的过程演示:
![image](http://vlambda.com/img?url=https://mmbiz.qpic.cn/mmbiz_gif/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7k24gc6OXgvPwJkFYPsQN2wgep9e8DHajlEo7zcQs2vj7268ch1w5pw/0?wx_fmt=gif)
当我们搜索关键字“pie”时，搜索引擎快速查找匹配到的记录
![image](http://vlambda.com/img?url=http://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7KwrhZea38OHicxYU26iaRQTVmpJvpIWojUHJqu2Ric9v2lo9YuMrYdjOw/0?wx_fmt=png)

## 2. MySQL 索引是如何实现的
在Mysql 中 索引属于存储引擎级别的概念，不同存储引擎对索引的实现方式是不同的，本文主要讨论MyIsAM 和 InnoDB两个存储引擎的索引实现方式。

2.1 MyIsAM 索引实现

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7FKSSeiaXcJwlebTpWYtkRicvjcLq2CXDkibZbcC1kLUn0xjU28B2Eliazw/0?wx_fmt=png)

MyISAM 表的索引和数据是分离的，索引保存在 “表名.MYI”文件内，而数据保存在“表名.MYD” 文件内。
MyISAM 的索引方式也叫“非聚集”的，之所以这么称呼是为了和 InnoDB 的聚集索引区分。

2.2 InnoDB 索引实现

虽然InnoDB 也使用 B+Tree 作为索引结构，但是具体实现方式却与MyISAM截然不同的。

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7Aico5nOuGODJanEg7GhfL0UQwISRUl9cpDTa8uHOlBz6MUEa2BjEltA/0?wx_fmt=png)

上图是InnoDB 主索引（同时也是数据文件）的示意图，可以看到叶节点包含完整的数据记录。这种索引叫做聚集索引。
因为InnoDB 的数据文件本身要按主键聚集，所以InnoDB 要求表必须有主键（MyISAM可以没有），如果没有显示指定，则MySQL系统会自动选择一个
可以唯一标示数据记录的列作为主键，如果不存在这种列，则MySQL自动个为InnoDB 表生成一个隐含字段作为主键，这个字段长度为6 个字节，类型为长整型。

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7yTaeicZEY7tYfLaQRJcgl1llquMkibLlicasibiclzJDibqh6mGu0zmPql3w/0?wx_fmt=png)

这里以英文字符的ASCII码作为比较准则。聚集索引这种实现方式使得按主键的搜索十分高效，但是辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。

---

了解不同存储引擎的索引实现方式对于正确使用和优化索引都非常有帮助，例如知道了InnoDB的索引实现后，就很容易明白为什么不建议使用过长的字段作为主键，
因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用自增字段作为主键则是一个很好的选择。

传统关系型数据库大部分采用B-Tree/B+Tree这样的数据结构。
我们知道二叉树的查找效率是logN，同时写入更新新的节点不必移动全部节点，所以树型结构存储索引，能同时兼顾写入和查询性能。
当我们不需要支持快速的更新的时候，则可以用预先排序等方式换取更小的存储空间，更快的检索速度等好处，其代价就是更新慢。下面我们进一步深入了解Lucene索引是如何实现的。

## 3. Lucene索引是如何实现的

3.1 inverted Index
Lucene 实现快速搜索的核心就是倒排索引，那什么是倒排索引？

Term Index -->  term dictionery --> Posting List

这里有好几个概念。 我们老看一个实际的例子，假如如下的数据：

|docid| 年龄 | 性别|
| ---- | ---- | ---- |
| 1| 18 | 女|
|2 | 20 | 女|
|3 | 18 | 男|

这里每一行是一个 document ，每个 document 都有一个 docid ，那么给这些document 建立的倒排索引就是：

| 年龄 | 所在docid|
|----|----|
| 19 | [1,3]|
| 20 | [2]|

| 性别 | 所在docid|
|----|----|
| 女 | [1,2]|
| 男 | [3]|

可以看到，倒排索引是 针对每个字段的（per field），每个字段都有自己的倒排索引。 18，20 这些叫做term，而[1,3]就是Posting list。
posting list 就是一个 int 的数组，存储了所有符合某个term 的文档id，那么什么是term dictionary 和term index。

3.2 term dictionary

为了快速的找到某个term，将所有的term排个序，二分法查找term，logN的查找效率，就像通过查找字典一样，这就是term dictionary，现在再看起来，似乎和传统数据库的方式类似啊，为什么说查询更快呢？

3.2 Term index

假设我们有很多个term，比如：

Carla,Sara,Elin,Ada,Patty,Kate,Selena

如果按照这样的顺序排列，找出某个特定的term一定很慢，因为term没有排序，需要全部过滤一遍才能找出特定的term。排序之后就变成了：

Ada,Carla,Elin,Kate,Patty,Sara,Selena

这样我们可以用二分查找的方式，比全遍历更快地找出目标的term。这个就是 term dictionary。有了term dictionary之后，可以用 logN 次磁盘查找得到目标。但是磁盘的随机读操作仍然是非常昂贵的（一次random access大概需要10ms的时间）。所以尽量少的读磁盘，有必要把一些数据缓存到内存里。但是整个term dictionary本身又太大了，无法完整地放到内存里。于是就有了term index。term index有点像一本字典的大的章节表。

比如：

A开头的term ……………. Xxx页

C开头的term ……………. Xxx页

E开头的term ……………. Xxx页

如果所有的term都是英文字符的话，可能这个term index就真的是26个英文字符表构成的了。但是实际的情况是，term未必都是英文字符，term可以是任意的byte数组。而且26个英文字符也未必是每一个字符都有均等的term，比如x字符开头的term可能一个都没有，而s开头的term又特别多。实际的term index是一棵树：

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7Z0quWicLxvaKLqECCkvmxdibVYKnQm7YYME0CVHKqUOaXsN1qzstAUgQ/0?wx_fmt=png)

这棵树不会包含所有的term，它包含的是term的一些前缀。通过term index可以快速地定位到term dictionary的某个offset，然后从这个位置再往后顺序查找。

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7rs1wCcL6QkMKQY9FwjeBQKOx5X4icIQ7bY1HuXno8eiaHXkqrpcMlrWg/0?wx_fmt=png)

所以term index不需要存下所有的term，而仅仅是他们的一些前缀与Term Dictionary的block之间的映射关系，再结合FST(Finite State Transducers)的压缩技术，可以使term index缓存到内存中。

- 现在我们可以回答“为什么Elasticsearch/Lucene检索可以比MySQL快了。Mysql只有term dictionary这一层，是以树的方式存储在磁盘上的。检索一个term需要若干次的random access的磁盘操作。而Lucene在term dictionary的基础上添加了term index来加速检索，term index以树的形式缓存在内存中。从term index查到对应的term dictionary的block位置之后，再去磁盘上找term，大大减少了磁盘的random access次数。 -

额外值得一提的两点是：term index在内存中是以FST（finite state transducers）的形式保存的，其特点是非常节省内存。Term dictionary在磁盘上是以分block的方式保存的，一个block内部利用公共前缀压缩，比如都是Ab开头的单词就可以把Ab省去。这样term dictionary可以更节约磁盘空间。

3.4 FST

FSTs are finite-state machines thatmapaterm (byte sequence)to an arbitraryoutput.

假设我们现在要将mop, moth, pop, star, stop and top(term index里的term前缀)映射到序号：0，1，2，3，4，5(term dictionary的block位置)。
最简单的做法就是定义个Map，大家找到自己的位置对应入座就好了，但从内存占用少的角度想想，有没有更优的办法呢？答案就是：FST

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK73oMD5ibqBgibyYwovranoo7emynFhXvEYl2nGicjtQwhoStd5OLa6sAlw/0?wx_fmt=png)


⭕️表示一种状态

–>表示状态的变化过程，上面的字母/数字表示状态变化和权重

将单词分成单个字母通过⭕️和–>表示出来，0权重不显示。如果⭕️后面出现分支，就标记权重，最后整条路径上的权重加起来就是这个单词对应的序号。

FSTs are finite-state machines that map a term (byte sequence) to an arbitrary output.

FST以字节的方式存储所有的term，这种压缩方式可以有效的缩减存储空间，使得term index足以放进内存，但这种方式也会导致查找时需要更多的CPU资源。

3.5 压缩技术

除了上面说到用FST压缩term index外，对posting list也有压缩技巧。
posting list不是只存储文档id，还需要压缩？答案当然是需要。
我们来看一个例子，如果需要对各们童鞋们的性别进行索引，且有上千万个童鞋，而世界上只有男/女这样两个性别，则每个posting list都会有至少百万个文档id。这时我们就需要对posting list进行压缩，那如何有效的对这些文档id进行压缩呢？


3.5.1 Frame Of Reference

增量编码压缩，将大数变小数，按字节存储。
posting list是有序的，这样做的一个好处是方便压缩，看下面这个图例：

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7mvQUhmmkmvFggkXbibXhu3RbgWGpeQr0gKzRZ5Kf1CL9LImfAwic36fA/0?wx_fmt=png)

通过增量，将原来的大数变成小数仅存储增量值，再精打细算按bit排好队，最后通过字节存储，而不是尽管是2也用int(4个字节)来存储


3.5.2 Roaring bitmaps

说到Roaring bitmaps，就必须先从bitmap说起。Bitmap是一种数据结构，假设有某个posting list：

[1,3,4,7,10]

对应的bitmap就是：

[1,0,1,1,0,0,1,0,0,1]

非常直观，用0/1表示某个值是否存在，比如10这个值就对应第10位，对应的bit值是1，这样用一个字节就可以代表8个文档id，旧版本(5.0之前)的Lucene就是用这样的方式来压缩的，
但这样的压缩方式仍然不够高效，如果有1亿个文档，那么需要12.5MB的存储空间，这仅仅是对应一个索引字段(我们往往会有很多个索引字段)。于是有人想出了Roaring bitmaps这样更高效的数据结构。

将posting list按照65535为界限分块，比如第一块所包含的文档id范围在0~65535之间，
第二块的id范围是65536~131071，以此类推。
再用 <商，余数> 的组合表示每一组id，这样每组里的id范围都在0-65535内了，剩下的就好办了，
既然每组id不会变得无限大，那么我们就可以通过最有效的方式对这里的id存储。

![image](url=https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7fJia48HzQhksSgmhwBDrccz6dJH7sunJ7aKHxmZwVibX00f4bqkEr8Mg/0?wx_fmt=png)

为什么是以65535为界限?因为它=2^16-1，正好是用2个字节能表示的最大数，一个short的存储单位，注意到上图里的最后一行"If a block has more than 4096 values, encode as a bit set, and otherwise as a simple array using 2 bytes per value"，如果是大块，用节省点用bitset存，小块就豪爽点，2个字节我也不计较了，用一个short[]存着方便。

那为什么用4096来区分采用数组还是bitmap的阀值呢？

这个是从内存大小考虑的，当block块里元素超过4096后，用bitmap更剩空间： 
采用bitmap需要的空间是恒定的: 65536/8 = 8192bytes，而如果采用short[]，所需的空间是: 2xN(N为数组元素个数) ，如下图所示：

![image](https://mmbiz.qpic.cn/mmbiz_png/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7oqxlVWicJVGWWRLju3yzNSibSGJbqy41aIG8fqXdSE9DqZZCicnLbgWVw/0?wx_fmt=png)

3.6 联合索引查询

以上都是单field索引，如果多个field索引的联合查询，比如查询age=18 AND gender=女，倒排索引如何满足快速查询的要求呢？

大致过程如下：
- 根据过滤条件 age=18 的先从term index找到18在term dictionary的大概位置，
- 再从term dictionary里精确地找到18这个term，然后得到一个posting list或者一个指向posting list位置的指针。
- 再查询gender=女的过程也是类似的。
- 最后得出 age=18 AND gender=女，就是把两个 posting list 做一个“与”的合并。

这个理论上的“与”合并的操作可不容易。对于mysql来说，如果你给age和gender两个字段都建立了索引，查询的时候只会选择其中最selective的来用，
然后另外一个条件是在遍历行的过程中在内存中计算之后过滤掉。那么要如何才能联合使用两个索引呢？有两种办法：

a）使用skip list数据结构。同时遍历gender和age的posting list，互相skip；
b）使用bitset数据结构，对gender和age两个filter分别求出bitset，对两个bitset做AND操作。

利用 Skip List 合并
![image](https://mmbiz.qpic.cn/mmbiz_jpg/nRQHVMtWYqiaMs0B1C7aDGibmj6Gnn2WK7zGFLDXxQDGkIlzDfwGWBBlG7UMXIYICx0bByrf9BOOkKO7W9ERfVYA/0?wx_fmt=jpeg)

以上是三个posting list。
我们现在需要把它们用AND的关系合并，得出posting list的交集。首先选择最短的posting list，然后从小到大遍历。遍历的过程可以跳过一些元素，比如我们遍历到绿色的13的时候，就可以跳过蓝色的3了，因为3比13要小。

整个过程如下:

Next -> 2
Advance(2) -> 13
Advance(13) -> 13
Already on 13
Advance(13) -> 13 MATCH!!!
Next -> 17
Advance(17) -> 22
Advance(22) -> 98
Advance(98) -> 98
Advance(98) -> 98 MATCH!!!
最后得出的交集是[13,98]，所需的时间比完整遍历三个posting list要快得多。但是前提是每个list需要指出Advance这个操作，快速移动指向的位置。什么样的list可以这样Advance往前做蛙跳？skip list：

从概念上来说，对于一个很长的posting list，比如：

[1,3,13,101,105,108,255,256,257]

我们可以把这个list分成三个block：

[1,3,13] [101,105,108] [255,256,257]

然后可以构建出skip list的第二层：

[1,101,255]

1,101,255分别指向自己对应的block。这样就可以很快地跨block的移动指向位置了。

Lucene自然会对block再次进行压缩。其压缩方式是上文提到的Frame Of Reference。补充一点的是，利用skip list，除了跳过了遍历的成本，也跳过了解压缩这些压缩过的block的过程，从而节省了cpu。



利用bitset合并

如果使用bitset，就很直观了，直接按位与，得到的结果就是最后的交集。

--- 

## 总结
Lucene/Elasticsearch就是尽量将磁盘里的东西搬进内存，减少磁盘随机读取次数(同时也利用磁盘顺序读特性)，结合各种压缩算法，高效使用内存，从而达到快速搜索的特性。

---
关键字： 

mysql索引：
- MyISAM
- InnoDB 

es 索引详解

压缩的方法
- FST （term index）
- Frame Of Reference （posting list）
- Roaring bitmaps 

posting list 合并
- skip list数据结构互相skip
- bitset合并



















