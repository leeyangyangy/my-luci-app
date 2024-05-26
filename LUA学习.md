# LUA学习 

lua提供了交互式编程方式和脚本编程模式

## 1. 交互式编程

```shell
lua -i
```

## 2. 脚本式编程

```shell
vim hello.lua
# hello
print("hello lua")
```

使用 lua 命令运行脚本

```shell
lua hello.lua
# output: hello lua
```

## 3. 语法

### 1).  注释：

```lua
-- 单行注释

--[[
多行注释
--]]
```



### 2). 标识符

以字母或下划线开头加上0个或多个字母、下划线、数字。最好不要使用下划线加大写字母的标识符，Lua的保留字也是这样设计。

Lua不允许使用特殊字符如 @ 、$ 、% 来表示。

Lua是区分大小写的编程语言

以下是正确的标识符

```lua
mohd         zara      abc     move_name    a_123
myname50     _temp     j       a23b9        retVal
```



### 3). 关键字

以下列出了 Lua 的保留关键词。保留关键字不能作为常量或变量或其他用户自定义标示符：

| and      | break | do    | else   |
| -------- | ----- | ----- | ------ |
| elseif   | end   | false | for    |
| function | if    | in    | local  |
| nil      | not   | or    | repeat |
| return   | then  | true  | until  |
| while    | goto  |       |        |

一般约定，以下划线开头连接一串大写字母的名字（比如 _VERSION）被保留用于 Lua 内部全局变量。



### 4). 全局变量

默认情况下，变量总是认为是全局。全局变量不需要声明，给一个变量赋值后即创建了这个全局变量，访问一个没有初始化的全局变量也不会出错，只不过得到的结果是：nil。

想要删除一个全局变量，只需要将变量赋值为 nil (换句话说, 当且仅当一个变量不等于nil时，这个变量即存在。)



### 5). 数据类型

Lua 是动态类型语言，变量不要类型定义,只需要为变量赋值。 值可以存储在变量中，作为参数传递或结果返回。

8 个基本类型分别为：nil、boolean、number、string、userdata、function、thread 和 table。

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| nil      | 这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）。 |
| boolean  | 包含两个值：false和true。                                    |
| number   | 表示双精度类型的实浮点数                                     |
| string   | 字符串由一对双引号或单引号来表示                             |
| function | 由 C 或 Lua 编写的函数                                       |
| userdata | 表示任意存储在变量中的C数据结构                              |
| thread   | 表示执行的独立线路，用于执行协同程序                         |
| table    | Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字、字符串或表类型。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。 |



### 6). 变量

Lua 变量有三种类型：全局变量、局部变量、表中的域。

Lua 中的变量全是全局变量，哪怕是语句块或是函数里，除非用 local 显式声明为局部变量。

局部变量的作用域为从声明位置开始到所在语句块结束。

变量的默认值均为 nil。

```lua
-- test.lua 文件脚本
a = 5               -- 全局变量
local b = 5         -- 局部变量

function joke()
    c = 5           -- 全局变量
    local d = 6     -- 局部变量
end

joke()
print(c,d)          --> 5 nil

do 
    local a = 6     -- 局部变量
    b = 6           -- 对局部变量重新赋值
    print(a,b);     --> 6 6
end

print(a,b)      --> 5 6

-- output
-- 5    nil
-- 6    6
-- 5    6
```



Lua 可以对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量。

```lua
a, b = 10, 2*x       <-->       a=10; b=2*x
a, b, c = 0, 1
print(a,b,c)             --> 0   1   nil
 
a, b = a+1, b+1, b+2     -- value of b+2 is ignored
print(a,b)               --> 1   2
 
a, b, c = 0
print(a,b,c)             --> 0   nil   nil
```



索引赋值

```lua
t[i]
t.i                 -- 当索引为字符串类型时的一种简化写法
gettable_event(t,i) -- 采用索引访问本质上是一个类似这样的函数调用
```



### 7). 函数

```lua
function max(num1,num2)
    if (num1>num2)
       	then return num1;
    else return num2;
    end
end
print("max num=",max(1,2))
```



可变参数,用 ... 表示

```lua
function add(...)  
local s = 0  -- local 表示本地变量
  for i, v in ipairs{...} do   --> {...} 表示一个由所有变长参数构成的数组  
    s = s + v  
  end  
  return s  
end  
print(add(3,4,5,6,7))  --->25
```

将可变参数赋值给一个变量

```lua
function average(...)
   result = 0
   local arg={...}    --> arg 为一个表，局部变量
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. #arg .. " 个数")
   return result/#arg
end

print("平均值为",average(10,5,3,4,5,6))
-- 总共传入 6 个数
-- 平均值为    5.5
```

也可以通过 select("#",...) 来获取可变参数的数量:

```lua
function average(...)
   result = 0
   local arg={...}
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. select("#",...) .. " 个数")
   return result/select("#",...)
end

print("平均值为",average(10,5,3,4,5,6))
-- 总共传入 6 个数
-- 平均值为    5.5
```



### 8). 数组

lua下标从1开始

```lua
-- 创建一个数组
local myArray = {10, 20, 30, 40, 50}

-- 循环遍历数组
for i = 1, #myArray do
    print(myArray[i])
end
-- output: 10 20 30 40 50

array = {"Lua", "Tutorial"}

for i= 0, 2 do
   print(array[i])
end

-- output: nil Lua Tutorial

-- 使用整数索引来访问数组元素，如果指定的索引没有值则返回 nil

-- 向数组中添加元素
-- 创建一个数组
local myArray = {10, 20, 30, 40, 50}

-- 添加新元素到数组末尾
myArray[#myArray + 1] = 60


-- 循环遍历数组
for i = 1, #myArray do
    print(myArray[i])
end

-- 删除数组中元素
-- 创建一个数组
local myArray = {10, 20, 30, 40, 50}

-- 删除第三个元素
table.remove(myArray, 3)

-- 循环遍历数组
for i = 1, #myArray do
    print(myArray[i])
end
```



多维数组

```lua
-- 初始化数组
array = {}
maxRows = 3
maxColumns = 3
for row=1,maxRows do
   for col=1,maxColumns do
      array[row*maxColumns +col] = row*col
   end
end

-- 访问数组
for row=1,maxRows do
   for col=1,maxColumns do
      print(array[row*maxColumns +col])
   end
end
```



### 9). 迭代器(iterator)

迭代器是一种对象，能够遍历标准模板库容器中的部分或全部元素，在 Lua 中迭代器是一种支持指针类型的结构，它可以遍历集合的每一个元素。

#### 9.1 泛型for迭代器

泛型 for 在自己内部保存迭代函数，实际上它保存三个值：迭代函数、状态常量、控制变量。

泛型 for 迭代器提供了集合的 key/value 对，语法格式如下：

```lua
-- k, v为变量列表；pairs(t)为表达式列表
for k, v in pairs(t) do
    print(k, v)
end
```

↓

```lua
array = {"Google", "Runoob"}
-- Lua 默认提供的迭代函数 ipairs
for key,value in ipairs(array)
do
  print(key, value)
end

-- output:
-- 1  Google
-- 2  Runoob
```

泛型 for 的执行过程：

- 首先，初始化，计算 in 后面表达式的值，表达式应该返回泛型 for 需要的三个值：迭代函数、状态常量、控制变量；与多值赋值一样，如果表达式返回的结果个数不足三个会自动用 nil 补足，多出部分会被忽略。
- 第二，将状态常量和控制变量作为参数调用迭代函数（注意：对于 for 结构来说，状态常量没有用处，仅仅在初始化时获取他的值并传递给迭代函数）。
- 第三，将迭代函数返回的值赋给变量列表。
- 第四，如果返回的第一个值为nil循环结束，否则执行循环体。
- 第五，回到第二步再次调用迭代函数

在Lua中常常使用函数来描述迭代器，每次调用该函数就返回集合的下一个元素。Lua 的迭代器包含以下两种类型：

- 无状态的迭代器
- 多状态的迭代器

#### 9.2 无状态的迭代器



#### 9.3 多状态的迭代器



### 10). table

​	

### 11). 文件I/O



### 12). 错误



### 13). 模块与包



### 14). 元表(Metatable)



### 15). 协同程序(coroutine)



### 16). 垃圾回收



### 17). 面向对象



### 18). 数据访问



### 19). 调试(Debug)









