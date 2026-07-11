# python 基础

## 变量类型

### 列表、元组、字典

> 元组与列表相似，只是元组不能更改

```python
# 列表
list = [1, 2, 's']
# 元组
list_tuple = (1, 'q')
# 字典
dict = {'as':1, 'ad':'sde'}
```

### 判断数据类型

```python
type(23)    # 返回数据类型
str = 'sd'
type(str)
```

### 类型转换

| 函数     | 说明                  |
| -------- | --------------------- |
| int(x)   | 将 x 转换为一个整数   |
| float(x) | 将 x 转换为一个浮点值 |
| str(x)   | 将 x 转换为一个字符串 |
| bool(x)  | 将 x 转换为一个布尔值 |

## 运算符

### 算数运算符

| 运算符 | 描述   | 实例                  |
| ------ | ------ | --------------------- |
| +      | 加     | 两个对象相加 a+b      |
| -      | 减     | 两个对象相减 a-b      |
| \*     | 乘     | 两个对象相乘 a\*b     |
| /      | 除     | 两个对象相除 a/b      |
| //     | 取整除 | 返回商的整数部分      |
| %      | 取余   | 返回除法的余数        |
| \*\*   | 指数   | 2\*\*3 为 2 的 3 次方 |
| ()     | 小括号 | 提高优先级            |

### 逻辑运算符

| 运算符 | 逻辑表达式 | 描述 |
| ------ | ---------- | ---- |
| and    | x and y    | 与   |
| or     | x or y     | 或   |
| not    | not x      | 非   |

#### 逻辑运算符优化

**逻辑中断**

```python
a = 30
a > 10 and print('a')   # 打印a
a < 10 and print('a')   # 无打印
```

## 输出

**格式化输出**

```python
age = 18
name = 'ss'
print('my age is %d, my name is %s' % (age, name))
```

## 输入

```python
scr = input('请输入密码:')
```

## 字符串

### 字符串常见操作

- 获取长度：`len`
  例：`len('abc')`
- 查找内容：`find`
  例：`'abc'.find('a')`，返回第一个匹配项的下标
- 判断字符串是不是以谁开头/结尾
  例：`'abc'.startswith('a')` `'abc'.endswith('a')`，返回 bool 值
- 计算出现的次数：`count`
  例：`'aaaccc'.count('a')`，返回 int 值
- 替换内容：`replace`
  例：`'axxxa'.replace('x', 'o')`，返回替换后的字符串
- 切割字符串：`split`
  例：`'a_b_c'.split('_', 1)`，返回切割后的字符串列表，第二个参数是切割的最大次数（不写就是从所有分隔符处断开）
- 转大写：`upper`
  例：`'abc'.upper()`，返回大写字符串
- 转小写：`lower`
  例：`'ABC'.lower()`，返回小写字符串
- 去空格：`strip`
  例：`'  a  '.strip()`，返回没有空格的字符串

### 格式化字符串字面量

```py
content = input("请输入要检索的内容:")
url = f"https://www.sogou.com/web?query={content}"
```

## 列表

### 增

- 在列表末尾添加元素：`append`
  例：`list.append('d')`
- 在指定位置插入元素：`insert`
  例：`list.insert(1, 'x')`
- 合并两个列表：`extend`
  例：`list.extend(l1)`

### 查

- 查看指定元素是否存在列表中：`in 或 not in`

### 删

- 根据下标删除：`del`
  例：`del list[0]`
- 删除最后一个元素：`pop`
  例：`list.pop()`
- 根据元素的值进行删除：`remove`
  例：`list.remove('s')`

## 切片

> 切片是指对操作的对象截取其中一部分的操作。`字符串`、`列表`、`元组`都支持切片操作

- 切片语法：`[起始:结束:步长]`，也可以简化为`[起始:结束]`  
  _选取区间不包含结束位的元素_
  例：对列表：`list[0:6:2]`
  对字符串：`'abc'[0:3:1]`

## 字典

### 查

> 用`get`方法更安全

```python
person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
# 如果[]内的值不存在则会报错
print(person['age'])
# 如果用get的方法访问不存在的键则返回none
person.get('age')
```

### 改

> 注意：如果要改的键不存在，则会向字典中添加键值对

```python
person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
person['age'] = 19
```

### 添加

> 注意：如果要添加的键存在，则会修改字典中的键值对

```python
person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
person['country'] = 'china'
```

### 删除

```python
person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
# 删除指定键值对
del person['gender']
# 删除整个字典，字典对象不存在了
del person
# 清除整个字典，字典对象依旧存在
person.clear()
```

### 遍历

- 遍历字典的 key：
  例：
  ```python
  person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
  for key in person.keys():
    print(key)
  ```
- 遍历字典的 value：
  例：
  ```python
  person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
  for value in person.values():
    print(value)
  ```
- 同时遍历 key 和 value：
  例：
  ```python
  person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
  for key,value in person.items():
    print(key, value)
  ```
- 遍历字典中的元素：
  例：
  ```python
  person = {'age': 18, 'name': 'ddw', 'gender': 'male'}
  for item in person.items():
    print(key, value)
  ```

## 函数

### 定义函数

```python
def func_name():
    # 函数体
```

## 文件

### 文件的打开和关闭

> 在 python，使用`open`函数，可以打开一个已经存在的文件，或者创建一个新文件（如果要打开的文件不存在）

```python
# 打开文件
f = open('文件路径', '访问模式')
# 关闭文件
f.close()
```

| 访问模式 | 描述               |
| -------- | ------------------ |
| r        | 对打开的文件只读   |
| w        | 对打开的文件覆盖写 |
| a        | 对打开的文件追加写 |

### 文件的读写

```python
# 打开文件
f = open('文件路径', '访问模式')
# 写入文件
f.write('nihao')
# 读数据
# read是一字节一字节的读取
f.read()
# 读取一行
f.readline()
# 一行一行的读取，以列表的方式返回，列表中包含所有行
f.readlines()
# 关闭文件
f.close()
```

### 序列化和反序列化

> 可以用`write`把字符串写入本地文件，但是无法写入一个对象（如列表、字典、元组等），可以通过将对象序列化成字符串就可以写入本地文件
> python 中 JSON 这个模块用来实现序列化和反序列化

1. 序列化

```python
import json
name_list = ['name']
names = json.dumps(name_list)     # names是字符串
#dump在序列化时可以同时指定文件对象，将序列化的结果写入文件对象
json.dump(name_list, fp)
```

2. 反序列化

```python
result = json.loads(content) # 将序列化字符串反序列成对象
```

## 异常

> 用 try 和 expect 捕获异常，except 后面是异常类型，可以输入 Error 来看用哪些异常类型

```python
try:
    fp = open('a.txt', 'r')
except FileNotFoundError:
    print('sd')
```

## 并发编程

### 多线程

```py
# 导入Thread类
from threading import Thread

def func(name):
    for i in range(1000):
        print("func", i, name)

# 创建多线程，线程传入要执行的任务
t = Thread(target=func, args=("周杰伦",))   # 传入参数必须传入元组，只有一个参数，必须带逗号
t.start()   # 多线程状态为开始工作状态，具体的执行时间由cpu决定
for i in range(100):
    print("main", i)
```

### 多进程

```py
# 导入Process类
from multiprocessing import Process

def func():
    for i in range(1000):
        print("子进程", i)

if __name__ == '__main__':
    p = Process(target=func)
    p.start()
    for i in range(100):
        print("main", i)
```

### 进程池和线程池

> 一次性开辟一定数量的线程，后面直接复用这些线程

```py
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

def fn(name, n2):
    for i in range(1000):
        print(name, i, n2)

with ThreadPoolExecutor(5) as t:
    for i in range(10):
        t.submit(fn, name=f"线程{i}", n2=f"执行{i}")

# with外的任务会等待线程池中所有任务执行完毕后才会执行
print("123")

# 使用进程池，把ThreadPoolExecutor换成ProcessPoolExecutor就行
```

### 协程 --- 异步编程

> `asyncio`是一个自带的协程库

> 使用`async`修饰的函数的运行结果是一个协程对象

```py
async def func():
    print("协程")
g = func()  # 此时的函数是异步协程函数，此时函数执行得到的是一个协程对象
print(g)  # 打印结果：<coroutine object func at 0x000001C3BFB12BC0>
```

> python 的协程也有事件机制，用于管理协程对象

```py
import asyncio

async def func():
    print("协程")

g = func()  # 此时的函数是异步协程函数，此时函数执行得到的是一个协程对象
print(g)
# asyncio.run 会建立事件循环，会为传入的协程对象创建任务
asyncio.run(g)  # 协程的运行需要使用asyncio模块，run方法需要一个协程对象参数
```

> python 的协程有点像 js 的非阻塞，当程序遇见阻塞时(如：IO 操作时)，可以选择切换到其它任务上

> python 协程中的`await`只能用于修饰一些内容(列的不全)，`await`会等待被它修饰的代码完成，它后面的代码会暂停执行，事件循环会调度其它协程对象执行。

- 协程对象
- asyncio.Future
- Task
- 实现 `__await__()` 的对象

> 被`await`修饰的代码会有如下动作，为被修饰的协程对象创建任务并纳入事件循环，本协程对象的运行权会转让给被修饰的协程对象，协程对象的返回值必须通过`await`才能拿到

```py
# 下面演示一下的协程需要执行3s多
async def fun1(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    print(f"started at {time.time()}")
    await fun1(1, 'hello')  # 需要等待 1s多 这个执行完才能运行下面，此时事件循环中没有其它任务，只能等待
    await fun1(2, 'world')  # 执行完需要 2s多
    print("dds")

asyncio.run(main())
```

> `create_task`:是`asyncio`提供的一个方法，可以为协程对象创建任务并立即执行且纳入事件循环，如果不用`await`或`gather`事件循环不会等待任何任务执行完

```py
async def fun1(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main2():
    task1 = asyncio.create_task(fun1(1, "hello"))
    task2 = asyncio.create_task(fun1(2, "world"))
    t1 = time.time()
    # 此时等待task1执行，因为在事件循环中有task2，所以去执行task2。task2也要等待，所以再去找task1(一直循环执行，看谁先醒来)
    await task1
    # task1的睡眠时间短，task1睡眠1s此时task2也睡眠了1s，只要等待1s就行了
    await task2
    print(f"执行时间{time.time() - t1}")

asyncio.run(main2())
```

> `asyncio.gather`:用于接受多个 task 来一次性接收多个返回值。也可以接受协程对象，此时会将接收到的协程对象都创建为任务并立即执行

```py
import asyncio
import time

async def fun1(delay, what):
    await asyncio.sleep(delay)
    print(what)
    return f"{what} - {delay}"

async def main3():
    t1 = time.time()
    task1 = asyncio.create_task(fun1(1, "hello"))
    task2 = asyncio.create_task(fun1(2, "world"))

    ret = await asyncio.gather(task1, task2)

    print(ret)

# main4与main3是一模一样的
async def main4():
    t1 = time.time()
    ret = await asyncio.gather(fun1(1, "hello"), fun1(2, "world"))
    print(ret)

asyncio.run(main3())
```

## python 虚拟环境

> `python` 用 `pip` 安装第三方包时默认时全局安装，如果不同项目需要同一个库的不同版本会导致冲突。可以创建一个虚拟环境，虚拟环境就像一个独立的“沙盒”，它拥有自己独立的 `Python` 解释器和包目录，与全局环境完全隔离。

```py
# 1. 在项目目录下创建虚拟环境，环境文件夹通常命名为 `venv` 或 `.venv`
python -m venv venv

# 2. 激活虚拟环境
# Linux/macOS:
source venv/bin/activate
# Windows:
venv\Scripts\activate

# 3. 激活后，你的命令行提示符前通常会显示环境名 (venv)
# 此时，使用 pip 安装的包都是局部安装到这个虚拟环境中的
(venv) pip install django

# 4. 工作完成后，退出虚拟环境
deactivate
```

## pip 下载加速

`pip install 包名 -i https://pypi.tuna.tsinghua.edu.cn/simple`

# 爬虫

> 有些网站会将数据和 html 一起发过来，有些则是先发 html 在通过 js(ajax(xhr))请求数据

## requests 模块

> requests 是一个第三方模块，常用的抓取页面的模块

### requests 安装

> pip install requests

### requests 简单使用 -- get

#### 1. 没有参数的 get

```py
import requests

# r是获取的响应对象
r = requests.get("http://www.baidu.com")
# 设置字符集
r.encoding = "utf-8"
# 打印获取页面源代码
print(r.text)
# 将“响应结果”由json转成字典格式(如果响应结果是json的话)
print(r.json())
```

#### 2. 有参数的 get

```py
import requests

url = "https://movie.douban.com/j/chart/top_list"

query = {
    "type":"13",
    "interval_id":"100:90",
    "action":"",
    "start": "0",
    "limit": "20"
}

headers = {
    "user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0"
}

resp = requests.get(url, params=query, headers=headers)

print(resp.text)
```

### requests 简单使用 -- post

```py
import requests

url = "https://fanyi.baidu.com/sug"


data = {
    "kw": input("请输入一个单词:")
}

resp = requests.post(url=url, data=data)
# 将结果由json转成字典格式
print(resp.json())
```

## 异步 http 请求模块 --- aiohttp

> `aiohttp`是一个第三方库，需要安装`pip install aiohttp`

## 数据解析

### 1. re 解析 --- 正则

> `re`是内置模块，用于正则

**re 模块的使用**

```py
import re

# 匹配所有结果
res = re.findall(r"\d+", "woa9dji80")  # 返回匹配结果数组

# 匹配所有符合的结果，这个用的比较多
result = re.finditer(r"\d+", "woa9dji80")    # 返回一个迭代器
for item in result:
    print(item.group()) # 用group方法拿到结果
    print(f"开始位置:{item.start()}") # 获取匹配结果在字符串开始的位置
    print(f"结束位置:{item.end()}")   # 获取匹配结果在字符串结束的位置

# 只匹配第一个结果，返回迭代器
res2 = re.search(r"\d+", "woa9dji80")
print(res2.group())

# 预加载，提前把正则对象加载完毕，(定义正则对象)
obj = re.compile(r"\d+")
obj.findall("woa9dji80")    # 结果与res一样

# 可以用re.S来让.匹配换行
bj = re.compile(r'<div class="item">.*?<span class="title">', re.S)
```

**re 提取分组数据**

```py
s = """
<div class='西游记'><span id='10010'>中国联通</span></div>
<div class='西游记'><span id='10086'>中国移动</span></div>
"""

reobj1 = re.compile(r"<span id='\d+'>.*?</span>")
# 匹配结果 ["<span id='10010'>中国联通</span>", "<span id='10086'>中国移动</span>"]
print(reobj1.findall(s))

reobj2 = re.compile(r"<span id='(\d+)'>(.*?)</span>")
# 匹配结果 [('10010', '中国联通'), ('10086', '中国移动')]
print(reobj2.findall(s))

#进行分组，使用(?P<名字>正则表达式) 进行分组
# 第一个数字匹配结果会分入id组(10010 和 10086)，第二个匹配结果会分入name组(中国联通 和 中国移动)
reobj3 = re.compile(r"<span id='(?P<id>\d+)'>(?P<name>.*?)</span>")
# 只能使用finditer
result2 = reobj3.finditer(s)
for item in result2:
    print(item.group("id")) # 输出10010和10086
    print(item.group("name"))   # 输出 中国联通 中国移动
```

#### 正则表达式

1. 元字符
   | 符号 | 作用 |
   | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | . | 匹配除换行符 \n 之外的任何单字符 |
   | \w | 匹配字母、数字、下划线 |
   | \s | \s 是匹配所有空白符，包括换行 |
   | \d | 匹配任意一个阿拉伯数字（0 到 9）。等价于 [0-9] |
   | \n | 匹配一个换行符。等价于 \x0a 和 \cJ |
   | \t | 匹配一个制表符。等价于 \x09 和 \cI。 |
   | ^ | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
   | $ | 匹配输入字符串的结尾位置。要匹配 $ 字符本身，请使用 \$。 |
   | a\|b | 匹配字符 a 或 b |
   | ( ) | 匹配括号内的表达式，也表示一个组 |
   | [...] | 匹配字符组中的字符，这个匹配的是一位字符 |
   | [^...] | 匹配除了字符组中的所有字符 |
2. 量词
   |符号|作用|
   |---|---|
   |\*| 匹配前面的子表达式零次或多次。要匹配 \* 字符，请使用 \\\*。例如，zo\* 能匹配 "z" 以及 "zoo"。|
   |+|匹配前面的子表达式一次或多次。例如，zo+ 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。|
   |?|匹配前面的子表达式零次或一次。例如，do(es)? 可以匹配 "do" 、 "does"、 "doxy" 中的 "do" 和 "does"。|
   |{n}|n 是一个非负整数。匹配确定的 n 次。例如，o{2} 不能匹配 "Bob" 中的 o，但是能匹配 "food" 中的两个 o。|
   |{n,}|n 是一个非负整数。至少匹配 n 次。例如，o{2,} 不能匹配 "Bob" 中的 o，但能匹配 "foooood" 中的所有 o。|
   |{n,m}|m 和 n 均为非负整数，其中 n <= m。最少匹配 n 次且最多匹配 m 次。例如，o{1,3} 将匹配 "fooooood" 中的前三个 o。o{0,1} 等价于 o?。请注意在逗号和两个数之间不能有空格。|
3. 贪婪匹配和惰性匹配
   `.*`：贪婪匹配
   `.*?`：惰性匹配
   效果展示：字符串 `<div><div>苹果</div><div>橘子</div><div>香蕉</div></div>`
   贪婪匹配`<div>.*</div>`结果：`<div><div>苹果</div><div>橘子</div><div>香蕉</div></div>`
   惰性匹配`<div>.*?</div>`结果：`<div><div>苹果</div>`、`<div>橘子</div>`、`<div>香蕉</div>`

### 2. bs4 解析 ---- BeautifulSoup

> bs4 是一个第三方模块，需要安装，主要是使用 BeautifulSoup

安装：`pip install bs4`

#### BeautifulSoup 的使用

```py
from bs4 import BeautifulSoup

html = """
<ul>
  <li><a href="zhangwuji.com">张无忌</a></li>
  <li id="abc"><a href="zhouxingchi.com">周星驰</a></li>
  <li><a href="zhubajie.com">猪八戒</a></li>
  <li><a href="wuzetian.com">武则天</a></li>
</ul>
"""
# 初始化BeautifulSoup对象，html是html文本，第二个参数是选择html解析器
page = BeautifulSoup(html, "html.parser")
# 查找某个元素，只找一个，获取的结果是一个html元素，获取的结果可以用find或其它BeautifulSoup对象方法
page.find("标签名", attrs={"属性":"值"})
# 查找全部符合条件的元素，获取的结果是一个html元素列表
page.find_all("标签名", attrs={"属性":"值"})
# 获取标签内的文本
page.text
# 获取标签属性
page.get("href")
```

### 3. xpath 解析

> Xpath 是一门在 xml 文档中查找信息的语言。xpath 可以用来在 xml 文档中对元素和属性进行遍历，html 属于 xml 的一个子集，所有可以用 xpath 去查找 html 中内容

> xpath 是一个第三方库，需要安装

安装：`pip install lxml`

==这一部分我跳过了，感觉没有 pyquery 好用==

### 4. pyquery 解析

> pyquery 是一个第三方库，仿照 jquery 写，需要安装

安装：`pip install pyquery`

#### pyquery 使用

```py
from pyquery import PyQuery

html = """
<ul>
  <li><a href="zhangwuji.com">张无忌</a></li>
  <li id="abc"><a href="zhouxingchi.com">周星驰</a></li>
  <li><a href="zhubajie.com">猪八戒</a></li>
  <li><a href="wuzetian.com">武则天</a></li>
</ul>
"""

p = PyQuery(html)
# p是一个PyQuery对象
print(type(p))
# el = p("css选择器")
a = p("#abc a")
print(a)
# 获取属性
href = a.attr("href")
print(href)
# 获取标签内文本
txt = a.text()

# 如果符合选择器的元素有多个，可以用方法items返回一个迭代器
a_s = p("li a").items()
print(a_s)
for a in a_s:
    print(a.text())


p2 = PyQuery(html)
# 返回标签内部的html
p2("#abc").html()
# 返回标签内部的文本，标签被过滤掉了
p2("#abc").text()
```

#### pyquery 改变 html 结构

下面仅做一个操作，其余查询文档

```py
from pyquery import PyQuery

html = """
<ul>
  <li><a href="zhangwuji.com">张无忌</a></li>
  <li id="abc"><a href="zhouxingchi.com">周星驰</a></li>
  <li><a href="zhubajie.com">猪八戒</a></li>
  <li><a href="wuzetian.com">武则天</a></li>
</ul>
"""
p3 = PyQuery(html)
p3("#abc").after("<div>div</div>")
print(p3)
#<ul>
#  <li><a href="zhangwuji.com">张无忌</a></li>
#  <li id="abc"><a href="zhouxingchi.com">周星驰</a></li>
#  <div>div</div><li><a href="zhubajie.com">猪八戒</a></li>
#  <li><a href="wuzetian.com">武则天</a></li>
#</ul>
```

## Selenium

> 这是一个自动化工具，需要安装:`pip install selenium`，安装是不带浏览器的，需要自己安装浏览器或指定浏览器

### Selenium 简单使用

==selenium 4.6+以上自动下载浏览器驱动，演示的版本是 4.6+的，但是自动下载会有网络问题还是手动下载好==

#### 1. 创建浏览器对象，并打开一个网页

```py
# 导入
from selenium import webdriver
# 用于设置火狐浏览器
from selenium.webdriver.firefox.options import Options

# 创建浏览器设置对象
q = Options()
# 禁用浏览器沙盒模式（增加兼容性的问题）
q.add_argument('--no-sandbox')
# 保持浏览器打开状态（默认是代码执行完毕自动关闭）。火狐没有这个设置，执行完默认不关闭
# q.add_experimental_option('detach', True)
# 指定浏览器位置
q.binary_location = "C:\\Program Files\\Mozilla Firefox\\firefox.exe"

# 创建浏览器对象，executable_path是浏览器驱动位置，并传入设置对象
firefox = webdriver.Firefox(executable_path="./geckodriver.exe",options=q)
# 打开网页是阻塞的，网页没有加载完成不会执行下面的代码
firefox.get('http://www.baidu.com')
```

#### 2. 打开网页，关闭当前标签页，退出浏览器

```py
# 导入
from selenium import webdriver
# 用于设置火狐浏览器
from selenium.webdriver.firefox.options import Options

import time

# 创建浏览器设置对象
q = Options()
# 禁用浏览器沙盒模式（增加兼容性的问题）
q.add_argument('--no-sandbox')
# 保持浏览器打开状态（默认是代码执行完毕自动关闭）。火狐没有这个设置，执行完默认不关闭
# q.add_experimental_option('detach', True)
# 指定浏览器位置
q.binary_location = "C:\\Program Files\\Mozilla Firefox\\firefox.exe"

# 创建浏览器对象，executable_path是浏览器驱动位置，并传入设置对象
firefox = webdriver.Firefox(executable_path="./geckodriver.exe",options=q)

firefox.get('http://www.baidu.com')

time.sleep(3)
# 关闭当前标签页
firefox.close()
time.sleep(2)
# 退出浏览器
firefox.quit()
```

#### 3. 浏览器对象属性和方法

> `浏览器最大化`、`浏览器最小化`、`设置浏览器显示位置`、`设置浏览器窗口大小`

```py
# 最大化
firefox.maximize_window()
# 最小化
firefox.minimize_window()

# 浏览器打开位置，第一个参数是离屏幕左侧的距离，第二个参数是离屏幕顶部的距离
firefox.set_window_position(200, 100)
# 设置浏览器窗口大小，第一个参数是宽，第二个是高
firefox.set_window_size(600, 200)

# 浏览器截图，参数是截图保存位置
firefox.get_screenshot_as_file('1.png')
# 刷新当前网页
firefox.refresh()
```

#### 4. html 元素的操作

##### 1. 获取 html 元素

```py

```

##### 2. 元素的交互操作

1. 元素的输入

```py
# 元素输入，el为html元素
el.send_keys('要输入的内容')
```

2. 元素的清空

```py
# 元素输入，el为html元素
el.send_keys('要输入的内容')
# 元素清空(例如将上面的输入内容清空)
el.clear()
```

3. 元素的点击

```py
# 元素点击
el.click()
```

## 简单应对反爬

> 这里主要是针对的 header 中的反爬手段

### 设置 cookie

### 防盗链

> 就是请求头中的`Referer`，这个属性的值是请求来源地址，有时要带上，且数据最终地址可能根这个地址有关

```py
headers= {
    "user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36 Edg/142.0.0.0",
    "Referer": "https://www.pearvideo.com/video_1803909"
}
```

### 代理

> 用于隐藏自己的真实 ip

```py
import requests

url = "http://www.baidu.com"

proxy = {
    "http": "http://xxx.xxx.xxx.xxx:xx",
    "https": "https://xxx.xxx.xxx.xxx:xx"
}

resp = requests.get(url=url, proxies=proxy)
```

## 加密

### MD5 加密逻辑

> `MD5`是一个非常常见的加密逻辑，其特点就是小巧、速度快、不可逆

```python
from hashlib import md5 # md5是一个哈希算法

# 哈希算法不可逆

# 加密器
obj = md5()
# 准备要加密的内容，防止被撞库，内容应该要长
passwd = "ding"
obj.update(passwd.encode("utf-8"))
# 获取到密文
mi = obj.hexdigest()
print(mi) # c2eaa4c670cd4c1c537b2a2ae4fbeb5d

# 防止被撞库，可以添加密钥增加破解难度
s = b'dsfwegergergerg'
obj2 = md5(s)
obj2.update(passwd.encode("utf-8"))
print(obj2.hexdigest()) # 128374fa8ee091d4cfcf33aa7682ae2a
```

### sha256

> 不论是 sha1、sha256、md5 都属于摘要算法。都是在计算 hash 值。只是散列的程度不同而已。都使用 hash 算法，所以不存在解密的逻辑。

> sha1、sha256 与 md5 的使用完全一样，下面仅作 sha256 演示

```py
from hashlib import md5, sha256 # md5是一个哈希算法
sha = sha256()
sha.update(b"ding")
print(sha.hexdigest()) # ca46a3f7dabfcc476e496bf8b5100d6091734186c6cdd1c50d132ff61e0f4b7c
```

### URLEncode 和 Base64

> 在访问一个 url 的时候，浏览器会自动的进行`URLEncode`操作。会对我们请求的 url 进行编码，这种编码规则被称为百分号编码，是专门为 url 准备的一套编码规则
> 其规则是把 url 中的参数部分转化为字节，每个字节再转化为 1 个 16 进制的数字，前面补%

```py

```

> Base64 是一种将二进制数据转换为可打印 ASCII 字符的编码方式，主要作用是在文本协议中安全地传输或存储二进制数据。`base64`利用 26 个大写字母+26 个小写字母+10 个数字+2 个特殊符号(+和/)组成了一组类似 64 进制的计算逻辑。

> 通常内容被加密后的结果是以字节形式呈现的，但是在 http 协议中想要传输字节是很麻烦的。如果传递的是字符串就好控制的多。

```py
import base64
bs = 'dnidjfrewjf'.encode("utf-8")
print(bs)   # 输出字节格式的dnidjfrewjf
print(base64.b64encode(bs)) # 输出字节格式的base64编码
s = base64.b64encode(bs).decode("utf-8")
print(s) # 输出字符串格式的base64编码 ZG5pZGpmcmV3amY=

# 解码成字节
cs = base64.b64decode(s) # 转成utf-8格式的字节
print(cs)
s_s = cs.decode("utf-8")  # 恢复成原字符串
print(s_s)
```

### 对称加密 -- AES 和 DES

> 对称加密就是加密和解密用的是同一个密钥，常见的对称加密:`AES`,`DES`,`3DES`

- 加密：密文 = Encrypt(明文, 密钥)
- 解密：明文 = Decrypt(密文, 密钥)

> 非对称加密算法，常见的有：`RSA`

- 使用公钥加密，使用私钥解密
- 公钥是公开的，私钥保密
- 加密处理安全，但是性能极差，单次加密长度有限制

## 反爬虫

### 艺术字体加密

> 页面中显示的数据是 css 渲染出来的字体(例如：woff 字体)，本身不可读，或者本身可读但是可读数据与渲染数据不一致(这是因为可读数据与渲染数据存在映射表)

> 解决思路：本身不可读，说明 html 页面中的数据就是艺术字体(例如：woff 字体)，可以把它爬下来写入文件内然后利用 fonttools(python 库)和

### 图片验证码

#### 第一代图片验证码

# js 逆向

## js 数据加密

### 响应数据加密

> 处理思路：响应数据被加密一定会被解密(否则没法显示)，可以使用`xhr断点`方式找到发送 `xhr`请求的文件，在这个文件中找解密函数。因为响应数据是`json`格式，所以在进行解密之前一定会用`JSON.parse`把`json`转成对象，可以通过搜索`JSON.parse`或`JSON`字样来进行定位。

> `xhr断点`使用方式：找到响应数据的 url，在源代码页面的右侧栏有 XHR/提取断点

### 浏览器环境 base64 加密

> 加密：btoa('str') ==> 密文
> 解密：atob('密文') ==> str

### 单向散列函数(消息摘要算法)

- 加密后的密文定长
- 明文不一样，散列后结果一定不一样
- 不可逆
- 一般用于签名 sign

| 加密方法   | 位数  |
| ---------- | ----- |
| MD5        | 32 位 |
| SHA1       | 40 位 |
| SHA256     |       |
| SHA512     |       |
| HmacMD5    |       |
| HmacSHA1   |       |
| HmacSHA256 |       |

### crypto-js 库

> 用途：轻量级前端/后端加密，包含以下加密方式

- AES / DES / TripleDES
- SHA1 / SHA256 / SHA512
- HMAC
- Base64 / Hex

```js
const cryptojs = require("crypto-js");
const hash = cryptojs.MD5("1");
console.log(hash.toString());
console.log(cryptojs.SHA1("1").toString());

const data = "helloworld";
const key = "123456789";

// AES加密与解密
const encrypted = cryptojs.AES.encrypt(data, key).toString();
console.log(encrypted);
const decrypted = cryptojs.AES.decrypt(encrypted, key).toString(
  cryptojs.enc.Utf8
);
console.log(decrypted);
```

## eval 函数

> `eval()`函数计算 js 字符串，并把它作为脚本代码来执行，返回值是表达式计算的结果。如果参数是一个表达式，`eval()` 函数将执行表达式。如果参数是 js 语句将执行 js 语句。

```js
eval(string);
// 例
let a = { i: 0 };
eval(`a.i = 1`);
console.log(a.i); // 1

console.log(eval(`1 === 1`)); // true
```

## Hook 操作

> 在 js 逆向中，通常把替换原函数的过程都称为 Hook，替换的时机是程序运行时但未运行到要替换的函数。

```js
// 这个目的是找到谁在调用test()，断住时去调用栈查看是谁在调用
let _test = test;
function test() {
  debugger; // debugger是程序性调试断点
  _test();
}
```

> 当调用 Hook 函数的无关函数太多时，不利于找到目标函数，可以使用判断来找目标函数。
> 例如：有一个函数生成 json 字符串(这个函数大概率会使用 JSON.stringify)，想要找到这个函数，可以使用下面 Hook

==注：`JSON`格式的数据在传输时是以字符串格式传输的，一定会使用 `JSON.stringify` 和 `JSON.parse`==

```js
let stringify = JSON.stringify;
JSON.stringify = function (params) {
  // 当参数params中有payload属性时才中断
  if (params["payload"]) {
    debugger;
  }
  return stringify(params);
};
```

==注：加密解密也会用到`JSON.stringify` 和 `JSON.parse`，因为需要字符串，解密得到的结果是字符串==

### Hook Cookie 操作

> 一般使用`Object.defineProperty()`来进行属性操作的 hook。可以用`Object.defineProperty()`监控特定对象。

```js
// Object.defineProperty() 使用介绍
let a = { i: 0 };
// defineProperty接收三个参数：第一个目标对象，第二个目标对象对应的属性，第三个配置
Object.defineProperty(a, "i", {
  // 当对a.i进行查询操作时调用get方法
  get() {
    console.log("get");
  },
  // 当对a.i进行设置操作时调用set方法
  set(v) {
    console.log(v);
  },
});
```

```js
// 当设置请求头的xxx属性时debugger
(function () {
  let set_header = window.XMLHttpRequest.prototype.setRequestHeader;
  window.XMLHttpRequest.prototype.setRequestHeader = function (header, val) {
    if (header.toUpperCase() === "xxx".toUpperCase()) {
      debugger;
    }
    return set_header.apply(this, header, val);
  };
})();
```

## 无限 debugger

1. 无限 debugger 实现原理 --- 递归调用
   > 在检测到适合的条件时，递归调用 debugger
   ```js
   if (flag === true) {
     // 也可以用定时器
     setTimeout(function foo() {
       debugger;
       console.log("foo");
       setTimeout(foo, 1000);
     }, 1000);
   }
   ```
2. 调用 debugger 的方式 ---- 通过 function 的原型链

### 解决无限 debugger 的思路

1. 停用断点或者 false 条件断点
2. 文件覆盖
3. hook 钩子，重写关键方法，比如：定时器、构造器、eval 等
