+++
title = "Basic"
date = 2023-05-23T20:45:45+08:00
menu = 'python'
+++

# **BASIC**

## **REQUIREMENTS.txt**

```shell
pip freeze > requirements.txt
pip install -r requirements.txt
```

## **STRING**

```python
s = 'abcde'
```

### **切片**

```python
s[start:stop]
# 裁剪 字符串 起止位置， 不包括止
# 负数从后往前数
```

```python
s[1,3] # bc
s[::-1] #edcba  反转字符串
```

### **Method**

```python
s.strip() # 删除开头和结尾的空白字符
s.lstrip() # 删除开头空白字符
s.rstrip() # 删除结尾空白字符

s.split(",") # 找到分隔符的实例时将字符串拆分为子字符串
s.replace(repl,s [,count]) # 替换原字符串的特定字符  count 替换次数 默认全部
'a'  in s # true
e not in s # true

l = ['a','b','c','d']
' '.join(l) # a b c d

# 组合多列表

for x,y in zip(list1,list2):
    print(x,y) 
```

## **List**

```python
l = [a, b, c, d, e]
l.extend(list) # 追加列表到列表中
l.pop([index]) # 弹出最后一个元素 可以指定index值弹出
l.remove('a') # 指定删除的内容
del l  # 删除列表
l[o] = value # 赋值
l.count('a') # 出现的次数
l.index('a) # 最小索引值
```

## **Dict**

```python
dict["key"] # 获取 key 的值
for x in thisdict.values():print(x) # 循环全部 value 的值
dict["newkey"] = "value"  #  创建新键值对
dict.pop("key") # 删除指定key的键值对
del dict["key"] # 删除指定key的键值对
dict.popitem() # 删除最后插入的项目
```
