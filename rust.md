# cargo

> rust的包管理器

## 使用cargo创建项目

> 创建项目：`cargo new 项目名`
> 
> - 会创建一个新的目录 以项目名命名 目录里包含以下内容
>   
>   - cargo.toml
>   
>   - src目录
>     
>     - main.rs
>   
>   - 以该目录初始化了一个git仓库
> 
> 创建库：`cargo new 库名 --lib`

### cargo.toml

> 该文件是项目的配置文件，包含以下内容
> 
> - `[package]`：是一个区域标题，表示下方内容是用来配置包（package）的
>   
>   - name，项目名
>   
>   - version，项目版本
>   
>   - authors，项目作者
>   
>   - edition，使用的rust版本
> 
> - `[dependencies]`：表示下方是项目的依赖
>   
>   - 添加依赖：在该区域下面，写入要引入的包名和版本号，下面引入rand包，版本号是0.3.14
>     
>     - 如：`rand = "0.3.14"`
> 
> - 在rust中，模块或者包或者库称作`crate`

## 添加依赖

> 命令：`cargo add 包名`

### src

> 源代码目录

## 构建cargo项目

> 构建命令：`cargo build`
> 
> 不加任何参数，这是为开发构建的，不是为了发布给用户构建的

## 构建并运行cargo项目

> 命令：`cargo run`

## cargo check

> - cargo check，检查代码，确保能通过编译，但是不产生任何可执行文件
> 
> - cargo check 要比cargo build快的多
>   
>   - 编写代码的时候可以连续反复的使用cargo check检查代码，提高效率

## 为发布构建

> 构建命令：cargo build --release
> 
> - 编译时会进行优化
>   
>   - 代码会运行的更快，但是编译时间更长
> 
> - 会在target/release而不是target/debug生成可执行文件

## 配置toml profile

### release profile

> - release profile:
>   
>   - 是预定义的
>   
>   - 可自定义：可使用不同的配置，对代码编译拥有更多的控制
> 
> - 每个profile的配置都独立于其它的profile
> 
> - Cargo主要的两个profile：
>   
>   - dev profile：适用于开发，cargo build
>   
>   - release profile：适用于发布，cargo build --release

### 自定义profile

> - 针对每个profile，Cargo都提供了默认的配置
> 
> - 如果想自定义`xxx profile`的配置：
>   
>   - 可以在`Cargo.toml`里添加`[profile.xxx]`区域，在里面覆盖默认配置的子集

```toml
[package]
name = "minigrep"
version = "0.1.0"
edition = "2024"

[dependencies]

#dev profile
[profile.dev]
opt-level = 0

# release profile
[profile.release]
opt-level = 3

# 自定义profile
[profile.xxx]
```

## 发布crate

> - 可以发布代码到crates.io共享自己的代码

## 文档注释

> - 文档注释：用于生成文档
>   
>   - 生成HTML文档
>   
>   - 显示公共API的文档注释：如何使用API
>   
>   - 使用///
>   
>   - 支持md
>   
>   - 放置在被说明条目之前
> 
> 相当于内置了OpenAPI

### 生成HTML文档的命令

> - cargo doc
>   
>   - 它会运行rustdoc工具（自带）
>   
>   - 把生成的html文档放在target/doc目录下
> 
> - cargo doc --open：
>   
>   - 构建当前crate的文档（也包含crate依赖项的文档）
>   
>   - 在浏览器打开文档

### 文档与测试

> 运行cargo test会把文档注释中的示例代码作为测试来运行

## cargo工作空间

> - cargo工作空间：帮助管理多个互相关联且需要协同开发的crate
> 
> - cargo工作空间是一套共享同一个cargo.lock和输出文件夹的包

### 创建工作空间

> - 例：有一个二进制crate（有main.rs），两个库crate（没有main.rs）
>   
>   - 二进制crate：main函数，依赖于其它2个库crate
>   
>   - 其中一个库crate提供add_one函数
>   
>   - 另一个库crate提供add_two函数



> - 创建步骤：
>   
>   - 创建一个目录
>   
>   - 进入目录，创建一个`Cargo.toml`文件，文件内写入以下内容





## 安装二进制crate

> cargo install：这是安装一个可以执行应用程序
> 
> cargo add：这是为项目安装一个依赖

# 标识符

## 变量

> - 声明变量使用`let`关键字
> 
> - 默认情况下，变量是不可变的（immutable）
> 
> - 声明变量时，在变量前面加上mut，就可以使变量可变
> 
> - 只能声明在局部作用域中

```rust
fn main() {
    let a = 90;    // 不可变
    // a = 0;    ❌
    let mut b = 80;
    b = 23;    // 正确
}
```

## 常量

> - 常量（constant），常量在绑定值以后也是不可变的，但是它与不可变的变量有很多区别：
>   
>   - 不可以使用`mut`，常量永远都是不可变的
>   
>   - 声明常量使用const关键字，它的类型必须被标注
>   
>   - 常量可以在任何作用域内进行声明，包括全局作用域
>   
>   - 常量值可以绑定到常量表达式，无法绑定到函数的调用结果或只能在运行时才能计算出的值
> 
> - 在程序运行期间，常量在其声明的作用域内一直有效
> 
> - 命名规范：rust里常量推荐使用全大写字母，每个单词之间使用下划线分开，如：`MAX_POINTS`

```rust
const MAX_POINTS:u32 = 90;
fn main() {
    const MIN_POINTS : u32 = 10;
    println!("{}", MAX_POINTS)
    // const mut CON : u32 = 90;    ❌，不能使用mut
    // const CON = 90;    ❌，必须使用类型标注
}
```

## 隐藏（Shadowing）

> - 可以使用相同的名字声明新的变量，新的变量就会shadow（隐藏）之前声明的同名变量
>   
>   - 在后续的代码中这个变量名代表的就是新的变量
> 
> - shadow和把变量标记为mut是不一样的：
>   
>   - 如果不使用`let`关键字，那么重新给非mut的变量赋值会导致编译时错误
>   
>   - 而使用`let`声明的同名新变量，也是不可变的
>   
>   - 使用`let`声明的同名新变量，它的类型可以与之前不同

```rust
let a = 90;
let a = a + 32;    // 隐藏之前的a
let a = format!("{}{}", "dds", a);    // 隐藏之前的a，可以类型不同
```

## 数据类型

> 这里介绍的基本类型和复合类型，都是在栈上分配空间的

### 基本类型

> rust有四个主要的基本类型：
> 
> - 整数类型
> 
> - 浮点类型
> 
> - 布尔类型
> 
> - 字符类型

#### 整数类型

> - 无符号整数
> 
> - 有符号整数
> 
> <img title="" src="./pic/rust/屏幕截图 2026-05-21 182535.png" alt="">

##### isize和usize类型

> - 分别是有符号和无符号整数，但是位数是根据设备的不同而有所不同
> 
> - isize和usize类型的位数由计算机架构决定：
>   
>   - 如果是64位计算机，那就是64位
>   
>   - 如果是32位，那就是32位

```rust
let i1 = 78usize;
let i2: usize = 29;
let y: isize = -89;
let y2 = -9isize;
```

##### 整数字面量

> - 除了byte类型外，所以的数值字面值都允许使用类型后缀
>   
>   - 如：57u8
> 
> - 整数的默认类型是i32

```rust
let a = 42;         // 十进制（默认 i32）
let b = 0xff;       // 十六进制（255）
let c = 0o52;       // 八进制（42）
let d = 0b101010;   // 二进制（42）
let e = 1_000_000;  // 下划线分隔（提高可读性）
let f = 42u32;      // 后缀指定类型（u32）
let g = 42i64;      // 后缀指定类型（i64）
let b = b'A';    // 字节类型，本质是u8类型
```

#### 浮点类型

> - rust有两种基础的浮点类型
>   
>   - f32，32位，单精度
>   
>   - f64，64位，双精度
> 
> - f64是默认浮点类型

#### 字符类型

> - rust中char类型被用来描述语言中最基本的单个字符（unicode字符），可以表示比ASCII多得多的字符内容，如：中文单字。
> 
> - 字符类型的字面值使用单引号
> 
> - 占用4字节大小
> 
> - 是unicode标量值

```rust
let c1 : char = '丁';
let c1 : char = '😀';
```

### 复合类型 -- 栈上分配

> - 复合类型可以将多个值放在一个类型里
> 
> - rust提供了两种基础的复合类型：元组（tuple）、数组

#### 元组（tuple）

> - tuple可以将多个类型的多个值放在一个类型里
> 
> - tuple的长度是固定的：一旦声明就无法改变

##### 创建tuple

> - 在小括号里，将值用逗号分开
> 
> - tuple中的每个位置都对应一个类型，tuple中各元素的类型不必相同

```rust
let t: (i32, u32, char) = (-2, 32, '😀');
let t2 = (-2, 32, '😀');
// 声明一个可变的元组
let mut mt : (i32, i32, i32) = (-2, 32 , 89);
mt.0 = 90;    // 改变单个值
mt = (-3, 34, 'd');    // 改变整个元组
```

##### 获取tuple的元素值

> - 可以使用模式匹配来解构一个Tuple来获取元素的值
> 
> - 在tuple变量使用点标记法，后面接元素的索引号

```rust
let t: (i32, u32, char) = (-2, 32, '😀');
let (m, n, p) = t;    // m=-2 n=32 p='😀'
let a = t.0;
```

#### 数组

```rust
// 逐个指定值
let arr = [1,2,3,4];
// 使用相同值填充
let arr = [3.14; 4];    // [3.14, 3.14, 3.14, 3.14]

// 可变数组
let mut ma = [1,2,3];
ma = [4,5,6];
ma[0] = 0;
```

##### 数组的类型

> - 数组的类型以这种形式表示：[类型；长度]，如：`let arr:[i32;2] = [1,2];`

# 函数

## rust表达式

> - Rust 是一门**以表达式为中心**的语言，这意味着它的绝大多数代码结构都会产生一个值。
> 
> - 语句与表达式的区别
>   
>   - **表达式（Expression）**：会计算并**返回一个值**。比如 `5 + 6` 会算出 `11`
>   
>   - **语句（Statement）**：执行某些操作，但**不返回值**（或者说返回一个空元组 `()`）
> 
> - rust中常见的表示式类型
>   
>   - 字面量与运算表达式
>     
>     - 比如 `42`、`"hello"`、`true` 都是字面量表达式；`1 + 2`、`a > b` 是运算表达式，它们都会算出一个具体的值。
>   
>   - 代码块表达式 `{}`
>     
>     - 在 Rust 中，一对大括号 `{}` 本身就是一个表达式。它会执行块内的语句，并**返回块内最后一个表达式的值**（前提是最后一个表达式后面没有分号）
>   
>   - 条件分支表达式 `if / else`
>     
>     - Rust 的 `if` 不是单纯的流程控制，它可以直接用来赋值！这完美替代了其他语言中的三元运算符（`condition ? a : b`）
>   
>   - 循环表达式 `loop`
>     
>     - 普通的 `while` 和 `for` 循环通常返回 `()`，但 `loop` 循环可以通过 `break` 携带一个值跳出，从而让整个循环变成一个有返回值的表达式。

```rust
// 代码块表达式
let y = {
    let x = 3;
    x + 1 // 注意这里没有分号，这个代码块会返回 4
}; // y 的值就是 4

// 条件分支表达式
let condition = true;
// number 的值会根据 if/else 的结果直接确定
let number = if condition { 5 } else { 6 };

// loop循环表达式
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2; // 循环在这里结束，并返回 20
    }
};
// result 的值就是 20
```

## 函数使用

> - 声明函数使用`fn`关键字
> 
> - 针对函数和变量名，rust使用snake case命名规范：
>   
>   - 所有的字母都是小写的，单词之间使用下划线分开、

<mark>块语句的最后一个表达式就是该块语句的返回值，那个表达式不能带分号</mark>

```rust
// 形参的类型必须要指明
fn my_function(str: String) -> String {
    return str;
}
// 函数的最后一个表达式就是函数的返回值
fn my_func() -> i32 {
    2 + 2;
    3    // 返回值，返回3
}
```

# 控制流

## if分支语句

```rust
let num = 3;
if num < 2 {
     println!("ddd");
} else if num < 1 {
     println!("sss");
} else {
    println!("aadd");
}    
```

### if也是表达式

```rust
let flag = true;
// if和else里的类型必须一致
// 块语句里的最后一个表达式就是块语句的返回值
let num = if flag { 5 } else { 6 };
```

## 循环

### loop循环

> - loop关键字：反复执行一块代码，直到主动跳出（break）

```rust
fn my_func() {
    let mut i = 0;
    loop {
        i += 1;
        println!("{}", i);
        if i > 10 {
            break;
        }
    }
}
```

### while循环

> 与其它语言一致

### for循环

```rust
fn my_for() {
    let mut arr = [1, 2, 3, 4];
    for mut el in arr {
        // 修改对原数组无影响
        el += 1;
    }
    for el in arr {
        println!("{}", el);
    }
    for el in arr.iter_mut() {
        // 这里el是&mut i32类型，修改el会对数组参数影响
        *el = *el + 1;
    }
    for el in arr {
        println!("{}", el);
    }
}
```

### Range

> - 在 Rust 中，**Range**（范围）是一种表示从起点到终点之间序列的类型。Rust 提供了多种范围类型，常用于循环、切片索引等场景。
> 
> - 指定一个开始数字和一个结束数字，range可以生成它们之间的数字（不包含结束）

```rust
let range = 1..5;        // 1, 2, 3, 4
let range = 1..=5;       // 1, 2, 3, 4, 5
// 在循环中应用
for number in 2..5 {
    println!("{}", number);
}
```

| 写法            | 类型名                             | 说明                  |
| ------------- | ------------------------------- | ------------------- |
| `start..end`  | `std::ops::Range<T>`            | 半开区间 `[start, end)` |
| `start..=end` | `std::ops::RangeInclusive<T>`   | 闭区间 `[start, end]`  |
| `start..`     | `std::ops::RangeFrom<T>`        | `[start, ∞)`        |
| `..end`       | `std::ops::RangeTo<T>`          | `(-∞, end)`         |
| `..`          | `std::ops::RangeFull`           | 全部范围                |
| `..=end`      | `std::ops::RangeToInclusive<T>` | `(-∞, end]`         |

# 所有权

> Rust 的**所有权**（Ownership）是其最核心、最独特的特性，它让 Rust 在**不需要垃圾回收**的情况下保证**内存安全**。

## 所有权规则

> Rust 的所有权机制**针对所有类型的变量**，但是不同类型在触发“所有权转移（Move）”时的表现是完全不同的。这取决于该类型是否实现了 `Copy` trait。

> **所有权的三大规则**
> 
> - **每个值都有一个变量作为它的所有者（owner）**
> 
> - **同一时刻只能有一个所有者，赋值后会导致先前的变量失去所有权**
> 
> - **当所有者离开作用域，值会被自动释放**（调用 `drop`）
>   
>   
> 
> **rust不会主动创建数据的深拷贝，默认行为是地址的转让**
> 
> **离开作用域自动释放，有点像c++的智能指针**

### 变量和数据交互的方式：移动（Move）

```rust
fn main() {
    let s1 = String::from("str");
    /*
     * s1将所有权转让给s2
     * 底层是：将s1内部的堆内存地址赋值给s2，然后s1失效了，堆上的内容不会复制
     */
    let s2 = s1; // 这里之后无法访问s1
    println!("{}", s2); 
}
```

### 变量和数据交互的方式：克隆（Clone）

> - 如果真想对heap上面的String数据进行深度拷贝，将堆上的内容复制一遍，可以使用clone方法

```rust
fn main() {
    let s3 = String::from("ss");
    let s4 = s3.clone();
    println!("{} {}", s3, s4);
}
```

### Copy trait

> - Rust 的所有权机制**针对所有类型的变量**，但是不同类型在触发“所有权转移（Move）”时的表现是完全不同的。这取决于该类型是否实现了 `Copy` trait。
> 
> - 实现了`Copy trait`的类型**会发生复制，而不是所有权转移**
> 
> - 实现 `Copy` Trait 的类型（简单类型和它们的组合类型（元组））：
>   
>   - **所有基本整数类型**：`i8`, `i32`, `u64`, `usize` 等
>   
>   - **布尔类型**：`bool`
>   
>   - **字符类型**：`char`
>   
>   - **浮点类型**：`f32`, `f64`
>   
>   - **仅包含上述类型的元组和定长数组**
> 
> - 任何需要分配堆内存或者某种资源的都不是Copy的

> **核心本质**：这些类型的数据完全存储在栈上，且大小固定、复制成本极低。Rust 认为为它们设计所有权转移毫无意义，所以直接赋予了自动复制的能力。

## 所有权与函数

> - 在语义上，将值传递给函数和把值赋值给变量是类似的：
>   
>   - 将值传递给函数将发送移动或复制
> 
> - 函数在返回值的过程中同样也会发生所有权的转移

```rust
fn main() {
    let s3 = String::from("ss"); 
    take_owner(s3);    // 发生所有权转让，s3转让所有权   
    let s4 = String::from("s4");
    let s5 = take_owner_back(s4);    // 所有权转移到函数内部，然后转移到s5
}

fn take_owner(str: String) {
    println!("str:{}", str);
}    // 运行结束，释放str所指向的堆空间

fn take_owner_back(str: String) -> String {
    str
}
```

## 引用和借用

### 引用与借用 介绍

**引用**

> - 允许引用某些值而不取得其所有权
> 
> - 引用实际上就是指针，s是指向堆内存的指针，s分配在栈上，sp是引用，它指向s地址，同时它也是分配在栈上
> 
> - 引用是指针，但是在大多数情况下rust会自动解引用
> 
> - rust引用是胖指针，它除了包含基本的数据地址外，还会有一些其它数据。如：切片还会包含数据长度等。

```rust
fn main() {
    let s = String::from("String");
    // 从头开始引用，这样会指向字符串开头地址
    let sp = &s[0..4];
    // 打印出的地址相同
    println!("sp address:{:p}, s address:{:p}", sp.as_ptr(), s.as_ptr());
}
```

**借用**

> - **借用**是引用所体现的行为/概念

```rust
let x = 5;
let r = &x;  // r 是"引用"，这个操作叫"借用"

// 可以说：
// - 创建了一个引用
// - 借用了 x
```

### 引用规则

> - 在任何给定的时刻，只能满足下列条件之一：
>   
>   - 最多有一个可变的引用
>   
>   - 可以有任意数量不可变的引用
>   
>   - 引用必须一直有效
> 
> - <mark>**引用（&T）本身实现了 Copy**</mark>

### 可变引用

> - 和变量一样，引用默认也是不可变的，需要使用`mut`关键字
> 
> - 可变引用有一个重要的限制：在特定作用域内，对某一块数据，只能有一个可变的引用。
>   
>   - 这样可以在编译时防止数据竞争
> 
> - 以下三种行为会发生数据竞争：
>   
>   - 两个或多个指针同时访问同一个数据
>   
>   - 至少有一个指针用于写入数据
>   
>   - 没有使用任何机制来同步对数据的访问
> 
> - 可以通过创建新的作用域，来允许非同时的创建多个可变引用
> 
> - <mark>**可变引用与不可变引用不能共存**</mark>

```rust
// 使用mut修饰引用
fn main() {
    let mut s1 = String::from("str");
    let len = get_length(&mut s1);
    println!("{} {}", s1, len);
}

fn get_length(s: &mut String) -> usize {
    s.push_str("!");
    s.len()
}

// 可变引用与不可变引用不能共存
// 因为可能会造成指针空悬
fn test() {
    let mut v = vec![1, 2, 3, 4, 5];
    let first = &v[0]; // 1. 获得一个指向第一个元素的不可变引用

    v.push(6); // 2. 尝试通过可变引用修改 v (这里编译器会报错！)

    println!("{}", first);
}
```

## 通过引用创建副本

### 1. 对实现了 `Copy` 的类型（自动复制）

```rust
fn main() {
    let num_ref = &42;  // 引用
    let num_copy = *num_ref;  // 解引用创建副本
    println!("原引用: {}, 副本: {}", num_ref, num_copy);

    // 标量类型通常都实现了 Copy
    let x = 10;
    let y = &x;
    let z = *y;  // z 是 x 的副本
    println!("x: {}, y: {}, z: {}", x, y, z);
}
```

### 2. 对实现了 `Clone` 的类型（需要显式克隆）

```rust
fn main() {
    let s = String::from("hello");
    let s_ref = &s;

    // 方法1: 使用 clone()
    let s_clone = s_ref.clone();
    println!("克隆: {}", s_clone);

    // 方法2: 使用 to_string()（String 实现了 ToString trait）
    let s_to_string = s_ref.to_string();

    // 方法3: 使用 to_owned()
    let s_owned = s_ref.to_owned();

    // 注意：这些方法都会进行深拷贝，分配新内存
    println!("原字符串: {}", s);  // s 仍然有效
}
```



## 栈内存 vs 堆内存

> 在rust中，一个值是在stack上还是在heap上对语言的行为有重大影响

> - stack按值的接收顺序来存储，按相反的顺序将它们移除（后进先出）
>   
>   - 添加数据叫入栈
>   
>   - 移除数据叫弹出栈
> 
> - 所有存储在stack上的数据必须拥有已知的固定的大小
>   
>   - 编译时大小未知的数据或运行时大小可能发送变化的数据必须放在heap上
> 
> - heap内存组织性差一点：
>   
>   - 把数据放入heap时，操作系统在heap里找到一块足够大的空间，把它标记为在用，并返回一个指针，指向这个空间的地址
>   
>   - 这个过程称之为heap上分配

## 所有权存在的原因

> - 所有权解决的问题：
>   
>   - 跟踪代码的哪些部分正在使用heap的哪些数据
>   
>   - 最小化heap上的重复数据量
>   
>   - 清理heap上未使用的数据以避免空间不足

# 切片

> 切片是**指向数据集合中一段连续区域的引用**。**它不拥有数据**，只是借用了数据的一部分。

## 字符串切片 &str

> - 字符串切片是指向字符串中一部分内容的引用
> 
> - `str`类型：是动态大小的类型，无法存放在栈上只能放在堆上，所以必须配合指针使用，如：`&str`

```rust
// 形式：[开始索引..结束索引]
fn my_splice() {
    let s: String = String::from("sfjiwfn fwjfie");
    // &str 是切片类型
    let s1: &str = &s[1..5];
    println!("{}", s1);

    // 错误：不能获取 &mut str
    // let slice = &mut s[1..3];
}
```

### 字符串切片的特点

> - rust中字符串是utf-8编码的，因此，像中文字符使用3个字节存储
> 
> - 字符串切片是按字节索引的，不是按字符
> 
> - 可以使用如下方法安全处理

```rust
fn main() {
    let s = String::from("你好，世界！");

    // ⚠️ 注意：按字节索引，不是字符！
    // 中文字符占3个字节
    let slice = &s[0..3];  // "你"
    println!("{}", slice);

    // ❌ 危险：如果切在字符中间会panic
    // let bad_slice = &s[0..2];  //运行时崩溃！
}

// 安全处理
fn main() {
    let s = "Hello 世界";

    // 使用 chars() 处理字符
    let chars: Vec<char> = s.chars().collect();
    let first_char = chars[0];  // 'H'
    let chinese = &chars[6..8];  // ['世', '界']

    println!("第一个字符: {}", first_char);
    println!("中文部分: {:?}", chinese);
}
```

### 字符串字面量是切片

> - 字符串字面量被直接存储在二进制程序中
> - 字符串字面量是不可变的，是常量

### 将字符串切片作为参数传递

> - 一般会将切片作为函数形参类型，这样就可以同时接收`&String`和`&str`类型了

```rust
fn main() {
    let mut s1 = String::from("str");
    test(&s1);
}
fn test(s: &str) {
    println!("{}", s);
}
```

### &str与String的转换

> `&String`可以隐式转换为`&str`

```rust
// &str → String
let s1: &str = "hello";
let s2: String = s1.to_string();
let s3: String = String::from(s1);
let s4: String = s1.to_owned();

// String → &str (自动解引用)
let s: String = String::from("hello");
let slice: &str = &s;  // 隐式转换
let slice2: &str = &s[..];  // 显式切片
```

## 数组/向量的切片

```rust
fn my_num() {
    let arr: [i32; 4] = [1, 2, 3, 4];
    let a1: &[i32] = &arr[1..3];

    // 可变切片
    let mut arr = [1, 2, 3, 4, 5];
    let slice: &mut [i32] = &mut arr[1..4];

    slice[0] = 10;  // 修改原数组
    slice[1] = 20;
    slice[2] = 30;

    println!("{:?}", arr);  // [1, 10, 20, 30, 5]
}
```

# struct

> 结构体：rust中一种自定义的类型，将多个相关数据组合在一起的自定义数据类型

## 定义struct

> - 使用`struct`关键字，并为整个`struct`命名
> 
> - 在花括号内，为所有字段（Field）定义名称和类型

```rust
struct User {
    name: String,
    age: u8,
    email: String,
}
```

## 实例化struct

> - 想要使用struct，需要创建struct的实例：
>   
>   - 为每个字段指定具体值
>   
>   - 无需按声明的顺序进行指定
>   
>   - 变量名与字段名相同时，可以简写
> 
> - struct实例要声明为可变的才可以修改字段

```rust
let user = User {
    name: String::from("yang"),
    age: 20,
    email: String::from("kkk"),
};

// 简写
let name = String::from("name");
let age = 20;
let email = String::from("email");
let u2 = User { name, age, email };

// 声明为可变的
let mut u3 = User { name, age, email };
u3.age = 29;
```

## 访问struct里的字段

> 使用点标记法

```rust
fn my_num() {
    let mut user = User {
        name: String::from("yang"),
        age: 20,
        email: String::from("kkk"),
    };
    user.age = 23;
}

struct User {
    name: String,
    age: u8,
    email: String,
}
```

<mark>注意：</mark>

- 一旦struct的实例是可变的，那么实例中所有的字段都是可变的

## struct作为函数的返回值

```rust
fn build_user(email: String, name: String, age: u8) -> User {
    User { name, age, email }
}
```

## struct更新语法

> 想从某个相同类型的struct实例来创建一个新的实例的时候，可以使用struct更新语法：

```rust
struct User {
    name: String,
    age: u8,
    email: String,
}

fn my_num() {
    let mut user = User {
        name: String::from("yang"),
        age: 20,
        email: String::from("kkk"),
    };
    let name = String::from("name");
    // 注意所有权转让，
    // email 是String类型，会发生所有权转让，user里的email失效了
    let u3 = User { name, ..user };
}
```

<mark>注意：使用更新语法时，可能发生所有权转让</mark>

## Tuple struct

> - 可定义类似tuple的struct，叫做结构体元组（tuple struct）
>   
>   - tuple struct整体有个名，但里面的元素没有名
>   
>   - 适用：想给整个tuple起名，并让它不同于其它tuple，而且又不需要给每个元素起名
> 
> - 定义 tuple struct：使用struct关键字，后边是名字，以及里面元素的类型

```rust
fn mmm() {
    struct Color(i32, i32, i32);
    let c = Color(0, 0, 0);
    println!("{}", c.0);
}
```

## 空结构体(Unit-Like Struct)

> - 可以定义没有任何字段的struct，叫做Unit-Like struct
> 
> - 适用于需要在某个类型上实现某个trait，但是在里面有没有想要存储的数据

```rust
struct Empty;
```

## struct数据的所有权

> - 结构体字段拥有所有权会发生所有权转让
> 
> - 结构体实例对象本身也是拥有所有权的

## struct的方法

> - 方法和函数类似
> 
> - 方法于函数不同之处：
>   
>   - 方法是在struct（或enum、trait对象）的上下文中定义
>   
>   - 第一个参数是self，表示方法被调用的struct实例
> 
> - 方法中的`self`和`Self`：
>   
>   - `self`表示实例自身
>   
>   - `Self`表示当前结构体类型

```rust
struct Size {
    a: u32,
    b: u32,
}
// 定义方法
impl Size {
    // 这里会发生所有权转让
    fn area(self) -> u32 {
        self.a * self.b
    }
    // 这里不会发生所有权转让
    fn aa(&self) -> u32 {
        self.a * self.b
    }
}

struct User {
    name: String,
    age: u8,
}

impl User {
    // 这里Self表示User类型
    fn Build(name: String, age: u8) -> Self {
        User { name, age }
    }
}
fn main() {
    let s1 = Size { a: 20, b: 30 };
    print!("{}", s1.area());
    // println!("{}", s1.a);    ❌，所有权已经转让，无法访问s1
}
```



### 方法调用

> rust在调用方法时会自动引用或解引用

```rust
struct MyStruct {
    value: i32,
}

impl MyStruct {
    fn get_value(&self) -> i32 {
        self.value
    }

    fn set_value(&mut self, value: i32) {
        self.value = value;
    }
}

fn main() {
    let mut s = MyStruct { value: 10 };

    // Rust 自动引用和解引用
    s.set_value(20);        // 等价于 (&mut s).set_value(20)
    let v = s.get_value();  // 等价于 (&s).get_value()

    println!("{}", v);
}
```

### 关联函数（静态方法）

> - 可以在`impl`块里定义不把self作为第一个参数的函数，它们叫做关联函数（不是方法）
>   
>   - 例如：`String::from()`
> 
> - 关联函数通常用于构造器
> 
> - `::`符号
>   
>   - 调用关联函数
>   
>   - 模块创建的命名空间

```rust
#[derive(Debug)]
struct Size {
    a: u32,
    b: u32,
}

impl Size {
    // 这里会发生所有权转让
    fn area(self) -> u32 {
        self.a * self.b
    }

    fn can_hold(&self, other: &Size) -> bool {
        self.a > other.a && self.b > other.b
    }
    // 关联函数
    fn square(num: u32) -> Size {
        Size { a: num, b: num }
    }
}
fn main() {
    // 调用关联函数
    let s2 = Size::square(20);
    println!("{:?}", s2);
}
```

### 多个impl块

> 每个结构体允许拥有多个`impl`块

```rust
struct Point {
    x: f64,
    y: f64,
}

impl Point {
    fn new(x: f64, y: f64) -> Self {
        Self { x, y }
    }

    fn distance_from_origin(&self) -> f64 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}

// 可以分开实现，通常用于组织代码
impl Point {
    fn translate(&mut self, dx: f64, dy: f64) {
        self.x += dx;
        self.y += dy;
    }
}
```

# 枚举

> Rust 的**枚举**（Enum，即 enumeration）是一种定义一个类型，该类型的值可以是**多个不同变体**（variant）中任意一个的数据结构。它比 C、Java 等语言的枚举强大得多，因为每个变体可以携带不同类型和数量的数据。

## 定义枚举

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

## 枚举值

```rust
let four:IpAddrKind = IpAddrKind::V4;
fn test() {
    let four:IpAddrKind = IpAddrKind::V4;
    let six:IpAddrKind = IpAddrKind::V6;
    route(four);
    route(six);
}
fn route(ip_kind: IpAddrKind) {}
```

## 带数据的枚举（Rust 的核心特性）

> **可以让一个类型能够表达多种不同形态的值，且每种形态可以携带完全不同类型和数量的数据**。

```rust
enum IpAddr {
    V4(String),
    V6(String),
    A(i32),
    Move { x: i32, y: i32 },
}
// 声明带有数据的枚举值
let i1 = IpAddr::V4(String::from("str"));
let a = IpAddr::A(32);
```

### 获取枚举中的数据

> 通过match获取，详细参考下面match章节

## 为枚举定义方法

> 与结构体一致

```rust
enum IpAddr {
    V4(String),
    V6(String),
    A(i32),
    Move { x: i32, y: i32 },
}

impl IpAddr {
    fn call(&self) {}
}
```

## Option枚举

> - 定义于标准库中
> 
> - 它包含在prelude（预导入模块）中，可以直接使用Option<T>、Some(T)、None
> 
> - 描述了：某个值可能存在（某种类型）或不存在的情况
> 
> - rust中没有Null，使用了类似Null概念的枚举Option<T>，彻底杜绝了其他语言中常见的**空指针异常**（Null Pointer Exception）

```rust
// option在标准库中的定义
enum Option<T> {
    Some(T),
    None,
}
```

**Option<T>比Null好在哪里**

- Option<T>和T是不同的类型，不可以把Option<T>直接当成T

- 若想使用Option<T>中的T，必须将它转换为T，彻底杜绝了其他语言中常见的**空指针异常**（Null Pointer Exception）

```java
// Java：引用可以为 null
String s = null;
int len = s.length();  // NullPointerException！
```

```rust
let s: Option<String> = None;
// let len = s.len();  // ❌ 编译错误！Option 没有 len 方法

// 必须显式处理 None 的情况
match s {
    Some(s) => println!("长度: {}", s.len()),
    None => println!("没有值"),
}
// 编译器强制你处理了空值情况，不可能出现空指针异常
```

## match

> - 允许一个值与一系列模式进行匹配，并执行匹配的模式对应的代码
> 
> - 模式可以是字面量、变量名、通配符...

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}
fn value_in_cents(coin: Coin) -> u8 {
    // 这里逻辑简单，每个分支都返回一个数字，这个数字作为块表达式的返回值
    // 而块表达式没有分号结尾，所以块表达式的返回值可以函数的返回值
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

### match获取绑定值

> - 匹配的分支可以绑定到被匹配对象的部分值
>   
>   - 因此，可以从enum变体中提取值

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(String),
}
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("{}", state);
            25
        }
    }
}
```

### match匹配分支

> - 一般情况下，match匹配必须穷举所有的可能性
> 
> - 但是，若不想列出所有可能性，可以用`_`通配符替代其余没列出的值

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(String),
}
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        _ => 4,
    }
}
```

## if let

> - match通常用于要关心很多分支的情况下，如果只在乎一种匹配模式而忽略其它情况可以使用`if let`
> 
> - 优势：
>   
>   - 更少的代码，更少的缩进，更少的模板代码
>   
>   - 放弃了穷举的可能
> 
> - 可以把`if let`看作是`match`的语法糖，可以配合`else if`和`else`使用
> 
> - <mark>注意：</mark>变量在后面

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(String),
}
fn if_let() {
    let c = Coin::Dime;
    // 变量在后面，不能写反了
    if let Coin::Penny = c {
        println!("Penny");
    } else if let Coin::Dime = c {
        println!("Nickel");
    } else {
        println!("other");
    }
}
```

# rust的代码组织

> 模块系统：
> 
> - Package（包）：最顶层，一个或多个 crate，有 Cargo.toml
> 
> - crate（单元包）：一个编译单元，产生一个库（依赖）或可执行文件（main.rs）
> 
> - Module（模块）：crate 内部的代码组织单元
> 
> - Path（路径）：为struct、function或module等项命名的方式
> 
> 相比于java中：
> 
> - package是多模块父项目目录
> 
> - crate是项目中的子模块，有main.rs的是一个微服务，没有main.rs的是类似common这样的模块
> 
> - Module

```textile
包（Package）
  ├── Cargo.toml（配置文件）
  └── 一个或多个 crate
       ├── 库 crate（lib.rs）
       └── 二进制 crate（main.rs）
            └── 模块（mod）
                 ├── 函数
                 ├── 结构体
                 ├── 枚举
                 └── 其他模块
```

## Package和Crate

> - Crate的类型：
>   
>   - binary
>   
>   - library
> 
> - Crate Root：
>   
>   - 是源代码文件
>   
>   - Rust编译器从这里开始，组成Crate的根Module
> 
> - 一个Package
>   
>   - 包含一个Cargo.toml，它描述了如何构建这些Crates
>   
>   - 只能包含0-1个library crate
>   
>   - 可以包含任意数量的binary crate
>   
>   - 但必须至少包含一个crate（library或binary）

## 模块 Module

> - Module:
>   
>   - 在一个crate内，将代码进行分组
>   
>   - 增加可读性，易于复用
>   
>   - 控制项目（item）的私有性，<mark>模块内默认情况下是私有的</mark>，可以用`pub`关键字来声明公有
> 
> - 建立module：
>   
>   - module名是文件名或者文件名
>   
>   - mod关键字，是用来创建命名空间的，创建模块不一定需要mod
>   
>   - mod可嵌套
>   
>   - 可包含其他项（struct、enum、常量、trait、函数等）的定义

### 单文件模块

> 注意自己写的模块必须先用`mod 文件名/文件夹名`引入，而用cargo安装的第三方模块不用，已经自动处理好了
> 
> 模块`utils.rs`与`main.rs`同级，在`main.rs`中引入`utils.rs`，案例如下：

```rust
// src/utils.rs
pub fn greet(name: &str) {
    println!("{}", name);
}
// 模块可以嵌套
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

// src/main.rs
// 声明模块，rust会自动找到utils.rs
mod utils;
fn main() {
    utils::greet("ding");
}
```

### 目录模块

> 目录模块必须在目录里放入一个模块入口`mod.rs`这个入口内的函数在模块内是公有的

```rust
// src/utils/hello.rs
pub fn say_hello(name: &str) {
    println!("hello {} !", name);
}

// src/utils/mod.rs
// 声明模块
pub mod hello;
// 模块内公共函数
pub fn init() {
    println!("初始化");
}

// src/main.rs
// 使用utils模块
mod utils;
fn main() {
    utils::say_hello("akll");
}
```

## 路径（Path）

> - 为了在Rust的模块中找到某个条目，需要使用路径
> 
> - 路径的两种形式：
>   
>   - 绝对路径：从crate root开始，使用crate名或字面值crate，crate root相等于src目录
>   
>   - 相对路径：从当前模块开始，使用self，super或当前模块的标识符
> 
> - 路径至少由一个标识符组成，标识符之间使用`::`

```rust
// src/utils.rs
pub fn greet(name: &str) {
    // 调用add_to_waitlist，使用绝对路径
    crate::my_math::modd::front_of_house::hosting::add_to_waitlist();
    println!("{}", name);
}
// 模块可以嵌套
mod front_of_house {
    pub mod hosting {
       pub fn add_to_waitlist() {}
    }
}

// src/main.rs 调用
```

### rust私有边界

> - rust中所有的条目（函数，方法，struct，enum，模块，常量）默认是私有的
> 
> - 父级模块无法访问子模块中的私有条目
> 
> - 子模块里可以使用所有祖先模块中的条目
> 
> - 使用`pub`关键字可以暴露条目

### super关键字

> 表示上级模块

```rust
pub fn add(a: i32, b: i32) -> i32 {
    crate::my_math::modd::front_of_house::hosting::add_to_waitlist();
    a + b
}
mod front_of_house {
    pub fn super_test() {
        super::add(23, 23);
    }
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

### use关键字

> 可以使用use关键字将路径导入到作用域内
> 
> - 仍遵循私有性规则
> 
> use的习惯用法：
> 
> - 函数：将函数的父级模块引入作用域（指定到父级）
> 
> - struct、enum，其它：指定完整路径（指定到本身）
> 
> <mark>跨文件引入本地模块内条目时，必须先声明模块mod 模块名，然后才能用use导入，但是用cargo安装的依赖不需要先声明模块</mark>

```rust
// 将hosting导入当前作用域中
use crate::my_math::modd::front_of_house::hosting;
pub fn add(a: i32, b: i32) -> i32 {
    // crate::my_math::modd::front_of_house::hosting::add_to_waitlist();
    // 不再需要很长的前缀
    hosting::add_to_waitlist();
    a + b
}
mod front_of_house {
    pub fn super_test() {
        super::add(23, 23);
    }
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

> use导入到作用域的路径也是私有的，可以使用pub修饰

```rust
pub use crate::my_math::modd::front_of_house::hosting
```

> 先声明模块，在use导入

```rust
mod my_math;
use my_math::modd;
fn main() {
    let num = my_math::modd::add(2, 3);
}
```

### as关键字

> `as`关键字可以为引入的路径指定本地的别名

```rust
use std::fmt::Result;
use std::io::Result as IoResult;
```

# 集合

## Vector

### 介绍

> - Vec<T>，叫做vector
>   
>   - 由标准库提供
>   
>   - 可存储多个值
>   
>   - 只能存储相同类型的数据
>   
>   - 值在内存中连续存放，存放在堆上，超出作用域后自动释放
>   
>   - vector要遵循所有权机制

### 创建 Vector

> 使用Vec::new()函数

```rust
fn main() {
    let v: Vec<i32> = Vec::new();
}
```

> 使用 vec!宏 创建

```rust
// 方式2：使用 vec! 宏（最常用）
let v2 = vec![1, 2, 3, 4, 5];

// 方式3：重复填充
let v3 = vec![0; 10];  // 10 个 0

// 方式4：从迭代器创建
let v4: Vec<i32> = (1..=5).collect();

// 方式5：with_capacity 预分配（性能优化）
let mut v5: Vec<i32> = Vec::with_capacity(10);
```

### 添加删除元素

**添加元素**

```rust
let mut v = vec![1, 2, 3];

// push：在末尾添加
v.push(4);
println!("{:?}", v);  // [1, 2, 3, 4]

// insert：在指定位置插入
v.insert(1, 10);  // 在索引 1 插入 10
println!("{:?}", v);  // [1, 10, 2, 3, 4]

// extend：追加多个元素
v.extend([5, 6, 7]);
println!("{:?}", v);  // [1, 10, 2, 3, 4, 5, 6, 7]

// append：追加另一个 Vec
let mut v2 = vec![8, 9];
v.append(&mut v2);
println!("{:?}", v);  // [1, 10, 2, 3, 4, 5, 6, 7, 8, 9]
println!("v2 现在为空: {:?}", v2);  // []
```

**删除元素**

```rust
let mut v = vec![1, 2, 3, 4, 5];

// pop：删除并返回最后一个元素
let last = v.pop();
println!("{:?}, v: {:?}", last, v);  // Some(5), v: [1, 2, 3, 4]

// remove：删除并返回指定索引的元素
let removed = v.remove(1);
println!("移除: {}, v: {:?}", removed, v);  // 移除: 2, v: [1, 3, 4]

// swap_remove：删除并返回，但用最后一个元素填充（顺序无关时性能更好）
let mut v2 = vec![1, 2, 3, 4];
let removed = v2.swap_remove(1);
println!("移除: {}, v: {:?}", removed, v2);  // 移除: 2, v: [1, 4, 3]

// clear：清空所有元素
v.clear();
println!("{:?}", v);  // []

// truncate：截断到指定长度
let mut v3 = vec![1, 2, 3, 4, 5];
v3.truncate(3);
println!("{:?}", v3);  // [1, 2, 3]

// retain：保留满足条件的元素
let mut v4 = vec![1, 2, 3, 4, 5, 6];
v4.retain(|&x| x % 2 == 0);  // 保留偶数
println!("{:?}", v4);  // [2, 4, 6]

// drain：删除指定范围的元素并返回迭代器
let mut v5 = vec![1, 2, 3, 4, 5];
let drained: Vec<_> = v5.drain(1..3).collect();  // 删除索引 1-2
println!("原 vec: {:?}, 删除的: {:?}", v5, drained);  // [1, 4, 5], [2, 3]
```

### 访问元素

**安全访问 vs 不安全访问**

```rust
let v = vec![1, 2, 3, 4, 5];

// 方式1：索引（会 panic）
let third = v[2];
println!("{}", third);  // 3

// 方式2：get（返回 Option，安全）
match v.get(2) {
    Some(value) => println!("找到: {}", value),
    None => println!("索引越界"),
}

// 方式3：get 越界返回 None
println!("{:?}", v.get(10));  // None

// 方式4：first 和 last
println!("第一个: {:?}", v.first());   // Some(&1)
println!("最后一个: {:?}", v.last());    // Some(&5)

// 方式5：get_mut 获取可变引用
let mut v = vec![1, 2, 3];
if let Some(value) = v.get_mut(1) {
    *value = 100;
}
println!("{:?}", v);  // [1, 100, 3]
```

<mark>Rust 的自动解引用发生在**方法调用**和**字段访问**时</mark>

### 可以利用枚举来让Vector存储不同的类型

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

fn main() {
    let row: Vec<SpreadsheetCell> = vec![
        SpreadsheetCell::Int(12),
        SpreadsheetCell::Float(2.3),
        SpreadsheetCell::Text(String::from("ddd")),
    ];
}
```

# String类型

> String类型是对`Vec<u8>`包装，因此有些集合操作，String也能用。
> 
> rust的String类型是支持utf-8的
> 
> String类型：
> 
> - 来自标准库而不是核心语言
> 
> - 可增长、可修改、可拥有

## 字符串

> - rust的核心语言层面，只有一个字符串类型：字符串切片`str`(或者`&str`)
> 
> - rust中讲的字符串一般指的是：String类型和字符串切片str

## 创建一个String

> - 很多`Vec<T>`的操作都可用于`String`
> 
> - `String::new()`函数可用于创建String类型变量
> 
> - 使用初始值来创建String：
>   
>   - `to_string()`方法，可用于实现了DIsplay trait的类型，包括字符串字面值
>   
>   - `String::from()`方法

```rust
let s1 = "str".to_string();
let s2 = String::from("str");
```

## String的一些操作

### 更新String

> - `push_str()`方法：把一个字符串切片附加到`String`
> 
> - `push()`方法：把一个字符添加到字符串上
> 
> - `+`：连接字符串
>   
>   - 底层是类似`fn add(self, s: &str) -> String{...}`语法糖
>   
>   - 所以存在所有权转移的问题
> 
> - `format!`：连接多个字符串
>   
>   - 不会取得所有权

## String索引

## 切割String

# HashMap

> HashMap<K, V>
> 
> HashMap以键值对的形式存储数据，一个键（key）对应一个值（value）
> 
> Hash函数：决定了如何在内存中存放K和V
> 
> 使用场景：通过K来查找V

## 创建HashMap

> 使用HashMap需要先引入

```rust
use std::collections::HashMap;

fn hashmap() {
    let mut scores: HashMap<String, String> = HashMap::new();
    // 添加数据
    scores.insert("ss".to_string(), "ssss".to_string());
}
```

> 使用`collect`方法创建
> 
> 在元素类型为tuple的Vector上使用collect方法，可以组建一个HashMap：
> 
> - 要求Tuple有两个值：一个作为K，一个作为V
> 
> - collect方法可以把数据整合成很多种集合类型，包括HashMap

```rust

```

## HashMap和所有权

> - 对于实现了Copy trait的类型，值会被复制到HashMap中
> 
> - 对于拥有所有权的值，值会被移动，所有权会转移给HashMap
> 
> - 如果将值的引用插入到HashMap，值本身不会移动
>   
>   - 在HashMap有效期间，被引用的值必须保持有效

## 访问HashMap中的值

> - `get`方法
>   
>   - 参数：K
>   
>   - 返回：`Option<&V>`

```rust
fn hashmap() {
    let mut scores: HashMap<String, String> = HashMap::new();
    // 添加数据
    scores.insert("ss".to_string(), "ssss".to_string());
    // 获取HashMap值
    let res = match scores.get(&"ss".to_string()) {
        Some(res) => res,
        None => &"".to_string(),
    };
}
```

## HashMap遍历

```rust
fn hashmap() {
    let mut scores: HashMap<String, String> = HashMap::new();
    // 添加数据
    scores.insert("ss".to_string(), "ssss".to_string());
    // 遍历
    for (key, val) in scores {
        println!("{}, {}", key, val);
    }
}
```

## 更新HashMap<K,V>

> - HashMap大小可变
> 
> - 每个k同时只能对应一个V

```rust
fn hashmap() {
    let mut scores: HashMap<String, String> = HashMap::new();
    // 添加数据
    scores.insert("ss".to_string(), "ssss".to_string());
}
```

还有一些没有记录

## Hash函数

> - 默认情况下，HashMap使用加密功能强大的Hash函数，可以抵抗拒绝服务（DoS）攻击
>   
>   - 不是可用的最快的Hash算法
>   
>   - 但具有更好安全性
> 
> - 可以指定不同的hasher来切换到另一个函数
>   
>   - hasher是实现BuildHasher trait的类型

# panic！错误

## rust错误处理概述

> - rust的可靠性在错误处理上也有体现
>   
>   - 大部分情况下：在编译时提示错误，并处理
> 
> - 错误的分类：
>   
>   - 可恢复
>     
>     - 例如：文件未找到，可再次尝试
>   
>   - 不可恢复
>     
>     - bug，例如：索引超出范围
> 
> - rust没有类似异常的机制
>   
>   - 针对可恢复错误，使用：`Result<T, E>`
>   
>   - 不可恢复，使用：`panic!`宏

## 不可恢复的错误与panic!

> - 当`panic!`宏执行时：
>   
>   - 程序会打印一个错误信息
>   
>   - 展开、清理调用栈
>   
>   - 退出程序
> 
> - 默认情况下，当panic发生时：
>   
>   - 程序展开调用栈（工作量大）
>     
>     - rust沿着调用栈往回走
>     
>     - 清理每个遇到的函数中的数据
>   
>   - 或立即中止调用栈：
>     
>     - 不进行清理，直接停止程序
>     
>     - 内存需要OS进行清理
> 
> - 想让二进制文件更小，可以把设置从“展开”改为“中止”：
>   
>   - 在Cargo.toml中的profile部分设置：
>     
>     - panic='abort'，不清理内存，由操作系统清理内存

### 使用panic！产生的回溯信息

> 可以通过调用panic！的函数的回溯信息来定位引起问题的代码
> 
> 通过设置环境变量RUST_BACKTRACE可得到回溯信息
> 
> 为了获取带有调试信息的回溯，必须启用调试符号（不带 --release）

## Result枚举

> 和`Option`枚举一样，`Result`及其变体也是由`prelude`带入作用域，无需导入

```rust
// Result 枚举
enum Result<T, E> {
    OK(T),
    Err(E),
}
```

> - T：操作成功情况下，OK变体里返回的数据的类型
> 
> - E：操作失败情况下，Err变体里返回的错误的类型

```rust
fn main() {
    let f = File::open("../hello.txt");
    let f = match f {
        Ok(file) => file,
        Err(err) => {
            panic!("{:?}", err);
        }
    };
}
```

### unwrap方法

> unwrap：match表达式的一个快捷方法，针对于返回Result的函数
> 
> - 如果返回的Result结果是OK，返回OK里面的值
> 
> - 如果返回的Result结果是Err，调用`panic!`宏中断程序

```rust
let f = File::open("../hello.txt").unwrap();
// 等价于
 let f = File::open("../hello.txt");
 let f = match f {
    Ok(file) => file,
    Err(err) => {
        panic!("{:?}", err);
    }
}
```

> unwrap有个缺点，无法自定义错误信息

### expect方法

> expect：和unwrap类似，但可指定错误信息

```rust
let f = File::open("../hello.txt").expect("无法打开文件");
```

## 传播错误

> 在 Rust 中，**传播错误**（Error Propagation）是指将错误返回给调用者处理，而不是在当前函数中处理。

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let file_result = File::open("hello.txt");

    let mut file = match file_result {
        Ok(file) => file,
        Err(e) => return Err(e),  // 传播错误
    };

    let mut username = String::new();
    match file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),  // 传播错误
    }
}
```

### ? 运算符

> `?` 运算符是错误传播的语法糖，它做的事情：
> 
> - 如果结果是`OK`，取出OK中的值作为表达式的结果，继续执行程序
> 
> - 如果是`Err`，将`Err`作为函数的返回值，就像使用了`return`

```rust
fn read_username_from_file1() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?;
    // 相当于
    // let mut f = match f {
    //     Ok(file) => file,
    //     Err(e) => return Err(e),
    // };
    let mut s = String::new();
    // 如果执行OK正常执行，如果不ok返回Err，函数结束
    f.read_to_string(&mut s)?;
    Ok(s)
    // 相当于
    // match f.read_to_string(&mut s) {
    //     Ok(_) => Ok(s),
    //     Err(e) => Err(e),
    // }
}
```

#### ?与from函数

> - `std::convert::From`上的`from`函数
>   
>   - 用于错误之间的转换
> 
> - 被？所应用的错误，会隐式的被from函数处理
> 
> - 当？调用from函数时：
>   
>   - 它所接收的错误类型会被转化为当前函数返回类型所定义的错误类型
> 
> - 用于：针对不同错误原因，返回同一种错误类型

<mark>？运算符只能返回类型为Result的函数</mark>

## 使用panic!的时机

> 总体原则
> 
> - 在定义一个可能失败的函数时，优先考虑Result
> 
> - 否则就panic!

# 泛型

> - 泛型：提高代码复用能力
>   
>   - 处理重复代码的问题
> 
> - 泛型是具体类型或其它属性的抽象代替

## 函数定义中的泛型

```rust
fn largest<T>(list: &[T]) -> &T {
    &list[0]
}
```

> 函数泛型需要注意的一些地方

```rust
// 下面是错误❌的
// 因为T是泛型，由调用者确定，调用者可能可能传入一个实现了Pp trait的非Tt类型，而函数返回Tt与传入的类型相矛盾
//fn test<T: Pp>() -> T {
//    Tt {
//        name: "dd".to_string(),
//    }
//}

// 下面是正确✔
fn test<T: Pp>() -> impl Pp {
    Tt {
        name: "dd".to_string(),
    }
}

```

## struct中的泛型

```rust
struct Point<T> {
    x: T,
    y: T,
}
// 多个泛型
struct Point1<T, K> {
    x: T,
    y: K,
}
// 为泛型结构体实现方法
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x        
    }
}
impl<K, V> Point1<K, V> {
    fn x(&self) -> &K {
        &self.x
    }
}
// 也可以直接指定类型
impl Point<i32> {
    fn x1(&self) -> &i32 {
        &self.x
    }
}
```

## 枚举中的泛型

```rust
enum Option<T> {
    Some(T),
    None,
}
//
enum Result<K, V> {
    Ok(K),
    Err(V),
}
```

## 泛型代码的性能

> 使用泛型和使用具体类型运行速度一致

# Trait

> - Trait告诉rust编译器：
>   
>   - 某种类型具有哪些并且可以与其它类型共享的功能
> 
> - Trait：抽象的定义共享行为
> 
> - Trait bounds（约束）：泛型类型参数指定为实现了特定行为的类型
> 
> - Trait与其它语言的接口（interface）类似，但有些区别

## 定义Trait

> - Trait的定义：把方法签名放在一起，来定义实现某种目的所必须的一组行为
>   
>   - 关键字：trait
>   
>   - 只有方法签名，没有具体实现
>   
>   - trait可以有多个方法：每个方法签名占一行，以`;`结尾
>   
>   - 实现该trait的类型必须提供具体的方法实现

```rust
pub trait Summary {
    fn summarize(&self) -> String;
    fn summarize1(&self) -> String
}
```

## 实现Trait

> - 与为类型实现方法类似
> 
> - 必须同时导入trait和对应结构体，才能使用结构体上实现trait里的方法

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
}
// 实现
impl Summary for NewArticle {
    fn summarize(&self) -> String {
        format!("{} {} {}", self.headline, self.location, self.author)
    }
}

// 使用
// 必须同时导入trait和对应结构体才能使用方法
use m_trait::{NewArticle, Summary};

fn main() {
    let n = NewArticle {
        headline: String::from("head"),
        location: String::from("location"),
        author: String::from("author"),
    };
    println!("{}", n.summarize())
}
```

### 实现trait的约束

> - 可以在某个类型上实现某个trait的前提条件是：
>   
>   - 这个类型或这个trait是在本地crate里定义的
> 
> - 无法为外部类型来实现外部的trait

### 默认实现

> - 默认实现：在trait已经声明方法的逻辑，在为结构体实现该trait时可以不用实现该方法，而直接使用trait中的逻辑
> 
> - 可以在默认实现中调用trait中其它方法，哪怕该方法还没有被实现
> 
> - 注意：

```rust
pub trait Summary {
    fn summarize(&self) -> String;
    fn print(&self) {
        println!("这是默认实现")
    }
}

pub struct NewArticle {
    pub headline: String,
    pub location: String,
    pub author:String,
}
// 虽然没有实现print
// 但是可以正常调用
impl Summary for NewArticle {
    fn summarize(&self) -> String {
        format!("{} {} {}", self.headline, self.location, self.author)
    }
}
// 调用
fn main() {
    let n = NewArticle {
        headline: String::from("head"),
        location: String::from("location"),
        author: String::from("author"),
    };
    println!("{}", n.summarize());
    n.print();
}
```

## Trait作为参数类型

> 类似于多态

```rust
pub trait Summary {
    fn summarize(&self) -> String;
    fn print(&self) {
        println!("这是默认实现")
    }
}

pub struct NewArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
}
pub struct Tweet {
    pub name: String,
}

impl Summary for NewArticle {
    fn summarize(&self) -> String {
        format!("{} {} {}", self.headline, self.location, self.author)
    }
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}", self.name)
    }
}
// 该函数接收一个实现了Summary的参数
pub fn notify(item: impl Summary) {
    println!("{}", item.summarize());
}
```

> Trait bound语法：`pub fn notify<T: Summary>(item: T){}`
> 
> * impl Trait语法是Trait bound的语法糖

```rust
// ...
pub fn notify(item: impl Summary) {
    println!("{}", item.summarize());
}
// Trait bound语法
// 上面的函数等价于
pub fn notify<T: Summary>(item: T) {
    println!("{}", item.summarize());
}
```

> 使用`+`指定多个Trait bound

```rust
// 要求同时实现多个trait，这里是同时实现Summary和Display
pub fn notify(item: impl Summary + Display) {
    println!("{}", item.summarize());
}
// Trait bound语法
// 上面的函数等价于
pub fn notify<T: Summary + Display>(item: T) {
    println!("{}", item.summarize());
}
```

> Trait bound使用where子句
> 
> - 有时Trait bound可能很长，可以使用where子句来减少函数签名的长度

```rust
pub fn notify2<T, K>(a: T, b: K)
where
    T: Summary + Display,
    K: Summary + Debug,
{
    println!("kkk");
}
```

## Trait作为返回类型约束

> impl trait作为返回类型约束

```rust
pub fn notify3(flag: bool) -> impl Summary {
    NewArticle {
        headline: String::from("head"),
        location: String::from("location"),
        author: String::from("author"),
    }
}
```

> **`impl Trait` 是返回类型的约束，函数只能返回单一的具体类型**
> 
> 下面是错误的

```rust
//下面是错的❌
// impl Summary 是指返回值类型实现了Summary，返回值类型只能是一个
// 这里返回值类型是两个都可能，编译器无法确定类型，所以报错
pub fn notify3(flag: bool) -> impl Summary {
    if flag {
        NewArticle {
            headline: String::from("head"),
            location: String::from("location"),
            author: String::from("author"),
        }
    } else {
        Tweet {
            name: String::from("ddd"),
        }
    }
}
```

## 使用Trait Bound有条件的实现方法

```rust
pub struct Pair<T> {
    x: T,
    y: T,
}
// 无论T是什么类型，都会有new方法
impl<T> Pair<T> {
    fn new(x: T, y: T) -> Pair<T> {
        Pair { x, y }
    }
}
// 只有当T实现了Display，才会有print方法
impl<T: Display> Pair<T> {
    fn print(&self) {
        println!("xxx");
    }
}
```

# 生命周期

> - 生命周期 不等于 作用域，生命周期是指**引用的有效范围**，也就是被引用者存活范围
> 
> - rust的每个引用都有自己的生命周期
> 
> - 大多数情况：生命周期是隐式的、可被推断的
> 
> - **rust要求编译器能在编译时推断和验证所有生命周期**
> 
> - **程序员只需在必要时标注**（当编译器无法自动推断时）
> 
> - 生命周期的主要目标：用于**保证引用的有效性**，避免悬垂引用
> 
> - 生命周期标注是写给编译器看的，只是为了告诉编译器某些操作不会导致悬垂引用，用户不应该过多思考生命周期标注的作用，因为可以看出来

## 借用检查器

> rust编译器的借用检查器：比较作用域来判断所有的借用是否合法

## 生命周期注解

### 为什么需要生命周期注解（配合下面的例子使用）

> - rust要求在编译时能推断出所有生命周期
> 
> - 函数返回一个引用，但这个引用指向 `x` 或 `y` 中的一个
> 
> - 编译器不知道 `x` 和 `y` 谁活得更久
> 
> - 它也无法知道返回的引用确切的生命周期

```rust
// 下面是❌的，rust确定返回值的生命周期
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
```

### 使用生命周期注解

> - 生命周期的标注不会改变引用的生命周期长度
> 
> - 当指定了泛型生命周期参数，函数可以接收带有任何生命周期的引用
> 
> - 生命周期的标注：描述了多个引用的生命周期的关系，但不影响生命周期

```rust
// 这个注解告诉编译器，x、y、返回值 拥有相同的生命周期 ==> 返回值的生命周期是小于等于三者之间最短的那个
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

//
fn main() {
    let string1 = String::from("long");
    let result;
    {
        let string2 = String::from("xyzuuuu");
        {
            let s1 = &string1;
            let s2 = &string2;
            // 这里返回值的生命周期是string2的存活范围
            // 哪怕返回是指向string1的引用
            result = longest(s1, s2);
        } // 返回x
        // 这里还在string2的存活范围内，可以使用
        println!("{}", result); // 这里 result 是有效的
    }
    // 离开了string2的存活范围内，不可以使用
    // println!("{}", result);    这里resutl是无效
}
```

### 生命周期标注语法

> - 生命周期参数名：
>   
>   - 以`'`开头
>   
>   - 通常全小写且非常短
>   
>   - 很多人使用`'a`
> 
> - 生命周期标注的位置：
>   
>   - 在引用的`&`符号后
>   
>   - 使用空格将标注和引用类型分开
> 
> - 例子：
>   
>   - `&i32`    一个引用
>   
>   - `&'a i32`  带有显式生命周期的引用
>   
>   - `&'a mut i32` 带有显式生命周期的可变引用
> 
> - 单个生命周期标注没什么意义

#### 函数签名中的生命周期标注

> - 泛型生命周期参数声明在：函数名和参数列表之间的`<>`里
> - `'a`生命周期是比较小的那个

```rust
fn ff<'a, T, K>(s: &'a T, y: &'a K) -> &'a T {
    s
}
```

#### struct定义中的生命周期标注

> 当结构体中包含引用时，需要使用生命周期标注

```rust
struct ImportantExcerpt<'a> {
    part1: &'a str,
    part2: &'a str,
}
```

#### 方法中的生命周期标注

```rust
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```

## 生命周期的省略

> - 在rust引用分析中所编入的模式称为生命周期省略规则
>   
>   - 这些规则无需开发者来遵守
>   
>   - 如果代码复合规则，那么无需显示标注生命周期

### 输入、输出生命周期

> 生命周期在：
> 
> - 函数/方法的参数：输入生命周期
> 
> - 函数/方法的返回值：输出生命周期

### 生命周期省略的三个规则

> - 规则1：每个引用类型的参数都有自己的生命周期
> 
> - 规则2：如果只有1个输入生命周期参数，那么该生命周期被赋给所有的输出生命周期参数
> 
> - 规则3：如果有多个输入生命周期参数，但其中一个是`&self`或`&mut self`，那么`self`的生命周期会被赋给所有的输出生命周期参数

## 静态生命周期

> - `'static`是一个特殊的生命周期：整个程序的持续时间
>   
>   - 例如：所有的字符串字面值都拥有`'static`生命周期

# 自动化测试

## 测试函数

> - 测试函数体通常执行的3个操作：
>   
>   - 准备数据/状态
>   
>   - 运行被测试的代码
>   
>   - 断言结果
> 
> - 测试函数需要使用test属性进行标注
>   
>   - 属性就是一段rust代码的元数据
>   
>   - 在函数上加`#[test]`，可把函数变成测试函数
> 
> - 运行测试
>   
>   - 使用`cargo test`命令运行所有测试函数
>   
>   - 当使用cargo创建library项目的时候，会生成一个test module，里面有一个test函数

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

## 测试失败

> - 测试函数panic就表示失败
> 
> - 每个测试运行在一个新的线程
> 
> - 当主线程看见某个测试线程挂掉了，那个测试标记为失败了

## 断言（Assert）

> 使用assert！宏检查测试结果
> 
> - `assert!`宏，来自标准库，用来确定某个状态是否为true
>   
>   - true：测试通过
>   
>   - false：调用`panic!`宏

```rust
#[test]
fn test_is_true() {
    assert!(is_true());
}
```

> `assert_eq!`和`assert_ne!`：测试相等性，前者判断是否相等，后者判断是否不等
> 
> - 这两个宏断言失败会自动打印出两个参数的值
>   
>   - 使用debug格式打印参数，这要求参数实现`PartialEq`和`Debug`的traits

```rust
assert_eq!(1, 1);
assert_ne!(1, 2);
```

## 添加自定义错误消息

> 可以向上面三个宏添加可选的自定义消息
> 
> - 这些自定义消息和失败消息都会被打印出来
> 
> - assert！：第一个参数必填，自定义消息作为第二个参数
> 
> - assert_eq!和assert_ne!：前两个参数必填，自定义消息作为第三个参数
> 
> - 自定义消息参数会被传递给format!宏，可以使用{}占位符

## 使用should_panic

> 可以给测试函数添加该属性，如果测试函数发生恐慌，测试函数就可以通过

```rust
#[test]
#[should_panic]
fn test_is_true() {
    assert!(is_true());
    assert_eq!(1, 1);
    assert_ne!(1, 1);
}
```

## 控制测试行为

> - 可以通过添加命令行参数来改变cargo test的行为
> 
> - 默认行为：
>   
>   - 并行运行
>   
>   - 所有测试
>   
>   - 捕获（不显示）所有输出，使读取与测试结果相关的输出更容易
> 
> - 命令行参数
>   
>   - 针对cargo test的参数：紧跟cargo test后
>   
>   - 针对测试可执行程序：放在 `--` 之后

# 应用

> 这里是做一个小工具时所使用的知识点

## 读取命令行参数

> 使用`use std::env::args`函数来接收
> 
> 第一个参数是：可执行文件的名称，后面的参数才是传入的参数

```rust
use std::env;
fn main() {
    let args: Vec<String> = env::args().collect();

    println!("{:?}", args);
}
```

## 获取环境变量

> 使用`use`

```rust
use std::env;
// var从环境变量获取值，返回一个Result枚举
// 这里不关心值，可以用is_err处理，如果是Err返回true，反之返回false
let flag = env::var("变量名").is_err();
```

# 闭包和迭代器

## 闭包

> 闭包：可以捕获其所在环境的匿名函数
> 
> 闭包：
> 
> - 是匿名函数
> 
> - 保存为变量、作为参数
> 
> - 可从其定义的作用域内捕获值

### 闭包的定义

```rust
let ec = |num: u32| {
    println!("function!");
    num
};
ec(32);
```

### 闭包的类型推断

> - 闭包不强制要求标注参数和返回值的类型
> 
> - 闭包通常很短小，只在狭小的上下文中工作，编译器通常能推断出类型
> 
> - 可以手动添加类型标注

```rust
let ec = |num: u32| -> u32 {
    println!("function!");
    num
};
// 下面也是正确
let ec = |num| {
    println!("function!");
    num
};
ec(32);
```

> <mark>注意：闭包的定义最终只会为参数/返回值推断出唯一具体的类型</mark>

```rust
let ec = |num| {
    println!("function!");
    num
};
// 下面的调用，使该匿名函数确定了参数和返回值的类型
let num = ec(32);
// 下面是❌的，不能再将其它类型传递给它
let s = ec("str");
```

### 让结构体持有闭包

> 目的：
> 
> - 只需要在需要结果时才执行该闭包
> 
> - 可以缓存结果
> 
> struct的定义需要知道所有字段的类型
> 
> - 需要指明闭包的类型
> 
> 每个闭包实例都有自己唯一的匿名类型，即便两个闭包签名完全一样

**Fn Trait**

> - Fn traits由标准库提供
> 
> - 所有的闭包都至少实现了以下trait之一：
>   
>   - Fn
>   
>   - FnMut
>   
>   - FnOnce

```rust
struct Cacher<T, K>
where
    T: Fn(K) -> K,
{
    calculation: T,
    value: Option<K>,
}

impl<T, K> Cacher<T, K>
where
    T: Fn(K) -> K,
{
    fn new(calculation: T) -> Cacher<T, K> {
        Cacher {
            calculation,
            value: None,
       }
    }
}
// 返回一个闭包
```

### 函数返回一个闭包

> 闭包类型是匿名的且每个闭包都有不同的类型，哪怕它们的签名一致
> 
> 方法：
> 
> - impl Trait 方法
>   
>   - 简单直观
>   
>   - 零开销抽象
>   
>   - 只能返回一种具体的闭包类型
> 
> - Box<dyn Trait>动态分发
>   
>   - 可以在运行时返回不同类型的闭包
>   
>   - 有少量堆分配和动态分发开销
>   
>   - 需要显式 `Box` 包装
> 
> 为什么

```rust
// impl Trait 方法
fn create_adder(x: i32) -> impl Fn(i32) -> i32 {
    move |y| x + y
}

fn main() {
    let add5 = create_adder(5);
    println!("{}", add5(3));  // 输出: 8
}
// 下面是错误❌的
// 返回一个闭包类型，但是每个闭包都有自己独特的类型，这个闭包类型找不到第二个类型来接收
//fn create_adder(x: i32) -> Fn(i32) -> i32 {
//    move |y| x + y
//}
```

### 闭包捕获所在环境的变量

> 闭包可以访问定义它的作用域内的变量，而普通函数则不能
> 
> 闭包从环境捕获值的方式：
> 
> - 取得所有权：FnOnce
> 
> - 可变借用：FnMut
> 
> - 不可变借用：Fn

**move关键字**

> - 在参数列表前使用move关键字，可以强制闭包取得它所使用的环境值的所有权

```rust
fn equal_to_x() -> impl Fn(i32) -> bool {
    let x = 2;
    // 将x的所有权转到闭包了，在之后不能再使用
    let b = move |z: i32| z == x;
    return b;
}

// 使用
fn main() {
    let e = equal_to_x();
    // 在x的作用域外面，但是依旧可以使用x
    println!("{}", e(2));
}
```

## 迭代器

> - 迭代器负责
>   
>   - 遍历每个项
>   
>   - 确定序列（遍历）何时完成
> 
> - rust的迭代器：
>   
>   - 懒惰的：除非调用消费迭代器的方法，否则迭代器本身没有任何效果

```rust
fn main() {
    let v1 = vec![1, 2, 3];
    // 产生一个迭代器
    let v1_iter = v1.iter();
    for ele in v1_iter{
        let e = ele;
        println!("{}", e);
    }
}
```

### Iterator trait

> - 所有迭代器都实现了`Iterator trait`
> 
> - `Iterator trait`定义于标准库，定义大致如下：

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item> {
        // 默认实现
    }
}
```

> - type Item和Self::Item定义了与此该trait关联的类型
>   
>   - 实现Iterator trait需要你定义一个Item类型，它用于next方法的返回类型
> 
> - `Iterator trait`仅要求实现一个方法：next
> 
> - next：
>   
>   - 每次返回迭代器中的一项
>   
>   - 返回结果包裹在Some里
>   
>   - 迭代结束，返回None
> 
> - 可直接在迭代器上调用next方法

### 迭代器类型

> 主要有三种迭代器类型
> 
> - `iter()`
> 
> - `iter_mut()`
> 
> - `into_iter()`

```rust
let v = vec![1, 2, 3, 4, 5];

// 1. iter() - 产生不可变引用
for item in v.iter() {
    println!("{}", item);  // item 是 &i32
}

// 2. iter_mut() - 产生可变引用
for item in v.iter_mut() {
    *item *= 2;  // item 是 &mut i32
}

// 3. into_iter() - 获取所有权（消费集合）
for item in v.into_iter() {
    println!("{}", item);  // item 是 i32，v 被移动
}
// println!("{:?}", v); // ❌ 错误：v 已失效
```

### 消耗迭代器的方法

> - 调用next的方法叫做“消耗型适配器”
>   
>   - 因为调用它们会把迭代器消耗尽
> 
> - 例如：
>   
>   - `sum()`方法
>   
>   - `collect()`方法：把结果收集到一个集合类型中

```rust

```

### 产生其它迭代器的方法

> - 把迭代器转换为不同种类的迭代器
> 
> - 可以通过链式调用使用多个迭代器适配器来执行复杂的操作，这种调用可读性较高
> 
> - 例如：map
>   
>   - 接收一个闭包，闭包作用于每个元素
>   
>   - 产生一个新的迭代器

### 使用Iterator trait来创建自定义迭代器

```rust
struct Counter {
    count: u32,
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}
```

### 循环和迭代器性能比较

> 使用迭代器并不会带来性能损失

# 智能指针

> - 指针：一个变量在内存中包含的是一个地址（指向其它数据）
> 
> - rust中最常用的指针就是“引用”
> 
> - 引用：
>   
>   - 使用&指向被借用的值
>   
>   - 没有其余开销
>   
>   - 最常见的指针类型
> 
> - 智能指针是这样一些数据结构：
>   
>   - 行为和指针相似
>   
>   - 有额外的元数据和功能
> 
> - 引用与智能指针的不同
>   
>   - 引用：只借用数据
>   
>   - 智能指针：很多时候都拥有它所指向的数据

**智能指针的例子**

> - `String`和`Vec<T>`：
>   
>   - 都指向并拥有一片内存区域，且允许用户对其操作
>   
>   - 还拥有元数据（例如：容量等）
>   
>   - 提供额外的功能或保障（`String`保障其数据是合法的`utf-8`编码）

**智能指针的实现**

> - 智能指针通常使用`struct`实现，并且实现了：
>   
>   - `Deref`和`Drop`这两个`trait`
> 
> - `Deref trait`：用于**重载解引用操作符 `*`**。它允许智能指针类型表现得像普通引用一样。
> 
> - `Drop trait`：允许自定义当智能指针实例走出作用域时的代码

**引用计数（reference counting）智能指针类型**

> - 通过记录所有者的数量，使一份数据被多个所有者同时持有
> 
> - 并在没有任何所有者自动清理数据

## Box<T>

> - Box<T>是最简单的智能指针：
>   
>   - Box<T>将数据存放在heap上，在stack（栈）上是指向heap数据的指针
>   
>   - 没有性能开销，与其它语言的指针类似
>   
>   - 没有其它额外功能
>   
>   - 实现了Deref trait和Drop trait

<img title="" src="./pic/rust/屏幕截图 2026-05-24 082643.png" alt="">

### 使用场景

> - 在编译时，某类型的大小无法确定。但使用该类型时，上下文却需要知道它的确切大小
> 
> - 当你有大量数据，想移交所有权，但需要确保在操作时数据不会被复制
> 
> - 使用某个值时，只关心它是否实现了特定的trait，而不关心它的具体类型

### 使用Box<T>

**简单使用**

```rust
pub fn my_box() {
    // Box::new()接受任意类型的数据，泛型T数据
    let b = Box::new(5);
    let b1 = Box::new(User {
        name: "ding".to_string(),
    });
    println!("b = {}\nb1 = {:?}", b, b1);
}   // 离开作用域时，会自动释放栈上的指针和堆上的数据
```

**使用Box赋能递归类型**

> - 在编译时，rust需要知道一个类型所占的空间大小
> 
> - 而递归类型的大小无法在编译时确定，直接定义递归类型会报错（递归类型：内部包含两个内容，一个i32类型，另一个是递归本身类型）
> 
> - 但Box类型的大小确定，在递归类型中使用Box就可解决上述问题
> 
> - 递归类型与函数式语言中的Cons List比较像，Cons List是来自Lisp语言的一种数据结构
> 
> <img title="" src="./pic/rust/屏幕截图 2026-05-24 085031.png" alt="">

```rust
// 下面是错误❌的，因为编译器无法确定这个枚举的大小，每个枚举中都包含一个枚举
//pub enum MyList {
//    Cons(i32, MyList),
//    Nil,
//}

// 下面是正确的✔ Box具有确定的大小，枚举内部的枚举放在堆上
pub enum MyList {
    Cons(i32, Box<MyList>),
    Nil,
}
```

## Deref Trait 定义解引用行为

> - 实现Deref Trait使我们可以自定义解引用运算符*的行为
> 
> - 通过实现Deref，智能指针可像常规引用一样来处理

**解引用运算符**

> - `*`是解引用运算符
> 
> - 常规应用是一种指针

```rust
pub fn my_box() {
    let x = 5;
    let y = &x;
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```

**把Box<T>当作引用**

> - Box<T>可以代替上例中的引用

```rust
pub fn my_box() {
    let x = 5;
    let y = Box::new(x);
    assert_eq!(5, x);
    // 可以使用*来进行解Box
    // *y实际上是*(y.deref)的语法糖
    assert_eq!(5, *y);    //正常运行
}
```

**定义自己的智能指针**

```rust
pub struct MyBox<T>(T);

impl<T> MyBox<T> {
    pub fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}
// 实现Deref，实现用*解引用
impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

**函数和方法的隐式引用转化（Deref Coercion）**

> - 隐式解引用转化是为函数和方法提供的一种便捷特性
> 
> - 假设T实现了Deref trait：
>   
>   - Deref Coercion可以把T的引用转化为T经过Deref操作后生成的引用
> 
> - 当把某类型的引用传递给函数或方法时，但它的类型与定义的参数类型不匹配：
>   
>   - Deref Coercion就会自动发生
>   
>   - 编译器会对deref进行一系列调用，来把它转为所需的参数类型
>     
>     - 在编译时完成，没有额外性能开销

```rust
fn hello(s: &str) {
    println!("hello {}", s);
}

fn my_test() {
    let m = Box::new("rust".to_string());
    // 所需类型为&str，传入的类型不对，自动deref
    // 因为Box实现了deref，所以：&Box<String> → &String
    // &String与&str类型不对，自动deref
    // 因为String实现了deref，所以：&String → &str
    // 正确，实际上，编译器自动执行了两次deref()方法
    hello(&m);
    // 所以上面等价于
    hello(m.deref().deref());
    // 等价于
    hello(&(**m));
}
```

**解引用与可变性**

> - 可使用DerefMut trait重载可变引用的`*`运算符
> 
> - 在类型和trait在下列三种情况发生时，rust会执行deref coercion：
>   
>   - 当`T:Deref<Target=U>`，允许`&T`转换为`&U`
>   
>   - 当`T:DerefMut<Target=U>`，允许`&mut T`转换为`&mut U`
>   
>   - 当`T:Deref<Target=U>`，允许`&mut T`转换为`&U`

## Drop Trait

> - 实现`Drop Trait`，可以让我们自定义当变量将要离开作用域时发生的动作
>   
>   - 例如：文件、网络资源释放等
>   
>   - 任何类型都可以实现Drop trait
> 
> - `Drop trait`只要你实现`drop`方法
>   
>   - 参数：对self的可变引用
> 
> - `Drop trait`在预导入模块里，可以无需导入就可使用
> 
> - 很难直接禁用自动的drop功能，也没必要
>   
>   - Drop trait的目的就是进行自动的释放处理逻辑
> 
> - rust不允许手动调用Drop trait的drop方法
> 
> - 可以调用标准库的`std::mem::drop`函数来提前调用drop

**使用案例**

```rust
pub struct CustomStruct {
    data: String,
}

impl Drop for CustomStruct {
    fn drop(&mut self) {
        println!("自动释放{}", self.data);
    }
}

pub fn droptest() {
    let a = CustomStruct {
        data: "tt".to_string(),
    };
} // a离开作用域，自动执行drop方法

// 提前调用drop
pub fn droptest() {
    let a = CustomStruct {
        data: "tt".to_string(),
    };
    drop(a);
    println!("函数运行结束")
}
```

## Rc<T>：引用计数智能指针

> - rust中，一个值通常只有一个所有者。有时，一个值，一个值会有多个所有者
> 
> - 为了支持多重所有权创造了：Rc<T> 引用计数智能指针
>   
>   - reference counting（引用计数）
>   
>   - 追踪所有到对该值的引用
>   
>   - 0个引用：该值可以被清理掉

**使用场景**

> - 需要在heap上分配数据，这些数据被程序的多个部分读取（只读），但在编译时无法确定哪个部分最后使用完这些数据
> 
> - Rc<T>只能用于单线程场景

**使用案例**

> - Rc<T>不在预导入模块，需要手动导入
> 
> - Rc::clone(&a)函数：增加引用计数，这个不会克隆数据，只会增加引用计数，性能较好
> 
> - Rc::strong_count(&a)：获得引用计数
>   
>   - Rc::weak_count函数

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
//
//fn main() {
//    let a = List::Cons(2, Box::new(List::Nil));
//    let b = List::Cons(3, Box::new(a));
    // 会报错，因为a的所有权转移了
//    let c = List::Cons(4, Box::new(a));
//}

// 下面是
fn main() {
    let a = Rc::new(List::Cons(2, Rc::new(List::Nil)));
    println!("创建a后的引用计数:{}", Rc::strong_count(&a)); // 创建a后的引用计数:1
    let b = List::Cons(3, Rc::clone(&a));
    println!("创建b后的引用计数:{}", Rc::strong_count(&a)); // 创建b后的引用计数:2
    {
        let c = List::Cons(4, Rc::clone(&a));
        println!("创建c后的引用计数:{}", Rc::strong_count(&a)); // 创建c后的引用计数:3
    }
    println!("c销毁后的引用计数:{}", Rc::strong_count(&a)); // c销毁后的引用计数:2
}
```

> `Rc::clone()` vs 类型的 `clone()`方法
> 
> - `Rc::clone()`：增加引用，不会执行数据的深度拷贝操作
> 
> - 类型的`clone()`：很多会执行数据的深度拷贝操作

<mark>Rc<T>通过不可变引用，使用户可以在程序不同部分之间共享只读数据</mark>

### RefCell和内部可变性

#### 内部可变性

> - 内部可变性是Rust的设计模式之一
> 
> - 它允许在只持有不可变引用的前提下对数据进行修改
>   
>   - 数据结构中使用了unsafe代码来绕过rust正常的可变性和借用规则
> 
> - 可变的借用一个不可变的值

#### RefCell<T>

> - 与Rc<T>不同，RefCell<T>类型代表了其持有数据的唯一所有权
> - 与Rc<T>一样，<mark>只能用于单线程场景</mark>

**Box与RefCell的区别**

| 类型         | 区别                     |
| ---------- | ---------------------- |
| Box<T>     | 编译阶段强制代码遵守借用规则，否则报错    |
| RefCell<T> | 只会在运行时检查借用规则，否则触发panic |

**不同阶段进行检查的比较**

> 编译阶段：
> 
> - 尽早暴露问题
> 
> - 没有任何运行时开销
> 
> - 对大多数场景是最佳选择
> 
> - 是rust的默认行为
> 
> 运行时：
> 
> - 问题暴露延后，甚至到生成环境
> 
> - 因借用计数产生些许性能损失
> 
> - 实现某些特定的内存安全场景（不可变环境中修改自身数据）

## 选择Box<T> Rc<T> RefCell<T>的依据

|          | Box<T>          | Rc<T>        | RefCell<T>      |
| -------- | --------------- | ------------ | --------------- |
| 同一数据的所有者 | 一个              | 多个           | 一个              |
| 可变性、借用检查 | 可变、不可变借用（编译时检查） | 不可变借用（编译时检查） | 可变、不可变借用（运行时检查） |

> 其中：<mark>即便RefCell<T>本身不可变，但仍能修改其中存储的值</mark>

## 循环引用导致内存泄漏





# 多线程

**语言实现线程的方式**

> - 通过系统的API来创建线程：1：1模型
>   
>   - 需要较小的运行时
> 
> - 语言自己实现的线程（绿色线程）：M：N模型
>   
>   - 需要更大的运行时
> 
> - rust标准库仅提供1：1模型的线程

## 使用多线程

**通过spawn创建新线程**

> - 通过`thread::spawn`函数可以创建新线程并立即执行：
>   
>   - 参数：一个闭包（在新线程里运行的代码）
>   
>   - 主线程不会等待创建的线程，如果main线程运行结束而子线程没有，则会直接关闭子线程

```rust
use std::{thread, time::Duration};

fn main(){
    // 创建一个线程并立即执行
    thread::spawn(|| {
        for i in 1..5 {
            println!("thread: num is {}", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
    for i in 1..5 {
        println!("main: num is {}", i);
        thread::sleep(Duration::from_millis(1));
    }
}
```

**通过join Handle来等待所有线程的完成**

> - `thread::spawn`函数的返回值类型是`JoinHandle`
> 
> - `JoinHandle`持有值的所有权
>   
>   - 调用其join方法，可以等待对应的其它线程的完成
> 
> - join方法：调用handle的join方法会阻止当前线程的执行，直到handle所表示的这些线程终结

```rust
use std::{thread, time::Duration};

fn main(){
    // 接收返回值
    let handle = thread::spawn(|| {
        for i in 1..5 {
            println!("thread: num is {}", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
    // join方法会阻塞main线程执行，直到上面那个线程执行完成
    handle.join().unwrap();
    for i in 1..5 {
        println!("main: num is {}", i);
        thread::sleep(Duration::from_millis(1));
    }
}
```

**使用move闭包**

> - `move闭包`通过和`thread::spawn`函数一起使用，它允许使用其它线程的数据
> 
> - 创建线程时，把值的所有权从一个线程转移到另一个线程

```rust
fn mm() {
    let v = vec![1, 2, 3];
    // 将所有权转移到闭包中
    let _handle = thread::spawn(move || {
        for i in 1..5 {
            println!("thread: num is {}", i);
            thread::sleep(Duration::from_millis(1));
        }
        println!("{:?}", v);
    });
}
```

## 线程消息传递

> - 一种很流行且能保证安全并发的技术就是：消息传递
>   
>   - 线程（或Actor）通过彼此发送消息（数据）来进行通信
> 
> - rust：使用`Channel`来进行消息传递（标准库提供）

**Channel**

> - Channel包含：发送端、接收端
> 
> - 调用发送端的方法，发送数据
> 
> - 接收端会检查和接收到达的数据
> 
> - 如果发送端、接收端中任意一端被丢弃了，那么Channel就关闭了

**创建Channel**

> - 使用`mpsc::channel`函数来创建Channel
>   
>   - `mpsc`表示multiple producer, single consumer（多个生产者、一个消费者）
>   
>   - 返回一个tuple（元组）：里面元素分别时发送端、接收端
> 
> - 发送端和接收端
>   
>   - 发送端的`send`方法：
>     
>     * 参数：想要发送的数据
>     
>     * 返回：`Result<T, E>`
>       
>       * 如果有问题，就返回一个错误
>   
>   - 接收端的方法
>     
>     * `recv`方法：<mark>阻止当前线程执行</mark>，直到`Channel`中有值被送来
>       
>       * 一旦有值收到，就返回`Result<T, E>`
>       
>       * 当发送端关闭，就会收到一个错误
>     
>     * `try_recv`方法：不会阻塞，当执行到该方法时会立即返回`Result<T, E>`
>       
>       * 如果有数据达到：返回OK，里面包含着数据
>       
>       * 否则，返回错误
>       
>       * 通常会使用循环调用来检查`try_recv`的结果

```rust
use std::{sync::mpsc, thread, time::Duration};

fn main() {
    // 创建channel，第一个时发送者，第二个是接收者
    let (tx, rx) = mpsc::channel::<String>();
    let _handle = thread::spawn(|| {
        for i in 1..5 {
            println!("thread: num is {}", i);
            thread::sleep(Duration::from_millis(1));
        }
        // 发送消息
        tx.send("ok".to_string()).expect("发送消息失败");
    });
    for i in 1..5 {
        println!("main: num is {}", i);
        thread::sleep(Duration::from_millis(1));
    }
    // 接收消息
    println!("{}", rx.recv().unwrap());
}
```

**Channel和所有权转移**

> - 所有权在消息传递中非常重要
> 
> - `send`方法会发生所有权转移

```rust
fn main() {
    let (tx, rx) = mpsc::channel::<String>();
    let _handle = thread::spawn(move || {
        for i in 1..5 {
            println!("thread: num is {}", i);
            thread::sleep(Duration::from_millis(1));
        }
        // 发送消息
        let s = "ok".to_string();
        // 发生所有权转移
        tx.send(s).expect("发送消息失败");
        // 错误❌，没有所有权了
        // println!("{}", s);
    });
    for i in 1..5 {
        println!("main: num is {}", i);
        thread::sleep(Duration::from_millis(1));
    }
    // 接收消息
    println!("{}", rx.recv().unwrap());
}
```

**通过克隆创建多个发送者**

> 通过`std::sync::mpsc::Sender::clone`方法可以创建多个发送者

```rust
fn main() {
    let (tx, rx) = mpsc::channel::<String>();
    // 创建多个发送者
    let tx1 = Sender::clone(&tx);
    thread::spawn(move || {
        let v = vec![
            "1: s1".to_string(),
            "1: s2".to_string(),
            "1: s3".to_string(),
        ];
        for e in v {
            tx.send(e).unwrap();
            thread::sleep(Duration::from_millis(1));
        }
    });
    thread::spawn(move || {
        let v = vec!["s1".to_string(), "s2".to_string(), "s3".to_string()];
        for e in v {
            tx1.send(e).unwrap();
            thread::sleep(Duration::from_millis(1));
        }
    });
    for ele in rx {
        println!("{}", ele);
    }
}
```

## 共享状态的并发 -- 锁

> Go语言的名言：不要用共享内存来通信，要用通信来共享内存
> 
> rust支持通过共享状态来失效并发
> 
> - Channel类似但所有权：一旦将值的所有权转移至Channel，就无法使用它了
> 
> - 共享内存并发类似多所有权：多个线程可以同时访问同一块内存

**使用Mutex互斥锁**

> - Mutex是互斥锁的简写
> 
> - 在同一时刻，Mutex只允许一个线程来访问某些数据
> 
> - 想要访问数据：
>   
>   - 线程必须首先获取互斥锁（lock）
>     
>     - lock数据结构是mutex的一部分，它能跟踪谁对数据拥有独占访问权
>   
>   - mutex通常被描述为：通过锁定系统来保护它所持有的数据
> 
> - `Mutex<T>`的api
>   
>   - 通过`Mutex::new(数据)`来创建`Mutex<T>`
>     
>     - 数据就是共享的数据，需要取得锁才能访问
>     
>     - `Mutex<T>`是一个智能指针
>     
>     - 访问数据前，通过lock方法来获取锁
>       
>       - 会阻塞当前线程
>       
>       - 获取lock可能会失败
>       
>       - 返回的是`MutexGuard`（智能指针，实现了Deref和Drop）

```rust
fn main() {
    let m: Mutex<i32> = Mutex::new(5);
    {
        let mut num: MutexGuard<'_, i32> = m.lock().unwrap();
        *num = 6;
    }
    println!("m = {:?}", m);
}
```

**使用Arc<T>来进行原子引用计数**

> - `Arc<T>`和`Rc<T>`类似，它可以用于并发情景
>   
>   - `A`：atomic，原子的
> 
> - 为什么所有的基础类型都不是原子的，为什么标准库类型不默认使用`Arc<T>`
>   
>   - 需要性能作为代价
> 
> - `Arc<T>`和`Rc<T>`的API是相同的

```rust
fn main() {
    // let mut n: i32 = 0;
    let counter: Arc<Mutex<i32>> = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    for _ in 0..10 {
        let c = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = c.lock().unwrap();
            *num += 1;
            thread::sleep(Duration::from_millis(1));
        });
        handles.push(handle);
    }
    for ele in handles {
        ele.join().unwrap();
    }
    println!("{:?}", counter);  // Mutex { data: 10, poisoned: false, .. }
}
```

## Send 和 Sync trait

> - rust语言的并发特性较少，上面所说的并发特性都来自标准库（而不是语言本身）
> 
> - 在rust语言中有两个并发概念：
>   
>   - `std::marker::Sync`和`std::marker::Send`这两个`trait`
> 
> - 手动实现Send 和 Sync trait是不安全的

### Send trait

> - `Send`允许线程间转移所有权
> 
> - rust中几乎所有类型都实现了`Send`
>   
>   - 但`Rc<T>`没有实现`Send`，它只用于单线程情景
> 
> - 任何完全由`Send`类型组成的类型也被标记为`Send`
> 
> - 除了原始指针之外，几乎所有的基础类型都是`Send`

### Sync trait

> - 实现`Sync`的类型可以安全的被多个线程引用
> 
> - 也就是说：如果`T`是`Sync`，那么`&T`就是`Send`
>   
>   - 引用可以被安全的送往另一个线程
> 
> - 基础类型都是`Sync`
> 
> - 完全是`Sync`类型组成的类型也是`Sync`
>   
>   - 但，`Rc<T>`不是`Sync`的
>   
>   - `RefCell<T>`和`Cell<T>`家族也不是`Sync`的
>   
>   - 而，`Mutex<T>`是`Sync`的

# 异步编程

> rust好像原生带异步编程了，但是用的比较多的是`tokio`库，所以下面介绍`tokio`库的使用。

**安装tokio**

> `cargo add tokio`

**使用tokio**

```rust
#[tokio::main]
// 在rust中不能直接将async修饰在main函数之前
// 这里使用了宏
async fn main() {
    // 使用tokio的fs，tokio还有一些其它的工具，例如：spawn等
    // 这里的fs返回一个Future
    let res = tokio::fs::read_to_string("../h.txt").await;
    let txt_str = match res {
        Ok(str) => str,
        Err(err) => panic!("读取文件出现错误了:{}", err),
    };
    println!("文件内容：{}", txt_str);
}


```

# 面向对象特性

> rust没有继承
> 
> 使用继承的原因：
> 
> - 代码复用
>   
>   - rust中使用trait来进行代码共享
> 
> - 多态
>   
>   - rust中使用泛型和trait约束实现多态

## 使用trait储存不同类型的值

> - rust避免将struct或enum称为对象，因为它们与impl块是分开的
> 
> - trait对象有些类似于其它语言中的对象：
>   
>   - 它们某种程度上组合了数据于行为
> 
> - trait对象与传统对象不同的地方
>   
>   - 无法为trait对象添加数据
> 
> - trait对象被专门用于抽象某些共有行为，它没其他语言中的对象那么通用

### dyn关键字

> - `dyn` 是 Rust 中用于**动态分发**（dynamic dispatch）的关键字，用于处理**trait 对象**
> 
> - `dyn Trait` 表示一个 trait 对象，指向实现了该 trait 的某个类型的实例，但具体类型在编译时未知。
> 
> - `dyn` 必须跟在指针后面，如：`&dyn`、`Box<dyn>`、`Rc<dyn>` 等
>   
>   - 因为**实现trait的类型在编译时大小不确定**，而Rust 要求所有在栈上的变量必须有编译时已知的固定大小。
>   
>   - 指针的大小是确定的

```rust
trait Animal {
    fn sound(&self);
}

struct Dog;
struct Cat;

impl Animal for Dog {
    fn sound(&self) {
        println!("woof!");
    }
}
impl Animal for Cat {
    fn sound(&self) {
        println!("Meow!");
    }
}
// 接收任何实现了Animal的类型
fn make_sound(animal: &dyn Animal) { 
  animal.sound();
}

fn main() {
    let cat = Cat {};
    let dog = Dog;
    make_sound(&cat);
    make_sound(&dog);
}
```

#### 对象安全（Object Safety）

> 只有**对象安全**的 trait 才能用作 trait 对象。需要满足：
> 
> * 方法不能是泛型
> 
> * 方法不能返回 `Self`（除非 `Self: Sized`）
> 
> * 方法不能有 `Self: Sized` 约束

```rust
// ❌ 不能作为 trait 对象
trait NotObjectSafe {
    fn generic<T>(&self, t: T);  // 有泛型方法
    fn returns_self(&self) -> Self;  // 返回 Self
}

// ✅ 对象安全
trait ObjectSafe {
    fn method(&self);
    fn method_with_arg(&self, arg: i32);
}
```

#### dyn vs 泛型（静态分发）

> `T: Trait`是`impl Trait`语言糖，但是它们两者有些差异，主要是在某些地方不能使用其中一种，只能用另一种

| 特性   | `dyn Trait`（动态分发） | 泛型 `T: Trait`（静态分发） |
| ---- | ----------------- | ------------------- |
| 分发时机 | 运行时               | 编译时                 |
| 性能   | 有虚函数调用开销          | 零开销，可内联             |
| 代码体积 | 更小（单份代码）          | 更大（单态化膨胀）           |
| 灵活性  | 可混合不同类型           | 编译时确定具体类型           |
| 对象安全 | 需要满足对象安全规则        | 无限制                 |

```rust
// 静态分发 - 编译时为每个类型生成单独代码
fn static_dispatch<T: Animal>(animal: T) {
    animal.sound();
}

// 动态分发 - 运行时通过虚表调用
fn dynamic_dispatch(animal: &dyn Animal) {
    animal.sound();
}
```

# 模式匹配

> - 模式是rust中一种特殊语法，用于匹配复杂和简单类型的结构
> 
> - 将模式与匹配表达式和其它构造结合使用，可以更好的控制程序的控制流
> 
> - 模式由以下元素（或它们的一些组合）组成：
>   
>   - 字面值
>   
>   - 解构的数组、enum、struct和tuple
>   
>   - 变量
>   
>   - 通配符
>   
>   - 占位符
> 
> - 想要使用模式，需要将其与某个值进行比较
>   
>   - 如果模式匹配，就可以在代码中使用这个值的相应部分

## 用到模式的地方

**match**

> - match的arm（分支）
> 
> - match表达式的要求
>   
>   - 详尽（包含所有的可能性）
> 
> - 一个特殊的模式：_（下划线）
>   
>   - 它会匹配任何东西
>   
>   - 不会绑定到变量
>   
>   - 通常用于match的最后一个arm，或用于忽略某些值

```rust
match value {
    pattern => expression,
    ...
}
```

**if let**

> - 条件if let表达式

**while let**

> while let条件循环
> 
> - 只要模式继续满足匹配的条件，那它允许while循环一直运行

```rust
fn tt() {
    let mut v = Vec::new();
    v.push(1);
    v.push(2);
    // 只要能取出数，就一直运行
    while let Some(i) = v.pop() {
        println!("{}", i);
    }
}
```

## 可辨驳性：模式是否会无法匹配

> - 模式有两种形式：可辨驳的、无可辩驳的
> 
> - 能匹配任何可能传递的值的模式：无可辩驳的（也就是一定能匹配成功）
>   
>   - 例如：let x = 5;
> 
> - 对某些可能的值，无法进行匹配的模式：可辨驳的（也就是不一定能匹配成功）
>   
>   - 例如：if let Some(x) = a_value    （如果a_value为None，则匹配失败）
> 
> - 函数参数、let语句、for循环只接受无可辩驳的模式
> 
> - if let和while let接受可辩驳和无可辩驳的模式

## 模式匹配的语法



# Rust高级特性

## Unsafe Rust

> - Unsafe Rust（不安全的Rust）：它没有强制内存安全保证
>   
>   - 和普通的rust一样，但提供了额外的“超能力”
> 
> - Unsafe Rust存在的原因：
>   
>   - 静态分析是保守的
>     
>     - 使用Unsafe Rust：知道自己在做什么，并承担相应风险
>   
>   - 计算机硬件本身就是不安全的，Rust需要能够进行底层系统编程
> 
> - 使用unsafe关键字来切换到unsafe rust，开启一个块，里面放着unsafe代码
> 
> - unsafe rust里可执行的四个动作（unsafe超能力）：
>   
>   - 解引用原始指针
>   
>   - 调用unsafe函数或方法
>   
>   - 访问或修改可变的静态变量
>   
>   - 实现unsafe trait
> 
> - 注意：
>   
>   - unsafe并没有关闭借用检查或停用其它安全检查
>   
>   - 任何内存安全相关的错误必须留着unsafe块里
>   
>   - 尽可能隔离unsafe代码，最好将其封装在安全的抽象里，提供安全的API

### 解引用原始指针

> - 原始指针：一种类似引用的新型指针，与c/c++指针类似
>   
>   - 可变的：`*mut T`
>   
>   - 不可变的：`*const T` 意味着指针在解引用后不能直接对其进行赋值
>   
>   - 注意：这里的*不是解引用符号，它是类型名的一部分
> 
> - 与引用不同，原始指针：
>   
>   - 允许通过同时具有不可变和可变指针或多个指向同一位置的可变指针来忽略借用规则
>   
>   - 无法保证能指向合理的内存
>   
>   - 允许为null
>   
>   - 不实现任何自动清理
> 
> - 放弃保证的安全，换取更好的性能/与其它语言或硬件接口的能力

```rust
fn point() {
    let mut num = 5;
    // 创建原始指针
    // 这里同时创建指向同一地址的可变和不可变引用（指针）
    let p1: *const i32 = &num as *const i32;
    let p2: *mut i32 = &mut num as *mut i32;
    unsafe {
        // 解引用原始指针
        println!("p1: {}", *p1);
        println!("p2: {}", *p2);
    }
}
```

### 调用unsafe函数或方法

> - unsafe函数或方法：在定义前加上unsafe关键字
>   
>   - 调用前需手动满足一些条件（主要靠看文档），因为rust无法对这些条件进行验证
>   
>   - 需要在unsafe块里进行调用
> 
> - 为什么要用unsafe修饰函数或方法：
>   
>   - 在unsafe函数或方法内部，所有不安全操作不再需要unsafe块
>   
>   - **让开发者明确承诺**：这个函数内部的操作可能破坏 Rust 的内存安全保证，但开发者**手动保证**了安全性

```rust
// unsafe 函数：开发者自己保证安全
unsafe fn dangerous_function() {
    let ptr = 0x12345 as *const i32;
    // 开发者也须确保这个地址是有效的
    // 不再需要unsafe块，因为整个函数块都是unsafe块
    println!("{}", *ptr); // 编译器不检查，后果自负
}

fn main() {
    // 在unsafe块中调用
    unsafe { dangerous_function() };
}
```

### 创建unsafe代码的安全抽象

> - 函数包含unsafe代码并不意味着需要将整个函数标记为unsafe
> 
> - 将unsafe代码包裹在安全函数中是一个常见的抽象

```rust

```

### 使用extern函数调用外部代码

> - extern关键字：简化创建和使用外部函数接口（FFI）的过程
> 
> - 外部函数接口：它允许一种编程定义函数，并让其它编程语言能调用这些函数
> 
> - 任何用extern修饰的块都是不安全的

**从其它语言调用rust函数**

> - 可以使用extern创建接口，其它语言通过它们可以调用rust的函数
> 
> - 在fn前添加extern关键字，并指定ABI
> 
> - 还需添加#[no_mangle]注解：避免rust在编译时改变它的名称

```rust

```

### 访问或修改一个可变静态变量

> - rust支持全局变量，但因为所有权机制可能产生某些问题，例如：数据竞争
> 
> - 在rust里，全局变量叫做静态（static）变量

**静态变量**

> - 静态变量名称建议全大写
> 
> - 必须标注类型
> 
> - 静态变量只能存储`'static`生命周期的引用，无需显示标注
> 
> - 访问不可变静态变量是安全的

```rust
// 声明静态变量
static HELLO: &str = "Hello, World!";
```

**常量和不可变静态变量的区别**

> - 静态变量：有固定的内存地址，使用它的值总会访问同样的数据
> 
> - 常量：允许使用它们的时候对数据进行复制
> 
> - 静态变量：可以是可变的，访问和修改可变的静态变量是不安全的

```rust

```

### 实现不安全trait

> - 当某个trait中存在至少一个方法拥有编译器无法校验的不安全因素时，就称这个trait是不安全的
> 
> - 声明unsafe trait：在定义前加unsafe关键字
>   
>   - 该trait只能在unsafe代码块中实现

```rust
unsafe trait Foo {
    // 内容
}

unsafe impl Foo for i32 {
    // 内容
}
```

## 高级 trait

### 在trait定义中使用关联类型

> - 关联类型：是trait中的类型占位符，它可以用于trait的方法签名中
>   
>   - 可以定义出包含某些类型的trait，而在实现前无需知道这些类型是什么

```rust
pub trait Iterator {
    // Item就是关联类型
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

**关联类型和泛型的区别**

| 关联类型 | 泛型  |
| ---- | --- |
|      |     |

### 默认泛型参数与类型重载

> - 可以在使用泛型参数时为泛型指定一个默认的具体类型
> 
> - 语法：`<PlaceholderType=ConcreteType>`
> 
> - 这种技术常用于运算符重载
> 
> - rust不允许创建自己的运算符及重载任意的运算符
> 
> - 但可以通过实现`std::ops`中列出的那些trait来重载一部分相应的运算符

**默认泛型参数**

```rust
// 泛型T默认是u32类型，但是也可以传入其它类
struct Point<T = u32> {
    x: T,
    y: T,
}
fn main() {
    // 默认是u32类型
    let p1: Point = Point { x: 1, y: 2 };
    let p2: Point = Point { x: 1, y: 2 };
    // 指定i32类型
    let p3: Point<i32> = Point::<i32> { x: -1, y: -9 };
    println!("{:?}", p1 + p2); // Point { x: 2, y: 4 }
}
```

**运算符重载**

```rust
use std::ops::Add;

fn main() {
    let p1: Point = Point { x: 1, y: 2 };
    let p2: Point = Point { x: 1, y: 2 };
    println!("{:?}", p1 + p2); // Point { x: 2, y: 4 }
}
#[derive(Debug)]
struct Point<T = u32> {
    x: T,
    y: T,
}
// 对 + 号 进行运算符重载
impl Add for Point {
    type Output = Point;

    fn add(self, other: Self) -> Self::Output {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}
```

### 完全限定语法，调用同名方法

#### 调用同名方法

> 不同的trait有同名的方法，被同一个结构体实现了，如何调用

**有self参数的同名方法**

```rust
trait A {
    fn print(&self) -> &str;
}

trait B {
    fn print(&self) -> &str;
}

struct Example;

impl Example {
    fn print(&self) -> &str {
        "print"
    }
}
impl A for Example {
    fn print(&self) -> &str {
        "A"
    }
}
impl B for Example {
    fn print(&self) -> &str {
        "B"
    }
}

fn test() {
    let e = Example;
    println!("{}", e.print()); // print 调用的是Example自身的方法
    // 调用trait中的同名方法
    println!("{}", A::print(&e));
    println!("{}", B::print(&e));
}
```

#### 完全限定语法

> - 当同名方法没有self参数时可以使用完全限定语法
> 
> - 完全限定语法：`<Type as Trait>::function(receiver_if_method, netx_arg, ...)`
>   
>   - 可以在任何调用函数或方法的地方使用
>   
>   - 允许忽略那些从其它上下文能推导出来的部分
>   
>   - 当rust无法区分期望调用哪个具体实现的时间，才需要使用这种语法

```rust
trait A {
    fn name() -> String;
}

trait B {
    fn name() -> String;
}

struct Example;

impl Example {
    fn name() -> String {
        "Example".to_string()
    }
}
impl A for Example {
    fn name() -> String {
        "A".to_string()
    }
}
impl B for Example {
    fn name() -> String {
        "B".to_string()
    }
}

fn test() {
    let e = Example;
    // 无self的同名方法
    println!("{}", Example::name()); // 调用Example自身的方法
    // 调用trait中的同名方法，使用完全限定语法
    println!("{}", <Example as A>::name());
    println!("{}", <Example as B>::name());
}
```

### 使用supertrait来让trait附带其它trait的功能

> - 需要在一个trait中使用其它trait的功能
>   
>   - 需要被依赖的trait也被实现
>   
>   - 那个被间接依赖的trait就是当前trait的supertrait

```rust
trait Father {
    // 必须有self
    fn print_name(&self, s: &str) {
        println!("{}", s);
    }
}

trait Son: Father {
    // 必须有self
    fn son_print_name(&self, s: &str) {
        // 可以使用被依赖的trait
        self.print_name("son");
    }
}

struct D;
// 必须同时实现trait和被依赖的trait的
impl Father for D {}
impl Son for D {
    // 这里可以重写
    fn son_print_name(&self, s: &str) {
        self.print_name(s);
    }
}
```

### 使用newtype模式在外部类型上实现外部trait

> - 孤儿规则：只有当trait或类型定义在本地包时，才能为该类型实现这个trait
> 
> - 可以通过newtype模式来绕过这一规则
>   
>   - 利用tuple struct（元组结构体）创建一个新的类型
> 
> - Vec<T>和Display都处于外部，正常是无法实现的

```rust
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

fn print() {
    let w = Wrapper(vec!["hello".to_string(), "rust".to_string()]);
    println!("w = {}", w); // w = [hello, rust]
}
```



## 高级类型

### 使用类型别名 创建类型同义词

> - Rust提供了类型别名的功能
>   
>   - 使用type关键字

```rust
type number = i32;

fn num_add() {
    let x: i32 = 8;
    let y: number = 90;
    println!("{}", x + y);
}

struct Type<T> {
    data: T,
}
// 别名里可以使用泛型
type t<T> = Type<T>;
```

### Never类型

> - 有一个名为`!`的特殊类型
>   
>   - 它没有任何值，称为空类型（empty type）
>   
>   - 倾向于称为never类型，因为他在不返回的函数中充当返回类型
> 
> - 不返回值的函数也被称为发散函数
> 
> - 一般函数，哪怕不返回值也会有一个空元组`()`返回

### 动态大小 和 Sized Trait

> - rust需要在编译时确定一个特定类型的值分配多少空间
> 
> - 动态大小的类型的概念：
>   
>   - 编写代码时使用只有在运行时才能确定大小的值
> 
> - str是动态大小的类型（注意不是&str）：只有运行时才能确定字符串的长度
>   
>   - 下面的代码是无法正常工作的，因为rust为相同类型分配相同的空间，下面str长度是不一样的：
>     
>     - `let s1: str = "hello there!";`
>     
>     - `let s2: str = "how's it going?";`
>   
>   - 使用&str来解决，这个切片的长度是确定的，存有：
>     
>     - str的地址
>     
>     - str的长度

**rust使用动态大小类型的通用方式**

> - 附带一些额外的元数据来存储动态信息的大小
>   
>   - 使用动态大小类型时总会把它的值放在某种指针后边

**另外一种动态大小的类型：trait**

> - 每个trait都是一个动态大小的类型，可以通过名称对其进行引用
> 
> - 为了将trait用作trait对象，必须将他放置在某种指针之后
>   
>   - 例如：`&dyn Trait`或`Box<dyn Trait>`或`Rc<dyn Trait>`之后

#### Sized trait

> - 为了处理动态大小的类型，rust提供了一个`sized trait`来确定一个类型的大小在编译时是否已知
>   
>   - 编译时可计算出大小的类型会自动实现这个trait
>   
>   - rust还会为每一个泛型函数隐式的添加Sized约束
> 
> - 默认情况下，泛型函数只能被用于编译时已经知道大小的类型，可以通过特殊语法`? Sized trait`解除这一限制

## 高级函数和闭包

### 函数指针

> - 可以将函数传递给其它函数
> 
> - 函数在传递过程中会被强制转换为fn类型
> 
> - fn类型就是函数指针

```rust
fn add_one(x: i32) -> i32 {
    x + 2
}
fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg)
}

fn ttt() {
    println!("{}", do_twice(add_one, 5)); // 7
}
```

### 函数指针与闭包的不同

> - 闭包是一种trait
> 
> - fn是一个类型，不是一个trait
>   
>   - 可以直接指定fn为参数类型，不用声明一个以Fn trait为约束的泛型参数
> 
> - 函数指针实现了全部3种闭包trait（Fn，FnMut，FnOnce）：
>   
>   - 总是可以把函数指针用作参数传递给一个接受闭包的函数
>   
>   - 所以，倾向于搭配闭包trait的泛型来编写函数：可以同时接受闭包和普通函数
> 
> - 某些情景，只想接收fn而不接收闭包：
>   
>   - 与外部不支持闭包的代码交互：C函数

### 返回闭包

> - 闭包使用trait进行表达，无法在函数种直接返回一个闭包，可以将一个实现了该trait的具体类型作为返回值

## 宏 macro

> - 宏在rust里指的是一组相关特性的集合称谓
>   
>   - 使用macro_rules构建的声明宏
>   
>   - 3种过程宏
>     
>     - 自定义`#[derive]`宏，用于struct或enum，可以为其指定随derive属性添加的代码
>     
>     - 类似属性的宏，在任何条目上添加自定义属性
>     
>     - 类似函数的宏，看起来像函数调用，对其指定为参数的token进行操作

**函数与宏的差别**

> - 本质上，宏是用来编写可以生成其它代码的代码（元编程）
> 
> - 函数在定义签名时，必须声明参数的个数和类型，宏可处理可变的参数
> 
> - 编译器会在解释代码前展开宏
> 
> - 宏的定义比函数复杂得多，难以阅读、理解、维护
> 
> - 在某个文件调用宏时，必须提前定义宏或将宏引入当前作用域
> 
> - 函数可以在任何位置定义并在任何位置使用

### macro_rules!声明宏

> - rust中最常见的宏形式：声明宏
>   
>   - 类似match的模式匹配
>   
>   - 需要使用marco_rules!

```rust
macro_rules! create_vec {
    ($($x: expr),*) => {
        // 这个括号内只能放表达式
        // 这里因为有多行，所以需要一个花括号包裹起来，变成一个表达式
        {
            let mut temp_vec = Vec::new();
            $(temp_vec.push($x);)*
            temp_vec
        }
    };
}
// 上面的宏等同于
//let mut temp_vec = Vec::new();
//temp_vec.push(1);
//temp_vec.push(2);
//temp_vec
fn mm() {
    let v = create_vec![1, 2, 3, 4];
}
```

### 基于属性来生成代码的过程宏

> - 这种形式更像函数（某种形式的过程）一些
>   
>   - 接收并操作输入的rust代码
>   
>   - 生成另外一些rust代码作为结果
> 
> - 三种过程宏：
>   
>   - 自定义派生
>   
>   - 属性宏
>   
>   - 函数宏
> 
> - 创建过程宏时：
>   
>   - 宏定义必须单独放在它们自己的包中，并使用特殊的包类型
