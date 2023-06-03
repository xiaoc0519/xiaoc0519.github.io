+++
title = "Requests"
date =  2023-05-22
description = "requests"
menu = "python"
+++

# **REQUESTS**

```python
import requests,json

res = requests.get(url,headers,params,json,verify=false,cookies,timeout)
# params get请求传参  a=1
# data 非get请求提交数据, res.body的格式为 a=1&b=2
# json 提交数据，res.body的格式为 {“a”: 1, “b”: 2}
res = res.text # 文本内容
res = res.content # 2进制内容
json.loads(json) # json数据
```



## **Requests**

```python
requests.get()
requests.post()
requests.head() # 获取网页头的信息
requests.put() # 提交put请求
requests.delete() # 向HTML页码提交删除请求
requests.patch() # 向HTML网页提交局部修改请求

requests.get(
    url,headers,cookies,timeout,
    json, # 提交数据，res.body的格式为 {“a”: 1, “b”: 2}
    params, # get请求传参  a=1
    data, # 非get请求提交数据, res.body的格式为 a=1&b=2
    auth, # 元组，支持HTTP认证功能
    files, # 字典类型，传输文件
    proxies, # 字典类型，设定访问代理服务器，可以增加登录认证
    allow_redirects, # True/False，默认为True，重定向开关
    stream, # True/False， 默认为True，获取内容立刻下载开关
    verify, # True/False，默认为True，认证SSL证书开关
    cert # 本地SSL证书路径
)
```

## **Response**

```python
# cookie 获取处理cookie
cookie = res.cookie
cookkie = requests.utils.dict_from_cookiejar(cookies)

res.encoding='utf-8' # 解码/修改编码格式
res.content # 字节码返回值， 图片/视频等
res.text # 网页文档 字符串
res.status_code #状态码 
res.url # 响应的url
res.cookie # 获取cookie
```

## **Session** 

```python
session = requests.session() # 维持会话

res = session.get()

# 手动添加 cookie
cookie = { "PHPSESSID":"value" }
req = requests.session()
requests.utils.add_dict_to_cookiejar(req.cookies,cookie)
req.get(url,cookies = cookie)
```

*[BeautifulSoup4](../bs4/)* 

*[RE](../../standard/re/)*  