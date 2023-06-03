+++
title = "Pyautogui"
description = "pyautogui"
date = "2023-05-22"
menu = "python"
+++

# **PYAUTOGUI**



```python
import pyautogui as pg

# 启用防故障功能, 鼠标强制移动到屏幕左上脚(0,0), 直接抛出 failSafeException 异常
pg.FAILSAFE = True

# 判断坐标是否在屏幕上
pg.onScreen(x,y)  #return True   Judge x,y is in screen

width, height = pg.size() # 获取运行环境屏幕尺度
```



## **MOUSE**

```python
X,Y = pg.position() # 获取鼠标当前坐标

# 鼠标移动
## 绝对移动
pg.moveTo(x,y,duration=1)  # duration 为移动时间 单位秒
## 相对当前位置移动
pg.moveRel(x,y,duration=1) # 向当前位置 右移X 下移Y 像素 (可以为负数)

# 拖拽
pg.dragTo(x,y(,sec),button='left') # 按住左键拖拽到x,y  sec为持续时间(s)
pg.dragRel(x,y(,sec),button='left') # 按住左键往右X 下Y 拖拽像素

# 点击
pg.click()  # 当前位置点击
pg.click(x,y)  # 在 x,y 位置点击
pg.click(x,y,duration=1) # 1秒时间移动到x,y并点击
pg.doubleClick() # 当前位置双击 和click 一样可以设置参数
pg.tripleClick() # 三击 同上
pg.mouseDown() # 鼠标左键按下
pg.mouseUp()  # 左键松开
pg.mouseDown(button='right') # 按下鼠标右键
pg.mouseUp(button='right', x=100, y=200) # 移动到(100, 200)位置，然后松开鼠标右键

# 滑轮
## scroll函数控制鼠标滚轮的滚动，amount_to_scroll参数表示滚动的格数。正数则页面向上滚动，负数则向下滚动
## pg.scroll(clicks=amount_to_scroll, x=moveToX, y=moveToY)
pg.scroll(10) # 向上滚动10格
pg.scroll(-10) # 向下滚动10格
pg.scroll(10, x=100, y=100) # 移动到(100, 100)位置再向上滚动10格

# 渐变函数 可以用于moveTo()，moveRel()，dragTo()和dragRel()函数
#  开始很慢，不断加速
pg.moveTo(100, 100, 2, pg.easeInQuad)
#  开始很快，不断减速
pg.moveTo(100, 100, 2, pg.easeOutQuad)
#  开始和结束都快，中间比较慢
pg.moveTo(100, 100, 2, pg.easeInOutQuad)
#  一步一徘徊前进
pg.moveTo(100, 100, 2, pg.easeInBounce)
#  徘徊幅度更大，甚至超过起点和终点
pg.moveTo(100, 100, 2, pg.easeInElastic)
```



## **KEYBOARD**

```python
pg.typewrite('type here'(,interval=0.25)) # 输入字符串 interval使每次键入间隔秒数
pg.press('enter') # 按一次enter
pg.press(['right','left','left'])  # 顺序完成每一个键位的敲击
pg.keyDown('shift') # 按下 shift 键
pg.keyUp('shift')  # 松开 shift 键

pg.keyDown('shift')
pg.press('4')
pg.keyUp('shift') # 输出 $ 符号的按键

pg.hotkey('ctrl', 'v') # 组合按键（Ctrl+V）

pg.KEYBOARD_KEYS # 显示press()，keyDown()，keyUp()和hotkey()函数可以输入的按键名称
```

## **POP-UPS**

```python
# 显示一个简单的带文字和OK按钮的消息弹窗。用户点击后返回button的文字。
pg.alert(text='', title='', button='OK')
b = pg.alert(text='要开始程序么？', title='请求框', button='OK')
print(b) # 输出结果为OK

# 显示一个简单的带文字、OK和Cancel按钮的消息弹窗，用户点击后返回被点击button的文字，支持自定义数字、文字的列表。
pg.confirm(text='', title='', buttons=['OK', 'Cancel']) # OK和Cancel按钮的消息弹窗
pg.confirm(text='', title='', buttons=range(10)) # 10个按键0-9的消息弹窗
a = pg.confirm(text='', title='', buttons=range(10))
print(a) # 输出结果为你选的数字

# 可以输入的消息弹窗，带OK和Cancel按钮。用户点击OK按钮返回输入的文字，点击Cancel按钮返回None。
pg.prompt(text='', title='', default='')

# 样式同prompt()，用于输入密码，消息用*表示。带OK和Cancel按钮。用户点击OK按钮返回输入的文字，点击Cancel按钮返回None。
pg.password(text='', title='', default='', mask='*')
```



## **IMAGE**

```python
im = pg.screenshot(r'path') # 截全屏并设置保存图片的位置和名称
print(im) # 打印图片的属性

# 不截全屏，截取区域图片。截取区域region参数为：左上角XY坐标值、宽度和高度
pg.screenshot(r'path', region=(0, 0, 300, 400))

pix = pg.screenshot().getpixel((220, 200)) # 获取坐标(220,200)所在屏幕点的RGB颜色
positionStr = ' RGB:(' + str(pix[0]).rjust(3) + ',' + str(pix[1]).rjust(3) + ',' + str(pix[2]).rjust(3) + ')'
print(positionStr) # 打印结果为RGB:( 60, 63, 65)
pix = pg.pixel(220, 200) # 获取坐标(220,200)所在屏幕点的RGB颜色与上面三行代码作用一样
positionStr = ' RGB:(' + str(pix[0]).rjust(3) + ',' + str(pix[1]).rjust(3) + ',' + str(pix[2]).rjust(3) + ')'
print(positionStr) # 打印结果为RGB:( 60, 63, 65)

# 如果你只是要检验一下指定位置的像素值，可以用pixelMatchesColor(x,y,RGB)函数，把X、Y和RGB元组值穿入即可
# 如果所在屏幕中(x,y)点的实际RGB三色与函数中的RGB一样就会返回True，否则返回False
# tolerance参数可以指定红、绿、蓝3种颜色误差范围
pg.pixelMatchesColor(100, 200, (255, 255, 255))
pg.pixelMatchesColor(100, 200, (255, 255, 245), tolerance=10)

# 获得传入文件图片在现在的屏幕上面的坐标，返回的是一个元组(top, left, width, height)
# 如果截图没找到，pg.locateOnScreen()函数返回None
a = pg.locateOnScreen(r'file')
print(a) # 打印结果为Box(left=0, top=0, width=300, height=400)
x, y = pg.center(a) # 获得文件图片在现在的屏幕上面的中心坐标
print(x, y) # 打印结果为150 200
x, y = pg.locateCenterOnScreen(r'file') # 这步与上面的四行代码作用一样
print(x, y) # 打印结果为150 200

# 匹配屏幕所有与目标图片的对象，可以用for循环和list()输出
pg.locateAllOnScreen(r'file')
for pos in pg.locateAllOnScreen(r'file'):
  print(pos)
# 打印结果为Box(left=0, top=0, width=300, height=400)
a = list(pg.locateAllOnScreen(r'file'))
print(a) # 打印结果为[Box(left=0, top=0, width=300, height=400)]
```