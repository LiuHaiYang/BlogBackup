---
title: python协程
date: 2019-05-27 12:40:03
tags: python
---

![image](https://images.pexels.com/photos/2363367/pexels-photo-2363367.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500)
### 实例code:

```
import asyncio
import concurrent.futures
from concurrent.futures import ThreadPoolExecutor
import time

def cpu_bound(i,j):
    for o in range(10):
        asyncio.sleep(1)
        print('a----->>',o)
    print(i,j)
    return sum((i,j))

async def main():

    loop = asyncio.get_event_loop()
    executor = ThreadPoolExecutor(1)

    i,j = 0,1
    res = loop.run_in_executor(executor, cpu_bound, i, j)

    n = 0
    for l in range(10):
        print('m--->',l)
        time.sleep(1)
        n+=l
        if n >20:
            await_res = await res
            print(n,await_res)
            break

if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
    ## 先起一个线程
```

其中用到的是 `run_in_executor` 函数
