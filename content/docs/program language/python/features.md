+++
title = "Features" 

description = "version new" 

date = "2023-06-03" 

menu = "python" 

+++



#  **FEATURES**

## F-string

>  `f'{expr=}'` 的 f-字符串将扩展表示为表达式文本，加一个等于号，再加表达式的求值结果  
> 
> 指定了转换符，表达式的求值结果会先转换再格式化

```python
des = 'string'
f'this is a {des}' # this is a string
f'{des = }' # des = string

# '!r' 为调用 repr()
name = "Fred"
f"He said his name is {name!r}." # "He said his name is 'Fred'."

# '!s' 即对结果调用 str()
# '!a' 为调用 ascii()

width = 10
precision = 4
value = decimal.Decimal("12.34567")
f"result: {value:{width}.{precision}}"  # nested fields
'result:      12.35'

today = datetime(year=2017, month=1, day=27)
f"{today:%B %d, %Y}"  # using date format specifier
'January 27, 2017'
f"{today=:%B %d, %Y}" # using date format specifier and debugging
'today=January 27, 2017'

number = 1024
f"{number:#0x}"  # using integer format specifier
'0x400'
foo = "bar"
f"{ foo = }" # preserves whitespace
" foo = 'bar'"

line = "The mill's closed"
f"{line = :20}" # "line = The mill's closed   "
f"{line = !r:20}" # 'line = "The mill\'s closed" '
```

### 变量注释

> 函数声明中的各个参数可以在：后增加注解表达式  
> 
> 参数由默认值，注解放在参数名和 = 号之间  
> 
> 注解返回值，在` ) `和函数末尾的`:`之间增加` ->` 和一个表达式。可以是任何类型。注解中最常用的类型是类（如 `str` 或 `int`）和字符串（如 `int > 0`）。

```python
primes: List[int] = []
captain: str
class Starship:
    stats: Dict[str, int] = {}
def sum(a:int,b: int = 20)-> int:pass
```

## 数字文字中的下划线

```python
1_000_000_000_000_000 # 1000000000000000
0x_FF_FF_FF_FF # 4294967295
```

## 海象赋值

> 在表达式内部为变量赋值

```python
if (n := len(a)) > 10:# 赋值表达式可以避免调用 len() 两次:
    print(f"List is too long ({n} elements, expected <= 10)")

discount = 0.0 # 在正则中需要使用两次匹配对象，一次检测用于匹配是否发生，另一次用于提取子分组:
if (mo := re.search(r'(\d+)% discount', advertisement)):
    discount = float(mo.group(1)) / 100.0

# 配合 while 循环计算一个值来检测循环是否终止，而同一个值又在循环体中再次被使用的情况
while (block := f.read(256)) != '':
    process(block)

# 列表推导式中，在筛选条件中计算一个值，而同一个值又在表达式中需要被使用:
[clean_name.title() for name in names
 if (clean_name := normalize('NFC', name)) in allowed_names]    
```

## 仅限位置形参

> `/` 用来指明某些函数形参必须使用仅限位置而非关键字参数的形式

```python
# a 和 b 为仅限位置形参，c 或 d 可以是位置形参或关键字形参，而 e 或 f 要求为关键字形参
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f) 
f(10, 20, 30, d=40, e=50, f=60)

# 在 / 左侧的形参不会被公开为可用关键字，其他形参名仍可在 **kwargs 中使用
def f(a, b, /, **kwargs):
    print(a, b, kwargs)
f(10, 20, a=1, b=2, c=3) # 10 20 {'a': 1, 'b': 2, 'c': 3}
```

## 字典合并与更新运算符

> 合并 (`|`) 与更新 (`|=`) 运算符已被加入内置的 `dict` 类。 它们为现有的 `dict.update` 和 `{**d1, **d2}` 字典合并方法提供了补充。

```python
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}
x | y # {'key1': 'value1 from x', 'key2': 'value2 from y', 'key3': 'value3 from y'}
y | x # {'key2': 'value2 from x', 'key3': 'value3 from y', 'key1': 'value1 from x'}
```

## 移除前缀和后缀的字符串方法

```python
# str.removeprefix(prefix, /)
# 字符串以 prefix 字符串开头，返回 string[len(prefix):]。 否则，返回原始字符串的副本
'TestHook'.removeprefix('Test') # 'Hook'
'BaseTestCase'.removeprefix('Test') # 'BaseTestCase'

# str.removesuffix(suffix, /)
# 如果字符串以 suffix 字符串结尾，并且 suffix 非空，返回 string[:-len(suffix)]。 否则，返回原始字符串的副本
'MiscTests'.removesuffix('Tests') # 'Misc'
'TmpDirMixin'.removesuffix('Tests') # 'TmpDirMixin'
```

## 带圆括号的上下文管理器
  
> 支持使用外层圆括号来使多个上下文管理器可以连续多行地书写。 这允许将过长的上下文管理器集能够以与之前 import 语句类似的方式格式化为多行的形式

```python
with (CtxManager() as example):
    ...

with (
    CtxManager1(),
    CtxManager2()
):
    ...

with (CtxManager1() as example,
      CtxManager2()):
    ...

with (CtxManager1(),
      CtxManager2() as example):
    ...

with (
    CtxManager1() as example1,
    CtxManager2() as example2
):
    ...
```

## 结构化模式匹配

> 接受一个表达式并将其值与以一个或多个 case 语句块形式给出的一系列模式进行比较

```python
match subject:
    case <pattern_1>:
        <action_1>
    case <pattern_2>:
        <action_2>
    case <pattern_3>:
        <action_3>
    case _: # _ 匹配任何
        <action_wildcard>
# 没有匹配成功，在 _ 的最后一个 case 语句，则它将被用作已匹配模式。  
# 如果match 没有匹配成功， 并不存在 _ 通配符， 则忽略整个匹配语句
```

### 简单模式：匹配一个字面值

> 变量名`_`将作为 通配符 并确保目标将总是被匹配。`_` 的使用是可选的。  
使用 `|` （“ or ”）在一个模式中组合几个字面值:

```python
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 401 | 403 | 404: # 使用 | （“ or ”）在一个模式中组合几个字面值:
    		return "Not allowed"
        case 404:
            return "Not found"
        case 418: #  status 为 418，则会返回 "I'm a teapot"
            return "I'm a teapot"
        case _: # status 为 500，则带有 _ 的 case 语句将作为通配符匹配
            return "Something's wrong with the internet"
```

### 无通配符的行为

> 如果不在 case 语句中使用 `_`，可能会出现不存在匹配的情况。如果不存在匹配，则行为是一个 no-op

```python
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
```

### 带有字面值和变量的模式

> 模式可以看起来像解包形式，而且模式可以用来绑定变量

```python
# point is an (x, y) tuple
match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"Y={y}")
    case (x, 0):
        print(f"X={x}")
    case (x, y):
        print(f"X={x}, Y={y}")
    case _:
        raise ValueError("Not a point")
# 第一个模式有两个字面值 (0, 0) ，可以看作是上面所示字面值模式的扩展。
# 接下来的两个模式结合了一个字面值和一个变量，而变量 绑定 了一个来自主词的值（point）。 
# 第四种模式捕获了两个值，这使得它在概念上类似于解包赋值 (x, y) = point 。
```

### 模式和类

```python
class Point:
    x: int
    y: int

def location(point):
    match point:
        case Point(x=0, y=0):
            print("Origin is the point's location.")
        case Point(x=0, y=y):
            print(f"Y={y} and the point is on the y-axis.")
        case Point(x=x, y=0):
            print(f"X={x} and the point is on the x-axis.")
        case Point():
            print("The point is located somewhere else on the plane.")
        case _:
            print("Not a point")
```

### 复杂模式和通配符

```python
match test_variable:
    case ('warning', code, 40):
        print("A warning has been received.")
    case ('error', code, _):
        print(f"An error {code} occurred.")
```

### 约束项

> 可以向一个模式添加 if 子句，称为“约束项”。  
	如果约束项为假值，则 match 将继续尝试下一个 case 语句块。 请注意值的捕获发生在约束项被求值之前

```python
match point:
    case Point(x, y) if x == y:
        print(f"The point is located on the diagonal Y=X at {x}.")
    case Point(x, y):
        print(f"Point is not on the diagonal.")
```