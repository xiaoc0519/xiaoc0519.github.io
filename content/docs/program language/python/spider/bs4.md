+++
title = "Bs4"
date = 2023-05-23T15:01:01+08:00
menu = 'python'
+++

# **BeautifulSoup LXML**

```python
# pip install bs4
# pip install lxml
from bs4 import BeautifulSoup

req=requests.get(url,headers=headers)
html=req.text
soup = BeautifulSoup(html, 'lxml') # 创建beautifulsoup解析对象
soup.prettify() # 格式化输出 html / xml 文档
```

> Beautiful Soup 是一个HTML/XML 的解析器，主要用于解析和提取 HTML/XML 数据。  

Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,  
所有对象可以归纳为4种: Tag , NavigableString , BeautifulSoup , Comment .

## **取值**

```python
# Tag有很多方法和属性，tag中最重要的属性: `name` 和 `attributes`。

soup=BeautifulSoup(html,'lxml')
soup.h1 # 网页匹配的第一个标签
soup.div.attrs # 获取标签div所有属性,返回字典
soup.ol['class'] # 获取标签ol属性为class的值

# NavigableString
## ** 标签内非属性字符串,格式：soup.<tag>.string    
## ** NavigableString 可以跨越多个层次

soup.h1.string # h1 标签内的字符串

# BeautifulSoup
## ** 表示的是一个文档的全部内容.大部分时候,可以把它当作 Tag 对象，是一个特殊的 Tag，我们可以分别获取它的类型，名称。

soup.name # [document]

# Comment
## ** 注释及特殊字符串，`Tag` , `NavigableString` , `BeautifulSoup` 几乎覆盖了html和xml中的所有内容,但是还有一些特殊对象.容易让人担心的内容是文档的注释部分

markup = "<b><!--it is a comment--></b>"
soup = BeautifulSoup(markup)
comment = soup.b.string # it is a comment
```

## **节点**

```python
# 子 子孙节点
soup.ol.contents # <ol> 标签下的子节点 -> 列表
soup.ol.children # <ol> 标签下的子节点 -> 生成器对象   遍历获取
soup.ol.descendants # <ol> 标签下的子节点的所有子孙节点 -> 生成器对象   遍历获取

# 父 祖父节点
soup.ol.parent # 标签下的父节点
soup.ol.parents # <ol> 标签的祖先节点 -> 生成器对象   遍历获取

# 兄弟节点
soup.li.next_sibling # <li> 的下一个兄弟节点
soup.li.next_siblings # <li> 的后面所有兄弟节点
soup.li.previous_sibling # <li> 的上一个兄弟节点
soup.li.previous_siblings # <li> 的前面所有兄弟节点

# 前进后退节点
soup.li.next_element # 下一个临近的任何节点
soup.li.next_elements # 后面所有的节点
soup.li.previous_element # 上一个临近的任何节点
soup.li.previous_elements # 前面所有的节点
```

## **文档树**

```python
# find_all(name, attrs, recursive, text, **kwargs) 返回列表
# find() 返回第一个匹配

## ** name可以是：字符串、正则表达式、列表、True、方法
for a in soup.find_all('a'):
    a.find_all('span')
    a.string

## ** attrs 通过属性来查询
## ** class是Python的保留字，所以在class的后面加上下划线。
soup.find_all(attrs={'id': 'link1'})
soup.find_all(attrs={'class_': 'classname'})
```

## **CSS选择器**

```python
# soup.select('css_selector')
soup.select('title') # <title>
soup.select('li a') # <li><a></></>

for ul in soup.select('ul'): # 获取属性
    print(ul['id'])
    print(ul.attrs['id'])

for li in soup.select('li'): # 获取文本
    print('String:', li.string)
    print('get text:', li.get_text())
```
