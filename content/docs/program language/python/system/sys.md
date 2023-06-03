+++
title = "System"
date = 2023-05-23T08:55:23+08:00
menu = 'python'
+++

# **SYSTEM**

## **SYS**
```python
import sys

# python name.py [arg1,[arg2]]
# 命令行调用 Python 脚本时提供的“命令行参数”
sys.argv  # ['name.py'[,'arg1'[,'arg2']]]

sys.platform # linux / win32

# 当前运行的 Python 解释器的可执行程序的绝对路径
sys.executable

sys.path # 调用 Python 解释器的脚本所在的绝对路径
sys.path.append(path) # 添加 path模块 到解释器路径
```

## **OS**
```python
import os

os.name # posix(Linux/Mac)  nt(Win)  java(Java)

os.listdir() # 列出（当前）目录下的全部路径（及文件）默认为 ./
def get_filelists(file_dir='.'):
	list_directory = os.listdir(file_dir)
    filelists = []
	for directory in list_directory:
        if(os.path.isfile(directory)):
        	filelists.append(directory)
       return filelists
       
os.mkdir(dir) # 新建一级路径 多级创建报错 FileNotFoundError  已有创建报错 FileExistsError
os.makedirs(path) # 新建多级路径

os.rmdir(dir) # 删除目录
os.makedirs(path) # 删除多级目录
os.remove(path) # 删除文件
os.rename(path,rname)
os.renames()
os.getcwd() # 当前工作路径
os.chdir(path) # 切换当前工作路径为指定路径
os.chdir("..")

'a/b/c'
os.chdir("..") # a/b
os.getcwd() # a/b
with open("./file", encoding="utf-8") as f:
    f.read()

os.path.join('str1','str2',...) # str1\\str2 多个传入路径组合为一个路径,
# 有绝对路径 D:/ 时，最后一个“绝对路径”及其之后的参数才会体现在返回结果中。

os.path.abspath(abspath[regpath]) # 可以是不存在的路径， 会自动组合成新的绝对路径
os.path.basename(path) # 传入路径的最下级目录 /分隔符 后面的最后一个字符串
os.path.dirname(path) # 最后一个分隔符前的整个字符串
os.path.split(path) # 传入路径以最后一个分隔符为界，分成两个字符串，并打包成元组的形式返回
os.path.exists(path) # True/False 判断路径所指向的位置是否存在
os.path.isabs(path) # 检测是否是绝对路径，不对其有效性进行任何核验 
os.path.isfile() # 判断传入路径是否是文件 会核验路径的有效性 无效路径返回False
os.path.isdir() # 判断传入路径是否是路径 会核验路径的有效性 无效路径返回False
```



