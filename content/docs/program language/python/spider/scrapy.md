+++
title = "Scrapy"
date = 2023-05-22T22:44:05+08:00
menu = "python"
+++

# **SCRAPY**

## **WORKFLOW**

```shell
scrapy startproject projectname # 创建项目文件
cd projectname 
scrapy genspider spidername spiderwebsite # 创建项目爬虫网页
# scrapy genspider douban movie.douban.com

scrapy crawl spidername [-o name.csv] # 运行爬虫 指定输出格式
```

## **FILES**

```shell
projectname/
    scrapy.cfg  # 项目的配置文件。
    projectname/ # 项目的Python模块，将会从这里引用代码
        __init__.py
        items.py # 项目的目标文件
        pipelines.py # 项目的管道文件
        settings.py # 项目的设置文件
        spiders/ # 存储爬虫代码目录
            __init__.py
            spidername.py
            ...
```

## **SETTINGS.py**

```python
# Scrapy settings for mySpider project
...

BOT_NAME = 'mySpider' # scrapy项目名

SPIDER_MODULES = ['mySpider.spiders']
NEWSPIDER_MODULE = 'mySpider.spiders'
.......

# Obey robots.txt rules
ROBOTSTXT_OBEY = False # 是否遵守协议,一般给位false,但是创建完项目是是True,我们把它改为False

# Configure maximum concurrent requests performed by Scrapy (default: 16)
CONCURRENT_REQUESTS = 32 # 最大并发量 默认16
......
DOWNLOAD_DELAY = 3 # 下载延迟 3秒

# Override the default request headers: # 请求报头,我们打开
DEFAULT_REQUEST_HEADERS = {
  'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'Accept-Language': 'en',
}
# 爬虫中间件
SPIDER_MIDDLEWARES = {
    'mySpider.middlewares.MyspiderSpiderMiddleware': 543,
}

# 下载中间件
DOWNLOADER_MIDDLEWARES = {
    'mySpider.middlewares.MyspiderDownloaderMiddleware': 543,
}
......
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
    'mySpider.pipelines.MyspiderPipeline': 300, # 管道
}
......
```

## **ITEM.py**

```python
class ZcoolItem(scrapy.Item):
    # define the fields for your item here like:
    imgLink = scrapy.Field() # 封面图片链接
    title = scrapy.Field() # 标题
    types = scrapy.Field() # 类型
    vistor = scrapy.Field() # 人气
    comment = scrapy.Field() # 评论数
    likes = scrapy.Field() # 推荐人
```

> 定义每个需要写入的信息

## **SPIDER.py**

```python
from projectname.items import ItemClassName
# 从当前项目下的item 导入 item类
# from zcool.items import ZcoolItem

def parse(self, response):
    divList = response.xpath('//div[@class="work-list-box"]/div') # 信息总的范围
    divList = response.css('.div::text')
    for div in divList:
        imgLink = div.xpath("./div[1]/a/img/@src").getall()[0] # 1.封面图片链接 
        # 2.title(标题）;3 types（类型）;4vistor（人气）;5comment（评论数）  ....
        likes = div.xpath("./div[2]/p[3]/span[3]/@title").get() # 6likes（推荐人数）
        item = ZcoolItem(imgLink=imgLink,title=title,types=types,vistor=vistor,comment=comment,likes=likes)
        # 上面可以写成
        item = ZcoolItem()
        item['imgLink'] = imglink
        # ...
        yield item

    # 翻页实现
    # 定位下一页按钮实现
    next_href = response.xpath("//a[@class='laypage_next']/@href").get()
    if next_href: # 如果获取到了下一页按钮
        next_url = response.urljoin(next_href) # 拼接url链接
        print('*' * 60)
        print(next_url)
        print('*' * 60)
        request = scrapy.Request(next_url) # 把下一页的url传递给Request函数,进行翻页循环数据采集。
        yield request

    # 翻页实现二， 循环出网页并传递
    # 更换 start_urls = 为下方函数
    def start_requests(self):
        for page in range(10):
            yield scrapy.Request(url = f'https://...?start={page}&...'
            [,meta={'proxy':'ip:port'}]) # 代理可选


```

## **PIPELINES.py**

```python
from itemadapter import ItemAdapter
import csv

class ZcoolPipeline:
    def __init__(self): 
        self.f = open('Zcool.csv','w',encoding='utf-8',newline='')  # line1 写文件，利用第3个参数把csv写数据时产生的空行消除
        self.file_name = ['name1', 'name2','name3',...]  # line2 设置文件第一行的字段名，注意要跟spider传过来的字典key名称相同
        self.writer = csv.DictWriter(self.f, fieldnames=self.file_name) # line3 指定文件的写入方式为csv字典写入，参数1为指定具体文件，参数2为指定字段名
        self.writer.writeheader() # line4 写入第一行字段名，因为只要写入一次，所以文件放在__init__里面

    def open_spider(self,spider):
        self.f = open('Zcool.csv','w',encoding='utf-8',newline='')  # line1 写文件，利用第3个参数把csv写数据时产生的空行消除
            self.file_name = ['name1', 'name2','name3',...]  # line2 设置文件第一行的字段名，注意要跟spider传过来的字典key名称相同
            self.writer = csv.DictWriter(self.f, fieldnames=self.file_name) # line3 指定文件的写入方式为csv字典写入，参数1为指定具体文件，参数2为指定字段名
            self.writer.writeheader() # line4 写入第一行字段名，因为只要写入一次，所以文件放在__init__里面


    def process_item(self, item, spider):
        self.writer.writerow(dict(item))  # line5 写入spider传过来的具体数值,注意在spider文件中yield的item,是一个由类创建的实例对象，我们写入数据时，写入的是 字典，所以这里还要转化一下。
        print(item) 
        return item  # line6  写入完返回

    def close_spider(self,spider):
        self.f.close()

import pymysql

def __init__(self):
    self.conn = pymysql.connect(host='',port=123,user='',password='',database='',charset='utf8mb4')
    self.cursor = self.conn.cursor()

def process_item(self,item,spider):    
    title = item.get('title','') # 取不到值返回参数2 ''
    rank = item.get('rank','')
    subject = item.get('subject','')
    self.cursor.execute(
        'insert into name (key1, key2, key3) value (%s, %s, %s)',(title,rank,subject)
    )

def clpse_spider(self,spider):
    self.conn.close()
```

## **MIDDLERWARES.py**

```python
# cookie , 代理 添加
def get_cookies_dict():
    cookie_str = 'aa=mdmkdm;bb=sakdad;'
    cookies_dict = {}
    for item in cookie_str.split(';'):
        key, value = item.split('=',maxsplit=1)
        cookies_dict[key] = value
    return cookies_dict 

COOKIE_DICT = cookies_dict()    

def process_requests(self,request,spider):
    request.cookie = COOKIE_DICT
    request.meta = {'proxy':'ip:port'}
    return None
```
