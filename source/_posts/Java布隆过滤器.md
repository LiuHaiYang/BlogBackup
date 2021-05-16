---
title: Java布隆过滤器
date: 2021-02-01 11:14:33
tags: java
---

### 布隆过滤器

在程序的世界中，布隆过滤器是程序员的一把利器，利用它可以快速地解决项目中一些比较棘手的问题。如网页 URL 去重、垃圾邮件识别、大集合中重复元素的判断和缓存穿透等问题。

布隆过滤器（Bloom Filter）是 1970 年由布隆提出的。它实际上是一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都比一般的算法要好的多，缺点是有一定的误识别率和删除困难。


#### 原理

假设我们的个人网站同时在线人数达到1亿，要存储这一亿人的在线状态，先构建一个16亿比特位即两亿字节的向量，然后把这16亿个比特位都记为0。对于每一个登录用的userId，使用8个不同的算法产出8个不同信息指纹，在用一个算法把这8个信息隐身到这16亿个比特位的8个位置上，把这8个位置都设置成1，这样就构建成了一个记录一亿用户在线状态的布隆过滤器。

![image](https://upload-images.jianshu.io/upload_images/6328467-1747a90dca69cf01.png?imageMogr2/auto-orient/strip|imageView2/2/w/758/format/webp)


检索就是同样的原理，使用相同的算法对要检索的userId产生8个信息指纹，然后在看这八个信息指纹在这16亿比特位对应的值是否为1，都为1就说明这个userId在线，下面就用java代码来实现一个布隆过滤器。

Java 实现布隆过滤器


	package edu.se;

	import java.util.BitSet;

	/**
	 * @author ZhaoWeinan
	 * @date 2018/10/28
	 * @description
	 */
	public class BloomFileter {

	    //使用加法hash算法，所以定义了一个8个元素的质数数组
	    private static final int[] primes = new int[]{2, 3, 5, 7, 11, 13, 17, 19};
	    //用八个不同的质数，相当于构建8个不同算法
	    private Hash[] hashList = new Hash[primes.length];
	    //创建一个长度为10亿的比特位
	    private BitSet bits = new BitSet(256 << 22);

	    public BloomFileter() {
	        for (int i = 0; i < primes.length; i++) {
	            //使用8个质数，创建八种算法
	            hashList[i] = new Hash(primes[i]);
	        }
	    }

	    //添加元素
	    public void add(String value) {
	        for (Hash f : hashList) {
	            //算出8个信息指纹，对应到2的32次方个比特位上
	            bits.set(f.hash(value), true);
	        }
	    }

	    //判断是否在布隆过滤器中
	    public boolean contains(String value) {
	        if (value == null) {
	            return false;
	        }
	        boolean ret = true;
	        for (Hash f : hashList) {
	            //查看8个比特位上的值
	            ret = ret && bits.get(f.hash(value));
	        }
	        return ret;
	    }

	    //加法hash算法
	    public static class Hash {

	        private int prime;

	        public Hash(int prime) {
	            this.prime = prime;
	        }

	        public int hash(String key) {
	            int hash, i;
	            for (hash = key.length(), i = 0; i < key.length(); i++) {
	                hash += key.charAt(i);
	            }
	            return (hash % prime);
	        }
	    }

	    public static void main(String[] args) {

	        BloomFileter bloomFileter = new BloomFileter();
	        System.out.println(bloomFileter.contains("5324512515"));
	        bloomFileter.add("5324512515");

	        //维护1亿个在线用户
	        for (int i = 1 ; i < 100000000 ; i ++){
	            bloomFileter.add(String.valueOf(i));
	        }

	        long begin = System.currentTimeMillis();
	        System.out.println(begin);
	        System.out.println(bloomFileter.contains("5324512515"));
	        long end = System.currentTimeMillis();
	        System.out.println(end);
	        System.out.println("判断5324512515是否在线使用了:" + (begin - end));
	    }
	}

这段代码是构建了一个10亿位的bitSet，然后把一亿个userId加入到了我们的布隆过滤器中，最近判断5324512515这个userId是否登录，打出代码的执行时间。

	false
	1612149793467
	true
	1612149793467
	判断5324512515是否在线使用了:0

BloomFileter这个类的实例，就占用了100多MB内存。


看来布隆过滤器对于储存的效率确实很高

#### 布隆过滤器的误识别问题

布隆过滤器的好处在于快速、省空间，但是有一定的误识别率，这个概率很小，要计算出现误识别的概率并不难，下面贴一段书上的话
假定布隆过滤器有m比特，里面有n个元素，每个元素对应k个信息指纹的hash函数，在这个布隆过滤器插入一个元素，那么比特位被设置成1的概率为1/m，它依然为0的概率为1-1/m，那么k个哈希函数都没有把他设置成1的概率为1-1/m的k次方，一个比特在插入了n个元素后，被设置为1的概率为1减1-1/m的kn次方，最后书上给出了一个公式，在这里就不贴了，就贴一个表吧，是m/n比值不同，以及K分别为不同的值得情况下的假阳性概率：

![image](https://upload-images.jianshu.io/upload_images/6328467-1aed750360b60627.png?imageMogr2/auto-orient/strip|imageView2/2/w/887/format/webp)

![image](https://upload-images.jianshu.io/upload_images/6328467-ce0ed15e85001191.png?imageMogr2/auto-orient/strip|imageView2/2/w/547/format/webp)

可以得出一个结论，当我们搜索一个值的时候，若该值经过 K 个哈希函数运算后的任何一个索引位为 ”0“，那么该值肯定不在集合中。但如果所有哈希索引值均为 ”1“，则只能说该搜索的值可能存在集合中。


事实上这是误报的情形，产生的原因是由于哈希碰撞导致的巧合而将不同的元素存储在相同的比特位上。幸运的是，布隆过滤器有一个可预测的误判率（FPP）：
![image](https://user-gold-cdn.xitu.io/2019/11/30/16eba609871807ab?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
- n 是已经添加元素的数量；
- k 哈希的次数；
- m 布隆过滤器的长度（如比特数组的大小）；


[布隆过滤器你值得拥有的开发利器](https://segmentfault.com/a/1190000021136424)
