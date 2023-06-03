+++
title = "MongoDB"
date = 2023-05-29T17:05:52+08:00
menu = 'python'
+++

# **MongoDB**

```python
import pymango
```

> [MongoDB ✈](https://docs.mongoing.com/mongodb-crud-operations)

## **WORKFLOW**

```python
myclient = pymongo.MongoClient('mongodb://localhost:27017/')
db = mongo_client.admin
db.authenticate('用户名', '密码')

# 创建集合
db.createCollection(name, options) 

# 删除集合
db.collectionName.drop()

# 指定数据库和集合
db = client.test # 获取数据库
db = client['test']

collection = db.stu # 指定集合
collection = db['stu']
```

## **插入**

```python
db.collection_name.insert(document)
# mongodb中文档的格式是json格式

#增加一条
stu1={'id':'001','name':'zhangsan','age':10}
result = collection.insert_one(stu1)
#增加多条
stu2={'id':'002','name':'lisi','age':15}
stu3={'id':'003','name':'wangwu','age':20}
result = collection.insert_many([stu2,stu3])
```

## **删除**

```python
#可以直接使用remove方法删除指定的数据
result = collection.remove({'name': 'zhangsan'})
#使用delete_one()删除一条数据
result = collection.delete_one({"name":"zhangsan"})
#delete_many()删除多条数据
result = collection.delete_many({"age":{'$lt':20}})
```

## **修改**

```python
#update_one,第 2 个参数需要使用$类型操作符作为字典的键名
#姓名为zhangsan的记录，age修改为22
condition = {'name': 'zhangsan'}
res = collection.find_one(condition)
res['age'] = 22
result = collection.update_one(condition, {'$set': res})
print(result) #返回结果是UpdateResult类型
print(result.matched_count,result.modified_count) #获得匹配的数据条数1、影响的数据条数1

#update_many,所有年龄为15的name修改为xixi
condition = {'age': 15}
res = collection.find_one(condition)
res['age'] = 30
result = collection.update_many(condition, {'$set':{'name':'xixi'}})
print(result) #返回结果是UpdateResult类型
print(result.matched_count,result.modified_count) #获得匹配的数据条数3、影响的数据条数3
```

## **查找**

```python
# 返回所有满足条件的结果，如果条件为空，则返回全部结果，返回结果是一个Cursor游标可迭代对象。
rets = collection.find({"age":20})

for ret in rets:
    print(ret)

# 查询结果有多少条数据
count = collection.find().count()

# 查询结果按年龄升序排序
results = collection.find().sort('age', pymongo.ASCENDING)
print([result['age'] for result in results])
```
