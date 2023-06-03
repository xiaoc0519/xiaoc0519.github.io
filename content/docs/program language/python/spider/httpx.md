+++
title = "Httpx"
date =  2023-05-22
description = "httpx"
menu = "python"
+++

# **HTTPX**

> 3.6+  pip install httpx,httpx[http2]

```python
import httpx

res = httpx.get('url',timeout=10.0) # 超时默认 5s  None 可以关闭超时
```



## **HTTP/2**

> httpx.Client() 类似于 requests.Session()

```python
client = httpx.Client(http2=True, verify=False) 
response = client.get(url, headers)
response.text
```

## **SPIDER**

```python
client = httpx.Client() #类似requests.Session()
try:
    do somting
finally:
    client.close() #关闭连接池

with httpx.Client() as client:
    r = client.get(url, headers=headers)

# Client 和 get 里面都可以添加 headers,最后这两个地方的 headers 可以合并到请求里
headers = {'X-Auth': 'from-client'}
params = {'client_id': 'client1'}
with httpx.Client(headers=headers, params=params) as client:
    headers = {'X-Custom': 'from-request'}
    params = {'request_id': 'request1'}
    r = client.get('https://example.com', headers=headers, params=params)

r.request.url # URL('https://example.com?client_id=client1&request_id=request1')
r.request.headers['X-Auth']   # 'from-client'
r.request.headers['X-Custom'] # 'from-request'
```

## **Proxy**

```python
proxies = {"all://": "http://localhost:8030",} # 允许所有请求都走代理, 值为 None 则不使用代理

# 不同的协议走不用的代理
proxies = {
    "http://": "http://localhost:8030",
    "https://": "http://localhost:8031",
}
```

> requests 代理写法
> 
>     proxies = {"http": "http://localhost:8030","https": ...}

## **链接池**

```python
# max_keepalive，允许的保持活动连接数或 None 始终允许。（预设10）
# max_connections，允许的最大连接数或 None 无限制。（默认为100）
limits = httpx.Limits(max_keepalive_connections=5, max_connections=10)
client = httpx.Client(limits=limits)
```

## **异步请求**

```python
import asyncio

async with httpx.AsyncClient() as client:
    resp = await client.get('http://httpbin.org/get')
    assert resp.status_code == 200
    html = resp.text
```

> 3.8+