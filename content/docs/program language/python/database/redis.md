+++
title = "Redis"
date = 2023-05-29T17:05:52+08:00
menu = 'python'
+++

# **REDIS**

```python
import redis
```

## **WORKFLOW**
```python
redis_conn = redis.Redis(host="192.168.31.196", port=6379,password="123456")

# 连接池
redis_pool = redis.ConnectionPool(host='127.0.0.1', port= 6379, password= 'your pw', db= 0)
redis_conn = redis.Redis(connection_pool= redis_pool)

redis_conn.set('name1', 'value1)   #添加
redis_conn.get('name1')  #获取
```

## **STRING 字符串**
> 一个键对应一个值
```python
# 设置单个键值
set(name, value[, ex=None, px=None, nx=False, xx=False])
# ex：过期时间（秒），时间到了后redis会自动删除
# px：过期时间（毫秒），时间到了后redis会自动删除。ex、px二选一即可
# nx：如果设置为True，则只有name不存在时，当前set操作才执行
# xx：如果设置为True，则只有name存在时，当前set操作才执行

redis_conn.set('name_2', 'ZZZ_2')

# 获取单个值
redis_conn.get('name_1')

# 设置多个键值
mset(*args, **kwargs)
redis_conn.mset(name_1= 'ZZZ1', name_2= 'ZZZ2')

name_dict = {
    'name_4' : 'ZZZ4',
    'name_5' : 'ZZZ5'
}
redis_conn.mset(name_dict)

# 获取多个值
mget(keys, *args)
m = redis_conn.mget('name_1', 'name_2')
#m = redis_conn.mget(['name_1', 'name_2']) 也行

# 给已有的键设置新值，并返回原有的值
getset(name, value)
v = redis_conn.getset('name_1', 'hi') # 所给的键不存在时，会设置其新值，但返回值为None

# String setrange 根据索引修改某个键的value值
setrange(name, offset, value) # 返回值为：修改后的字符串长度

# name：键，所给不存在时自动添加
# offset：偏移量，以0开始，全部修改 
# value：修改的字符或字符串，字符串时以offset向后顺延

length = redis_conn.setrange('name_2', 1, 'zhihu')

# String getrange 根据索引获取某个键的部分value值
getrange(key, start, end) # 若所给的键不存在时，返回空值 b''
v = redis_conn.getrange('name_4', 0, 2) # 返回前3个字符

# String strlen 获取value的长度
strlen(name) # 所给的键不存在时，返回值为0
length = redis_conn.strlen('name_2')

# String incr int类型的value自增（自减 decr(name, amount=1)
incr(name, amount=1) # 值必须是整数或字符串的数值，不然会报错。默认自增幅度为1

redis_conn.set('num_2', 2) # redis_conn.set('num_2', '2') 都行
v = redis_conn.incr('num_2') # 返回值：修改后的值，int类型

# String incrbyfloat 浮点数类型的value自增
incrbyfloat(name, amount=1.0)

v = redis_conn.incrbyfloat('num_2') # 返回值：浮点数类型float

# String append value后面追加
append(key, value) # 若所给的键不存在，则设置新值
length = redis_conn.append('name_5', '666') # 返回值为修改后的字符串的长度
```

## **LIST 列表**
> 一个键对应一个列表
```python
# List lpush 列表左边添加值 rpush（右边）
lpush(name, *values)
# value值有多个时，从左到右依次向列表左边添加，类型可以不同
# 所给的键不存在时，新建一个列表
# 返回值：列表的大小
v = redis_conn.lpush('Zarten', 1,2,3,4,5)

# List lpushx 键存在时，添加到列表左边 rpushx（最右边）
lpushx(name, value) # 只有键存在时，才添加。若键不存在则不添加，也不新创建列表
v = redis_conn.lpushx('Zarten_1', 'hehe') # 返回值为：列表大小

# List llen 获取所给键的列表大小
llen(name)
v = redis_conn.llen('Zarten')

# List linsert 在列表中间插入新值
linsert(name, where, refvalue, value) # 返回：插入后列表的长度，若返回-1，则refvalue不存在
# name：键名
# where：位置，前面（BEFORE）或后面（AFTER）
# refvalue：指定哪个值的前后插入
# value：插入的新值
v = redis_conn.linsert('Zarten', 'AFTER', 6, 'b') # 值 6 的后面插入  'b'

# List lset 列表中通过索引赋值
lset(name, index, value) # 返回值：成功 True 否则 False
v = redis_conn.lset('Zarten', 2, 'cc')

# List lindex 通过索引获取列表值
lindex(name, index)
v = redis_conn.lindex('Zarten', 2)

# List lrange 列表中获取一段数据
lrange(name, start, end) # 返回值：List类型的一段数据
v = redis_conn.lrange('Zarten', 2, 5)

# List lpop 删除左边的第一个值 rpop（右边）
lpop(name) # 返回值：被删除元素的值
v = redis_conn.rpop('Zarten')

# List lrem 删除列表中N个相同的值
lrem(name, value, num=0) # 返回删除的个数
# name：键名
# value：需删除的值
# num：删除的个数 整数表示从左往右 负数表示从右往左 例如：2 -2
v = redis_conn.lrem('Zarten', 'hehe', -2)

# List ltrim 删除列表中范围之外的所有值
ltrim(name, start, end) # 返回：成功 True
v = redis_conn.ltrim('Zarten', 5, 10)

# List blpop 删除并返回列表最左边的值 brpop（最右边）
blpop(keys, timeout=0) # 返回值：tuple类型 形如： (键名, 删除的值) (b'Zarten', b'hehe')
# keys：给定的键
# timeout：等待超时时间，默认为0，表示一直等待
v = redis_conn.blpop('Zarten')

# List rpoplpush 一个列表中最右边值取出后添加到另一个列表的最左边 brpoplpush阻塞版本
rpoplpush(src, dst) # 返回值：取出的元素值
# brpoplpush(src, dst, timeout=0)为rpoplpush的阻塞版本，timeout为0时，永远阻塞
v = redis_conn.rpoplpush('Zarten', 'Zhihu')
```

## **HASH 哈希**
> 内部存储为各个键值对
```python
# Hash hset 哈希中添加一个键值对
hset(name, key, value) # 返回添加成功的个数 int
# key存在，则修改，否则添加
v = redis_conn.hset('Zarten', 'age', 10)

# Hash hmset 设置哈希中的多个键值对
hmset(name, mapping) # 返回值：成功 True
# mapping：dict 类型
v = redis_conn.hmset('Zarten', {'sex':1, 'tel':'123'})

# Hash hmget 获取哈希中多个键值对
hmget(name, keys, *args) # 返回值：值的列表 list 形如： [b'1', b'123'] <class 'list'>
v = redis_conn.hmget('Zarten', ['sex', 'tel'])
#v = redis_conn.hmget('Zarten', 'sex', 'tel') 也ok

# Hash hget 获取指定key的值
hget(name, key)
v = redis_conn.hget('Zarten', 'age')

# Hash hgetall 获取哈希中所有的键值对
hgetall(name) # 返回值：dict类型
v = redis_conn.hgetall('Zarten')

# Hash hlen 获取哈希中键值对的个数
hlen(name)
v = redis_conn.hlen('Zarten')

# Hash hkeys 获取哈希中所有的键key
hkeys(name) # 返回值：list类型
v = redis_conn.hkeys('Zarten')

# Hash hvals 获取哈希中所有的值value
hvals(name) # 返回值：list类型
v = redis_conn.hvals('Zarten')

# Hash hexists 检查哈希中是否有某个键key
hexists(name, key) # 返回值：有 True ；否则 False
v = redis_conn.hexists('Zarten', 'b')

# Hash hdel 删除哈希中键值对（key-value）
hdel(self, name, *keys) # 返回值：int 删除的个数
v = redis_conn.hdel('Zarten', 'age')

# Hash hincrby 自增哈希中key对应的value值（必须整数数值类型）
hincrby(name, key, amount=1) # 返回int 增加后的数值
# 若所给的key不存在则创建，amount默认增加1，可以为负数
v = redis_conn.hincrby('Zarten', 'sex', -3)

# Hash hincrbyfloat 自增浮点数 同上hincrby
hincrbyfloat(name, key, amount=1.0)

# Hash expire 设置整个键的过期时间
expire(name, time)
# time：秒，时间一到，立马自动删除
v = redis_conn.expire('Zarten', 10)

# Hash hscan 增量迭代获取哈希中的数据
hscan(name, cursor=0, match=None, count=None) # 返回值：tuple 类型 ；（扫描位置，所有dict数据）
# name：redis的name
# cursor：游标（基于游标分批取获取数据）
# match：匹配指定key，默认None 表示所有的key
# count：每次分片最少获取个数，默认None表示采用Redis的默认分片个数
v = redis_conn.hscan('Zarten')

# Hash hscan_iter 返回hscan的生成器
hscan_iter(name, match=None, count=None)

v = redis_conn.hscan_iter('Zarten')
for i in v:
    print(type(i), i)
```

## **SET 集合**
> 集合中的元素不重复，一般用于过滤元素
```python
# Set sadd 添加元素到集合中
sadd(name, *values)
# 若插入已有的元素，则自动不插入
v = redis_conn.sadd('Zarten', 'apple', 'a', 'b', 'c')

# Set scard 返回集合中元素的个数
scard(name)
v = redis_conn.scard('Zarten')

# Set smembers 获取集合中的所有元素
smembers(name) # 返回值：set类型，形如： {b'a', b'apple', b'c', b'b'}
v = redis_conn.smembers('Zarten')

# Set srandmember 随机获取一个或N个元素
srandmember(name, number=None) # 返回值：返回一个值或一个列表
# name：键名
# number：一个或N个，默认返回一个。若返回N个，则返回List类型
v = redis_conn.srandmember('Zarten', 2)

# Set sismember 判断某个值是否在集合中
sismember(name, value) # 返回值：True 在 False 不在
v = redis_conn.sismember('Zarten', 'appl')

# Set spop 随机删除并返回集合中的元素
spop(name)
v = redis_conn.spop('Zarten')

# Set srem 删除集合中的一个或多个元素
srem(name, *values) # 返回值：返回删除的个数 int
v = redis_conn.srem('Zarten', 'c', 'a')

# Set smove 将一个集合中的值移动到另一个集合中
smove(src, dst, value) # 返回值：成功 True
# 若value不存在时，返回False
v = redis_conn.smove('Zarten', 'Fruit', 'apple')

# Set sdiff 返回在一个集合中但不在其他集合的所有元素（差集）
sdiff(keys, *args) # 返回值：set类型 {b'2', b'4', b'3', b'1'}
# 在keys集合中，不在其他集合中的元素
v = redis_conn.sdiff('Zarten', 'Fruit')

# Set sdiffstore 上面的sdiff的返回值（差集）保存在另一个集合中
sdiffstore(dest, keys, *args) # 返回值：int 返回作用的个数
# 在keys集合中，不在其他集合中的元素保存在dest集合中
# dest：新的集合，设置的新集合，旧集合会被覆盖
v = redis_conn.sdiffstore('Left', 'Zarten', 'Fruit')

# Set sinter 返回一个集合与其他集合的交集
sinter(keys, *args) # 返回值：set类型
v = redis_conn.sinter('Zarten', 'Fruit')

# Set sinterstore 返回一个集合与其他集合的交集，并保存在另一个集合中
sinterstore(dest, keys, *args)
# dest：另一个集合，设置新集合，旧集合元素会被覆盖
v = redis_conn.sinterstore('Left', 'Zarten', 'Fruit')

# Set sunion 返回一个集合与其他集合的并集
sunion(keys, *args)
v = redis_conn.sunion('Zarten', 'Fruit')

# Set sunionstore 返回一个集合与其他集合的并集，并保存在另一个集合中
sunionstore(dest, keys, *args) # 返回新集合元素个数
# dest：另一个集合，设置新集合，旧集合元素会被覆盖
v = redis_conn.sunionstore('Left', 'Zarten', 'Fruit')
```

## **ZSET 有序集合**
> 有序集合比集合多了一个分数的字段，可对分数升序降序
```python
# Zset zadd 有序集合中添加元素
zadd(name, *args, **kwargs) # 返回添加的个数
# 添加元素时需指定元素的分数
v = redis_conn.zadd('Zarten', 'a', 3, 'b', 4)
#v = redis_conn.zadd('Zarten', c= 5, d= 6)

# Zset zcard 返回有序集合中元素个数
zcard(name)
v = redis_conn.zcard('Zarten')

# Zset zcount 返回有序集合中分数范围内的元素个数
zcount(name, min, max) # 返回个数 int
# 包含min max
v = redis_conn.zcount('Zarten', 3, 5)

# Zset zscore 返回有序集合中指定某个值的分数
zscore(name, value) # 返回值：float 类型的分数；形如： -5.0 <class 'float'>
v = redis_conn.zscore('Zarten', 'zhi')

# Zset zincrby 增加有序集合中某个值的分数
zincrby(name, value, amount=1) # 返回增加后的分数 float类型 ；形如： -5.0 <class 'float'>
# value：若存在，则增加其amount分数；若不存在，则增加新值以及对应的分数
# amount：增加的值，可以为负数
v = redis_conn.zincrby('Zarten', 'zhi', -5)

# Zset zrem 删除有序集合中的某个或多个值
zrem(name, *values) # 返回删除的个数
v = redis_conn.zrem('Zarten', 'zhi', 'a')

# Zset zremrangebyrank 删除有序集合元素根据排序范围
zremrangebyrank(name, min, max) # 返回删除个数 int
v = redis_conn.zremrangebyrank('Zarten', 1, 3)

# Zset zremrangebyscore 删除有序集合根据分数范围
zremrangebyscore(name, min, max) # 返回删除个数 int
v = redis_conn.zremrangebyscore('Zarten', 8, 15)

# Zset zrank 返回某个值在有序集合中的分数排名（从小到大） zrevrank（从大到小）
zrank(name, value) # 返回value在name中的分数排名值，分数从小到大排名，从0开始
v = redis_conn.zrank('Zarten', 'b')

# Zset zrange 返回有序集合分数排序的一段数据
zrange(name, start, end, desc=False, withscores=False, score_cast_func=float) # 返回list类型 [(b'tt', 10.0), (b'd', 6.0), (b'c', 5.0)] <class 'list'>
# name：redis的name
# start：有序集合索引起始位置（非分数）
# end：有序集合索引结束位置（非分数）
# desc：排序规则，默认按照分数从小到大排序
# withscores：是否获取元素的分数，默认只获取元素的值
# score_cast_func：对分数进行数据转换的函数
v = redis_conn.zrange('Zarten', 1, 3, True, True, score_cast_func=float)
```

## **BITMAP 位图**
> bitmap中存放二进制的位0和1，类似位数组。典型应用是基于redis的布隆过滤器。  
属于String字符串数据结构，固bit 映射被限制在 512 MB 之内（2^32）
```python
# Bitmap setbit 设置位图的值
setbit(name, offset, value)
# name：redis键名
# offset：偏移量，大于等于0。当偏移伸展时，空白位置以0填充
# value：二进制值 0或1
v = redis_conn.setbit('Zarten_2', 100, 1)

# Bitmap getbit 返回位图指定偏移量的值
getbit(name, offset) # 返回0或1
v = redis_conn.getbit('Zarten_2', 101)

# Bitmap bitcount 返回位图中二进制为1的总个数
bitcount(key, start=None, end=None)
# start end指定开始和结束的位，默认整个位图
v = redis_conn.bitcount('Zarten_2', 100, 1000)
```