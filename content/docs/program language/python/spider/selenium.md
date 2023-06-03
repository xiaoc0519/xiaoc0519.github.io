+++
title = "Selenium"
date =  2023-05-22
menu = "python"
+++

# **SELENIUM 4**

> 驱动下载:   [Firefox](https://github.com/mozilla/geckodriver/releases)    [Chrome](https://registry.npmmirror.com/binary.html?path=chromedriver/)   [Edge](https://developer.microsoft.com/zh-cn/microsoft-edge/tools/webdriver/)

## **启动**

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService

service = ChromeService(executable_path=CHROMEDRIVER_PATH)
driver = webdriver.Chrome(service=service)
```

## **参数**

```python
option = webdriver.Options()

options.add_argument('user-agent="value"') # 添加UA
options.add_argument('window-size=1920x3000') # 指定浏览器分辨率
options.add_argument('--disable-gpu') # 谷歌文档提到需要加上这个属性来规避bug
options.add_argument('--hide-scrollbars') # 隐藏滚动条, 应对一些特殊页面
options.add_argument('blink-settings=imagesEnabled=false') # 不加载图片, 提升速度
options.add_argument('--headless') # 不提供可视化页面. linux系统不支持可视化不加这条会启动失败
options.add_argument('--no-sandbox') # 以最高权限运行
options.binary_location = r"path" # 手动指定使用的浏览器位置
options.add_extension('crx') # 添加 CRX 插件
options.add_argument("--disable-javascript") # 禁用JavaScript
options.add_argument("--proxy-server=http://XXXXX.com:80") # 添加代理

# 设置开发者模式启动，该模式下webdriver属性为正常值
options.add_experimental_option('excludeSwitches', ['enable-automation']) 

# 禁用浏览器弹窗
prefs = {  
    'profile.default_content_setting_values' :  {  
        'notifications' : 2  
     }  
}  
options.add_experimental_option('prefs',prefs)


driver=webdriver.Chrome(options=options)
```

## **元素定位**

```python
driver.find_element(By.XPATH,'XPATH')
driver.find_element(By.CLASS_NAME,'CLASS_NAME')
driver.find_element(By.CSS_SELECTOR,'CSS_SELECTOR')
driver.find_element(By.ID,'ID')
driver.find_element(By.LINK_TEXT,'LINK_TEXT')
driver.find_element(By.PARTIAL_LINK_TEXT,'PARTIAL_LINK_TEXT')
driver.find_element(By.TAG_NAME,'TAG_NAME')
```

> 查找多个元素，只需要将其中的 `find_element` 替换成 `find_elements` 即可。

## **等待元素**

```python
from selenium.webdriver.support.ui import WebDriverWait

el = WebDriverWait(driver, timeout=3).until(lambda d: d.find_element_by_tag_name("p"))
```

## **动作API**

```python
clickable = driver.find_element(By.ID, "clickable")
ActionChains(driver)\
        .move_to_element(clickable)\
        .pause(1)\
        .click_and_hold()\
        .pause(1)\
        .click()\ # 点击元素
        .clear()\ # 清除元素内的文本
        .send_keys("abc")\ # 写入文本
        .perform()

# 当前有动作执行时，可以使用以下方法停止这些动作，
ActionBuilder(driver).clear_actions() # 释放所有动作
```

## **Keyboard**

```python
ActionChains(driver)\ # 按下 shift+abc
    .key_down(Keys.SHIFT)\
    .send_keys("abc")\
    .perform()

ActionChains(driver)\ # 浏览器输入某串字符（不指定元素）
    .send_keys("abc")\
    .perform()

text_input = driver.find_element(By.ID, "textInput") # 指定元素输入字符串
    ActionChains(driver)\
    .send_keys_to_element(text_input, "abc")\
    .perform()
```

## **Mouse**

```python
# 鼠标点击保持，该方法将鼠标移动到元素中心与按下鼠标左键相结合。这有助于聚焦特定元素
clickable = driver.find_element(By.ID, "clickable")
    ActionChains(driver)\
    .click_and_hold(clickable)\
    .perform()

clickable = driver.find_element(By.ID, "click") # 鼠标点击释放
    ActionChains(driver)\
    .click(clickable)\
    .perform()

# 鼠标定义的5种按键
# 0——鼠标左键
# 1——鼠标中键
# 2——鼠标右键
# MouseButton.BACK ---（后退键）
# MouseButton.FORWARD ---（前进键）

clickable = driver.find_element(By.ID, "clickable") # 鼠标右击
    ActionChains(driver)\
    .context_click(clickable)\
    .perform()

action = ActionBuilder(driver) # 按下鼠标3键
    action.pointer_action.pointer_down(MouseButton.BACK)
    action.pointer_action.pointer_up(MouseButton.BACK)
    action.perform()

hoverable = driver.find_element(By.ID, "hover") # 鼠标移动到元素上
    ActionChains(driver)\
    .move_to_element(hoverable)\
    .perform()

clickable = driver.find_element(By.ID, "clickable") # 鼠标双击
    ActionChains(driver)\
    .double_click(clickable)\
    .perform()   

# 拖拽元素
## 单击并按住源元素，移动到目标元素的位置，然后释放鼠标
draggable = driver.find_element(By.ID, "draggable")
    droppable = driver.find_element(By.ID, "droppable")
    ActionChains(driver)\
        .drag_and_drop(draggable, droppable)\
        .perform()

## 通过位移拖拽
draggable = driver.find_element(By.ID, "draggable")
    start = draggable.location
    finish = driver.find_element(By.ID, "droppable").location
    ActionChains(driver)\
        .drag_and_drop_by_offset(draggable, finish['x'] - start['x'], finish['y'] - start['y'])\
        .perform()
```

## **Scroll**

> *support chromedriver only*

```python
iframe = driver.find_element(By.TAG_NAME, "iframe") # 滚动到某元素位置
    ActionChains(driver)\
        .scroll_to_element(iframe)\
        .perform()

footer = driver.find_element(By.TAG_NAME, "footer") # 定量滚动
    delta_y = footer.rect['y']
    ActionChains(driver)\
        .scroll_by_amount(0, delta_y)\
        .perform()

iframe = driver.find_element(By.TAG_NAME, "iframe") # 从一个元素滚动指定量
    scroll_origin = ScrollOrigin.from_element(iframe)
    ActionChains(driver)\
        .scroll_from_origin(scroll_origin, 0, 200)\
        .perform()

footer = driver.find_element(By.TAG_NAME, "footer") # 从一个元素滚动，并指定位移
    scroll_origin = ScrollOrigin.from_element(footer, 0, -50)
    ActionChains(driver)\
        .scroll_from_origin(scroll_origin, 0, 200)\
        .perform()

ActionChains(driver)\ # 从一个元素的原点位移
        .scroll_from_origin(scroll_origin, 0, 200)\
        .perform()
```
