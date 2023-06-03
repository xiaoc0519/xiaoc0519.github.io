+++
title = "Threading"
date = 2023-05-30T15:35:56+08:00
menu = 'python'
+++

# **THREADING**
> 多线程 不利用计算机的多核优势， 适合多线程 IO密集型  文件读写   网络数据传输 
```python
list = [('name1','url'),
    ('name2','url'),
    ('name3','url')]

def task(name,url):
    res = requests.get(url)
    with open(name,'wb') as f:
        f.write(res.content)

for name,url in list:
    # 创建线程，让每个线程都去执行函数 (参数不同)
    t = threading.Thread(target=task,args=(name,url))
    t.start()
    # 创建新的线程 并把任务加入线程池进行任务(后台任务)， 不影响当前循环
```