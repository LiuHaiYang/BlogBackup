---
title: LRU-cache算法的实现
date: 2021-01-25 21:09:27
tags: python
---

	pthon demo

	from functools import lru_cache
	@lru_cache(100)
	def fib(n):
	    if n < 2:
	        return 1
	    else:
	        return fib(n - 1) + fib(n - 2)

	使用lru_cache的效率是最高的，直接递归的效率低的惊人，毕竟是指数级别的时间复杂度。

	lru_cache比起成熟的缓存系统还有些不足之处
	= 它不能设置缓存的时间，只能等到空间占满后再利用LRU算法淘汰出空间出来，
	= 不能自定义淘汰算法，但在简单的场景中很适合使


### 什么是LRU

LRU Cache是一个Cache置换算法，（least recently use）含义是“最近最少使用”，当Cache满（没有空闲的cache块）时，把满足“最近最少使用”的数据从Cache中置换出去，并且保证Cache中第一个数据是最近刚刚访问的。由“局部性原理”，这样的数据更有可能被接下来的程序访问。

### LRU基本算法

#### 要求

提供两个接口：一个获取数据int get(int key),一个写入数据void put(int key, int value)。get 和 put 方法必须都是 O(1) 的时间复杂度，

无论获取数据还是写入数据，这个数据要保持在最容易访问的位置。

缓存的数据大小有限，当缓存满时置换出最长时间没有被访问的数据，否则直接把数据加入缓存即可。

LRU 算法实际上是让你设计数据结构：首先要接收一个 capacity 参数作为缓存的最大容量，然后实现两个 API，一个是 put(key, val) 方法存入键值对，另一个是 get(key) 方法获取 key 对应的 val，如果 key 不存在则返回 -1。


#### DEMO 

	/* 缓存容量为 2 */
	LRUCache cache = new LRUCache(2);
	// 你可以把 cache 理解成一个队列
	// 假设左边是队头，右边是队尾
	// 最近使用的排在队头，久未使用的排在队尾
	// 圆括号表示键值对 (key, val)

	cache.put(1, 1);
	// cache = [(1, 1)]
	cache.put(2, 2);
	// cache = [(2, 2), (1, 1)]
	cache.get(1);       // 返回 1
	// cache = [(1, 1), (2, 2)]
	// 解释：因为最近访问了键 1，所以提前至队头
	// 返回键 1 对应的值 1
	cache.put(3, 3);
	// cache = [(3, 3), (1, 1)]
	// 解释：缓存容量已满，需要删除内容空出位置
	// 优先删除久未使用的数据，也就是队尾的数据
	// 然后把新的数据插入队头
	cache.get(2);       // 返回 -1 (未找到)
	// cache = [(3, 3), (1, 1)]
	// 解释：cache 中不存在键为 2 的数据
	cache.put(1, 4);    
	// cache = [(1, 4), (3, 3)]
	// 解释：键 1 已存在，把原始值 1 覆盖为 4
	// 不要忘了也要将键值对提前到队头


#### 算法

 - 使用list来保存缓存数据的访问序列，头指针指向刚刚访问过的数据。
 - 使用map保存数据的key和数据所在链表中的位置。map主要用于加速查找（log(n)的时间复杂度）。
 - 每次获取数据时，遍历map判断数据（使用数据的key）是否在缓存中，在则返回数据（存在则通过数据所在链表中的位置返回数据值）；不在则返回-1。
 - 每次写入新的数据时，可能有三种情况：
	- 数据已在缓存中
	- 遍历map判断数据是否已在缓存中，在则更新数据值，并置数据缓存在链表头
	- 数据不在缓存中，缓存已满
	- 剔除掉链表末尾的数据和map指定的数据key，并把新的数据加入到缓存链表的头，并更新map
	- 数据不在缓存中，缓存未满
	- 直接把新的数据加入到缓存链表的头，并更新map


### LRU实现

#### java 版本

	class LRUCache {
	    int capacity;
	    LinkedHashMap<Integer, Integer> cache;

	    public LRUCache(int capacity) {
	        this.capacity = capacity;
	        cache = new LinkedHashMap<Integer, Integer>(capacity, 0.75f, true) {
	            @Override
	            protected boolean removeEldestEntry(Map.Entry eldest) {
	                return cache.size() > capacity;
	            }
	        };
	    }

	    public int get(int key) {
	        return cache.getOrDefault(key, -1);
	    }

	    public void put(int key, int value) {
	        cache.put(key, value);
	    }
	}

#### PYTHON DEMO

	"""
	所谓LRU缓存，根本的难点在于记录最久被使用的键值对，这就设计到排序的问题，
	在python中，天生具备排序功能的字典就是OrderDict。
	注意到，记录最久未被使用的键值对的充要条件是将每一次put/get的键值对都定义为
	最近访问，那么最久未被使用的键值对自然就会排到最后。
	如果你深入python OrderDict的底层实现，就会知道它的本质是个双向链表+字典。
	它内置支持了
	1. move_to_end来重排链表顺序，它可以让我们将最近访问的键值对放到最后面
	2. popitem来弹出键值对，它既可以弹出最近的，也可以弹出最远的，弹出最远的就是我们要的操作。
	"""
	
	from collections import OrderedDict
	class LRUCache:
	  def __init__(self, capacity: int):
	    self.capacity = capacity  # cache的容量
	    self.visited = OrderedDict()  # python内置的OrderDict具备排序的功能
	    
	  def get(self, key: int) -> int:
	    if key not in self.visited:
	      return -1
	    self.visited.move_to_end(key)  # 最近访问的放到链表最后，维护好顺序
	    return self.visited[key]

	  def put(self, key: int, value: int) -> None:
	    if key not in self.visited and len(self.visited) == self.capacity:
	      # last=False时，按照FIFO顺序弹出键值对
	      # 因为我们将最近访问的放到最后，所以最远访问的就是最前的，也就是最first的，故要用FIFO顺序
	      self.visited.popitem(last=False)
	      self.visited[key]=value
	      self.visited.move_to_end(key)    # 最近访问的放到链表最后，维护好顺序



### 涉及的数据结构
std::map：主要用于实现快速访问（logn），存储key对应的list的位置信息，这里对应key对应list的迭代器
std::list：主要用于存储key和value值