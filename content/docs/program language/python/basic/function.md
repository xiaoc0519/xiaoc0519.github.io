+++
title = "Function"
date = 2023-05-23T15:35:56+08:00
menu = 'python'
+++

# **FUNCTION**

## Lambda

```python
lambda [arg1 [,arg2,..]]:expression
```

```python
# [arg…] 是参数列表，它的结构与 Python 中函数(function)的参数列表是一样的。
## [arg…] 可以有非常多的形式。
a, b  a=1, b=2  *args   **kwargs  a, b=1  *args  None  ...

# expression 是一个参数表达式
# 表达式中出现的参数需要在[arg......]中有定义，并且表达式只能是单行的，只能有一个表达式。
1  None  a + b  sum(a)  1 if a >10 else 0  ...

lambda x, y: x*y # 函数输入是x和y，输出是它们的积x*y
lambda:None # 函数没有输入参数，输出是None
lambda *args: sum(args) # 输入是任意个数参数，输出是它们的和(隐性要求输入参数必须能进行算术运算)
lambda **kwargs: 1 # 输入是任意键值对参数，输出是1
```

`lambda` 函数是匿名的, 有输入和输出, 拥有自己的命名空间

> 不能访问自己参数列表之外或全局命名空间里的参数，只能完成非常简单的功能

```python
# 将lambda函数赋值给一个变量，通过这个变量间接调用该lambda函数
add = lambda x, y: x+y
add(1, 2) # 3

# 将lambda函数赋值给其他函数，从而将其他函数用该lambda函数替换
time.sleep=lambda x: None
time.sleep(3) # None
```

### map()

```python
map(function, iterable, ...) # 返回 迭代器  
# 会根据提供的函数对指定序列做映射
function ----> 函数
iterable ----> 一个或多个序列
```

```python
def square(x):return x ** 2
map(square, [1,2,3,4,5]) # [1, 4, 9, 16, 25]

map(lambda x: x ** 2, [1, 2, 3, 4, 5]) # [1, 4, 9, 16, 25]

map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10]) # [3, 7, 11, 15, 19]
```

### reduce()

```python
reduce(function, iterable[, initializer])
# 函数会对参数序列中元素进行累积
function  ----> 函数有两个参数
iterable   ----> 可迭代对象
initializer ----> 可选

# 函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：
## 用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，
## 得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。
```

```python
def add(x, y):   return x + y
reduce(add, [1, 3, 5, 7, 9])    # 列表和   25
reduce(lambda x, y: x + y, [1, 2, 3, 4, 5])
```

### sorted()

```python
sorted(iterable[, cmp[, key[, reverse]]])  # 返回重新排序的列表
# 函数对所有可迭代的对象进行排序操作
iterable  ----> 可迭代对象
cmp       ----> 比较的函数, 这个具有两个参数, 参数的值都是从可迭代对象中取出, 此函数必须遵守的规则为，大于则返回1，小于则返回-1. 等于则返回0
key       ----> 主要是用来进行比较的元素, 只有一个参数, 具体的函数的参数就是取自于可迭代对象中, 指定可迭代对象中的一个元素来进行排序
reverse   ----> 排序规则, reverse = True 降序 , reverse = False 升序(默认)

```

```python
a = [5,7,6,3,4,1,2]
b = sorted(a)  #  [1, 2, 3, 4, 5, 6, 7]  使用sorted，保留原列表，不改变列表a的值

L=[('b',2),('a',1),('c',3),('d',4)]
sorted(L, cmp=lambda x,y:cmp(x[1],y[1]))   # 利用参数 cmp 排序  [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
sorted(L, key=lambda x:x[1]) # 利用参数 key 排序 [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```

### filter()

```python
filter(function, iterable)  # 返回 迭代器对象
# 过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表
function ----> 判断函数
iterable ----> 可迭代对象

# 接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判，然后返回 True 或 False，最后将返回 True 的元素放到新列表中
```

```python
def is_odd(n): return n % 2 == 1
newlist = filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) # [1, 3, 5, 7, 9]
newlist = filter(lambda x: x % 3 == 0, [1, 2, 3])
list(newlist) # 3
```

## 装饰器Decorator

### 闭包

```python
def outer(x):
    def inner(y):
        return x + y
    return inner
outer(6)(5) # 11
# outer(6)(5) 第一个参数传给自身函数 outer， 得到返回值 inner
# 在把  第二个参数传给 inner(y)
```

### 装饰器

> 拓展原来函数功能的函数，返回值也是一个函数

```python
def who(func):
    def run():
        print('who is running')
        func() # running()
    return run

@who
def running():
    print('out  func running')

func()
# who is running
# out  func running
```

> 把函数当作参数传入装饰器里面， 在装饰器里面特定位置进行调用函数

### 带参数装饰器

```python
def who(msg=None):
    def count(func)
        def run(*args,**kwargs):
            print('who is running')
            func(*args,**kwargs) # running()
            print('xiao')
        return run
    return count

 @who(msg='xiao')
 def where(func):
    print('where')
    func()
    print('you?')


def running():
    print('out  func running')

where(running)
# who is running
# where
# out  func running  
# you?
# xiao
```

### 类装饰器

```python
class deco:
    def __init__(self,func):
        self.func = func
        print("class init function")

    def __call__(self,*args,**kwargs):
        print('class call function')
        self.func(*args,**kwargs)
        print('class end')

@deco
def chocolate():
    print('eat chocolate')

@deco
def market():
    print('run and buy fruit')

chcoclate()
market()
# class init function
# class call function
# eat chocolate
# class end
```

### 带参数类装饰器

```python
class Decorator:
    def __init__(self, arg1, arg2):  # init()方法里面的参数都是装饰器的参数
        print('执行类Decorator的__init__()方法')
        self.arg1 = arg1
        self.ag2 = arg2
 
    def __call__(self, func):  # 因为装饰器带了参数，所以接收传入函数变量的位置是这里
        print('执行类Decorator的__call__()方法')
 
        def b_warp(*args):  # 这里装饰器的函数名字可以随便命名，只要跟return的函数名相同即可
            print('执行wrap()')
            print('装饰器参数：', self.arg1, self.arg2)
            print('执行' + func.__name__ + '()')
            func(*args)
            print(func.__name__ + '()执行完毕')
 
        return b_warp
 
 
@Decorator('Hello', 'Xiao')
def example(a1, a2, a3):
    print('传入example()的参数：', a1, a2, a3)
 
 
if __name__ == '__main__':
    print('准备调用example()')
    example('Baiyu', 'Happy', 'Coder')
```

### 装饰器顺序

```python
def Decorator_1(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        print('我是装饰器1')
 
    return wrapper
 
def Decorator_2(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        print('我是装饰器2')
 
    return wrapper
 
def Decorator_3(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        print('我是装饰器3')
 
    return wrapper
 
@BaiyuDecorator_1
@BaiyuDecorator_2
@BaiyuDecorator_3
def xiaoc():
    print("xiaoc")
 
 
if __name__ == '__main__':
    xiaoc()
# xiaoc
# 我是装饰器3
# 我是装饰器2
# 我是装饰器1
```
