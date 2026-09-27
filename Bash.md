# 简介





# 命令

## 命令格式

> 命令行环境中，主要通过使用 Shell 命令，进行各种操作。Shell 命令基本都是下面的格式。

```bash
$ command [ arg1 ... [ argN ]]
```

> 上面代码中，`command`是具体的命令或者一个可执行文件，`arg1 ... argN`是传递给命令的参数，它们是可选的。参数以空格隔开。

```bash
# ls是命令 -l是参数
ls -l
```

> 有些参数是命令的配置项，这些配置项一般都以一个连词线开头，比如上面的`-l`。同一个配置项往往有长和短两种形式，比如`-r`是短形式，`--reverse`是长形式，它们的作用完全相同。

> Bash 单个命令一般都是一行，用户按下回车键，就开始执行。有些命令比较长，写成多行会有利于阅读和编辑，这时可以在每一行的结尾加上反斜杠，Bash 就会将下一行跟当前行放在一起解释。

```bash
echo foo \
> bar
foo bar
```

## 分号

> 分号（`;`）是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令。

```bash
clear; ls
```

> 上面例子中，Bash 先执行`clear`命令，执行完成后，再执行`ls`命令。
> 
> 注意，使用分号时，第二个命令总是接着第一个命令执行，不管第一个命令执行成功或失败。

## 命令的组合符`&&`和`||`

> `Command1 && Command2`：如果`Command1`命令运行成功，则继续运行`Command2`命令
> 
> `Command1 || Command2`：如果`Command1`命令运行失败，则继续运行`Command2`命令

```bash
# 只有mkdir foo失败才执行mkdir bar
mkdir foo || mkdir bar
```

# 常用命令

## echo命令

> `echo`命令的作用是在屏幕输出一行文本，可以将该命令的参数原样输出。

```bash
echo hello world
hello world
```

> 如果想要输出的是多行文本，即包括换行符。这时就需要把多行文本放在引号里面。

```bash
echo "<HTML>
    <HEAD>
          <TITLE>Page Title</TITLE>
    </HEAD>
    <BODY>
          Page body.
    </BODY>
</HTML>"
```

**参数**

> 这里只介绍两种参数：
> 
> - `-n`参数：可以取消末尾的回车符，使得下一个提示符紧跟在输出内容的后面。
> 
> - `-e`参数：释引号（双引号和单引号）里面的特殊字符（比如换行符`\n`）。如果不使用`-e`参数，即默认情况下，引号会让特殊字符变成普通字符，`echo`不解释它们，原样输出。

```bash
# -n 参数
$ echo -n hello world
hello world$

# -e 参数
$ echo -e "Hello\nWorld"
Hello
World
```

## type命令

> Bash 本身内置了很多命令，同时也可以执行外部程序。怎么知道一个命令是内置命令，还是外部程序呢？
> 
> `type`命令用来判断命令的来源。

```bash
$ type echo
echo is a shell builtin
$ type ls
ls is hashed (/bin/ls)
```

**`-a`参数**

> 如果要查看一个命令的所有定义，可以使用`type`命令的`-a`参数。

```bash
$ type -a echo
echo is shell builtin
echo is /usr/bin/echo
echo is /bin/echo
```

**`-t`参数**

> `type`命令的`-t`参数，可以返回一个命令的类型：别名（alias），关键词（keyword），函数（function），内置命令（builtin）和文件（file）。

```bash
$ type -t bash
file
$ type -t if
keyword
```

# 模式扩展

> Shell 接收到用户输入的命令以后，会根据空格将用户的输入，拆分成一个个词元（token）。然后，Shell 会扩展词元里面的特殊字符，扩展完成后才会调用相应的命令。
> 
> 这种特殊字符的扩展，称为模式扩展（globbing）
> 
> Bash 一共提供八种扩展:
> 
> * 波浪线扩展
> * `?` 字符扩展
> * `*` 字符扩展
> * `[]`方括号扩展
> * `{}`大括号扩展
> * 变量扩展
> * 子命令扩展
> * 算术扩展
> 
> <mark>Bash 是先进行扩展，再执行命令。因此，扩展的结果是由 Bash 负责的，与所要执行的命令无关。命令本身并不存在参数扩展，收到什么参数就原样执行。</mark>

**关闭扩展**

```bash
$ set -o noglob
# 或者
$ set -f
```

**打开扩展**

```bash
$ set +o noglob
# 或者
$ set +f
```

## 波浪线扩展

> 波浪线`~`会自动扩展成当前用户的主目录。

```shell
$ echo ~
/home/me
```

> `~/dir`表示扩展成主目录的某个子目录，`dir`是主目录里面的一个子目录名。

```bash
# 进入 /home/me/foo 目录
$ cd ~/foo
```

> `~user`表示扩展成用户`user`的主目录。
> 
> 如果`~user`的`user`是不存在的用户名，则波浪号扩展不起作用。

```bash
$ echo ~foo
/home/foo
```

`?` 字符扩展
--------

> `?`字符代表文件路径里面的任意单个字符，不包括空字符。比如，`Data???`匹配所有`Data`后面跟着三个字符的文件名。

```bash
# 存在文件 a.txt 和 b.txt
$ ls ?.txt
a.txt b.txt
```

> `?` 字符扩展属于文件名(目录名)扩展，只有文件确实存在的前提下，才会发生扩展。如果文件不存在，扩展就不会发生。

```bash
# 当前为空目录
cat ??.txt
cat: '??.txt': No such file or directory
```

`*` 字符扩展
--------

> `*`字符代表文件路径里面的任意数量的任意字符，包括零个字符

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls *.txt
a.txt b.txt ab.txt
```

> 如果想输出当前目录的所有文件，直接用`*`即可。

```bash
ls *
# 匹配隐藏文件，需要写成.*
echo .*
```

> 注意，`*`不会匹配隐藏文件（以`.`开头的文件），即`ls *`不会输出隐藏文件

> 注意，`*`字符扩展属于文件名扩展，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。

```bash
# 当前目录不存在 c 开头的文件
$ echo c*.txt
c*.txt
```

方括号扩展
-----

> 方括号扩展的形式是`[...]`，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。
> 
> 方括号扩展会匹配括号之中的任意一个字符。比如，`[aeiou]`可以匹配五个元音字母中的任意一个。

```bash
# 存在文件 a.txt 和 b.txt
$ ls [ab].txt
a.txt b.txt

# 只存在文件 a.txt
$ ls [ab].txt
a.txt

# 不存在文件f.txt
# 文件不存在不会扩展，原样输出
$ echo [f].txt
[f].txt
```

> 方括号扩展属于文件名匹配，即扩展后的结果必须符合现有的文件路径。如果不存在匹配，就会保持原样，不进行扩展。

```bash
# 不存在文件 a.txt 和 b.txt
$ ls [ab].txt
ls: 无法访问'[ab].txt': 没有那个文件或目录
```

### 两种变体

> 方括号扩展还有两种变体：`[^...]`和`[!...]`。它们表示匹配不在方括号里面的字符，这两种写法是等价的。
> 
> 比如，`[^abc]`或`[!abc]`表示匹配除了`a`、`b`、`c`以外的字符。

```bash
# 存在 aaa、bbb、aba 三个文件
$ ls ?[!a]?
aba bbb
```

> <mark>注意</mark>，如果需要匹配`[`字符，可以放在方括号内，比如`[[aeiou]`。如果需要匹配连字号`-`，只能放在方括号内部的开头或结尾，比如`[-aeiou]`或`[aeiou-]`。

[start-end] 扩展
--------------

> 方括号扩展有一个简写形式`[start-end]`，表示匹配一个连续的范围。
> 
> 比如，`[a-c]`等同于`[abc]`，`[0-9]`等同于`[0123456789]`。

```bash
# 存在文件 a.txt、b.txt 和 c.txt
$ ls [a-c].txt
a.txt
b.txt
c.txt

# 存在文件 report1.txt、report2.txt 和 report3.txt
$ ls report[0-9].txt
report1.txt
report2.txt
report3.txt
...
```

> 下面是一些常用简写的例子。
> 
> * `[a-z]`：所有小写字母。
> * `[a-zA-Z]`：所有小写字母与大写字母。
> * `[a-zA-Z0-9]`：所有小写字母、大写字母与数字。
> * `[!a-zA-Z]`：表示匹配非英文字母的字符

花括号扩展
-----

> 花括号扩展`{...}`表示分别扩展成花括号里面的所有值，各个值之间使用逗号分隔。比如，`{1,2,3}`扩展成`1 2 3`。

```bash
$ echo {1,2,3}
1 2 3

$ echo d{a,e,i,u,o}g
dag deg dig dug dog

$ echo Front-{A,B,C}-Back
Front-A-Back Front-B-Back Front-C-Back
```

> <mark>注意，花括号扩展不是文件名扩展。它会扩展成所有给定的值，而不管是否有对应的文件存在。</mark>
> 
> <mark>另一个需要注意的地方是，花括号内部的逗号前后不能有空格。否则，花括号扩展会失效。</mark>

> 逗号前面可以没有值，表示扩展的第一项为空。

```bash
$ cp a.log{,.bak}

# 等同于
# cp a.log a.log.bak
```

> 花括号可以嵌套。

```bash
$ echo {j{p,pe}g,png}
jpg jpeg png

$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b
```

> 花括号也可以与其他模式联用，并且<mark>总是先于其他模式进行扩展</mark>。

```bash
$ echo /bin/{cat,b*}
/bin/cat /bin/b2sum /bin/base32 /bin/base64 ... ...

# 基本等同于
$ echo /bin/cat;echo /bin/b*
```

> 上面例子中，会先进行大括号扩展，然后进行`*`扩展，等同于执行两条`echo`命令。

{start..end} 扩展
---------------

> 花括号扩展有一个简写形式`{start..end}`，表示扩展成一个连续序列。比如，`{a..z}`可以扩展成26个小写英文字母

```bash
$ echo {a..c}
a b c

$ echo d{a..d}g
dag dbg dcg ddg

$ echo {1..4}
1 2 3 4

$ echo Number_{1..5}
Number_1 Number_2 Number_3 Number_4 Number_5
```

> <mark>注意</mark>，如果遇到无法理解的简写，花括号模式就会原样输出，不会扩展。

```bash
$ echo {a1..3c}
{a1..3c}
```

> 这种简写形式可以嵌套使用，形成复杂的扩展

```bash
$ echo .{mp{3..4},m4{a,b,p,v}}
.mp3 .mp4 .m4a .m4b .m4p .m4v
```

> 花括号模式常见用途为创建一系列目录

```bash
$ echo {2007..2009}-{01..12}
2007-01 2007-02 2007-03 2007-04 2007-05 2007-06 2007-07 2007-08 2007-09 2007-10 2007-11 2007-12 2008-01 2008-02 2008-03 2008-04 2008-05 2008-06 2008-07 2008-08 2008-09 2008-10 2008-11 2008-12 2009-01 2009-02 2009-03 2009-04 2009-05 2009-06 2009-07 2009-08 2009-09 2009-10 2009-11 2009-12
```

> 这个写法的另一个常见用途，是直接用于`for`循环。

```bash
for i in {1..4}
do
  echo $i
done
```

> 这种简写形式还可以使用第二个双点号（`start..end..step`），用来指定扩展的步长

```bash
$ echo {0..8..2}
0 2 4 6 8
```

变量扩展
----

> Bash 将美元符号`$`开头的词元视为变量，将其扩展成变量值，详见《Bash 变量》

> `${!string*}`或`${!string@}`返回所有匹配给定字符串`string`的变量名

```bash
$ echo ${!S*}
SECONDS SHELL SHELLOPTS SHLVL SSH_AGENT_PID SSH_AUTH_SOCK
```

> 上面例子中，`${!S*}`扩展成所有以`S`开头的变量名。

### 参数扩展中的条件

| 符号  | 语法             | 含义                  | 示例（`var=old`）                   |
| --- | -------------- | ------------------- | ------------------------------- |
| `+` | `${var:+word}` | 变量**有值**时替换为 `word` | `echo ${var:+new}` → `new`      |
| `-` | `${var:-word}` | 变量**无值**时替换为 `word` | `echo ${var:-new}` → `old`      |
| `=` | `${var:=word}` | 变量**无值**时赋值为 `word` | `echo ${var:=new}` → `old`（不改变） |
| `?` | `${var:?word}` | 变量**无值**时报错         | `echo ${var:?error}` → 不报错      |

子命令扩展
-----

> `$(...)`可以扩展成另一个命令的运行结果，该命令的所有输出都会作为返回值。

```bash
$ echo $(date)
Tue Jan 28 00:01:13 CST 2020
```

> 还有另一种较老的语法，子命令放在反引号之中，也可以扩展成命令的运行结果。

```bash
$ echo `date`
Tue Jan 28 00:01:13 CST 2020
```

> `$(...)`可以嵌套，比如`$(ls $(pwd))`

算术扩展
----

> `$((...))`可以扩展成整数运算的结果，详见《Bash 的算术运算》

```bash
$ echo $((2 + 2))
4
```

字符类
---

> `[[:class:]]`表示一个字符类，扩展成某一类特定字符之中的一个。常用的字符类如下：
> 
> * `[[:alnum:]]`：匹配任意英文字母与数字
> * `[[:alpha:]]`：匹配任意英文字母
> * `[[:blank:]]`：空格和 Tab 键。
> * `[[:cntrl:]]`：ASCII 码 0-31 的不可打印字符。
> * `[[:digit:]]`：匹配任意数字 0-9。
> * `[[:graph:]]`：A-Z、a-z、0-9 和标点符号。
> * `[[:lower:]]`：匹配任意小写字母 a-z。
> * `[[:print:]]`：ASCII 码 32-127 的可打印字符。
> * `[[:punct:]]`：标点符号（除了 A-Z、a-z、0-9 的可打印字符）。
> * `[[:space:]]`：空格、Tab、LF（10）、VT（11）、FF（12）、CR（13）。
> * `[[:upper:]]`：匹配任意大写字母 A-Z。
> * `[[:xdigit:]]`：16进制字符（A-F、a-f、0-9）

**示例**

```bash
$ echo [[:upper:]]*
```

> 上面命令输出所有大写字母开头的文件名。

> 字符类的第一个方括号后面，可以加上感叹号`!`（或`^`），表示否定。比如，`[![:digit:]]`(或`[^[:digit:]]`)匹配所有非数字。

> <mark>字符类也属于文件名扩展</mark>，如果没有匹配的文件名，字符类就会原样输出。

## 模式扩展使用注意

**（1）通配符是先解释，再执行**

> Bash 接收到命令以后，发现里面有通配符，会进行通配符扩展，然后再执行命令
> 
> 例如：`ls a*.txt`
> 
> 上面命令的执行过程是，Bash 先将`a*.txt`扩展成`ab.txt`，然后再执行`ls ab.txt`

**（2）文件名扩展在不匹配时，会原样输出**

> 文件名扩展在没有可匹配的文件时，会原样输出
> 
> 花括号扩展`{...}`不是文件名扩展

**（3）只适用于单层路径**

> 所有文件名扩展只匹配单层路径，不能跨目录匹配，即无法匹配子目录里面的文件。或者说，`?`或`*`这样的通配符，不能匹配路径分隔符（`/`）。
> 
> 如果要匹配子目录里面的文件，可以写成下面这样：

```bash
$ ls */*.txt
```

量词语法
----

> 量词语法用来控制模式匹配的次数。它只有在 Bash 的`extglob`参数打开的情况下才能使用，不过一般是默认打开的。下面的命令可以查询：

```bash
$ shopt extglob
extglob            on
```

> 如果`extglob`参数是关闭的，可以用下面的命令打开：

```bash
$ shopt -s extglob
```

> 量词语法有下面几个。
> 
> * `?(pattern-list)`：模式匹配零次或一次。
> * `*(pattern-list)`：模式匹配零次或多次。
> * `+(pattern-list)`：模式匹配一次或多次。
> * `@(pattern-list)`：只匹配一次模式。
> * `!(pattern-list)`：匹配给定模式以外的任何内容。

**案例?()**

> `?(.)`匹配零个或一个`.`

```bash
$ ls abc?(.)txt
abctxt abc.txt
```

shopt 命令
--------

> `shopt`命令可以调整 Bash 的行为。它有好几个参数跟通配符扩展有关。

> `shopt`命令的使用方法如下：

```bash
# 打开某个参数
$ shopt -s [optionname]

# 关闭某个参数
$ shopt -u [optionname]

# 查询某个参数关闭还是打开
$ shopt [optionname]
```

**（1）dotglob 参数**

> `dotglob`参数可以让扩展结果包括隐藏文件（即点开头的文件）。
> 
> 正常情况下，扩展结果不包括隐藏文件。

**（2）nullglob 参数**

> `nullglob`参数可以让通配符不匹配任何文件名时，返回空字符。
> 
> 默认情况下，通配符不匹配任何文件名时，会保持不变。

**（3）failglob 参数**

> `failglob`参数使得通配符不匹配任何文件名时，Bash 会直接报错，而不是让各个命令去处理。

**（4）extglob 参数**

> `extglob`参数使得 Bash 支持 ksh 的一些扩展语法。它默认应该是打开的。

**（5）nocaseglob 参数**

> `nocaseglob`参数可以让通配符扩展不区分大小写。

```bash
$ shopt -s nocaseglob
$ ls /windows/program*
/windows/ProgramData
/windows/Program Files
/windows/Program Files (x86)
```

**（6）globstar 参数**

> `globstar`参数可以使得`**`匹配零个或多个子目录。该参数默认是关闭的。

# 引号和转义

> Bash 只有一种数据类型，就是字符串。不管用户输入什么数据，Bash 都视为字符串。

## 转义

> 某些字符在 Bash 里面有特殊含义（比如`$`、`&`、`*`）。

```bash
$ echo $date

$
```

> 上面例子中，输出`$date`不会有任何结果，因为`$`是一个特殊字符。
> 
> 如果想要原样输出这些特殊字符，就必须在它们前面加上反斜杠，使其变成普通字符。这就叫做“转义”（escape）。

```bash
$ echo \$date
$date
```

> 反斜杠除了用于转义，还可以表示一些不可打印的字符。
> 
> * `\a`：响铃
> * `\b`：退格
> * `\n`：换行
> * `\r`：回车
> * `\t`：制表符

**将命令写成多行**

> 换行符前面加上反斜杠转义，就使得换行符变成一个普通字符，Bash 会将其当作长度为`0`的空字符处理，从而可以将一行命令写成多行。

```bash
$ mv \
/path/to/foo \
/path/to/bar
```

单引号
---

> 单引号用于保留字符的字面含义，各种特殊字符在单引号里面，都会变为普通字符，比如星号（`*`）、美元符号（`$`）、反斜杠（`\`）等。

```bash
$ echo '*'
*

$ echo '$USER'
$USER

$ echo '$((2+2))'
$((2+2))

$ echo '$(echo foo)'
$(echo foo)
```

> 由于反斜杠在单引号里面变成了普通字符，所以如果单引号之中，还要使用单引号，不能使用转义，需要在外层的单引号前面加上一个美元符号（`$`），然后再对里层的单引号转义。

```bash
# 不正确
$ echo it's

# 不正确
$ echo 'it\'s'

# 正确
$ echo $'it\'s'
```

双引号
---

> 双引号比单引号宽松，大部分特殊字符在双引号里面，都会失去特殊含义，变成普通字符。
> 
> 但是，三个特殊字符除外：美元符号（`$`）、反引号（`` ` ``）和反斜杠（`\`）。这三个字符在双引号之中，依然有特殊含义，会被 Bash 自动扩展。

```bash
$ echo "$SHELL"
/bin/bash

$ echo "`date`"
Mon Jan 27 13:33:18 CST 2020

# 文件名有空格
$ ls "two words.txt"
```

> 双引号还有一个作用，就是保存原始命令的输出格式。

```bash
# 单行输出
$ echo $(cal)
一月 2020 日 一 二 三 四 五 六 1 2 3 ... 31

# 原始格式输出
$ echo "$(cal)"
      一月 2020
日 一 二 三 四 五 六
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31
```

Here 文档
-------

> Here 文档（here document）是一种输入多行字符串的方法，格式如下。

```bash
<< token
text
token
```

> 它的格式分成开始标记（`<< token`）和结束标记（`token`）。开始标记是两个小于号 + Here 文档的名称，名称可以随意取，后面必须是一个换行符；结束标记是单独一行顶格写的 Here 文档名称，如果不是顶格，结束标记不起作用。两者之间就是多行字符串的内容。

**示例**

```bash
$ cat << _EOF_
<html>
<head>
    <title>
    The title of your page
    </title>
</head>

<body>
    Your page content goes here.
</body>
</html>
_EOF_
```

> Here 文档内部会发生变量替换，同时支持反斜杠转义，但是不支持通配符扩展，双引号和单引号也失去语法作用，变成了普通字符。

```bash
$ cat << token
> tt
> "$foo"
> hh
> token
# 输出
tt
""
hh
```

> Here 文档的本质是重定向，它将字符串重定向输出给某个命令，相当于包含了`echo`命令。

```bash
$ command << token
  string
token

# 等同于

$ echo string | command
```

Here 字符串
--------

> Here 文档还有一个变体，叫做 Here 字符串（Here string），使用三个小于号（`<<<`）表示。
> 
> 它的作用是将字符串通过标准输入，传递给命令。

```bash
<<< string
```

# Bash变量

## 简介

> Bash 变量分成环境变量和自定义变量两类。
> 
> <mark>用户设置修改导出的变量都是临时的，只要关闭或新建bash都会消失，所以希望永久生效需要写入.bashrc里</mark>

### 环境变量

> 环境变量是 Bash 环境自带的变量，进入 Shell 时已经定义好了，可以直接使用。它们通常是系统定义好的，也可以由用户从父 Shell 传入子 Shell。
> 
> `env`命令或`printenv`命令，可以显示所有环境变量。

**一些常见的环境变量**

> * `BASHPID`：Bash 进程的进程 ID。
> * `BASHOPTS`：当前 Shell 的参数，可以用`shopt`命令修改。
> * `DISPLAY`：图形环境的显示器名字，通常是`:0`，表示 X Server 的第一个显示器。
> * `EDITOR`：默认的文本编辑器。
> * `HOME`：用户的主目录。
> * `HOST`：当前主机的名称。
> * `IFS`：词与词之间的分隔符，默认为空格。
> * `LANG`：字符集以及语言编码，比如`zh_CN.UTF-8`。
> * `PATH`：由冒号分开的目录列表，当输入可执行程序名后，会搜索这个目录列表。
> * `PS1`：Shell 提示符。
> * `PS2`： 输入多行命令时，次要的 Shell 提示符。
> * `PWD`：当前工作目录。
> * `RANDOM`：返回一个0到32767之间的随机数。
> * `SHELL`：Shell 的名字。
> * `SHELLOPTS`：启动当前 Shell 的`set`命令的参数，参见《set 命令》一章。
> * `TERM`：终端类型名，即终端仿真器所用的协议。
> * `UID`：当前用户的 ID 编号。
> * `USER`：当前用户的用户名。
> 
> 很多环境变量很少发生变化，而且是只读的，可以视为常量。由于它们的变量名全部都是大写，所以传统上，如果用户要自己定义一个常量，也会使用全部大写的变量名。
> 
> <mark>注意，Bash 变量名区分大小写</mark>

### 自定义变量

> 自定义变量是用户在当前 Shell 里面自己定义的变量，仅在当前 Shell 可用。一旦退出当前 Shell，该变量就不存在了。
> 
> `set`命令可以显示所有变量（包括环境变量和自定义变量），以及所有的 Bash 函数。

```bash
$ set
```

创建变量
----

> 用户创建变量的时候，变量名必须遵守下面的规则。
> 
> * 字母、数字和下划线字符组成。
> * 第一个字符必须是一个字母或一个下划线，不能是数字。
> * 不允许出现空格和标点符号。
> 
> 变量声明的语法如下：

```bash
variable=value
```

> 上面命令中，等号左边是变量名，右边是变量。注意，等号两边不能有空格。
> 
> 如果变量的值包含空格，则必须将值放在引号中。

```bash
myvar="hello world"
```

> <mark>Bash 没有数据类型的概念，所有的变量值都是字符串。</mark>

```bash
a=z                     # 变量 a 赋值为字符串 z
b="a string"            # 变量值包含空格，就必须放在引号里面
c="a string and $b"     # 变量值可以引用其他变量的值
d="\t\ta string\n"      # 变量值可以使用转义字符
e=$(ls -l foo.txt)      # 变量值可以是命令的执行结果
f=$((5 * 7))            # 变量值可以是数学运算的结果
```

## 读取变量

> 读取变量的时候，直接在变量名前加上`$`就可以了。

```bash
$ foo=bar
$ echo $foo
bar
```

> 每当 Shell 看到以`$`开头的单词时，就会尝试读取这个变量名对应的值。
> 
> 如果变量不存在，Bash 不会报错，而会输出空字符。
> 
> 由于`$`在 Bash 中有特殊含义，把它当作美元符号使用时，一定要非常小心，

```bash
$ echo The total is \$100.00
The total is $100.00
```

> 读取变量的时候，变量名也可以使用花括号`{}`包围，比如`$a`也可以写成`${a}`。这种写法可以用于变量名与其他字符连用的情况。

```bash
$ a=foo
$ echo ${a}_file
foo_file

$ myvar="hello wsl"
$ echo myvar:${myvar}
myvar:hell ws
```

> 如果变量的值本身也是变量，可以使用`${!varname}`的语法，读取最终的值。

```bash
$ myvar=USER
$ echo ${!myvar}
dingw
```

> <mark>注意：在shell脚本中，凡是变量在获取其值时应该用引号括起来，因为如果变量不存在会返回空，shell脚本以空格为分隔符，这可能会造成意外的结果</mark>

```bash
if [ -f "$FILE" ]; then
    echo "文件存在"
else
    echo "文件不存在或未定义"
fi
```

删除变量
----

> `unset`命令用来删除一个变量。

```bash
unset 变量名
```

> 这个命令不是很有用。因为不存在的 Bash 变量一律等于空字符串，所以即使`unset`命令删除了变量，还是可以读取这个变量，值为空字符串。
> 
> 所以，删除一个变量，也可以将这个变量设成空字符串。

```bash
$ foo=''
# 或
$ foo=
```

输出变量，export 命令
--------------

> 用户创建的变量仅可用于当前 Shell，子 Shell 默认读取不到父 Shell 定义的变量。为了把变量传递给子 Shell，需要使用`export`命令。这样输出的变量，对于子 Shell 来说就是环境变量。
> 
> `export`命令用来向子 Shell 输出变量。

```bash
NAME=foo
export NAME
```

> 上面命令输出了变量`NAME`。变量的赋值和输出也可以在一个步骤中完成。

```bash
export NAME=value
```

特殊变量
----

> Bash 提供一些特殊变量。这些变量的值由 Shell 提供，用户不能进行赋值。

**`$?`变量**

> `$?`为上一个命令的退出码，用来判断上一个命令是否执行成功。返回值是`0`，表示上一个命令执行成功；如果不是零，表示上一个命令执行失败。

```bash
$ echo $?
0
```

**`$$`变量**

> `$$`为当前 Shell 的进程 ID。

```bash
$ echo $$
10662
```

**`$_`变量**

> `$_`为上一个命令的最后一个参数。

```bash
$ grep dictionary /usr/share/dict/words
dictionary

$ echo $_
/usr/share/dict/words
```

**`$!`变量**

> `$!`为最近一个后台执行的异步命令的进程 ID。

```bash
$ firefox &
[1] 11064

$ echo $!
11064
```

**`$0`变量**

> `$0`为当前 Shell 的名称（在命令行直接执行时）或者脚本名（在脚本中执行时）。

```bash
$ echo $0
bash
```

**`$-`变量**

> `$-`为当前 Shell 的启动参数。

```bash
$ echo $-
himBHs
```

**`$@`和`$#`变量**

> `$#`表示脚本的参数数量，`$@`表示脚本的参数值，参见脚本。

变量的默认值
------

> Bash 提供四个跟变量的默认值有关的特殊语法，目的是保证变量不为空。

> 如果变量`varname`存在且不为空，则返回它的值，否则返回`word`。它的目的是返回一个默认值，比如`${count:-0}`表示变量`count`不存在时返回`0`。

```bash
${varname:-word}
```

> 如果变量`varname`存在且不为空，则返回它的值，否则将它设为`word`，并且返回`word`。它的目的是设置变量的默认值，比如`${count:=0}`表示变量`count`不存在时返回`0`，且将`count`设为`0`。

```bash
${varname:=word}
```

> 如果变量名存在且不为空，则返回`word`，否则返回空值。它的目的是测试变量是否存在，比如`${count:+1}`表示变量`count`存在时返回`1`（表示`true`），否则返回空值。

```bash
${varname:+word}
```

> 如果变量`varname`存在且不为空，则返回它的值，否则打印出`varname: message`，并中断脚本的执行。如果省略了`message`，则输出默认的信息“parameter null or not set.”。它的目的是防止变量未定义，比如`${count:?"undefined!"}`表示变量`count`未定义时就中断执行，抛出错误，返回给定的报错信息`undefined!`。

```bash
${varname:?message}
```

declare 命令
----------

> `declare`命令可以声明一些特殊类型的变量，为变量设置一些限制，比如声明只读类型的变量和整数类型的变量。
> 
> 它的语法形式如下：

```bash
declare OPTION VARIABLE=value
```

> `declare`命令的主要参数（OPTION）如下。
> 
> * `-a`：声明数组变量。
> * `-f`：输出所有函数定义。
> * `-F`：输出所有函数名。
> * `-i`：声明整数变量。
> * `-l`：声明变量为小写字母。
> * `-p`：查看变量信息。
> * `-r`：声明只读变量。
> * `-u`：声明变量为大写字母。
> * `-x`：该变量输出为环境变量。

readonly 命令
-----------

> `readonly`命令等同于`declare -r`，用来声明只读变量，不能改变变量值，也不能`unset`变量。

```bash
$ readonly foo=1
$ foo=2
bash: foo：只读变量
$ echo $?
1
```

> 上面例子中，更改只读变量`foo`会报错，命令执行失败。

> `readonly`命令有三个参数。
> 
> * `-f`：声明的变量为函数名。
> * `-p`：打印出所有的只读变量。
> * `-a`：声明的变量为数组。

let 命令
------

> `let`命令声明变量时，可以直接执行算术表达式。

```bash
$ let foo=1+2
$ echo $foo
3
```

> `let`命令的参数表达式如果包含空格，就需要使用引号

```bash
$ let "foo = 1 + 2"
```

> `let`可以同时对多个变量赋值，赋值表达式之间使用空格分隔

```bash
$ let "v1 = 1" "v2 = v1++"
$ echo $v1,$v2
2,1
```

# 字符串操作

字符串的长度
------

> 获取字符串长度的语法如下

```bash
${#varname}
```

**示例**

```bash
$ myPath=/home/cam/book/long.file.name
$ echo ${#myPath}
29
```

子字符串
----

> 字符串提取子串的语法如下

```bash
${varname:offset:length}
```

> 上面语法的含义是返回变量`$varname`的子字符串，从位置`offset`开始（从`0`开始计算），长度为`length`。

**示例**

```bash
$ count=frogfootman
$ echo ${count:4:4}
foot
```

> 注意：这种语法不能直接操作字符串，只能通过变量来读取字符串，并且<mark>不会改变原始字符串</mark>。

搜索和替换
-----

> 这里略过了

改变大小写
-----

> 下面的语法可以改变变量的大小写

```bash
# 转为大写
${varname^^}

# 转为小写
${varname,,}
```

**示例**

```bash
$ foo=heLLo
$ echo ${foo^^}
HELLO
$ echo ${foo,,}
hello
```

> <mark>注意：这不会改变原变量</mark>

Bash 的算术运算
==========

算术表达式
-----

> `((...))`语法可以进行<mark>整数</mark>的算术运算，并且会自动忽略内部的空格

```bash
$ ((foo = 5 + 5))
$ echo $foo
10

# 下面都是正确
$ ((2+2))
$ (( 2+2 ))
$ (( 2 + 2 ))
```

> `((...))`命令执行的结果根据算术运算的结果而定。只要算术结果不是`0`，命令就算执行成功。

```bash
$ (( 3 + 2 ))
$ echo $?
0    # 成功

$ (( 3 - 3 ))
$ echo $?
1    # 失败
```

> 如果要读取算术运算的结果，需要在`((...))`前面加上美元符号`$((...))`，使其变成算术表达式，返回算术运算的值

```bash
$ echo $((2 + 2))
4
```

> `((...))`语法支持的算术运算符如下。
> 
> * `+`：加法
> * `-`：减法
> * `*`：乘法
> * `/`：除法（整除）
> * `%`：余数
> * `**`：指数
> * `++`：自增运算（前缀或后缀）
> * `--`：自减运算（前缀或后缀）
> * `()`：括号（改变运算顺序）

> `$((...))`的圆括号之中，不需要在变量名之前加上`$`，不过加上也不报错

```bash
$ number=2
$ echo $(($number + 1))
3
```

> 如果在`$((...))`里面使用字符串，Bash 会认为那是一个变量名。如果不存在同名变量，Bash 就会将其作为空值，空值被当作0来处理

```bash
$ echo $(( "hello" + 2))
2
$ echo $(( "hello" * 2))
0
```



## 逻辑运算

> `$((...))`支持以下的逻辑运算符。
> 
> * `<`：小于
> * `>`：大于
> * `<=`：小于或相等
> * `>=`：大于或相等
> * `==`：相等
> * `!=`：不相等
> * `&&`：逻辑与
> * `||`：逻辑或
> * `!`：逻辑否
> * `expr1?expr2:expr3`：三元条件运算符。若表达式`expr1`的计算结果为非零值（算术真），则执行表达式`expr2`，否则执行表达式`expr3`。

> 如果逻辑表达式为真，返回`1`，否则返回`0`

```bash
$ echo $((3 > 2))
1
$ echo $(( (3 > 2) || (4 <= 1) ))
1
```

赋值运算
----

> 算术表达式`$((...))`可以执行赋值运算

```bash
$ echo $((a=1))
1
$ echo $a
1
```

> `$((...))`支持的赋值运算符，有以下这些。
> 
> * `parameter = value`：简单赋值。
> * `parameter += value`：等价于`parameter = parameter + value`。
> * `parameter -= value`：等价于`parameter = parameter – value`。
> * `parameter *= value`：等价于`parameter = parameter * value`。
> * `parameter /= value`：等价于`parameter = parameter / value`。
> * `parameter %= value`：等价于`parameter = parameter % value`。
> * `parameter <<= value`：等价于`parameter = parameter << value`。
> * `parameter >>= value`：等价于`parameter = parameter >> value`。
> * `parameter &= value`：等价于`parameter = parameter & value`。
> * `parameter |= value`：等价于`parameter = parameter | value`。
> * `parameter ^= value`：等价于`parameter = parameter ^ value`。

求值运算
----

> 逗号`,`在`$((...))`内部是求值运算符，执行前后两个表达式，并返回后一个表达式的值。

```bash
$ echo $((foo = 1 + 2, 3 * 4))
12
$ echo $foo
3
```

# history 历史

简介
--

> Bash 会保留用户的操作历史，即用户输入的每一条命令都会记录，默认是保存最近的500条命令。有了操作历史以后，就可以使用方向键的`↑`和`↓`，快速浏览上一条和下一条命令。
> 
> 退出当前 Shell 的时候，Bash 会将用户在当前 Shell 的操作历史写入`~/.bash_history`文件，该文件默认储存500个操作。
> 
> 环境变量`HISTFILE`总是指向这个文件。

```bash
$ echo $HISTFILE
/home/me/.bash_history
```

history 命令
----------

> `history`命令会输出`.bash_history`文件的全部内容，即输出操作历史。

```bash
$ history
...
498 echo Goodbye
499 ls ~
500 cd
```

> `history`命令的`-c`参数可以清除操作历史，即清空`.bash_history`文件。

```bash
$ history -c
```

目录堆栈
====

> 为了方便用户在不同目录之间切换，Bash 提供了目录堆栈功能

cd -
----

> Bash 可以记忆用户进入过的目录。默认情况下，只记忆前一次所在的目录，`cd -`命令可以返回前一次的目录。

```bash
# 当前目录是 /path/to/foo
$ cd bar

# 重新回到 /path/to/foo
$ cd -
```

pushd，popd
----------

> 如果希望记忆多重目录，可以使用`pushd`命令和`popd`命令。它们用来操作目录堆栈。
> 
> `pushd`命令的用法类似`cd`命令，可以进入指定的目录。

```bash
$ pushd dirname
```

> 略过

dirs 命令
-------

> `dirs`命令可以显示目录堆栈的内容，一般用来查看`pushd`和`popd`操作后的结果。

```bash
$ dirs
~/foo/bar ~/foo ~
```

> 该命令会输出一行文本，列出目录堆栈，目录之间使用空格分隔。栈顶（最晚入栈的目录）在最左边，栈底（最早入栈的目录）在最右边。

> 它有以下参数。
> 
> * `-c`：清空目录栈。
> * `-l`：用户主目录不显示波浪号前缀，而打印完整的目录。
> * `-p`：每行一个条目打印目录栈，默认是打印在一行。
> * `-v`：每行一个条目，每个条目之前显示位置编号（从0开始）。
> * `+N`：`N`为整数，表示显示堆顶算起的第 N 个目录，从零开始。
> * `-N`：`N`为整数，表示显示堆底算起的第 N 个目录，从零开始。

# Bash脚本

Shebang 行
---------

> 脚本的第一行通常是指定解释器。这一行以`#!`字符开头，这个字符称为 Shebang，所以这一行就叫做 Shebang 行。
> 
> `#!`后面就是脚本解释器的位置，Bash 脚本的解释器一般是`/bin/sh`或`/bin/bash`。

```bash
#!/bin/sh
# 或者
#!/bin/bash
```

> 如果 Bash 解释器不放在目录`/bin`，脚本就无法执行了。为了保险，可以写成下面这样。

```bash
#!/usr/bin/env bash
```

> 上面命令使用`env`命令（这个命令总是在`/usr/bin`目录），返回 Bash 可执行文件的位置。

执行权限和路径
-------

**权限**

> 脚本需要有执行权限。可以使用下面的命令，赋予脚本执行权限。

```bash
# 给所有用户执行权限
$ chmod +x script.sh

# 给所有用户读权限和执行权限
$ chmod +rx script.sh
# 或者
$ chmod 755 script.sh

# 只给脚本拥有者读权限和执行权限
$ chmod u+rx script.sh
```

**路径**

> 脚本调用时，一般需要指定脚本的路径（比如`path/script.sh`）。如果将脚本放在环境变量`$PATH`指定的目录中，就不需要指定路径了。因为 Bash 会自动到这些目录中，寻找是否存在同名的可执行文件。
> 
> 建议在主目录新建一个`~/bin`子目录，专门存放可执行脚本，然后把`~/bin`加入`$PATH`。

```bash
export PATH=$PATH:~/bin
```

> 上面命令改变环境变量`$PATH`，将`~/bin`添加到`$PATH`的末尾。可以将这一行加到`~/.bashrc`文件里面，然后重新加载一次`.bashrc`，这个配置就可以生效了。

env 命令
------

> `env`命令总是指向`/usr/bin/env`文件，或者说，这个二进制文件总是在目录`/usr/bin`。
> 
> `#!/usr/bin/env NAME`这个语法的意思是，让 Shell 查找`$PATH`环境变量里面第一个匹配的`NAME`。如果不知道某个命令的具体路径，或者希望兼容其他用户的机器，这样的写法就很有用。
> 
> `/usr/bin/env bash`的意思就是，返回`bash`可执行文件的位置，前提是`bash`的路径是在`$PATH`里面。
> 
> 其他脚本文件也可以使用这个命令：

```bash
#!/usr/bin/env node
```

> `env`命令的参数如下。
> 
> * `-i`, `--ignore-environment`：不带环境变量启动。
> * `-u`, `--unset=NAME`：从环境变量中删除一个变量。
> * `--help`：显示帮助。
> * `--version`：输出版本信息。

注释
--

> Bash 脚本中，`#`表示注释，可以放在行首，也可以放在行尾

```bash
# 本行是注释
echo 'Hello World!'

echo 'Hello World!' # 井号后面的部分也是注释
```

脚本参数
----

> 调用脚本的时候，脚本文件名后面可以带有参数。

```bash
$ script.sh word1 word2 word3
```

> 上面例子中，`script.sh`是一个脚本文件，`word1`、`word2`和`word3`是三个参数。

> 脚本文件内部，可以使用特殊变量，引用这些参数。
> 
> * `$0`：脚本文件名，即`script.sh`。
> * `$1`~`$9`：对应脚本的第一个参数到第九个参数。
> * `$#`：参数的总数。
> * `$@`：全部的参数，参数之间使用空格分隔。
> * `$*`：全部的参数，参数之间使用变量`$IFS`值的第一个字符分隔，默认为空格，但是可以自定义。
> 
> 如果脚本的参数多于9个，那么第10个参数可以用`${10}`的形式引用，以此类推。

> <mark>注意</mark>，如果命令是`command -o foo bar`，那么`-o`是`$1`，`foo`是`$2`，`bar`是`$3`。

```bash
#!/bin/bash
# script.sh

echo "全部参数：" $@
echo "命令行参数数量：" $#
echo '$0 = ' $0
echo '$1 = ' $1
echo '$2 = ' $2
echo '$3 = ' $3
```

> 使用for循环读取

```bash
#!/bin/bash

for i in "$@"; do
  echo $i
done
```

shift 命令
--------

> `shift`命令可以改变脚本参数，每次执行都会移除脚本当前的第一个参数（`$1`），使得后面的参数向前一位，即`$2`变成`$1`、`$3`变成`$2`、`$4`变成`$3`，以此类推。
> 
> `while`循环结合`shift`命令，也可以读取每一个参数。

```bash
#!/bin/bash

echo "一共输入了 $# 个参数"

while [ "$1" != "" ]; do
  echo "剩下 $# 个参数"
  echo "参数：$1"
  shift
done
```

> `shift`命令可以接受一个整数作为参数，指定所要移除的参数个数，默认为`1`

```bash
shift 3
```

getopts 命令
----------

> `getopts`命令用在脚本内部，可以解析复杂的脚本命令行参数，通常与`while`循环一起使用，取出脚本所有的带有前置连词线（`-`）的参数。

```bash
getopts optstring name
```

> 它带有两个参数。第一个参数`optstring`是字符串，给出脚本所有的连词线参数。

> 比如，某个脚本可以有三个配置项参数`-l`、`-h`、`-a`，其中只有`-a`可以带有参数值，而`-l`和`-h`是开关参数，那么`getopts`的第一个参数写成`lha:`，顺序不重要。
> 
> 注意，`a`后面有一个冒号，表示该参数带有参数值。

> `getopts`的第二个参数`name`是一个变量名，用来保存当前取到的配置项参数，即`l`、`h`或`a`

**示例**

```bash
while getopts 'lha:' OPTION; do
  case "$OPTION" in
    l)
      echo "linuxconfig"
      ;;

    h)
      echo "h stands for h"
      ;;

    a)
      avalue="$OPTARG"
      echo "The value provided is $OPTARG"
      ;;
    ?)
      echo "script usage: $(basename $0) [-l] [-h] [-a somevalue]" >&2
      exit 1
      ;;
  esac
done
shift "$(($OPTIND - 1))"
```

> 上面例子中，`while`循环不断执行`getopts 'lha:' OPTION`命令，每次执行就会读取一个连词线参数（以及对应的参数值），然后进入循环体。变量`OPTION`保存的是，当前处理的那一个连词线参数（即`l`、`h`或`a`）。如果用户输入了没有指定的参数（比如`-x`），那么`OPTION`等于`?`。循环体内使用`case`判断，处理这四种不同的情况。
> 
> 如果某个连词线参数带有参数值，比如`-a foo`，那么处理`a`参数的时候，环境变量`$OPTARG`保存的就是参数值。
> 
> 注意，只要遇到不带连词线的参数，`getopts`就会执行失败，从而退出`while`循环。比如，`getopts`可以解析`command -l foo`，但不可以解析`command foo -l`。另外，多个连词线参数写在一起的形式，比如`command -lh`，`getopts`也可以正确处理。
> 
> 变量`$OPTIND`在`getopts`开始执行前是`1`，然后每次执行就会加`1`。等到退出`while`循环，就意味着连词线参数全部处理完毕。这时，`$OPTIND - 1`就是已经处理的连词线参数个数，使用`shift`命令将这些参数移除，保证后面的代码可以用`$1`、`$2`等处理命令的主参数。

配置项参数终止符 `--`
-------------

> 置项参数终止符`--`，它的作用是告诉 Bash，在它后面的参数开头的`-`和`--`不是配置项，只能当作实体参数解释。

```bash
# 输出文件-f内容
$ cat -- -f
# 输出文件--file内容
$ cat -- --file
```

exit 命令
-------

> `exit`命令用于终止当前脚本的执行，并向 Shell 返回一个退出值。

```bash
$ exit
```

> 上面命令中止当前脚本，将最后一条命令的退出状态，作为整个脚本的退出状态。

```bash
# 退出值为0（成功）
$ exit 0

# 退出值为1（失败）
$ exit 1
```

命令执行结果
------

> 命令执行结束后，会有一个返回值。`0`表示执行成功，非`0`（通常是`1`）表示执行失败。环境变量`$?`可以读取前一个命令的返回值。

> 利用这一点，可以在脚本中对命令执行结果进行判断。

```bash
cd /path/to/somewhere
if [ "$?" = "0" ]; then
  rm *
else
  echo "无法切换目录！" 1>&2
  exit 1
fi
```

source 命令
---------

> `source`命令用于执行一个脚本，通常用于重新加载一个配置文件

```bash
$ source .bashrc
```

> `source`命令最大的特点是在当前 Shell 执行脚本，不像直接执行脚本时，会新建一个子 Shell。所以，`source`命令执行脚本时，不需要`export`变量。

> `source`有一个简写形式，可以使用一个点（`.`）来表示。

```bash
$ . .bashrc
```

别名，alias 命令
-----------

> `alias`命令用来为一个命令指定别名，这样更便于记忆。下面是`alias`的格式：

```bash
alias NAME=DEFINITION
```

> 上面命令中，`NAME`是别名的名称，`DEFINITION`是别名对应的原始命令。注意，等号两侧不能有空格，否则会报错。

## read 命令 -- 输入

> 有时，脚本需要在执行过程中，由用户提供一部分数据，这时可以使用`read`命令。它将用户的输入存入一个变量，方便后面的代码使用。用户按下回车键，就表示输入结束。
> 
> `read`命令的格式如下：

```bash
read [-options] [variable...]
```

> 上面语法中，`options`是参数选项，`variable`是用来保存输入数值的一个或多个变量名。如果没有提供变量名，环境变量`REPLY`会包含用户输入的一整行数据。

```bash
#!/bin/bash

echo -n "输入一些文本 > "
read text
echo "你的输入：$text"
```

> `read`命令可以接受多个参数：

```bash
#!/bin/bash
echo Please, enter your firstname and lastname
read FN LN
echo "Hi! $LN, $FN !"
```

> 如果`read`命令之后没有定义变量名，那么环境变量`REPLY`会包含所有的输入。

```bash
#!/bin/bash
# read-single: read multiple values into default variable
echo -n "Enter one or more values > "
read
echo "REPLY = '$REPLY'"
```

> `read`命令除了读取键盘输入，可以用来<mark>读取文件</mark>。

```bash
#!/bin/bash

filename='/etc/hosts'

while read myline
do
  echo "$myline"
done < $filename
```

> 上面的例子通过`read`命令，读取一个文件的内容。`done`命令后面的定向符`<`，将文件内容导向`read`命令，每次读取一行，存入变量`myline`，直到文件读取完毕。

### read参数

**（1）-t 参数**

> `read`命令的`-t`参数，设置了超时的秒数。如果超过了指定时间，用户仍然没有输入，脚本将放弃等待，继续向下执行。

```bash
#!/bin/bash

echo -n "输入一些文本 > "
if read -t 3 response; then
  echo "用户已经输入了"
else
  echo "用户没有输入"
fi
```

> 上面例子中，输入命令会等待3秒，如果用户超过这个时间没有输入，继续往下执行。

**（2）-p 参数**

> `-p`参数指定用户输入的提示信息。

```bash
read -p "Enter one or more values > "
echo "REPLY = '$REPLY'"
```

> 上面例子中，先显示`Enter one or more values >`，再接受用户的输入。

**（3）-a 参数**

> `-a`参数把用户的输入赋值给一个数组，从零号位置开始

```bash
$ read -a people
alice duchess dodo
$ echo ${people[2]}
dodo
```

**（4）-n 参数**

> `-n`参数指定只读取若干个字符作为变量值，而不是整行读取。

```bash
$ read -n 3 letter
abcdefghij
$ echo $letter
abc
```

> 上面例子中，变量`letter`只包含3个字母。

**（5）-e 参数**

> `-e`参数允许用户输入的时候，使用`readline`库提供的快捷键，比如自动补全。

## 条件判断

### if 结构

> `if`是最常用的条件判断结构，只有符合给定条件时，才会执行指定的命令。它的语法如下：

```bash
if commands; then
  commands
[elif commands; then
  commands...]
[else
  commands]
fi
```

> `if`关键字后面是主要的判断条件，`elif`用来添加在主条件不成立时的其他判断条件，`else`则是所有条件都不成立时要执行的部分。

```bash
if test $USER = "foo"; then
  echo "Hello foo."
else
  echo "You are not foo."
fi
```

> 上面的例子中，判断条件是环境变量`$USER`是否等于`foo`，如果等于就输出`Hello foo.`，否则输出其他内容。

> `if`和`then`写在同一行时，需要分号分隔。分号是 Bash 的命令分隔符。它们也可以写成两行，这时不需要分号。

```bash
if true
then
  echo 'hello world'
fi

if false
then
  echo 'it is false' # 本行不会执行
fi
```

> 除了多行的写法，`if`结构也可以写成单行。

```bash
$ if true; then echo 'hello world'; fi
hello world
```

> <mark>注意</mark>，`if`关键字后面也可以是一条命令，该条命令执行成功（返回值`0`），就意味着判断条件成立

```bash
$ if echo 'hi'; then echo 'hello world'; fi
hi
hello world
```

> `if`后面可以跟任意数量的命令。这时，所有命令都会执行，但是判断真伪只看最后一个命令，即使前面所有命令都失败，只要最后一个命令返回`0`，就会执行`then`的部分。

```bash
$ if false; true; then echo 'hello world'; fi
hello world
```

### test 命令

> `if`结构的判断条件，一般使用`test`命令，有三种形式：

```bash
# 写法一
test expression

# 写法二
[ expression ]

# 写法三
[[ expression ]]
```

> 上面三种形式是等价的，但是第三种形式还支持正则判断，前两种不支持。
> 
> 上面的`expression`是一个表达式。这个表达式为真，`test`命令执行成功（返回值为`0`）；表达式为伪，`test`命令执行失败（返回值为`1`）。
> 
> <mark>注意</mark>，第二种和第三种写法，`[`和`]`与内部的表达式之间必须有<mark>空格</mark>。

```bash
$ test -f /etc/hosts
$ echo $?
0
# 上面等价于下面
$ [ -f /etc/hosts ]
$  echo $?
0
```

> 实际上，`[`这个字符是`test`命令的一种简写形式，可以看作是一个独立的命令，这解释了为什么它后面必须有空格。

> 下面把`test`命令的三种形式，用在`if`结构中，判断一个文件是否存在。

```bash
# 写法一
if test -e /tmp/foo.txt ; then
  echo "Found foo.txt"
fi

# 写法二
if [ -e /tmp/foo.txt ] ; then
  echo "Found foo.txt"
fi

# 写法三
if [[ -e /tmp/foo.txt ]] ; then
  echo "Found foo.txt"
fi
```

### 判断表达式

#### 文件判断表达式

> 以下表达式用来判断文件状态。
> 
> * `[ -b file ]`：如果 file 存在并且是一个块（设备）文件，则为`true`。
> * `[ -c file ]`：如果 file 存在并且是一个字符（设备）文件，则为`true`。
> * `[ -d file ]`：如果 file 存在并且是一个目录，则为`true`。
> * `[ -e file ]`：如果 file 存在，则为`true`。
> * `[ -f file ]`：如果 file 存在并且是一个普通文件，则为`true`。
> * `[ -g file ]`：如果 file 存在并且设置了组 ID，则为`true`。
> * `[ -G file ]`：如果 file 存在并且属于有效的组 ID，则为`true`。
> * `[ -h file ]`：如果 file 存在并且是符号链接，则为`true`。
> * `[ -k file ]`：如果 file 存在并且设置了它的“sticky bit”，则为`true`。
> * `[ -L file ]`：如果 file 存在并且是一个符号链接，则为`true`。
> * `[ -N file ]`：如果 file 存在并且自上次读取后已被修改，则为`true`。
> * `[ -O file ]`：如果 file 存在并且属于有效的用户 ID，则为`true`。
> * `[ -p file ]`：如果 file 存在并且是一个命名管道，则为`true`。
> * `[ -r file ]`：如果 file 存在并且可读（当前用户有可读权限），则为`true`。
> * `[ -s file ]`：如果 file 存在且其长度大于零，则为`true`。
> * `[ -S file ]`：如果 file 存在且是一个网络 socket，则为`true`。
> * `[ -t fd ]`：如果 fd 是一个文件描述符，并且重定向到终端，则为`true`。 这可以用来判断是否重定向了标准输入／输出／错误。
> * `[ -u file ]`：如果 file 存在并且设置了 setuid 位，则为`true`。
> * `[ -w file ]`：如果 file 存在并且可写（当前用户拥有可写权限），则为`true`。
> * `[ -x file ]`：如果 file 存在并且可执行（有效用户有执行／搜索权限），则为`true`。
> * `[ FILE1 -nt FILE2 ]`：如果 FILE1 比 FILE2 的更新时间更近，或者 FILE1 存在而 FILE2 不存在，则为`true`。
> * `[ FILE1 -ot FILE2 ]`：如果 FILE1 比 FILE2 的更新时间更旧，或者 FILE2 存在而 FILE1 不存在，则为`true`。
> * `[ FILE1 -ef FILE2 ]`：如果 FILE1 和 FILE2 引用相同的设备和 inode 编号，则为`true`。

```bash
#!/bin/bash

FILE=~/.bashrc

if [ -e "$FILE" ]; then
  if [ -f "$FILE" ]; then
    echo "$FILE is a regular file."
  fi
  if [ -d "$FILE" ]; then
    echo "$FILE is a directory."
  fi
  if [ -r "$FILE" ]; then
    echo "$FILE is readable."
  fi
  if [ -w "$FILE" ]; then
    echo "$FILE is writable."
  fi
  if [ -x "$FILE" ]; then
    echo "$FILE is executable/searchable."
  fi
else
  echo "$FILE does not exist"
  exit 1
fi
```

> 上面代码中，`$FILE`要放在双引号之中，这样可以防止变量`$FILE`为空，从而出错。因为`$FILE`如果为空，这时`[ -e $FILE ]`就变成`[ -e ]`，这会被判断为真。而`$FILE`放在双引号之中，`[ -e "$FILE" ]`就变成`[ -e "" ]`，这会被判断为伪。
> 
> <mark>shell脚本中变量获取时最好加引号</mark>

#### 字符串判断表达式

> 以下表达式用来判断字符串。
> 
> * `[ string ]`：如果`string`不为空（长度大于0），则判断为真。
> * `[ -n string ]`：如果字符串`string`的长度大于零，则判断为真。
> * `[ -z string ]`：如果字符串`string`的长度为零，则判断为真。
> * `[ string1 = string2 ]`：如果`string1`和`string2`相同，则判断为真，这个相较于下面标准一点。
> * `[ string1 == string2 ]` 等同于`[ string1 = string2 ]`。
> * `[ string1 != string2 ]`：如果`string1`和`string2`不相同，则判断为真。
> * `[ string1 '>' string2 ]`：如果按照字典顺序`string1`排列在`string2`之后，则判断为真。
> * `[ string1 '<' string2 ]`：如果按照字典顺序`string1`排列在`string2`之前，则判断为真。
> 
> <mark>注意</mark>，`test`命令内部的`>`和`<`，必须用引号引起来（或者是用反斜杠转义）。否则，它们会被 shell 解释为重定向操作符。

```bash
#!/bin/bash

ANSWER=maybe

if [ -z "$ANSWER" ]; then
  echo "There is no answer." >&2
  exit 1
fi
if [ "$ANSWER" = "yes" ]; then
  echo "The answer is YES."
elif [ "$ANSWER" = "no" ]; then
  echo "The answer is NO."
elif [ "$ANSWER" = "maybe" ]; then
  echo "The answer is MAYBE."
else
  echo "The answer is UNKNOWN."
fi
```

#### 整数判断表达式

> 下面的表达式用于判断整数。
> 
> * `[ integer1 -eq integer2 ]`：如果`integer1`等于`integer2`，则为`true`。
> * `[ integer1 -ne integer2 ]`：如果`integer1`不等于`integer2`，则为`true`。
> * `[ integer1 -le integer2 ]`：如果`integer1`小于或等于`integer2`，则为`true`。
> * `[ integer1 -lt integer2 ]`：如果`integer1`小于`integer2`，则为`true`。
> * `[ integer1 -ge integer2 ]`：如果`integer1`大于或等于`integer2`，则为`true`。
> * `[ integer1 -gt integer2 ]`：如果`integer1`大于`integer2`，则为`true`。

**示例**

```bash
#!/bin/bash

num=-23

if [ -z "$num" ]; then
        echo "num is empty"
        exit 1
fi

if [ "$num" -gt 0 ]; then
        echo "$num > 0"
else
        echo "$num < 0"
fi
```

#### 正则判断

> `[[ expression ]]`这种判断形式，支持正则表达式，正则表达式于模式匹配是两种语法

```bash
[[ string1 =~ regex ]]
```

> 上面的语法中，`regex`是一个正则表示式，`=~`是正则比较运算符。

**示例**

```bash
#!/bin/bash

INT=-5

if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
  echo "INT is an integer."
  exit 0
else
  echo "INT is not an integer." >&2
  exit 1
fi
```

#### 判断的逻辑运算

> 通过逻辑运算，可以把多个`test`判断表达式结合起来，创造更复杂的判断。
> 
> 三种逻辑运算`AND`，`OR`，和`NOT`，都有自己的专用符号：
> 
> * `AND`运算：符号`&&`，也可使用参数`-a`
> * `OR`运算：符号`||`，也可使用参数`-o`
> * `NOT`运算：符号`!`

**示例**

> 在`[ ]`中使用`()`时最好用转义符号

```bash
if [ ! \( $INT -ge $MIN_VAL && $INT -le $MAX_VAL \) ]; then
    echo "$INT is outside $MIN_VAL to $MAX_VAL."
else
    echo "$INT is in range."
fi
```

#### 算术判断

> Bash 还提供了`((...))`作为算术条件，进行算术运算的判断

```bash
if ((3 > 2)); then
  echo "true"
fi
```

> <mark>注意</mark>，算术判断不需要使用`test`命令，而是直接使用`((...))`结构。这个结构的返回值，决定了判断的真伪。

> 如果算术计算的结果是非零值，则表示判断成立。这一点跟命令的返回值正好相反，需要小心。

```bash
$ if ((1)); then echo "It is true."; fi
It is true.
$ if ((0)); then echo "It is true."; else echo "it is false."; fi
It is false.
```

#### 普通命令的逻辑运算

> 以使用 Bash 的命令控制操作符`&&`（AND）和`||`（OR），进行多个命令的逻辑运算。

```bash
$ command1 && command2
$ command1 || command2
```

> 对于`&&`操作符，先执行`command1`，只有`command1`执行成功后， 才会执行`command2`。对于`||`操作符，先执行`command1`，只有`command1`执行失败后， 才会执行`command2`。

> 命令会测试目录`temp`是否存在，如果不存在，就会执行第二个命令，创建这个目录。

```bash
$ [ -d temp ] || mkdir temp
```

**示例**

```bash
[[ -d "$dir_name" ]] && cd "$dir_name" && rm *

# 等同于

if [[ ! -d "$dir_name" ]]; then
  echo "No such directory: '$dir_name'" >&2
  exit 1
fi
if ! cd "$dir_name"; then
  echo "Cannot cd to '$dir_name'" >&2
  exit 1
fi
if ! rm *; then
  echo "File deletion failed. Check results" >&2
  exit 1
fi
```

### case 结构

> `case`结构用于多值判断，可以为每个值指定对应的命令，跟包含多个`elif`的`if`结构等价，但是语义更好。
> 
> 它的语法如下：

```bash
case expression in
  pattern )
    commands ;;
  pattern )
    commands ;;
  ...
esac
```

> 上面代码中，`expression`是一个表达式，`pattern`是表达式的值或者一个模式，可以有多条，用来匹配多个值，每条以两个分号（`;`）结尾。

**示例**

```bash
#!/bin/bash

echo -n "输入一个1到3之间的数字（包含两端）> "
read character
case $character in
  1 ) echo 1
    ;;
  2 ) echo 2
    ;;
  3 ) echo 3
    ;;
  * ) echo 输入不符合要求
esac
```

> `case`的匹配模式可以使用各种通配符，下面是一些例子。
> 
> * `a)`：匹配`a`。
> * `a|b)`：匹配`a`或`b`。   可以用`[ab])`，效果一样
> * `[[:alpha:]])`：匹配单个字母。
> * `???)`：匹配3个字符的单词。
> * `*.txt)`：匹配`.txt`结尾。
> * `*)`：匹配任意输入，通常作为`case`结构的最后一个模式。

## 循环

> Bash 提供三种循环语法`for`、`while`和`until`

### while循环

> `while`循环有一个判断条件，只要符合条件，就不断循环执行指定的语句。

```bash
while condition; do
  commands
done
```

**示例**

```bash
#!/bin/bash

number=0
while [ "$number" -lt 10 ]; do
  echo "Number = $number"
  number=$((number + 1))
done
```

### for...in 循环

> `for...in`循环用于遍历列表的每一项。

```bash
for variable in list
do
  commands
done
```

**示例**

```bash
#!/bin/bash

for i in word1 word2 word3; do
  echo $i
done
```

> 列表可以由通配符产生

```bash
for i in *.png; do
  ls -l $i
done
```

> 列表也可以通过子命令产生

```bash
#!/bin/bash

count=0
for i in $(cat ~/.bash_profile); do
  count=$((count + 1))
  echo "Word $count ($i) contains $(echo -n $i | wc -c) characters"
done
```

### for 循环

> `for`循环还支持 C 语言的循环语法

```bash
for (( expression1; expression2; expression3 )); do
  commands
done
```

> <mark>注意</mark>，循环条件放在双重圆括号之中。另外，圆括号之中使用变量，不必加上美元符号`$`。

```bash
for (( i=0; i<5; i=i+1 )); do
  echo $i
done
```

### break，continue

> Bash 提供了两个内部命令`break`和`continue`，用来在循环内部跳出循环。
> 
> `break`命令立即终止循环，程序继续执行循环块之后的语句，即不再执行剩下的循环。

```bash
#!/bin/bash

for number in 1 2 3 4 5 6
do
  echo "number is $number"
  if [ "$number" = "3" ]; then
    break
  fi
done
```

> `continue`命令立即终止本轮循环，开始执行下一轮循环。

```bash
#!/bin/bash

while read -p "What file do you want to test?" filename
do
  if [ ! -e "$filename" ]; then
    echo "The file does not exist."
    continue
  fi

  echo "You entered a valid file.."
done
```

### select 结构

> `select`结构主要用来生成简单的菜单:

```bash
select name
[in list]
do
  commands
done
```

> Bash 会对`select`依次进行下面的处理。
> 
> 1. `select`生成一个菜单，内容是列表`list`的每一项，并且每一项前面还有一个数字编号。
> 2. Bash 提示用户选择一项，输入它的编号。
> 3. 用户输入以后，Bash 会将该项的内容存在变量`name`，该项的编号存入环境变量`REPLY`。如果用户没有输入，就按回车键，Bash 会重新输出菜单，让用户选择。
> 4. 执行命令体`commands`。
> 5. 执行结束后，回到第一步，重复这个过程。

**示例**

```bash
#!/bin/bash
# select.sh

select brand in Samsung Sony iphone symphony Walton
do
  echo "You have chosen $brand"
done
```

**输出**

```textile
$ ./select.sh
1) Samsung
2) Sony
3) iphone
4) symphony
5) Walton
#?
```

> 如果用户没有输入编号，直接按回车键。Bash 就会重新输出一遍这个菜单，直到用户按下`Ctrl + c`，退出执行。

> `select`可以与`case`结合，针对不同项，执行不同的命令。

```bash
#!/bin/bash

echo "Which Operating System do you like?"

select os in Ubuntu LinuxMint Windows8 Windows10 WindowsXP
do
  case $os in
    "Ubuntu"|"LinuxMint")
      echo "I also use $os."
    ;;
    "Windows8" | "Windows10" | "WindowsXP")
      echo "Why don't you try Linux?"
    ;;
    *)
      echo "Invalid entry."
      break
    ;;
  esac
done
```

## Bash 函数

> 函数总是在当前 Shell 执行，这是跟脚本的一个重大区别，Bash 会新建一个子 Shell 执行脚本。如果函数与脚本同名，函数会优先执行。但是，函数的优先级不如别名，即如果函数与别名同名，那么别名优先执行。

> Bash 函数定义的语法有两种：

```bash
# 第一种
funcName() {
  # codes
}
# 等价于
# 第二种
function funcName {
  # codes
}
# 等价于
function funcName() {
    # codes
}
```

**示例**

```bash
hello() {
    echo "Hello $1"
}
```

> 删除一个函数，可以使用`unset`命令

```bash
unset -f functionName
```

> 查看当前 Shell 已经定义的所有函数，可以使用`declare`命令

```bash
$ declare -f
```

### 参数变量

> 函数体内可以使用参数变量，获取函数参数。函数的参数变量，与脚本参数变量是一致的。
> 
> * `$1`~`$9`：函数的第一个到第9个的参数。
> * `$0`：函数所在的脚本名。
> * `$#`：函数的参数总数。
> * `$@`：函数的全部参数，参数之间使用空格分隔。
> * `$*`：函数的全部参数，参数之间使用变量`$IFS`值的第一个字符分隔，默认为空格，但是可以自定义。
> 
> 如果函数的参数多于9个，那么第10个参数可以用`${10}`的形式引用，以此类推。

```bash
#!/bin/bash

# 定义函数
function alice {
        echo "alice:$@"
        echo "$0: $1 $2 $3"
        echo "$# args"
}
# 调用函数
alice jk kl
```

### return 命令

> `return`命令用于从函数返回一个值。函数执行到这条命令，就不再往下执行了，直接返回了。

```bash
function func_return_value {
  return 10
}
```

> 函数将返回值返回给调用者。如果命令行直接执行函数，下一个命令可以用`$?`拿到返回值。

> `return`后面不跟参数，只用于返回也是可以的。

```bash
function name {
  commands
  return
}
```

### 全局变量和局部变量，local 命令

> <mark>Bash 函数体内直接声明的变量，属于全局变量，整个脚本都可以读取。</mark>

```bash
# 脚本 test.sh
fn () {
  foo=1
  echo "fn: foo = $foo"
}

fn
echo "global: foo = $foo" # 可以读取
```

> 函数里面可以用`local`命令声明局部变量。

```bash
#! /bin/bash
# 脚本 test.sh
fn () {
  local foo
  foo=1
  echo "fn: foo = $foo"
}

fn
echo "global: foo = $foo"  # 无法读取
```

## 数组

> 数组（array）是一个包含多个值的变量。成员的编号从0开始，数量没有上限，也没有要求成员被连续索引。

### 创建数组

> 数组可以采用逐个赋值的方法创建

```bash
ARRAY[INDEX]=value
```

> 上面语法中，`ARRAY`是数组的名字，可以是任意合法的变量名。`INDEX`是一个大于或等于零的整数，也可以是算术表达式。
> 
> 注意数组第一个元素的下标是0， 而不是1。

> 下面创建一个三个成员的数组。

```bash
$ array[0]=val
$ array[1]=val
$ array[2]=val
```

> 数组也可以采用一次性赋值的方式创建。

```bash
ARRAY=(value1 value2 ... valueN)

# 等同于

ARRAY=(
  value1
  value2
  value3
)
```

> 采用上面方式创建数组时，可以按照默认顺序赋值，也可以在每个值前面指定位置。

```bash
$ array=(a b c)
$ array=([2]=c [0]=a [1]=b)

$ days=(Sun Mon Tue Wed Thu Fri Sat)
$ days=([0]=Sun [1]=Mon [2]=Tue [3]=Wed [4]=Thu [5]=Fri [6]=Sat)
```

> 只为某些值指定位置。

```bash
names=(hatter [5]=duchess alice)
```

> 上面例子中，`hatter`是数组的0号位置，`duchess`是5号位置，`alice`是6号位置。

> <mark>没有赋值的数组元素的默认值是空字符串。</mark>

> 定义数组的时候，可以使用通配符。

```bash
$ mp3s=( *.mp3 )
```

> `read -a`命令则是将用户的命令行输入，存入一个数组。

```bash
$ read -a dice
```

### 读取数组

**通过索引获取元素**

> 读取数组指定位置的成员，要使用下面的语法

```bash
$ echo ${array[i]}     # i 是索引
```

> <mark>上面语法里面的花括号是必不可少的</mark>

**遍历数组**

> `@`和`*`是数组的特殊索引，表示返回数组的所有成员。
> 
> `*`和`@`用法是有差异的，下面只介绍`@`

```bash
$ foo=(a b c d e f)
$ echo ${foo[@]}
a b c d e f
```

> 这两个特殊索引配合`for`循环，就可以用来遍历数组。

```bash
for i in "${names[@]}"; do
  echo $i
done
```

> <mark>注意：要把@和*放在双引号之中</mark>

```bash
$ activities=( swimming "water skiing" canoeing "white-water rafting" surfing )
$ for act in ${activities[@]}; \
do \
echo "Activity: $act"; \
done

# 输出
Activity: swimming
Activity: water
Activity: skiing
Activity: canoeing
Activity: white-water
Activity: rafting
Activity: surfing
```

> 上面的例子中，数组`activities`实际包含5个成员，但是`for...in`循环直接遍历`${activities[@]}`，导致返回7个结果。
> 
> 原因为：返回的元素中存在空格，bash误认为是分隔符。
> 
> 为了避免这种情况，一般把`${activities[@]}`放在双引号之中。

**默认位置**

> 如果读取数组成员时，没有读取指定哪一个位置的成员，默认使用`0`号位置。

```bash
$ declare -a foo
$ foo=A
$ echo ${foo[0]}
A
```

### 数组的长度

> 要想知道数组的长度（<mark>即一共包含多少成员</mark>），可以使用下面两种语法。

```bash
${#array[*]}
${#array[@]}
```

**示例**

```bash
$ a[100]=foo

$ echo ${#a[*]}
1
```

> 上面例子中，把字符串赋值给`100`位置的数组元素，这时的数组只有一个元素。

> <mark>注意</mark>，如果用这种语法去读取具体的数组成员，就会返回该成员的字符串长度。

```bash
$ a[100]=foo
$ echo ${#a[100]}
3
```

### 提取数组序号

> `${!array[@]}`或`${!array[*]}`，可以返回数组的成员序号，即哪些位置是有值的。

```bash
$ arr=([5]=a [9]=b [23]=c)
$ echo ${!arr[@]}
5 9 23
$ echo ${!arr[*]}
5 9 23
```

> 利用这个语法，也可以通过`for`循环遍历数组

```bash
arr=(a b c d)

for i in ${!arr[@]};do
  echo ${arr[i]}
done
```

### 提取数组成员

> `${array[@]:position:length}`的语法可以提取数组成员。

```bash
arr=([0]="a" [3]="b" [100]="c")
echo ${arr[@]:0:2}
# 输出
a b
```

> 注意：该语法提取的是从`position`位置开始往后的`length`个元素

### 追加数组成员

> 数组末尾追加成员，可以使用`+=`赋值运算符。它能够自动地把值追加到数组末尾。否则，就需要知道数组的最大序号，比较麻烦。

```bash
$ foo=(a b c)
$ echo ${foo[@]}
a b c

$ foo+=(d e f)
$ echo ${foo[@]}
a b c d e f
```

### 删除数组

> 删除一个数组成员，使用`unset`命令。

```bash
$ foo=(a b c d e f)
$ echo ${foo[@]}
a b c d e f

$ unset foo[2]
$ echo ${foo[@]}
a b d e f
```

> 上面例子中，删除了数组中的第三个元素，下标为2。

> `unset ArrayName`可以清空整个数组。

```bash
$ unset ARRAY

$ echo ${ARRAY[*]}
<--no output-->
```

### 关联数组

> Bash 的新版本支持关联数组。
> 
> 关联数组使用字符串而不是整数作为数组索引。

> `declare -A`可以声明关联数组。

```bash
declare -A colors
colors["red"]="#ff0000"
colors["green"]="#00ff00"
colors["blue"]="#0000ff"
```

> 关联数组必须用带有`-A`选项的`declare`命令声明创建。相比之下，整数索引的数组，可以直接使用变量名创建数组，关联数组就不行。

> 访问关联数组成员的方式，几乎与整数索引数组相同。

```bash
echo ${colors["blue"]}
```

## set命令

> `set`命令是 Bash 脚本的重要环节，可以提高脚本的安全性和可维护性。

> Bash 执行脚本时，会创建一个子 Shell。

```bash
$ bash script.sh
```

> 上面代码中，`script.sh`是在一个子 Shell 里面执行。这个子 Shell 就是脚本的执行环境，Bash 默认给定了这个环境的各种参数。
> 
> `set`命令用来修改子 Shell 环境的运行参数，即定制环境。

### set -u 未定义变量报错

> 执行脚本时，如果遇到不存在的变量，Bash 默认忽略它。
> 
> 大多数情况下，这不是开发者想要的行为，遇到变量不存在，脚本应该报错。
> 
> `set -u`就用来改变这种行为。

```bash
#!/usr/bin/env bash

echo $a # 不存在a
echo bar
```

**使用`set -u`**

```bash
#!/usr/bin/env bash
set -u

echo $a
echo bar
```

**运行结果如下**

```textile
$ bash script.sh
bash: script.sh:行4: a: 未绑定的变量
```

> `-u`还有另一种写法`-o nounset`，两者是等价的。

```bash
set -o nounset
```

### set -x 输出执行的命令

> 默认情况下，脚本执行后，只输出运行结果，没有其他内容。如果多个命令连续执行，它们的运行结果就会连续输出。有时会分不清，某一段内容是什么命令产生的。

> `set -x`用来在运行结果之前，先输出执行的那一行命令。

```bash
#!/usr/bin/env bash
set -x

echo "bar"
```

**输出**

```textile
$ bash script.sh
+ echo bar
bar
```

> 可以看到，执行`echo bar`之前，该命令会先打印出来，行首以`+`表示。

> `-x`还有另一种写法`-o xtrace`。

```bash
set -o xtrace
```

**关闭命令输出**

> 脚本当中如果要关闭命令输出，可以使用`set +x`。

```bash
#!/bin/bash

number=1

set -x
if [ $number = "1" ]; then
  echo "Number equals 1"
else
  echo "Number does not equal 1"
fi
set +x
```

### set -e 错误处理

> 如果脚本里面有运行失败的命令（返回值非`0`），Bash 默认会继续执行后面的命令。这种行为很不利于脚本安全和除错。
> 
> `set -e`解决了这个问题，它使得脚本只要发生错误，就终止执行。

```bash
#!/usr/bin/env bash
set -e

foo
echo bar
```

**执行结果如下**

```textile
$ bash script.sh
script.sh:行4: foo: 未找到命令
```

> 有时开发者希望在命令失败的情况下，脚本继续执行下去。
> 
> 可以使用`set +e`来关闭`-e`选项，在需要时再用`set -e`重新打开`-e`选项。

```bash
set +e
command1
command2
set -e
```

> 上面的需要还有一种方法是使用`command || true`，使得该命令即使执行失败，脚本也不会终止执行。

```bash
#!/bin/bash
set -e

foo || true
echo bar
```

> `-e`还有另一种写法`-o errexit`。

```bash
set -o errexit
```

### set -o pipefail 管道错误处理

> `set -e`有一个例外情况，就是不适用于管道命令。
> 
> 管道符只在乎它后面的命令是否执行成功，也就是说，只要最后一个子命令不失败，管道命令总是会执行成功，因此它后面命令依然会执行，`set -e`就失效了。

```bash
#!/usr/bin/env bash
set -e

foo | echo a
echo bar
```

**输出**

```bash
$ bash script.sh
a
script.sh:行4: foo: 未找到命令
bar
```

> 上面代码中，`foo`是一个不存在的命令，但是`foo | echo a`这个管道命令会执行成功，导致后面的`echo bar`会继续执行。

> `set -o pipefail`用来解决这种情况，只要一个子命令失败，整个管道命令就失败，脚本就会终止执行。

```bash
#!/usr/bin/env bash
set -eo pipefail

foo | echo a
echo bar
```

**运行后，结果如下**

```bash
$ bash script.sh
a
script.sh:行4: foo: 未找到命令
```

### set -E

> 一旦设置了`-e`参数，会导致函数内的错误不会被`trap`命令捕获（参考《trap 命令》）。
> 
> `-E`参数可以纠正这个行为，使得函数也能继承`trap`命令。

```bash
#!/bin/bash
set -e

trap "echo ERR trap fired!" ERR

myfunc()
{
  # 'foo' 是一个不存在的命令
  foo
}

myfunc
```

**输出**

```bash
$ bash test.sh
test.sh:行9: foo：未找到命令
```

> 由于设置了`set -e`，函数内部的报错并没有被`trap`命令捕获，需要加上`-E`参数才可以。

```bash
#!/bin/bash
set -Eeuo pipefail

trap "echo ERR trap fired!" ERR

myfunc()
{
  # 'foo' 是一个不存在的命令
  foo
}

myfunc
```

**输出**

```bash
$ bash test.sh
test.sh:行9: foo：未找到命令
ERR trap fired!
```

### 其他参数

> `set`命令还有一些其他参数。
> 
> * `set -n`：等同于`set -o noexec`，不运行命令，只检查语法是否正确。
> * `set -f`：等同于`set -o noglob`，表示不对通配符进行文件名扩展。
> * `set -v`：等同于`set -o verbose`，表示打印 Shell 接收到的每一行输入。
> * `set -o noclobber`：防止使用重定向运算符`>`覆盖已经存在的文件。
> 
> 上面的`-f`和`-v`参数，可以分别使用`set +f`、`set +v`关闭。

### set 命令总结

> 上面重点介绍的`set`命令的几个参数，一般都放在一起使用。

```bash
# 写法一
set -Eeuxo pipefail

# 写法二
set -Eeux
set -o pipefail
```

> 这两种写法建议放在所有 Bash 脚本的头部。

## 脚本除错

### 常见错误

> 编写 Shell 脚本的时候，一定要考虑到命令失败的情况，否则很容易出错。

```bash
#! /bin/bash

dir_name=/path/not/exist

cd $dir_name
rm *
```

> 上面脚本中，如果目录`$dir_name`不存在，`cd $dir_name`命令就会执行失败。这时，就不会改变当前目录，脚本会继续执行下去，导致`rm *`命令删光当前目录的文件。

> 改进

```bash
cd $dir_name && rm *
```

> 上面脚本中，只有`cd $dir_name`执行成功，才会执行`rm *`。但是，如果变量`$dir_name`为空，`cd`就会进入用户主目录，从而删光用户主目录的文件。

> 下面的写法才是正确的。

```bash
[[ -d "$dir_name" ]] && cd "$dir_name" && rm *
```

> 上面代码中，先判断目录`$dir_name`是否存在，然后才执行其他操作。

## mktemp 命令

> Bash 脚本有时需要创建临时文件或临时目录。常见的做法是，在`/tmp`目录里面创建文件或目录，这样做有很多弊端，使用`mktemp`命令是最安全的做法。

### 临时文件的安全问题

> 直接创建临时文件，尤其在`/tmp`目录里面，往往会导致安全问题。
> 
> `/tmp`目录是所有人可读写的，任何用户都可以往该目录里面写文件。创建的临时文件也是所有人可读的。临时文件使用完毕，应该删除。但是，脚本意外退出时，往往会忽略清理临时文件。
> 
> 生成临时文件应该遵循下面的规则。
> 
> * 创建前检查文件是否已经存在。
> * 确保临时文件已成功创建。
> * 临时文件必须有权限的限制。
> * 临时文件要使用不可预测的文件名。
> * 脚本退出时，要删除临时文件（使用`trap`命令）。

### mktemp 命令的用法

> `mktemp`命令就是为安全创建临时文件而设计的。虽然在创建临时文件之前，它不会检查临时文件是否存在，但是它支持唯一文件名和清除机制，因此可以减轻安全攻击的风险。

> 直接运行`mktemp`命令，就能生成一个临时文件。

**使用**

```bash
$ mktemp
/tmp/tmp.4GcsWSG4vj

$ ls -l /tmp/tmp.4GcsWSG4vj
-rw------- 1 ruanyf ruanyf 0 12月 28 12:49 /tmp/tmp.4GcsWSG4vj
```

> 上面命令中，`mktemp`命令生成的临时文件名是随机的，而且权限是只有用户本人可读写。

> Bash 脚本使用`mktemp`命令的用法如下

```bash
#!/bin/bash

TMPFILE=$(mktemp)
echo "Our temp file is $TMPFILE"
```

> 为了确保临时文件创建成功，`mktemp`命令后面最好使用 OR 运算符（`||`），保证创建失败时退出脚本。

```bash
#!/bin/bash

TMPFILE=$(mktemp) || exit 1
echo "Our temp file is $TMPFILE"
```

> 为了保证脚本退出时临时文件被删除，可以使用`trap`命令指定退出时的清除操作。

```bash
#!/bin/bash

trap 'rm -f "$TMPFILE"' EXIT

TMPFILE=$(mktemp) || exit 1
echo "Our temp file is $TMPFILE"
```

### mktemp 命令的参数

> `-d`参数可以创建一个临时目录。

```bash
$ mktemp -d
/tmp/tmp.Wcau5UjmN6
```

> `-p`参数可以指定临时文件所在的目录。
> 
> 默认是使用`$TMPDIR`环境变量指定的目录，如果这个变量没设置，那么使用`/tmp`目录。

```bash
$ mktemp -p /home/ruanyf/
/home/ruanyf/tmp.FOKEtvs2H3
```

> `-t`参数可以指定临时文件的文件名模板，模板的末尾必须至少包含三个连续的`X`字符，表示随机字符，建议至少使用六个`X`。默认的文件名模板是`tmp.`后接十个随机字符。

```bash
$ mktemp -t mytemp.XXXXXXX
/tmp/mytemp.yZ1HgZV
```

## trap 命令

> `trap`命令用来在 Bash 脚本中响应系统信号。
> 
> 最常见的系统信号就是 SIGINT（中断），即按 Ctrl + C 所产生的信号。`trap`命令的`-l`参数，可以列出所有的系统信号（输出的信号都有一个前缀`SIG`，使用时需要把这个前缀去掉）。

> `trap`的命令格式如下：

```bash
$ trap [动作] [信号1] [信号2] ...
```

> 上面代码中，“动作”是一个 Bash 命令，“信号”常用的有以下几个。
> 
> * HUP：编号1，脚本与所在的终端脱离联系。
> * INT：编号2，用户按下 Ctrl + C，意图让脚本终止运行。
> * QUIT：编号3，用户按下 Ctrl + 斜杠，意图退出脚本。
> * KILL：编号9，该信号用于杀死进程。
> * TERM：编号15，这是`kill`命令发出的默认信号。
> * EXIT：编号0，这不是系统信号，而是 Bash 脚本特有的信号，不管什么情况，只要退出脚本就会产生。

> `trap`命令响应`EXIT`信号的写法如下

```bash
$ trap 'rm -f "$TMPFILE"' EXIT
```

> 上面命令中，脚本遇到`EXIT`信号时，就会执行`rm -f "$TMPFILE"`。

**使用案例**

```bash
#!/bin/bash

trap 'rm -f "$TMPFILE"' EXIT

TMPFILE=$(mktemp) || exit 1
ls /etc > $TMPFILE
if grep -qi "kernel" $TMPFILE; then
  echo 'find'
fi
```

> <mark>注意</mark>，`trap`命令必须放在脚本的开头。否则，它上方的任何命令导致脚本退出，都不会被它捕获。

> 如果`trap`需要触发多条命令，可以封装一个 Bash 函数。

```bash
function egress {
  command1
  command2
  command3
}

trap egress EXIT
```

Bash 启动环境
=========

Session
-------

> 用户每次使用 Shell，都会开启一个与 Shell 的 Session（对话）。
> 
> Session 有两种类型：登录 Session 和非登录 Session，也可以叫做 login shell 和 non-login shell。

### 登录 Session

> 登录 Session 一般进行整个系统环境的初始化，启动的初始化脚本依次如下。
> 
> * `/etc/profile`：所有用户的全局配置脚本。
> * `/etc/profile.d`目录里面所有`.sh`文件
> * `~/.bash_profile`：用户的个人配置脚本。如果该脚本存在，则执行完就不再往下执行。
> * `~/.bash_login`：如果`~/.bash_profile`没找到，则尝试执行这个脚本（C shell 的初始化脚本）。如果该脚本存在，则执行完就不再往下执行。
> * `~/.profile`：如果`~/.bash_profile`和`~/.bash_login`都没找到，则尝试读取这个脚本（Bourne shell 和 Korn shell 的初始化脚本）。

> Linux 发行版更新的时候，会更新`/etc`里面的文件，比如`/etc/profile`，因此不要直接修改这个文件。如果想修改所有用户的登陆环境，就在`/etc/profile.d`目录里面新建`.sh`脚本。

> 如果想修改你个人的登录环境，一般是写在`~/.bash_profile`里面。下面是一个典型的`.bash_profile`文件。

```bash
# .bash_profile
PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/bin
PATH=$PATH:$HOME/bin

SHELL=/bin/bash
MANPATH=/usr/man:/usr/X11/man
EDITOR=/usr/bin/vi
PS1='\h:\w\$ '
PS2='> '

if [ -f ~/.bashrc ]; then
. ~/.bashrc
fi

export PATH
export EDITOR
```

> 这个脚本定义了一些最基本的环境变量，然后执行了`~/.bashrc`。

### 非登录 Session

> 非登录 Session 是用户进入系统以后，手动新建的 Session，这时不会进行环境初始化。比如，在命令行执行`bash`命令，就会新建一个非登录交互式 Session。

> 非登录交互式 Session 的初始化脚本依次如下。
> 
> * `/etc/bash.bashrc`：对全体用户有效。
> * `~/.bashrc`：仅对当前用户有效。
> 
> <mark>注意</mark>，一个非登录非互动 Session的 Bash 环境是不会调用`~/.bashrc`的。

### 总结

| 模式             | 登录 (Login) | 交互 (Interactive) | 典型场景                                      | 加载的配置文件                                                           |
| -------------- | ---------- | ---------------- | ----------------------------------------- | ----------------------------------------------------------------- |
| **1. 交互式登录**   | ✅ 是        | ✅ 是              | 通过 SSH 登录、或虚拟控制台 (Ctrl+Alt+F2) 登录         | `/etc/profile`, `~/.bash_profile`, `~/.bashrc` (通常由 profile 文件调用) |
| **2. 交互式非登录**  | ❌ 否        | ✅ 是              | **在终端里直接输入 `bash`**，或打开一个新的终端窗口           | `~/.bashrc`                                                       |
| **3. 非交互式登录**  | ✅ 是        | ❌ 否              | 很少见，如 `bash --login -c 'echo'`            | `/etc/profile`, `~/.bash_profile`                                 |
| **4. 非交互式非登录** | ❌ 否        | ❌ 否              | **运行脚本 `./script.sh`**，或 `bash -c 'echo'` | **无** (默认不加载任何 `~/.bashrc`)                                       |

命令提示符
=====

> 用户进入 Bash 以后，Bash 会显示一个命令提示符，用来提示用户在该位置后面输入命令。

环境变量 PS1
--------

> 命令提示符通常是美元符号`$`，对于根用户则是井号`#`。这个符号是环境变量`PS1`决定的，执行下面的命令，可以看到当前命令提示符的定义。

```bash
$ echo $PS1
```

> 命令提示符的定义，可以包含特殊的转义字符，表示特定内容。
> 
> * `\a`：响铃，计算机发出一记声音。
> * `\d`：以星期、月、日格式表示当前日期，例如“Mon May 26”。
> * `\h`：本机的主机名。
> * `\H`：完整的主机名。
> * `\j`：运行在当前 Shell 会话的工作数。
> * `\l`：当前终端设备名。
> * `\n`：一个换行符。
> * `\r`：一个回车符。
> * `\s`：Shell 的名称。
> * `\t`：24小时制的`hours:minutes:seconds`格式表示当前时间。
> * `\T`：12小时制的当前时间。
> * `\@`：12小时制的`AM/PM`格式表示当前时间。
> * `\A`：24小时制的`hours:minutes`表示当前时间。
> * `\u`：当前用户名。
> * `\v`：Shell 的版本号。
> * `\V`：Shell 的版本号和发布号。
> * `\w`：当前的工作路径。
> * `\W`：当前目录名。
> * `\!`：当前命令在命令历史中的编号。
> * `\#`：当前 shell 会话中的命令数。
> * `\$`：普通用户显示为`$`字符，根用户显示为`#`字符。
> * `\[`：非打印字符序列的开始标志。
> * `\]`：非打印字符序列的结束标志。

> 举例来说，`[\u@\h \W]\$`这个提示符定义，显示出来就是`[user@host ~]$`（具体的显示内容取决于系统）。

```bash
[user@host ~]$ echo $PS1
[\u@\h \W]\$
```

> 改写`PS1`变量，就可以改变这个命令提示符。

```bash
$ PS1="\A \h \$ "
17:33 host $
```

> 注意，`$`后面最好跟一个空格，这样的话，用户的输入与提示符就不会连在一起。
