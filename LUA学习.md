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

Lua I/O 库用于读取和处理文件。分为简单模式（和C一样）、完全模式。

- 简单模式（simple model）拥有一个当前输入文件和一个当前输出文件，并且提供针对这些文件相关的操作。
- 完全模式（complete model） 使用外部的文件句柄来实现。它以一种面对对象的形式，将所有的文件操作定义为文件句柄的方法

简单模式在做一些简单的文件操作时较为合适。但是在进行一些高级的文件操作的时候，简单模式就显得力不从心。例如同时读取多个文件这样的操作，使用完全模式则较为合适。

#### 11.1 简单模式

| 模式 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| r    | 以只读方式打开文件，该文件必须存在。                         |
| w    | 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。 |
| a    | 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。（EOF符保留） |
| r+   | 以可读写方式打开文件，该文件必须存在。                       |
| w+   | 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。 |
| a+   | 与a类似，但此文件可读可写                                    |
| b    | 二进制模式，如果文件是二进制文件，可以加上b                  |
| +    | 号表示对文件既可以读也可以写                                 |



简单模式使用标准的 I/O 或使用一个当前输入文件和一个当前输出文件。

```lua
-- 以只读方式打开文件
file = io.open("test.lua", "r")

-- 设置默认输入文件为 test.lua
io.input(file)

-- 输出文件第一行
print(io.read())

-- 关闭打开的文件
io.close(file)

-- 以附加的方式打开只写文件
file = io.open("test.lua", "a")

-- 设置默认输出文件为 test.lua
io.output(file)

-- 在文件最后一行添加 Lua 注释
io.write("--  test.lua 文件末尾注释")

-- 关闭打开的文件
io.close(file)
```



在以上实例中我们使用了 io."x" 方法，其中 io.read() 中我们没有带参数，参数可以是下表中的一个：

| 模式         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| "*n"         | 读取一个数字并返回它。例：file.read("*n")                    |
| "*a"         | 从当前位置读取整个文件。例：file.read("*a")                  |
| "*l"（默认） | 读取下一行，在文件尾 (EOF) 处返回 nil。例：file.read("*l")   |
| number       | 返回一个指定字符个数的字符串，或在 EOF 时返回 nil。例：file.read(5) |

其他的 io 方法有：

- **io.tmpfile():**返回一个临时文件句柄，该文件以更新模式打开，程序结束时自动删除
- **io.type(file):** 检测obj是否一个可用的文件句柄
- **io.flush():** 向文件写入缓冲中的所有数据
- **io.lines(optional file name):** 返回一个迭代函数，每次调用将获得文件中的一行内容，当到文件尾时，将返回 nil，但不关闭文件。



#### 11.2 完全模式

通常需要在同一时间处理多个文件。需要使用 file:function_name 来代替 io.function_name 方法。以下实例演示了如何同时处理同一个文件:

```lua
-- 以只读方式打开文件
file = io.open("test.lua", "r")

-- 输出文件第一行
print(file:read())

-- 关闭打开的文件
file:close()

-- 以附加的方式打开只写文件
file = io.open("test.lua", "a")

-- 在文件最后一行添加 Lua 注释
file:write("--test")

-- 关闭打开的文件
file:close()
```



执行以上代码，输出了 test.lua 文件的第一行信息，并在该文件最后一行添加了 lua 的注释。如输出的是：

```
-- test.lua 文件
```

read 的参数与简单模式一致。

其他方法:

- **file:seek(optional whence, optional offset):** 设置和获取当前文件位置,成功则返回最终的文件位置(按字节),失败则返回nil加错误信息。参数 whence 值可以是:

  - "set": 从文件头开始
  - "cur": 从当前位置开始[默认]
  - "end": 从文件尾开始
  - offset:默认为0

  不带参数file:seek()则返回当前位置,file:seek("set")则定位到文件头,file:seek("end")则定位到文件尾并返回文件大小

- **file:flush():** 向文件写入缓冲中的所有数据

- **io.lines(optional file name):** 打开指定的文件 filename 为读模式并返回一个迭代函数，每次调用将获得文件中的一行内容，当到文件尾时，将返回 nil，并自动关闭文件。
  若不带参数时io.lines() <=> io.input():lines(); 读取默认输入设备的内容，但结束时不关闭文件，如：

  ```
  for line in io.lines("main.lua") do
  　　print(line)
  end
  ```

以下实例使用了 seek 方法，定位到文件倒数第 25 个位置并使用 read 方法的 *a 参数，即从当前位置(倒数第 25 个位置)读取整个文件。


```lua
-- 以只读方式打开文件
file = io.open("test.lua", "r")

file:seek("end",-25)
print(file:read("*a"))

-- 关闭打开的文件
file:close()
```

边输出的结果是：

```
st.lua 文件末尾--test
```



### 12). 错误

错误类型有：

- 语法错误
- 运行错误

#### 12.1 语法错误

语法错误通常是由于对程序的组件（如运算符、表达式）使用不当引起的。

```lua
-- test.lua 文件
a == 2

-- 以上代码执行结果为：lua: test.lua:2: syntax error near '=='
-- 以上出现了语法错误，一个 "=" 号跟两个 "=" 号是有区别的。一个 "=" 是赋值表达式两个 "=" 是比较运算。

for a= 1,10
   print(a)
end
-- 执行以上程序会出现如下错误：lua: test2.lua:2: 'do' expected near 'print'
-- 语法错误比程序运行错误更简单，运行错误无法定位具体错误，而语法错误我们可以很快的解决，如以上只要在for语句下添加 do 即可：
for a= 1,10
do
   print(a)
end
```


#### 12.2 运行错误

```lua
-- 运行错误是程序可以正常执行，但是会输出报错信息。如下实例由于参数输入错误，程序执行时报错：
function add(a,b)
   return a+b
end

add(10)

-- 当我们编译运行以下代码时，编译是可以成功的，但在运行的时候会产生如下错误：
-- lua: test2.lua:2: attempt to perform arithmetic on local 'b' (a nil value)
-- stack traceback:
--    test2.lua:2: in function 'add'
--    test2.lua:5: in main chunk
--    [C]: ?
-- lua 里调用函数时，即使实参列表和形参列表不一致也能成功调用，多余的参数会被舍弃，缺少的参数会被补为 nil。
-- 以上报错信息是由于参数 b 被补为 nil 后，nil 参与了 + 运算。
-- 假如 add 函数内不是 "return a+b" 而是 "print(a,b)" 的话，结果会变成 "10 nil" 不会报错。
```

#### 12.3 错误处理

可以使用两个函数：assert 和 error 来处理错误。

```lua
local function add(a,b)
   assert(type(a) == "number", "a 不是一个数字")
   assert(type(b) == "number", "b 不是一个数字")
   return a+b
end
add(10)
-- lua: test.lua:3: b 不是一个数字
-- stack traceback:
--    [C]: in function 'assert'
--    test.lua:3: in local 'add'
--    test.lua:6: in main chunk
--    [C]: in ?
-- assert首先检查第一个参数，若没问题，assert不做任何事情；否则，assert以第二个参数作为错误信息抛出。
-- error函数语法格式：
error (message [, level])
-- 功能：终止正在执行的函数，并返回message的内容作为错误信息(error函数永远都不会返回)
-- 通常情况下，error会附加一些错误位置的信息到message头部。
-- Level参数指示获得错误的位置:
-- Level=1[默认]：为调用error位置(文件+行号)
-- Level=2：指出哪个调用error的函数的函数
-- Level=0:不添加错误位置信息

-- 

```



pcall和xpcall、debug

pcall接收一个函数和要传递给后者的参数，并执行，执行结果：有错误、无错误；返回值true或者或false, errorinfo。

语法格式如下:

```lua
if pcall(function_name, ….) then
-- 没有错误
else
-- 一些错误
end
```





### 13). 模块与包



### 14). 元表(Metatable)



### 15). 协同程序(coroutine)



### 16). 垃圾回收



### 17). 面向对象



### 18). 数据访问

Lua 连接MySql 数据库：

```lua
require "luasql.mysql" -- 5.2 版本之后，require 不再定义全局变量，需要保存其返回值。 require "luasql.mysql" => luasql = require "luasql.mysql"

--创建环境对象
env = luasql.mysql()

--连接数据库
conn = env:connect("数据库名","用户名","密码","IP地址",端口)

--设置数据库的编码格式
conn:execute"SET NAMES UTF8"

--执行数据库操作
cur = conn:execute("select * from role")

row = cur:fetch({},"a")

--文件对象的创建
file = io.open("role.txt","w+");

while row do
    var = string.format("%d %s\n", row.id, row.name)
    print(var)
    file:write(var)
    row = cur:fetch(row,"a")
end

file:close()  --关闭文件对象
conn:close()  --关闭数据库连接
env:close()   --关闭数据库环境
```



### 19). 调试(Debug)









