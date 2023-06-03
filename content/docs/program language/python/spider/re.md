+++
title = "Re"
date = 2023-05-23T08:56:23+08:00
menu = 'python'
+++

# **RE**

```python
import re

result = re.match(pattern, string, re.I)
result.group() # ab  返回整体结果 
result.group(1) # b 返回第一个()匹配部分
```

## **Flags**

```python
re.I # 忽略大小写
re.L # 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
re.M # 多行模式
re.S #  即为 . 并且包括换行符在内的任意字符（. 不包括换行符）
re.U # 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
re.X # 为了增加可读性，忽略空格和 # 后面的注释
```

## **Pattern**

### 单字符

```shell
.    # 匹配任意1个字符（除了\n）    
[ ]    # 匹配[ ]中列举的字符    
\d    # 数字，即0-9 可以写在字符集[...]中
\D    # ⾮数字，即不是数字 可以写在字符集[...]中
\s    # 空⽩，即空格，tab键 可以写在字符集[...]中
\S    # ⾮空⽩字符 可以写在字符集[...]中
\w    # 单词字符，即a-z、A-Z、0-9、_ 可以写在字符集[...]中
\W    # ⾮单词字符 可以写在字符集[...]中
```

### 多字符

```shell
* # 前⼀个字符出现0次或者⽆限次，可有可⽆ abc*  abccc/ab
+ # 前⼀个字符出现1次或者⽆限次，即⾄少有1次 abc+  abccc
? # 前⼀个字符出现1次或者0次，即要么有1次，要么没有 abc?  ab,abc
{m} # 前⼀个字符出现m次 ab{2}c  abbc
{m,n} # 前⼀个字符出现m到n次，省略m，匹配0到n次，省略n，匹配m到无限次 ab{1,2}c  abc,abbc
```

### 开头结尾

```shell
^ # 开头
$ # 结尾
```

### 分组

```python
|    # 匹配左右任意⼀个表达式  "[1-9]?\d$|100","8"   >>> 8
(ab) # 将括号中字符作为⼀个分组 (163|126|qq) >>> 163/126/qq
\num # 引⽤分组num匹配到的字符串 r"<([a-zA-Z]*)>\w*</\1>", "<html>hh</html>"
'(?P<name>)' # 分组起别名，匹配到的子串组在外部是通过定义的 name 来获取的
'(?P=name)' # 引⽤别名为name分组匹配到的字符串
```

## **匹配模式**

### Match

```python
result = re.match('a(b)','abcdefg', flags=0)
result.group()
```
> 从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。匹配成功re.match方法返回一个匹配的对象。

### Findall

```python
result = re.findall(r"\d+", "py9999ii7890lk12345")
result[index]
```
> 在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。  
match 和 search 是匹配一次 findall 匹配所有。

### Search

```python
result = re.search(r"\d+", "asdaad93321")
result[index]
```
> re.search 扫描整个字符串并返回第一个成功的匹配，如果没有匹配，就返回一个 None。
re.match与re.search的区别：re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配

### Finditer

```python
result = re.finditer(r"\d+", "12a32bc43jf3")
 [i.group() for i in result]  // ['12','32','43','3']
```
> 在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

### Sub

```python
re.sub(pattern, repl, string, count=0, flags)
re.sub(r"\d+", '666, "eee320") # 666
```
> 将匹配到的数据进⾏替换。  
`repl` 要替换的字符串，也可为一个函数。  
`count`  替换的最大次数，非负整数。省略或为 0，所有的匹配都会被替换

### Subn

```python
s = 'a b, c d'
print(re.subn(r'(\w+) (\w+)', r'\2 \1', s)) # ('b a, d c', 2)
def func(m):
    return m.group(1).title() + ' ' + m.group(2).title()
print(re.subn(pattern, func, s)) # ('a b, c d', 2)
```
> 与sub()相同，但是返回一个元组 (字符串, 替换次数)

### Split

```python
re.split(pattern, string, maxsplit=0, flags=0)
result = re.split(r":| ","info:xiaoZhang 33 shandong") # 
```
> `maxsplit`	分隔次数，`maxsplit=1` 分隔一次，默认为 0，不限制次数

### 贪婪和⾮贪婪

```shell
'abbbbb'
ab*  # abbbbb
ab*? # a

s="This is a number 123-321-12-345"
r=re.match(".+(\d+-\d+-\d+-\d+)",s) 
r.group(1) # 3-321-12-345
#⾮贪婪操作符“？”，这个操作符可以⽤在"*","+","?"的后⾯，要求正则匹配的越少越好
r = re.match(".+?(\d+-\d+-\d+-\d+)",s)
r.group(1) # 123-321-12-345
```
> `maxsplit`	分隔次数，`maxsplit=1` 分隔一次，默认为 0，不限制次数  
正则使⽤通配字，那它在从左到右的顺序求值时，会尽量“抓取”满⾜匹配最⻓字符串  
`.+` 会从字符串的启始处抓取满⾜模式的最⻓字符，包括第⼀个整型字段的中的⼤部分  
`\d+` 只需⼀位字符就可以匹配，所以它匹配了数字“3”  
⽽ `.+` 则匹配了从字符串起始到这个第⼀位数字4之前的所有字符
