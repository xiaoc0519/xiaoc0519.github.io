+++
title = "DateTime"
date = 2023-05-23T11:50:23+08:00
menu = 'python'
+++
# **DateTime**
## **Time**
```python
import time

# 将一个时间戳转换为当前时区的struct_time。secs参数未提供，则以当前时间为准。
time.localtime([secs]) 
# time.struct_time(tm_year=2023, tm_mon=5, tm_mday=23, tm_hour=13, tm_min=13, tm_sec=16, tm_wday=1, tm_yday=143, tm_isdst=0) 

time.gmtime([secs]) # gmtime()方法是将一个时间戳转换为UTC时区（0时区）的struct_time
time.time() # 返回当前时间的时间戳
time.mktime(t) # 将一个struct_time转化为时间戳
time.sleep(secs) # 线程推迟指定的时间运行。单位为秒
time.asctime([t]) # 把一个表示时间的元组或者struct_time表示为这种形式：'Sun Jun 20 23:21:05 1993'
time.ctime([secs]) # 把一个时间戳（按秒计算的浮点数）转化为time.asctime()的形式。如果参数未给或者为None的时候，将会默认time.time()为参数
time.strftime(format[, t]) 
# 把一个代表时间的元组或者struct_time转化为格式化的时间字符串。
# 如果t未指定，将传入time.localtime()。如果元组中任何一个元素越界，ValueError的错误将会被抛出。
time.strftime("%Y-%m-%d %X", time.localtime())
```

## **Datetime**
```python
import datetime 

datetime.date.today() # 今日日期 y-m-d
```

### datetime.date()
```python
import datetime 

today = datetime.date(year=2023,month=5,day=19)   #  使用参数创建日期
today.year # 2023
today.month # 5
today.day # 19
today.timetuple() #  date对象的struct_time结构
today.toordinal() # 返回当前公历日期的序数
today.weekday() # 当前日期为星期0   0 = 星期一
today.isoweekday() # 当前日期为星期1   1 = 星期一
today.isocalendar() # 当前日期的年份、第几周、周几(其中返回为元组): (2023, 20, 5)
today.isoformat() # 以ISO 8601格式‘YYYY-MM-DD’返回date的字符串
today.ctime() # 返回一个表示日期的字符串 Mon May 19 00:00:00 2023
today.replace(2019,9,29) # 替换后的日期
today.strftime("%Y/%m/%d") # 指定格式
```

### datetime.time()
```python
import datetime 

ctime = datetime.time(hour=11,minute=18,second=31)
ctime.hour # 时 11
ctime.minute # 分 18 
ctime.second # 秒 31
ctime.isoformat() # 字符串形式 11:18:31
ctime.strftime("%H/%M/%S") # 指定格式 11/18/31
ctime.replace(20,9,29) # 替换后的时间 20:09:29
```

### datetime.datetime()
```python
import datetime 

datetime.datetime.today() # 现在的时间 2020-08-31 11:32:10.438292
datetime.datetime.now() # 更高精度 2020-08-31 11:32:10.439289
datetime.datetime.utcnow() # 当前UTC日期和时间是： 2020-08-31 03:32:10.439289
datetime.datetime.fromtimestamp(1234567896) # 对应时间戳的日期和时间是： 2009-02-14 07:31:36
datetime.datetime.utcfromtimestamp(1234567896) # 对应UTC时间戳的日期和时间是： 2009-02-13 23:31:36 
datetime.datetime.fromordinal(1) # 公历序列对应的日期和时间是： 0001-01-01 00:00:00
datetime.datetime.combine(datetime.date(2020, 8, 31), datetime.time(12, 12, 12)) # 日期和时间的合体为： 2020-08-31 12:12:12

now = datetime.datetime(2020,8,31,12,10,10)
now.date()  # 当前日期为 2020-08-31
now.time()  # 当前时间 12:10:10
# ... 其他和 date time 一样
```

### datetime.timedelta()

```python
import datetime 

now = datetime.date.today() # 表示现在的日期
before_5_date = now + datetime.timedelta(days=-5 [,minutes=-10]) # 表示五天[10分前]前的日期

now # 表示现在的日期
```
> `timedelta`类代表两个`datetime`对象之间的时间差，即两个日期或者日期时间之差。  
支持参数:`weeks` `days` `hours` `minutes` `seconds`  `milliseconds` `microseconds`


## **格式化**
```python
%a	# 本地（locale）简化星期名称	
%A	# 本地完整星期名称	
%b	# 本地简化月份名称	
%B	# 本地完整月份名称	
%c	# 本地相应的日期和时间表示	
%d	# 一个月中的第几天（01 - 31）	
%H	# 一天中的第几个小时（24小时制，00 - 23）	
%I	# 第几个小时（12小时制，01 - 12）	
%j  #  一年中的第几天（001 - 366）	
%m	# 月份（01 - 12）	
%M	# 分钟数（00 - 59）	
%p	# 本地am或者pm的相应符	
%S	# 秒（01 - 61）
%U	# 一年中的星期数。（00 - 53星期天是一个星期的开始。）第一个星期天之前的所有天数都放在第0周。
%w	# 一个星期中的第几天（0 - 6，0是星期天）
%W	# 和%U基本相同，不同的是%W以星期一为一个星期的开始。	
%x	# 本地相应日期	
%X	# 本地相应时间	
%y	# 去掉世纪的年份（00 - 99）	
%Y	# 完整的年份	
%Z	# 时区的名字（如果不存在为空字符）	
%%	# ‘%’字符
```