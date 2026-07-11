# lua简介

> Lua是一种轻量小巧的动态脚本语言，用标注C语言编写并以源代码形式开放。

# 变量声明

> 变量声明与python变量声明很像
> 
> 默认声明的是全局变量，局部变量需要`local`标识符

```lua
Name = "ding" --全局变量，全局变量最好以大写开头
print(Name)
Name = 23
local nn = 95                 --局部变量
local age, addr = 23, "anhui" --批量赋值
print(Name)
```

# 数组

## 声明与使用

> 数组下标是从1开始的

```lua
Array = { 10, "ding", { "ss" } }
print(Array[1], Array[2])
```

## 遍历

```lua
-- 方式1：按索引遍历
for i = 1, #Array do
    print(Array[i])
end
-- 方式2：用pairs遍历（会遍历所有键值对）遇到nil跳过
for key, value in pairs(Array) do
    print(key, value)
end
```

# 表

> `table`是一种**键值对**集合：
> 
> - 键可以是除了nil以外的任意类型
> 
> - 值可以是任意类型
> 
> - table是动态的，可以随时添加/删除键值对
> 
> - table是引用类型（赋值、传参时传递的是地址，而不是拷贝）

```lua
local user = {
    name = "ding",
    age = 20
}
-- 动态添加键值对
user["addr"] = "anhui"
user.like = "game"
-- 删除键值对,赋值为nil
user.like = nil
-- 遍历
for key, value in pairs(user) do
    print(key, value)
end

-- 表混合
local test = {
    10, -- 隐藏的有一个键1
    20, -- 2
    30, -- 3
    name = "user"
}
for key, value in pairs(test) do
    print(key, value)
end
print(test[1])
```

# 流程控制

## 条件语句

```lua
local scope = 40
if scope > 20 then
    print(">20")
else
    if scope > 10 then
        print(">10")
    else
        print("<10")
    end
end
```

## 循环语句

```lua
local i = 1
local sum = 0
while i < 11 do
    sum = sum + i
    i = i + 1
end
local sum2 = 0
-- 起始值，结束值，步长
for j = 1, 10, 1 do
    sum2 = sum2 + j
end
print(sum, sum2)
```

## 流程跳转

> 用于中断或跳转代码执行，Lua支持`break`、`return`、`goto`
> 
> - 不等于用 `~=`
> 
> - lua没有`continue`
> 
> - 谨慎使用`goto`

```lua
for j = 1, 10, 1 do
    sum2 = sum2 + j
    ::end_loop::    -- 定义标签
end
```

# 函数

> lua中函数也能作为一个变量值被赋值给其它变量

```lua
-- 声明一个简单的局部函数
local function print_str()
    print("str")
end
print_str()
-- 声明一个有参数和返回值的函数
local function add(a, b)
    return a + b
end
local sum = add(2, 3)
print(sum)

-- 默认参数
local function default(name)
    -- 其实利用了逻辑中断
    name = name or "dd"
    print(name)
end
default()

-- 可变参数
local function add_plus(...)
    -- 先把参数放入表中
    local array = { ... }
    local sum = 0
    for _, value in pairs(array) do
        sum = sum + value
    end
    print(sum)
end
add_plus(1, 2, 3, 4, 5)

-- 多返回值
local function get_user()
    return "ding", 21
end
local name, age = get_user()
```

## 表方法和对象方法

> 1. 表方法
>    定义：直接绑定在表上的函数，本质是“表的键值对，值为函数”
>    
>    - 表是lua最核心的数据结构，任何函数都可以作为表的一个字段，这个函数就称为“表方法”
>    
>    - 表方法不强制依赖`self`，可以独立调用也可结合`self`使用
> 
> 2. 对象方法
>    定义：模拟面向对象编程时，给“对象（本质还是表）”绑定的、依赖`self`访问对象属性的表方法
>    
>    - lua中“对象”是通过模拟的（表包含属性+方法），“对象方法”就是这个模拟对象的方法
>    
>    - 必须依赖`self`（指向对象本身），用于访问对象的属性，是表方法的“面向对象化”用法

```lua
-- 表方法
local user = {
    name = "表方法",
    eat = function()
        print("eat")
    end,
    -- 必须显示声明出来self
    study = function(self)
        print(self.name .. "在学习")
    end
}
-- 调用
user.study(user)
user.eat()

-- 对象方法
local girl = {
    name = "yzz",
    age = 20,
}
-- 对象方法，默认包含self
function girl:study()
    print(self.name .. "在学习")
end

-- 调用
girl:study()

```

# 元表

> 元表本质是给普通table附加“自定义行为”的特殊table，比如table支持加减运算、自定义索引查找规则等
> 
> - 元表：一个特殊的table，用来定义另一个table（被元表修饰的table称为“原表”）的特殊行为
> 
> - 元方法：元表中以`__`开头的键（比如`__add`、`__index`），对应原表的自定义行为（比如`__add`定义加减运算）
> 
> - 核心函数：
>   
>   - `setmetable(t, mt)`：给table`t`设置元表`mt`（返回`t`）
>   
>   - `getmetable(t)`：获取table`t`的元表（返回元表或nil）

## __index（索引查找）

> 当访问原表中不存在的键时，lua会去元表的`__index`中查找，这是lua实现继承、默认值的核心
> 
> `__index`可以是table，也可以是函数

```lua
local info = {
    name = "ddw"
}

local obj = {
    name = "obj",
    mm = 100
}
-- 相当于实现了继承
local info1 = setmetatable(info, { __index = obj })
print(info == info1) -- true
print(info.name)
print(info.mm)       -- 会去obj中找

local user = {
    name = "张三"
}
setmetatable(user, {
    __index = function(table, key)
        print(table.name)
        return key
    end
})
print(user.name)
print("====")
print(user.name1) -- 找不到，执行__index指向的函数
```

## __newindex（索引赋值）

> 当给原表中不存在的键赋值时，lua会调用元表的`__newindex`，而非直接给原表添加键

```lua
local user = {
    name = "张三"
}
setmetatable(user, {
    __index = function(table, key)
        print(table.name)
        return key
    end,
    __newindex = function(table, key, value)
        print(table, key, value)
    end
})
user.age = 20 -- 执行print(table, key, value)
```

## 元表 -- 算数运算相关

```lua
local vec1 = { x = 1, y = 2 }
local vec2 = { x = 2, y = 4 }
local meta = {
    -- 加法
    __add = function(a, b)
        return { x = a.x + b.x, y = a.y + b.y }
    end,
    -- 减法
    __sub = function(a, b)
        return { x = a.x - b.x, y = a.y - b.y }
    end,
    -- 乘法
    __mul = function(a, b)
        return { x = a.x * b.x, y = a.y * b.y }
    end,
    -- 除法
    __div = function(a, b)
        return { x = a.x / b.x, y = a.y / b.y }
    end,
    -- 等于
    __eq = function(a, b)
        if a.x == b.x and a.y == b.y then
            return true
        else
            return false
        end
    end,
}
setmetatable(vec1, meta)
setmetatable(vec2, meta)
local vec = vec1 + vec2 -- {x=3, y=6}
print(vec.x, vec.y)
```

## __call

> 把table当成函数来执行

```lua
local user = {
    age = 20
}
setmetatable(user, {
    -- 参数table，是表本身，后面是传入的参数
    __call = function(table, ...)
        print(table)
        local arr = { ... }
        print(arr[1])
    end
})
user(1)
```

## __tostring

> 打印表时不是显示默认的内存地址，而是自己在该函数中定义的内容

```lua
local user = {
    age = 20
}
setmetatable(user, {
    __tostring = function(table)
        local str = ""
        for key, value in pairs(table) do
            str = str .. key .. "=" .. value .. ","
        end
        return "user{" .. str .. "}"
    end
})
print(user) -- 打印拼接后的字符串，而不是默认的内存地址
```

# 模块与包

> - 模块：一个lua文件就是一个模块，通常把一组相关的函数、变量、数据封装在这个文件里，对外暴露指定接口
> 
> - 包：多个功能相关的模块组成的集合（比如放在同一个目录下），lua中包的管理主要靠目录结构和`package.path`配置
> 
> lua模块的本质是：把模块内的功能挂载到一个table上，最后返回这个table，让外部能访问到指定功能（未挂载则为私有）

```lua
-- 定义模块
Mod = {}
Mod.add = function(a, b)
    return a + b
end
Mod.PI = 3.1415
return Mod

-- 导入模块
-- require后面是文件名
local mod = require("mymath")
print(mod.PI)
```

## package.path

> 找包的路径

```lua
print(package.path)
-- require会去package.path里的路径找包
-- 拼接一个路径上去，这样可以引入这个路径里的包
package.path = package.path .. ";C:/xx/"
```

# 内置库

> 内置库可以不用导入，直接使用

## math库

```lua
-- 向下取整
print(math.floor(2.3))
-- 随机数
print(math.random(10))
```

## string库

```lua
local name = "ddw lua"
local age = 22
-- 字符串格式化
local str1 = string.format("name:%s age:%d", name, age)
print(str1) -- name:ddw age:22

-- 截取子串，包括头尾，注意lua下标是从1开始的
print(string.sub(name, 1, 2)) -- dd

-- 查找子串的位置,返回两个值，开始位置和结束位置
print(string.find(name, "lua"))

-- 替换，将name中的lua替换成python，返回替换后的字符串和替换的个数
print(string.gsub(name, "lua", "python"))

-- 模式匹配，返回能匹配的子串
print(string.match(name, "d+"))
-- 返回一个迭代器，可以匹配多个值
local func = string.gmatch(name, "dd")
print("==gmatch==")
for key in func do
    print(key)
endch(name, "d+"))

-- 字符串对象方法
print(name:upper()) -- 转大写
print(name:lower()) -- 转小写
print(name:len())   -- 获取长度
```

## table

> Lua 的 `table` 库提供了一系列操作表的辅助函数，主要用于**将表作为数组（列表）进行管理**。它的核心功能包括**插入、删除、拼接和排序**等

```lua
local tb = { 1, 2, 3 }

-- 返回以插入分隔符列表的字符串
print(table.concat(tb, ",")) -- 1,2,3

-- 在末尾插入数据4
table.insert(tb, 4)
print(table.concat(tb, ",")) -- 1,2,3,4

-- 指定位置插入数据，1是位置，0是值
table.insert(tb, 1, 0)
print(table.concat(tb, ",")) -- 0,1,2,3,4

-- 移除数据，移除最后一个数据
table.remove(tb)
print(table.concat(tb, ",")) -- 0,1,2,3

-- 移除指定位置的数据
table.remove(tb, 1)
print(table.concat(tb, ",")) -- 1,2,3

local nums = { 9, 3, 23, 33, 5 }
-- 排序，升序
table.sort(nums)
print(table.concat(nums, ","))
-- 降序
table.sort(nums, function(a, b)
    return a > b
end)
print(table.concat(nums, ","))
```

## os库

> 获取系统信息、执行系统命令、处理时间等

```lua
-- 时间
print(os.date("%Y-%m-%d %H:%M:%S")) -- 026-06-13 19:26:04
-- 时间戳
print(os.time())

-- 获取环境变量
print(os.getenv("LUA_PATH"))

-- 执行系统命令
os.execute("ping baidu.com")

```

## io库

> 处理文件的读写、打开、关闭，分为“简单IO”和“文件句柄IO”

```lua
-- 简单io
local data = io.read() -- 接收终端输入
print(data)

-- 文件io
local f = io.open("./13.string.lua")
if f ~= nil then
    print("输出内容是" .. f:read()) -- 输出读取内容的一行
    f:close()
end

-- 写文件
local w = io.open("./hello.txt", "w")
if w then
    w:write("文件io")
    w:close()
end
```

# 协程（异步编程）

## 核心API

```lua
local co = coroutine.create(function()
    print("协程开始")
    -- 暂停协程，并将yield中的数据传递给resume（开启这个协程的）的返回值，这个例子中是result
    -- a是恢复这个协程的resume传递的参数，在这个例子中接收到的值是 world
    local a = coroutine.yield(100)
    print("收到参数", a)
    return "协程结束" -- 返回给resume的返回值
end)

print(coroutine.status(co)) --> suspended (刚创建，还没运行)

-- 启动协程
local ok, result = coroutine.resume(co, "hello")
-- 输出
print(ok, result) -- true    100

-- 恢复协程
local ok, result = coroutine.resume(co, "world")
-- 输出
print(ok, result)           -- true    协程结束

print(coroutine.status(co)) -- dead，协程执行结束
```

## API速查表

| 函数                          | 作用             | 返回值                                   |
| --------------------------- | -------------- | ------------------------------------- |
| `coroutine.create(f)`       | 创建协程           | 协程对象                                  |
| `coroutine.resume(co, ...)` | 启动/恢复协程        | `ok, result...`                       |
| `coroutine.yield(...)`      | 暂停协程，返回给resume | 传给resume的参数                           |
| `coroutine.status(co)`      | 获取状态           | "suspended"/"running"/"dead"/"normal" |
| `coroutine.running()`       | 返回当前正在运行的协程    | 协程对象（主线程返回nil）                        |
| `coroutine.wrap(f)`         | 创建协程，返回函数      | 每次调用该函数相当于resume                      |

## `wrap` 函数的使用（更简洁的写法）

```lua
local f = coroutine.wrap(function(...)
    print("步骤1")
    coroutine.yield(100)
    print("步骤2")
    return "完成"
end)

print(f()) -- 100
print(f()) -- 完成
```

**`wrap` vs `create/resume`**：

| 特性   | `create/resume`     | `wrap`             |
| ---- | ------------------- | ------------------ |
| 返回值  | 多返回值（第一个是状态码）       | 直接返回yield/return的值 |
| 错误处理 | 不会抛出异常，返回false+错误信息 | 出错时直接抛出异常          |
| 使用场景 | 需要精细控制错误处理          | 简洁调用，类似普通函数        |


