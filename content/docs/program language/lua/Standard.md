+++
title = "Basic"
description = "Lua_basic"
date = "2023-05-22"
menu = "Lua"
+++

# **LUA BASIC**

### **变量声明**

```lua
-- 默认为全局变量
a = 1
b,c = 2,3
local d = 4 -- local 声明局部变量
print(e)  -- 未声明变量的值为 nil
```



### **数值**`NUMBER`

```lua
a = 1
b = 0x11 --17
c = 2e3 --2000.00
```



### **运算**

```lua
a,b = 1,2
a+b --3
b^2 -- 4
1<<3 -- 8  1左移3位为 1000(二进制) = 8 十进制
a~=b -- 不等于
```



### **字符串**`STRING`

```lua
a = 'abcdefg'
b = "abcdefg"
a = [[dsajkdhakw\nada
saddnaskdjalkw]] -- 不转义多行文本
a..b -- a+b 字符串相加
tostring(num) -- 转字符串
tonumber(str) -- 转数字   无法转换时 返回值为 nil
#a -- 字符串a的长度 类似于len(a)
```



### **函数**`FUNCTION`

```lua
function function_name(...) -- == function_name = function(...)
    -- body
    return nil -- 函数默认返回 nil
end
function_name() -- 调用

local a,b = function_name(1,2) -- return 返回多个值 可以多重赋值
```



### **数组**`TABLE`

```lua
a = {1,'ab',{},function() end} -- index 起始为 1
a[1] -- 1
a[5] = 'add five' --赋值
a[5] -- nil index超出后, 返回值为 nil
#a -- 5   len(a) 获取数组长度
table.insert(a,2,'six') -- 'six' 插入a表 下标2(不填默认插入最后)
table.remove(a,2) -- 移除 a 表的 index2 的值  返回值为移除的元素
function table.newf() -- 在 table 添加一个 newf 的函数
    -- body
end
table.newf()

b = {
    a  =1,
    b = '2',
    c = function()

    end,
    d = 1314,
    [',;']=123
}
b['a'] -- 1
b.b -- 1
b[',;'] -- 1
b.a = 3 --直接赋值
```



### **表**`_G`

```lua
a = 1
print(_G['a']) -- 1
-- 所有的全局变量都在 _G 里面
```



### **布尔**`BOOL`

```lua
a = 1
b = 11
and -- true and false  --false
or -- true or true(false)  -- true
not -- not true --false -- 返回值 为 true / false
0 -- true
false --false
nil -- false
b > 10 and "yes" or "no" -- true and true or false
```



### **分支判断**`IF`

```lua
if 1 then
    -- body
elseif 2 then
    -- body
else
    -- body
end
```



### **循环**`LOOP`

```lua
for i=1,10 do -- for i=1,10,step do
    --body
    if i == 5 then break end
end

local a = 10
while a>1 do
    a = a-1
    if n == 5 then break end
end
```



### **多文件调用**`require`

```lua
require('filename') -- 不需要写后缀 默认为 lua 后缀文件
require('path.filename') -- ./path/filename.lua
```



### **迭代器**

```lua
a = {'a','b','c','d'}
for i=1,#a do
    -- i == index   a[i] == value
    print(i,a[i])
end


for i,j in ipairs(a) do -- 只能循环连续的数标值
    -- i == index  j == value
    print(i,j)
end


for i,j in pairs(a) do -- 能循环所有值
    -- i == index  j == value
    print(i,j)
end
```

### **正则**

```shell
string.find(s,pattern) # 返回  起止 下标
string.match(s,pattern) # 返回值
string.gsub(s,p#tern,'x') # 把 pattern 替换成 x

# 转义 %# #（一个点）可表示任何字符。
%a # 表示任何字母。
%c #表示任何控制字符。
%d # 表示任#字。
%g # #任何除空白符外的可打印字符。
%l# 表示所有小写字母。
%## 表示所有标点符号。
%s## 表示所有空白字符。
# # 表示所有大写字母。
%w # 表示所#母及数字。
%x # 表示所#16 进制数字符号。
%x: #这里的 x 是任意非字母或数#字符） 表示字符 x。 这是#法字符转义的标准方法。 所有非#或数字的字符 （包括所有标点，也包括非魔法#） 都可以用前置一个 '%' 放在模式串中表示自身。
[set] #  表示 set　中所有字符的联合。 可以以 '-' 连接，升序书写范围两端的字符来表示一个范围的字符集。 上面提到的 %x 形式也可以在 se#中使用 表示其中的一个元素。 其它出现在 set 中的字符则代表它们自己。 例如，[%w_] （或 [_%w]） 表示所有的字母数字加下划线）， [0-7] 表示 8 进制数字， [0-7%l%-]　表示 8 进制数字加小写字母与 '-' 字符。

# 单个字符类匹配该类别中任意单个字符；
* #匹配零或多个该类的字符。 贪婪匹配
+ # 匹配一或更多个该类的字符。 贪婪匹配
- # 将匹配零或更多个该类的字符。 和 '*' 不同， 这个条目总是匹配尽可能短的串；
? # 将匹配零或一个该类的字符。 只要有可能，它会匹配一个；
%n， 这里的 n 可以从 1 到 9； 这个条目匹配一个等于 n 号捕获物（后面有描述）的子串。
%bxy， 这里的 x 和 y 是两个明确的字符； 这个条目匹配以 x 开始 y 结束， 且其中 x 和 y 保持 平衡 的字符串。 意思是，如果从左到右读这个字符串，对每次读到一个 x 就 +1 ，读到一个 y 就 -1， 最终结束处的那个 y 是第一个记数到 0 的 y。 举个例子，条目 %b() 可以匹配到括号平衡的表达式。
%f[set]， 指 边境模式； 这个条目会匹配到一个位于 set 内某个字符之前的一个空串， 且这个位置的前一个字符不属于 set 。 集合 set 的含义如前面所述。 匹配出的那个空串之开始和结束点的计算就看成该处有个字符 '\0' 一样。
```



### **元表 元方法**

```lua

```

> [Lua 5.3 参考手册  ✈](https://wiki.luatos.com/_static/lua53doc/contents.html)
