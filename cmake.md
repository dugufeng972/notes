# c/c++编译的过程

预处理：替换宏，展开头文件，去掉注释
编译：将预处理后的代码编译成汇编代码
汇编：汇编器将汇编代码生成目标文件（Windows上的.obj linux上的.o文件）
链接：将目标文件链接到一起，生成可执行文件

# makefile

> 当项目比较大时，使用命令生成可执行文件比较困难，可以使用makefile来帮助生成，但是makefile是依赖于平台的，不同平台的makefile不太一样，而makefile的编写比较困难，所以当在不同平台编译时，makefile同样不太好用。

## makefile的规则

```makefile
# 目标文件是要生成的文件
# 依赖文件是生成这个文件需要哪些文件
# 命令是生成目标文件要使用的命令
目标文件 : 依赖文件
    要执行的命令
    ...
```



# cmake

> cmake比makefile更高级，它是平台无关的，cmake最终也是生成对应平台的makefile

## 使用

> cmake在一个名为`CMakeLists.txt`文件中编辑

### 注释

```cmake
# 行注释

#[[
    块注释
]]
```

### 编写一个简单的cmakelists

```cmake
# 指定cmake最低版本
cmake_minimum_required(VERSION 3.16)

# 指定项目名字，可选，如果不加可能会有警告
project(CMakeDemo)

# add_executable：定义工程会生成一个可执行程序
# add_executable(可执行程序名 源文件名称) 源文件可以是1个或者多个
add_executable(app main.cpp add.cpp sub.cpp)

# 执行cmake命令，生成makefile或者ninja
# cmake CMakeLists.txt文件所在路径
# 例如：CMakeLists.txt在当前目录中，所以
# cmake .
```

### cmake中set的使用

> set在cmake中用于定义遍历，cmake中所有变量都是字符串类型

**set语法**

```cmake
# [] 中的参数都是可选值
# VAR - 变量名
# VALUE - 变量值
set(VAR [VALUE] [CACHE TYPE DOCSTRING])

# 使用变量
${VAR}
```

> 上面的简单案例中，一个个手写源文件名称很容易出现错误

```cmake
# 很容易出现错误
add_executable(app main.cpp add.cpp sub.cpp)

# 使用set
set(SRC_LIST add.cpp main.cpp sub.cpp)
add_executable(app ${SRC_LIST})
```

#### 使用set指定使用C++标准

在使用命令编译时，使用下述方法指定标准

```sh
g++ *.cpp -std=c++11 -o app
```

> C++标准对应的宏为`DCMAKE_CXX_STANDARD`，在cmake中指定C++标准有两种方式

**1. 通过set命令**

```cmake
# 指定标准C++11
set(CMAKE_CXX_STANDARD 11)
# 指定标准C++14
set(CMAKE_CXX_STANDARD 14)
```

**2. 在使用cmake命令时，指定宏的值**

```sh
cmake . -DCMAKE_CXX_STANDARD=11
```

#### 使用set指定可执行文件输出的路径

> 在cmake中指定可执行文件程序输出的路径也对应一个宏，叫做`EXECUTABLE_OUTPUT_PATH`,它的值还是通过`set`设置

> ==如果设置的路径不存在，cmake会自动创建==

```cmake
# 推荐设置绝对路径，可以不用
set(HOME /home/ddw)
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
```

### 搜索文件

==搜索文件得到的结果都是文件的绝对路径==

> 如果一个项目里的源文件很多，在写cmake的时候需要将一个个源文件名给列出来，这个不太现实，cmake提供了搜索文件的命令

**1. 方式一 aux_source_directory**

```cmake
# 语法格式
# dir：要搜索的目录
# var：将dir目录下的原件存储到var中(如果变量不存在会自动创建)
aux_source_directory(<dir> <var>)

aux_source_directory(${HOME}/src SRC_LIST)
add_executable(app SRC_LIST)
```

**2. 方式二 file**

> GLOB：将指定目录下搜索到的满足条件的所有文件名生成一个列表，并将其存储到变量中

> GLOB_RECURSE：递归搜索指定目录，将搜索到的满足条件的文件名生成一个列表，并存储到变量中

```cmake
file(GLOB/GLOB_RECURSE 变量名 要搜索的文件路径和文件类型)

# 使用案例
file(GLOB MAIN_SRC /home/src/*.cpp)
file(GLOB MAIN_INCLUDE /home/include/*.h)
```

### 指定头文件

> 在项目中，很多时候需要将源文件对应的头文件存放在与源文件不同位置的目录中，这时候需要将头文件路径指定出来，这样才能保证编译器能够找到这些头文件。

```cmake
# 指定头文件路径
include_directories(${DIR}/include)
```

### 通过cmake制作库文件

> 库文件不需要main函数

#### 制作库

> 在linux中，静态库名字分为三部分：`lib` + `库名字` + `.a`，此处只需要指定出库的名字就可以了，另外两部分在生成该文件的时候会自动补全

> 在Windows中虽然库名和linux格式不同，但也只需要指定出名字即可。 

```cmake
# 语法，源文件可以是一个也可以是多个
# 制作动态库
add_library(库名称 STATIC 源文件1 [源文件2] ...)

# 制作动态库
add_library(库名称 SHARED 源文件1 [源文件2] ...)
```

**案例**

```cmake
# 指定cmake最低版本
cmake_minimum_required(VERSION 3.16)

# 指定项目名字，可选，如果不加可能会有警告
project(CMakeDemo)

set(DIR C:\\Users\\dwl\\Desktop\\cpp\\m_cmake)
set(DCMAKE_CXX_STANDARD 11)

# 指定头文件路径
include_directories(${DIR}/include)

file(GLOB SRC ${DIR}/src/*.cpp)

add_library(calc SHARED ${SRC})
```

#### 指定库生成输出路径

> 宏`LIBRARY_OUTPUT_PATH`用于指定库输出路径,这个宏对应静态库文件和动态库文件都适用。

```cmake
set(LIBRARY_OUTPUT_PATH /home/lib)
```

### 链接库文件

==使用cmake函数去链接库时，只需要库的名字，而file命令得到的是库的地址，所以不能使用file==

==链接库，更推荐`target_link_libraries`命令，使用该命令链接静态库和动态库没有区别==

==`target_link_libraries`要放在生成可执行文件命令后面==

#### 链接静态库

==静态库会被打包到可执行文件里==

> 参数(静态库名)：
> 
> - 可以是全名：`libxxx.a`
> - 也可以是掐头(`lib`)去尾(`.a`)之后的名字`xxx`

```cmake
# 这个只能链接静态库，静态库可以有一个或多个
link_libraries(静态库名1 [静态库名2] ...)
```

> 如果这个库是一个第三方库，可能会出现静态库找不到的情况，需要将库的路径指定出来

```cmake
# 库路径可以有多个
link_directories(库路径 [库路径1] ...)
```

**链接静态库的案例**

```cmake
# 指定cmake最低版本
cmake_minimum_required(VERSION 3.16)

# 指定项目名字，可选，如果不加可能会有警告
project(CMakeDemo)

# add_executable：定义工程会生成一个可执行程序
# add_executable(可执行程序名 源文件名称) 源文件可以是1个或者多个
# set(SRC main.cpp add.cpp sub.cpp)

set(DIR C:\\Users\\dwl\\Desktop\\cpp\\m_cmake)
set(EXECUTABLE_OUTPUT_PATH ${DIR}/bin)
set(DCMAKE_CXX_STANDARD 11)

# 指定头文件路径
include_directories(${DIR}/include)

file(GLOB SRC ${DIR}/*.cpp)
link_directories(${DIR}/lib)
link_libraries(calc)

add_executable(app ${SRC})
```

#### 链接动态库

==这里的命令也能链接静态库，推荐这里的命令，不管链接静态库还是动态库，都放在生成可执行文件命令后面==

> 目标：指定要加载动态库的文件的名字
> 
> - 该文件可能是一个源文件
> - 该文件可能是一个动态库文件
> - 该文件可能是一个可执行文件
> 
> 权限(PRIVATE | PUBLIC | INTERFACE)：动态库的访问权限，默认是`public`
> 
> - 如果各个动态库之间没有依赖关系，无需做任何设置，三者没有区别，一般无需指定
> - `动态库的链接具有传递性`，如果动态库A链接了动态库B、C，动态库D链接了动态库A，此时动态库D相当于也链接了动态库B、C，并可以使用动态库B、C中定义的方法
> - `PUBLIC`: 上述的传递性，要求是权限必须是public
> - `PRIVATE`: 被private修饰的库，没有传递性，不能被第三方感知

```cmake
target_link_libraries(
    目标 
    权限 动态库名
    权限 动态库名1
    ...
)
```

> 链接动态库的命令应该放在生成可执行文件命令后面，因为动态库不会打包进可执行文件

```cmake
# 生成可执行文件
add_executable(app ${SRC})
# 链接动态库，动态库名也遵循掐头去尾
target_link_libraries(app 动态库名)
```

### 日志

> mode(日志类型):
> 
> - (无)：重要消息
> - STATUS：非重要消息
> - WARNING：CMake警告，继续处理
> - AUTHOR_WARNING：CMake警告(dev)，继续处理
> - SEND_ERROR：CMake错误，继续处理，但跳过生成
> - FATAL_ERROR：CMake错误，停止处理和生成

```cmake
# 打印一条消息
message([<mode>] "message text" ...)
```

### 字符串操作

> cmake中所有变量都是字符串类型，所以不可避免的要进行很多字符串操作

#### 使用set命令进行字符串拼接

```cmake
# 实际上将多个变量值存入一个新的变量名
set(拼接后的变量名 ${变量名1} ${变量名2} ...)
```

#### list命令进行字符串操作

**1. 使用list命令进行字符串追加**

> `list`命令比`set`要更强大，字符串拼接只是它的其中一个功能，所以需要在第一个参数指定要进行的操作，`APPEND`表示进行数据追加，`target_str`表示目标字符串，`element`表示要被追加的字符串（一个或多个）

```cmake
list(APPEND <target_str> [<element> ...])
```

**2. 字符串移除**

> 案例：在使用`file`命令搜索某个目录下的所有文件，其中有些源文件不是我们所需要的，需要将其移除。
> 
> 例如：目录下有三个源文件：`main.cpp`、`add.cpp`、`sub.cpp`，我们想编译一个动态库，所以需要将`main.cpp`排除

> 指定操作`REMOVE_ITEM`，指定源字符串`target_str`，指定要被移除的字符串`value`(可以有一个或多个)

```cmake
list(REMOVE_ITEM <target_str> <value1> [<value2> ...])
```

**3. list其它操作**

> list很像js中的数组，可以获取长度、在指定位置插入元素、指定位置元素移除 ...

### 在cmake中自定义宏

> 在进行程序测试的时候，我们可以在代码中添加一些自定义宏，通过这些宏来控制代码是否生效，如下所示：

```cpp
#include <stdio.h>

int main() {
    int a = 10;
#ifdef DEBUG
    printf("debug\n");
#endif
    printf("c\n");
    return 0;
}
```

> 通过命令传递宏`gcc test.cpp DEBUG -o test`

> 在cmake中，通过如下方式添加宏定义(一个或多个)

```cmake
add_definitions(-D宏名称 [-D宏名称 ...])
```

### 嵌套的cmake

> 如果项目很大，只有一个cmake文件很难管理，有一种方式就是给每个源码目录添加一个cmake文件（在每一个模块里添加一个cmake，意思是最后将模块编译成一个静态或动态库链接到main文件里）

```txt
project/
├── CMakeLists.txt              # 主 CMake 文件
├── include/                    # 公共头文件
├── src/                        # 主程序源码
│   ├── CMakeLists.txt
│   └── main.cpp
├── math/                       # 数学库模块
│   ├── CMakeLists.txt
│   ├── include/math/
│   │   ├── add.h
│   │   └── sub.h
│   └── src/
│       ├── add.cpp
│       └── sub.cpp
├── utils/                      # 工具库模块
│   ├── CMakeLists.txt
│   ├── include/utils/
│   │   └── helper.h
│   └── src/
│       └── helper.cpp
└── bin/                        # 可执行文件输出目录
```

> 根节点 `CMakeLists.txt` 中的变量全局有效
> 父节点 `CMakeLists.txt` 中的变量可以在子节点中使用
> 子节点 `CMakeLists.txt` 中的变量只能在当前节点中使用

#### 添加子目录

> cmake中父子节点使用如下方式进行建立

> `source_dir`：指定了`CMakeLists.txt`和代码文件的位置，其实就是指定子目录
> `binary_dir`：指定了输出文件的路径，一般不需要指定，忽略即可
> `EXCLUDE_FROM_ALL`：在子路径下的目标默认不会被包含到父路径的`ALL`目标里，并且也会被排除在IDE工程文件之外，用户必须显示构建在子路径下的目标

```cmake
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```
