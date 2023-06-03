+++
title = "Taobao"
date = 2023-05-30T15:48:25+08:00
tags = 'python'
menu = "main"
+++

website taobao spider  

```shell
scrapy startproject projectname
cd projectname 
scrapy genspider spidername spiderwebsite
pip install -r requirements.txt
```

## 创建浏览器

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService

def creat_chrome_driver(*,headless=False):
    option = webdriver.Options()
    if headless:
        options.add_argument('--headless')
    options.add_experimental_option('excludeSwitches',['enable-automation'])
    options.add_experimental_option('useAutomationExtension',False)
    browser = webdriver.Chrome(options=options)
    browser.execute_cdp_cmd(
        'Page.addScriptToEvaluateOnNewDocument',
        {'source':'Object.defineProperty(navigator,"webdriver",{get: () => undefined})'}
    )
    return browser
    
 def add_cookies(browser,cookie_file):
 	with open(cookie_file,'r') as f:
    	cookie_list = json.load(f)
        for cookie_dict in cookies_list:
        	if cookie_dict['secure']:
            	browser.add_cookie(cookie_dict)
```

## 从浏览器获取cookie

```python
browser = creat_chrome_driver()
browser.get('https://login.taobao.com')
# 隐式等待
browser.implicitly_wait(10)
user_input = browser.find_element(By.CSS_SELECTOR,'#')
user_input.send_keys('name')
...
login_button.click()

with open('taobao.json','w') as f:
    json.dump(browser.get_cookies(),f)
```

## 访问淘宝页面 并设置cookie
```python
browser = creat_chrome_driver()
browser.get('https:www/taobao.com')
add_cookies(browser,'taobao.json')
browser.get('https://s.taobao.com/search?q')
```

## Items.py
```python
class a:
	title = scrapy.Field()
    .....
```

## Spider.py
```python
def start_requests(self):
	keywords = [key1, key2]
    for key in keywords:
    	for page in range(2):
        	url = f'https://s.taobao.com/search?q{key}&s={page * 48}' # 一页48数据
            yield scrapy.Request(url = url)
```

## Middlewares.py
```python
def __inti__(self):
	self.browser = creat_chrome_driver(headless=True)
    self.browser.get('https://www.taobao.com')
    add_cookies(self.browser,'taobao.json')
    
def __del__(self)   :
	self.browser.close()
    
def process_request(self,request,spider):
	self.browser.get(requests.url)
    return HtmlResponse(url=request.url,body=self.browser.page_source,request=request,encoding='utf-8') # 拿到动态加载的网页代码，返回响应体
```

## Setting.py
```python
DOWNLOADER_MIDDLEWARES = {
    'mySpider.middlewares.MyspiderDownloaderMiddleware': 543,
}
```