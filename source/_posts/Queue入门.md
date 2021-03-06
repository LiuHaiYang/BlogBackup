---
title: Queue入门
date: 2020-05-07 17:30:57
tags: 服务
---

![image](https://images.pexels.com/photos/3689772/pexels-photo-3689772.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

---

### Java 实例 - 队列（Queue）用法

队列是一种特殊的线性表，它只允许在表的前端进行删除操作，而在表的后端进行插入操作。

LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。

```
import java.util.LinkedList;
import java.util.Queue;
 
public class Main {
    public static void main(String[] args) {
        //add()和remove()方法在失败的时候会抛出异常(不推荐)
        Queue<String> queue = new LinkedList<String>();
        //添加元素
        queue.offer("a");
        queue.offer("b");
        queue.offer("c");
        queue.offer("d");
        queue.offer("e");
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("poll="+queue.poll()); //返回第一个元素，并在队列中删除
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("element="+queue.element()); //返回第一个元素 
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("peek="+queue.peek()); //返回第一个元素 
        for(String q : queue){
            System.out.println(q);
        }
    }
}
```

- queue.remove      移除并返回队列头部的元素         如果队列为空，则抛出一个NoSuchElementException异常
- queue.poll        移除并返回队列头部的元素         如果队列为空，则返回null
- queue.take        移除并返回队列头部的元素
- queue.add         增加一个元索                   如果队列已满，则抛出一个IIIegaISlabEepeplian异常
- queue.offer       添加一个元素并返回true          如果队列已满，则返回false
- queue.put         添加一个元素                   如果队列满，则阻塞
- queue.element     返回队列头部的元素              如果队列为空，则抛出一个NoSuchElementException异常
- queue.peek        返回队列头部的元素              如果队列为空，则返回null


#### offer，add 区别：

一些队列有大小限制，因此如果想在一个满的队列中加入一个新项，多出的项就会被拒绝。

这时新的 offer 方法就可以起作用了。它不是对调用 add() 方法抛出一个 unchecked 异常，而只是得到由 offer() 返回的 false。

#### poll，remove 区别：

remove() 和 poll() 方法都是从队列中删除第一个元素。remove() 的行为与 Collection 接口的版本相似， 但是新的 poll() 方法在用空集合调用时不是抛出异常，只是返回 null。因此新的方法更适合容易出现异常条件的情况。

#### peek，element区别：

element() 和 peek() 用于在队列的头部查询元素。与 remove() 方法类似，在队列为空时， element() 抛出一个异常，而 peek() 返回 null。

### python队列 [Queue](https://docs.python.org/zh-cn/3/library/queue.html)

Queue是python标准库中的线程安全的队列（FIFO）实现,提供了一个适用于多线程编程的先进先出的数据结构，即队列，用来在生产者和消费者线程之间的信息传递。

#### 基本FIFO队列 `class Queue.Queue(maxsize=0)`

FIFO即First in First Out,先进先出。Queue提供了一个基本的FIFO容器，使用方法很简单,maxsize是个整数，指明了队列中能存放的数据个数的上限。一旦达到上限，插入会导致阻塞，直到队列中的数据被消费掉。如果maxsize小于或者等于0，队列大小没有限制。

举个栗子：
```
import Queue

q = Queue.Queue()

for i in range(5):
    q.put(i)

while not q.empty():
    print q.get()
输出：

0
1
2
3
4
```

#### LIFO队列 `class Queue.LifoQueue(maxsize=0)`

LIFO即Last in First Out,后进先出。与栈的类似，使用也很简单,maxsize用法同上

再举个栗子：
```
import Queue

q = Queue.LifoQueue()

for i in range(5):
    q.put(i)

while not q.empty():
    print q.get()
输出：

4
3
2
1
0
```

#### 优先级队列 `class Queue.PriorityQueue(maxsize=0)`

构造一个优先队列。maxsize用法同上。
```
import Queue
import threading

class Job(object):
    def __init__(self, priority, description):
        self.priority = priority
        self.description = description
        print 'Job:',description
        return
    def __cmp__(self, other):
        return cmp(self.priority, other.priority)

q = Queue.PriorityQueue()

q.put(Job(3, 'level 3 job'))
q.put(Job(10, 'level 10 job'))
q.put(Job(1, 'level 1 job'))

def process_job(q):
    while True:
        next_job = q.get()
        print 'for:', next_job.description
        q.task_done()

workers = [threading.Thread(target=process_job, args=(q,)),
        threading.Thread(target=process_job, args=(q,))
        ]

for w in workers:
    w.setDaemon(True)
    w.start()

q.join()
```

#### 一些常用方法

- task_done()
意味着之前入队的一个任务已经完成。由队列的消费者线程调用。每一个get()调用得到一个任务，接下来的task_done()调用告诉队列该任务已经处理完毕。

如果当前一个join()正在阻塞，它将在队列中的所有任务都处理完时恢复执行（即每一个由put()调用入队的任务都有一个对应的task_done()调用）。

- join()
阻塞调用线程，直到队列中的所有任务被处理掉。

只要有数据被加入队列，未完成的任务数就会增加。当消费者线程调用task_done()（意味着有消费者取得任务并完成任务），未完成的任务数就会减少。当未完成的任务数降到0，join()解除阻塞。

- put(item[, block[, timeout]])
将item放入队列中。

如果可选的参数block为True且timeout为空对象（默认的情况，阻塞调用，无超时）。
如果timeout是个正整数，阻塞调用进程最多timeout秒，如果一直无空空间可用，抛出Full异常（带超时的阻塞调用）。
如果block为False，如果有空闲空间可用将数据放入队列，否则立即抛出Full异常
其非阻塞版本为put_nowait等同于put(item, False)

- get([block[, timeout]])
从队列中移除并返回一个数据。block跟timeout参数同put方法

其非阻塞方法为｀get_nowait()｀相当与get(False)

- empty()
如果队列为空，返回True,反之返回False