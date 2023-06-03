+++
title = "Multiprocessing"
date = 2023-05-30T15:35:56+08:00
menu = 'python'
+++

# **MULTIPROCESSING**
> 多进程   适合 利用计算机的多核优势 ，让cpu同时处理一些任务， 适合多进程开发(资源开销大)  
计算密集型   大量的数据计算

```python
import multiprocessing
```

```python
def task(start,end,queue):
    result =0
    for i in range(start,end):
        result += i
    queue.put(result)


if __name__ == '__main__':
    queue = multiprocessing.Queue()
    
    start_time = time.time()

	# 创建两个进程分别处理数据
    p1 = multiprocessing.Process(target=task,args=(0,50000,queue))
    p1.start()

    p2 = multiprocessing.Process(target=task,args=(50000,100000,queue))
    p2.start()

    v1 = queue.get(block=True)
    v2 = queue.get(block=True)
    print(v1+v2)

    end_time = time.time()

    print(f'cost time : {end_time - start_time}')
```
> win 下 多进程 需要写入 __main__ 语句下 为 spawn 模式