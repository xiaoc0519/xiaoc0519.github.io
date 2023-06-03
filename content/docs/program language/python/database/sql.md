+++
title = "Sql"
date = 2023-05-29T17:05:52+08:00
menu = 'python'
+++

# **SQL**

```python
import pymysql
```

## **WORKFLOW**

```python
# 连接数据库
db = pymysql.connect(host='localhost',
                     port = 3366,
                     user='root',
                     password='123456',
                     database='DB',
                     charset='utf8mb4)

# 建立游标
cur = db.cursor()

# 使用 execute() 方法执行 SQL，如果表存在则删除
cursor.execute("DROP TABLE IF EXISTS EMPLOYEE")

# 执行sql语句
sql = "DELETE FROM STUDENT WHERE NAME='ZZZ'"
cursor.execute(sql)

# 提交修改
db.commit()

# 关闭数据库连接
```

## **插入**

```python
sql = "INSERT INTO STUDENT(NAME,AGE, SEX, ID)VALUES ('ZZZ', 22, 女,1)"
#try语句防止连接数据库时发生错误
try:
   cursor.execute(sql)
   db.commit()
    print("数据插入成功")
except:
   # 如果发生错误则回滚
   db.rollback()
```

## **查询**

```python
sql = "SELECT * FROM STUDENT WHERE NAME='ZZZ'" 
try:
   cursor.execute(sql)
   # 获取所有记录列表
   results = cursor.fetchall()
   # result = cursor.fetchone() 获取单条数据
   for row in results:#遍历查询结果
      name = row[0]
      age = row[1]
      sex = row[2]
      id = row[3]

      print ("name:%s,age:%d,sex:%s,id:%s" % (name,age,sex,id))
except:
   print ("查询失败")
```

## **更新**

```python
sql = "UPDATE STUDENT SET AGE = 20 WHERE SEX = '%s'" % ('女')
try:
   cursor.execute(sql)
   db.commit()
   print("数据更新成功")
except:
   db.rollback()
   print("数据更新失败")
```

## **删除**

```python
sql = "DELETE FROM STUDENT WHERE NAME='ZZZ"
try:
   cursor.execute(sql)
   db.commit()
   print("数据删除成功")
except:
   db.rollback()
   print("数据删除失败")
```

## **新建表**

```python
sql = """CREATE TABLE EMPLOYEE (
              FIRST_NAME  CHAR(20) NOT NULL,
              LAST_NAME  CHAR(20),
              AGE INT,
              SEX CHAR(1),
              INCOME FLOAT )"""
cursor.execute(sql)
```
