+++
title = "Pandas"
date = 2023-05-29T17:14:59+08:00
menu = 'python'
+++



# **PANDAS**

```python
import pandas as pd
```

```python
mydataset = {
  'sites': ["Google", "Runoob", "Wiki"],
  'number': [1, 2, 3]
}
myvar = pd.DataFrame(mydataset)
```

## **SERIES**

>  Series 类似表格中的一个列（column），类似于一维数组，可以保存任何数据类型。  

Series 由索引（index）和列组成

```python
pandas.Series( data, index, dtype, name, copy)
# data：一组数据(ndarray 类型)。
# index：数据索引标签，如果不指定，默认从 0 开始。
# dtype：数据类型，默认会自己判断。
# name：设置名称。
# copy：拷贝数据，默认为 False。
a = [1, 2, 3]
mva = pd.Series(a)
# **********
# 索引  数据
#  0    1
#  1    2 
#  2    3
# dtype: int64 数据类型
# **********
mva[1] # 2

# 可以指定索引值
a = ["Google", "Edge", "Firefox"]
mva = pd.Series(a, index = ["x", "y", "z"])
# **********
#  x    Google
#  y    Edge 
#  z    Firefox
# dtype: object
# **********
mva['x'] # Google

# 可以使用 key/value 对象，类似字典来创建 Series
mva = {1: "Google", 2: "Edge", 3: "Firefox"}
mva = pd.Series(mva)
# **********
#  1    Google
#  2    Edge 
#  3    Firefox
# dtype: object
# **********

# 只需要字典中的一部分数据，只需要指定需要数据的索引
mva = {1: "Google", 2: "Edge", 3: "Firefox"}
mva = pd.Series(mva, index = [1, 2])
# **********
#  1    Google
#  2    Edge 
# dtype: object
# **********

# 设置 Series 名称参数
mva = pd.Series(mva, index = [1, 2],name='NameHere')
```

## **DATAFRAME**
> DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）  
DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）

```python
pandas.DataFrame( data, index, columns, dtype, copy)
# data：一组数据(ndarray、series, map, lists, dict 等类型)。
# index：索引值，或者可以称为行标签。
# columns：列标签，默认为 RangeIndex (0, 1, 2, …, n) 。
# dtype：数据类型。
# copy：拷贝数据，默认为 False。
data = [['Google',10],['Edge',12],['Firefox',13]]
df = pd.DataFrame(data,columns=['Site','Age'],dtype=float)
# **********
# 		site     age
#  0    Google  10.0
#  1    Edge    12.0
#  2    Firefox 13.0
# **********

data = {'Site':['Google', 'Edge', 'Firefox'], 'Age':[10, 12, 13]}
df = pd.DataFrame(data)
# **********
# 		site     age
#  0    Google  10.0
#  1    Edge    12.0
#  2    Firefox 13.0
# **********

data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data) # 没有对应的部分数据为 NaN。
# **********
# 	   a   b    c
#  0   1   2   NaN
#  1   5   10  20.0
# **********

# 可以使用 loc 属性返回指定行的数据，如果没有设置索引，第一行索引为 0，第二行索引为 1
data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}
df = pd.DataFrame(data)
df.loc[0]
df.loc[1] 
# **********
# calories    420
# duration     50
# Name: 0, dtype: int64
# calories    380
# duration     40
# Name: 1, dtype: int64
# **********

# 返回多行数据
df.loc[[0, 1]]
# **********
#    calories  duration
# 0       420        50
# 1       380        40
# **********

# 可以指定返回数据索引值
pd.DataFrame(data, index = ["day1", "day2", "day3"])
# **********
#       calories  duration
# day1       420        50
# day2       380        40
# day3       390        45
# **********

# 可以使用 loc 属性返回指定索引 对应到某一行
data = {
  "calories": [420, 380, 390],
  "duration": [50, 40, 45]
}
df = pd.DataFrame(data, index = ["day1", "day2", "day3"])
df.loc["day2"]
# **********
# calories    380
# duration     40
# Name: day2, dtype: int64
# **********

# 获取 dataframe 中其中几列
data = {
  "mango": [420, 380, 390],
  "apple": [50, 40, 45],
  "pear": [1, 2, 3],
  "banana": [23, 45,56]
}
df = pd.DataFrame(data)
df[["apple","banana"]]
```

## **CSV**

```python

```

## **JSON**

```python

```

## **数据清洗**

```python

```

## **FUNCTIONS**

```python

```