+++
title = "Random"
date = 2023-05-23T11:48:35+08:00
menu = 'python'
+++

# **RANDOM**

```python
import random

# 0-1的随机浮点数 0<=n<1.0
random.random()

# 指定范围内的随机浮点数 if a<b,则生成的随机数n：a<=n<b；else 同理
random.uniform(a,b) 

# 指定范围内的整数 n：a<=n<=b。
random.randint(a,b) 

# 从序列中获取一个随机元素 list,tuple,字符串
random.choice(sequence) 

# 参数必须为整数
random.randrange([start],stop,[step]) 

# 将一个列表中的元素打乱，即将列表中的元素随机排列
random.shuffle(x,[random]) 

# 指定序列中随机获取指定长度k的片段。sample函数不会修改原有的序列。
random.sample(sequence,k) 
```


