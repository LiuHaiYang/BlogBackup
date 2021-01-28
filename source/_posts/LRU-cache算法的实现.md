---
title: LRU-cache算法的实现
date: 2021-01-25 21:09:27
tags: python
---

	from functools import lru_cache
	@lru_cache(100)
	def fib(n):
	    if n < 2:
	        return 1
	    else:
	        return fib(n - 1) + fib(n - 2)

使用lru_cache的效率是最高的，直接递归的效率低的惊人，毕竟是指数级别的时间复杂度。

lru_cache比起成熟的缓存系统还有些不足之处，比如它不能设置缓存的时间，只能等到空间占满后再利用LRU算法淘汰出空间出来，并且不能自定义淘汰算法，但在简单的场景中很适合使


### 什么是LRU

LRU Cache是一个Cache置换算法，（least recently use）含义是“最近最少使用”，当Cache满（没有空闲的cache块）时，把满足“最近最少使用”的数据从Cache中置换出去，并且保证Cache中第一个数据是最近刚刚访问的。由“局部性原理”，这样的数据更有可能被接下来的程序访问。

### LRU基本算法

#### 要求

提供两个接口：一个获取数据int get(int key),一个写入数据void set(int key, int value)。

无论获取数据还是写入数据，这个数据要保持在最容易访问的位置。

缓存的数据大小有限，当缓存满时置换出最长时间没有被访问的数据，否则直接把数据加入缓存即可。

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

实现方案：使用stl的map和list
这里map使用unordered_map（hashmap）+list(双向链表），因此本质上使用hashmap和双向链表

	class LRUCache{
	    int m_capacity;
	    
	    //m_map_iter->first: key, m_map_iter->second: list iterator;
	    unordered_map<int,  list<pair<int, int>>::iterator> m_map; 
	    
	    //m_list_iter->first: key, m_list_iter->second: value;
	    list<pair<int, int>> m_list;                              
	public:
	    LRUCache(int capacity): m_capacity(capacity) {
	      
	    }
	    
	    int get(int key) {
	        auto found_iter = m_map.find(key);
	        // key 不存在
	        if (found_iter == m_map.end()) 
	            return -1;
	        
	        // key存在时，更新list的key所在节点的位置为list第一元素
	        m_list.splice(m_list.begin(), m_list, found_iter->second); 
	        
	        return found_iter->second->second; 
	    }
	    
	    void set(int key, int value) {
	        auto found_iter = m_map.find(key);
	        // key存在
	        if (found_iter != m_map.end()) 
	        {
	            // 移动key所在list中的位置
	            m_list.splice(m_list.begin(), m_list, found_iter->second); 
	            // 更新值
	            found_iter->second->second = value; 
	            return;
	        }
	        // 缓存已满且key不存在，删除list中最后一个节点，并从map中删除该key；
	        // 注意：这里就是为什么list存储的是key和value 键值对的原因，需要删除map中指定key
	        if (m_map.size() == m_capacity) 
	        {
	           int key_to_del = m_list.back().first; 
	           m_list.pop_back();           
	           m_map.erase(key_to_del);  
	        }
	        // 缓存不满，list把key和value加入到list的第一个位置，并存储到map中
	        m_list.emplace_front(key, value); 
	        m_map[key] = m_list.begin();      
	    }
	};

### 涉及的数据结构
std::map：主要用于实现快速访问（logn），存储key对应的list的位置信息，这里对应key对应list的迭代器
std::list：主要用于存储key和value值