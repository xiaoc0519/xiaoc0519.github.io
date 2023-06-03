+++
title = "Json"
date = 2023-05-23T11:50:58+08:00
menu = 'python'
+++

# **Json**

```python
import json

# 数据类型（obj）转化为 JSON 字符串
json.dumps(obj) 

# 把数据写入文件中
with open(file,"w") as f:
    date = json.dump(dic,f) 
    
# 将已编码的 JSON 字符串解码为 Python 对象
json.loads(jsondata) 

# 把数据文件读出
with open(file, 'r')as f:
    data = json.load(f) 
    
    
data = { "name":"name1" , "url":"url1" ,'data':{'pic':'pic1'}}
data['name'] # name1
data.get('data') # {'pic':'pic1'}
data['data'] # {'pic':'pic1'}
data['data']['pic'] # pic1
```


