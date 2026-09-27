# go 语言

## 变量

### 变量声明

`var name type`
整型和浮点型变量的默认值为 0。 字符串变量的默认值为空字符串。 布尔型变量默认为 false。 切片、函数、指针变量的默认为 nil

例：

```go
var a int = 10
var str string = "sdfe"
var n float
var xx,yy,aa int = 200, 300, 400
var (
    vv int = 90
    jj bool = true
)
//省略类型
var i = 20
var kk, ll = 400, "sdf"
// 省略var同时省略类型，只能在函数内使用(也就是只能声明局部变量)
a := 20
```

### 常量

```go
//不需要var
const op int = 90
//const定义枚举类型
const (
    A = 0
    B = 1
    C = 2
)
// iota关键字，可以在const()添加一个关键字iota，第一行的iota的值默认值为0(第二行的iota默认是1 依次类推)，后面每行iota都会累加1（且可以省略）
// iota只能出现在const中
const (
    Q = iota    // 0
    M   // 1
    L   // 2
    P   // 3
)
const (
    Q = iota * 10   // 0 * 10
    M   // 1 * 10
    L   // 2 * 10
    P   // 3 * 10
)
const (
        Q = -1
        M = iota    // M = 1
        L
        P
    )
// 可以跳过某些值
const (
    I = iota
    _
    L
    P
)
```

### 变量类型

#### 基本数据类型

1. 整形
   
   - 有符号：int、int8、int16、int32、int64
   - 无符号：uint、uint8、uint16、uint32、uint64
   - 整形转换：`a`是`int8`转成`int16` int16(a)

2. 浮点型
   
   - 类型关键字：float32、float64
     
     > 注：浮点型存在精度问题

3. 布尔型
   
   - 类型关键字：bool
     
     > 注：
     > 
     > - 布尔类型变量的默认值为 false
     > - Go 语言中不允许将整型强制转换为布尔型
     > - 布尔型无法参与数值运算，也无法与其它类型进行转换

4. 字符串 -- 双引号定义
   
   - 类型关键字：string
   - 转义字符
     1. `\n`
     2. `\r`
     3. `\t`
     4. `\`
   - 常用方法
     1. `len(str)`：求长度
     2. `+`或`fmt.Sprintf`：拼接字符串
     3. `strings.Split`：分割
     4. `strings.contains`：判断是否包含
     5. `strings.HasPrefix`和`strings.HasSuffix`：前缀/后缀判读
     6. `strings.Index()`和`strings.LastIndex()`：子串出现的位置
     7. `strings.Join(str[], string)`：切片转成字符串，元素之间用第二个参数连接

5. 字符类型 -- 单引号定义
   
   - byte、rune
   - 注：
     - 直接打印字符会输出对应 ASCII 码
     - byte 型字符(uint8 类型)是指 ASCII 表中一个字符，rune 类型代表一个 utf-8 字符
     - 字符转字符串: `string(by)`

```go
// 循环输出字符串中的字符
for _, s := range str {
    fmt.Println(s)
}
```

#### 基本数据类型之间的转换

> 注意：最好从范围小的转成范围大的

1. 整型之间的转换

```go
var a int8 = 10
var b int16 = 20
fmt.Println(int16(a) + b)
```

2. 浮点型之间的转换

```go
fmt.Println(float64(a) + b)
```

3. 其它类型转成 string 类型
- 使用`strconv`

```go
// 将
const A string = "sss"
const N uint = 20
fmt.Println(A + strconv.FormatUint(uint64(N), 10))
```

4. 将 string 类型转成其它类型
- 使用`strconv`

```go
const N string = "2345"
n,_ := strconv.ParseInt(SN, 10, 64)
fmt.Println(n)
```

5. bool 类型不能转成其它类型，其它类型也不能转成 bool 类型

#### 复合数据类型 -- 详细看后面

- 数组 -- 值类型
- 切片 -- 引用类型
- 结构体 -- 值类型
- 函数
- map -- 引用类型
- 通道(channel)
- 接口

## 运算符

### 算术运算符

> +、-、\*、/、%、++、--(++、--只能单独使用,没有前置++、--)

```go
var a = 0
// var b = a++ // 不能这样写
a++
var b = a
```

### 位运算符

> 针对 2 进制位进行运算的

1. `&`
   
   > 两位均为 1 才为 1

2. `|`
   
   > 两位有一个为 1 就为 1

3. `^`
   
   > 异或，两位不一样则为 1

4. `<<`
   
   > 左移 n 位就是乘以 2 的 n 次方

5. `>>`
   
   > 右移 n 位就是乘以 2 的 n 次方

## 流程控制

### if 语句

```go
age := 20
if age >= 18 {
    fmt.Println(age)
} else if age >= 10 {
    fmt.Println(age)
} else {
    //
}
// 也可以写成下面这样，下面的age是局部变量只能在if语句中使用
if age := 20; age >= 18 {
    //
}
```

### for 语句

> for 初始语句; 条件表达式; 自增 {
> // 语句
> }

```go
for i:=0; i<10; i++ {
    fmt.Print(i)
}
// 可以把初始化语句写在外面
i:=0
for ; i<10;i++ {
    fmt.Print(i)
}
// 可以把初始化语句写在外面，同时省略自增语句
i:=0
for i<10 {
    fmt.Print(i)
    i++
}
```

### for-range 语句

> go 语言中可以使用 for-range 遍历数组、切片、字符串、map、通道(channel)。
> 通过 for-range 遍历的返回值有以下规律：
> 
> - 数组、切片、字符串返回索引和值
> - map 返回键和值
> - 通道(channel)只返回通道内的值

```go
str := "你好golang"
for k, v := range str {
    fmt.Println(k, string(v))
}
```

### switch-case 语句

### break、continue、goto

## 复合数据类型

### 数组

> go 中的数组是值类型的

#### 定义

```go
// 定义
// 未赋值的数组会有初始值
var arr [6]int
// 定义同时赋值
var arr1 = [3]int{1, 2, 3}
// 简写定义
arr1 := [3]int{1, 2, 3}
// 省略数组长度，确定长度后数组的长度不能再改变
var arr1 = [...]int{1, 2, 3}
// // 定义，同时赋值并指定下标
arr3 := [...]int{0:1, 3:1}
```

#### 遍历

```go
// 普通for
for i:=0; i < len(arr3); i++ {
    sum += arr3[i]
}
// for-range
for k, v := range arr3 {
    sum += v
}
```

#### Go 的数组是值类型

```go
arr2 := [...]int{1, 2, 3}
arr4 := arr2
arr2[0] = 10
fmt.Println(arr2, arr4)    // [10 2 3] [1 2 3]
```

#### 二维数组

```go
var arr = [3][2]string{
    {"ss", "as"},
    {"asd", "were"}
}
```

### 切片

> 切片是`引用类型`，切片是一个拥有相同类型元素的可变长度的序列，支持自动扩容。它的内部结构包含地址、长度和容量。切片是对数组的一种封装。

#### 定义切片

```go
var arr5 = []int{1,2,3,4}
arr5 := []int{1,2,3,4}
// 指定下标
arr := []int{0:1,2:1}
//定义一个长度为0，容量为0的切片
var arr []int
// 从数组中获取切片
a := [5]int{1,2,3,4,5}
b := a[:]    // 包括数组中的所有元素
c := a[1:4]    // 从下标1-下标3，左包右不包
d := a[:4]    // 从下标0-下标3
// 基于切片的切片
f := d[1:]
```

#### 切片的长度和容量

> 切片拥有自己的长度和容量，可以用`len()`函数求长度，使用`cap()`函数求切片的容量

> 切片的长度：就是它所包含的元素个数

> 切片的容量：从第一个元素到底层数组元素末尾的个数

```go
a := [5]int{1,2,3,4,5}
s1 := a[3:5]    // 长度1，容量2，底层数组为[4,5]（从下标3-到数组末尾
```

#### 使用 make()函数构造切片

> 语法：`make([]T, size, cap)`

```go
var sliceA = make([]int, 4, 8)
fmt.Println(sliceA) // [0,0,0,0]，都是默认值
```

#### 切片常用方法

1. append()
   
   > `append()`可以为切片动态添加元素，当容量不够时可以自动扩容。
   > `append()`可以合并两个切片
   
   ```go
   // 定义一个长度、容量为0的切片
   var arr6 []int
   fmt.Println(len(arr6), cap(arr6))    // 0，0
   arr6 = append(arr6, 12)
   fmt.Println(len(arr6), cap(arr6))    //1，1
   var arr7 = []int{1,2}
   arr7 = append(arr6, arr7)
   ```

2. copy()
   
   > 切片是引用数据类型，用切片复制给切片会有引用问题，`copy()`函数复制切片是值复制
   
   ```go
   var arr5 = []int{1,2,3,4}
   copyarr := arr5
   // 同时会改变arr5
   copyarr[0] = 20
   fmt.Println("copy", copyarr, arr5)   // copy [20 2 3 4] [20 2 3 4]
   // 使用copy
   var arr5 = []int{1,2,3,4}
   copyarr := make([]int, 4, 4)
   copy(copyarr, arr5)
   copyarr[0] = 20
   fmt.Println("copy", copyarr, arr5)   // copy [20 2 3 4] [1 2 3 4]
   ```

3. 删除切片中元素的方法
   
   > go 语言中没有专门删除切片的方法，可以使用切片本身的特性来删除
   
   ```go
   a := []int{1,2,3,4,5}
   // 只要前两个
   a = append(a[:2])
   // 只要前两个和后两个,最后一个要加展开符...
   a = append(a[:2], a[3:]...)
   ```

4. 切片中的排序 sort 包

### map

> map 是一种无序的 key-value 类型的数据结构，go 中的 map 是引用类型，必须初始化才能使用。

#### 定义 map 变量

1. 使用 make 创建 map 变量
   
   ```go
   // 不初始化的map值为nil
   useinfo := make(map[string]string)
   useinfo["name"] = "wang"
   useinfo["age"] = "20"
   fmt.Println(useinfo)    // map[age:20 name:wang]
   ```

2. 声明 map 变量同时填充元素
   
   ```go
    user := map[string]string{
       "name":"li",
       "age":"22", // 这个 , 号必需要加
   }
   fmt.Println(user)    // map[age:22 name:li]
   ```

#### 遍历 map 变量

```go
user := map[string]string{
    "name":"li",
    "age":"22",
}
for key, value := range user {
    fmt.Println(key, value)
}
```

#### map 变量的增删改查

1. 增

```go
useinfo := make(map[string]string)
useinfo["name"] = "wang"
```

2. 查

```go
useinfo := make(map[string]string)
useinfo["name"] = "wang"
v, ok := userinfo["name"]   // 如果name键值对存在，则v是对应值，ok为ture
fmt.Println(useinfo["name"])
```

3. 改

```go
useinfo := make(map[string]string)
useinfo["name"] = "wang"
useinfo["name"] = "li"
```

4. 删 -- delete 函数

```go
// 第一个参数是map变量，第二个是要删除的键
delete(useinfo, "name")
```

#### 元素为 map 变量的切片

```go
// 下面切片定义会初始化map变量，map变量的初始值是nil
sliceUser := make([]map[string]string, 5,5)
if sliceUser[0] == nil {
    // 因为map变量的初始值是nil，所以这里必须要用make初始化一下
    sliceUser[0] = make(map[string]string)
    sliceUser[0]["name"] = "wang"
    sliceUser[0]["age"] = "24"
}
fmt.Println(sliceUser[0])
```

#### 值的类型为切片的 map 变量

```go
mapSlice := make(map[string][]string)
mapSlice["love"] = []string{
    "yang",
    "li",
    "ling",
}
fmt.Println(mapSlice["love"])   // [yang li ling]
```

## 函数

### 函数声明

```go
// 单返回值
func foo1(a string, b int) int {
    fmt.Println(a)
    fmt.Println(b)
    c := 90
    return c
}

// 对形参的定义可以简化
func foo1(a, b int) int {
    fmt.Println(a)
    fmt.Println(b)
    c := 90
    return c
}

// 可变参数，可变参数只能放在最后
// 这里的x是一个切片
func sumFn1(x ...int) int {
    sum := 0
    for _, v := range x {
        sum += v
    }
    return sum
}

// 多返回值，匿名的
func foo2(a string, b int) (int, string) {
    fmt.Println(a)
    fmt.Println(b)
    c := 90
    return c, "sdg"
}

// 多返回值，有名的
func foo3(a string, b int) (r1 int, r2 string) {
    fmt.Println(a)
    fmt.Println(b)
    // 未赋值前，r1, r2均为默认值，即为0、''
    //给返回值赋值
    r1 = 300
    r2 = "sdf"
    return  //直接写个return就可以返回了
}

// 多返回值且类型相同，有名的
func foo3(a string, b int) (r1, r2 int ) {
    fmt.Println(a)
    fmt.Println(b)
    //给返回值赋值
    r1 = 300
    r2 = 400
    return  //直接写个return就可以返回了
}

// 调用
func main() {
    c := foo1("sdf", 39)
    fmt.Println(c)

    v1, v2 := foo2("sd", 9)
    fmt.Println(v1, v2)

    d1, d2 := foo3("dd", 45)
    fmt.Println(d1, d2)
}
```

### 引用类型的形参

> 给函数传递引用类型的实参，在函数内对形参的更改会影响实参

### 函数类型的形参、返回值、匿名函数

#### 用 type 自定义类型

```go
// 自定义类型calc
type calc func(int, int) int
func foo1(a, b int) int {
    fmt.Println(a)
    fmt.Println(b)
    c := 90
    return c
}
var cc calc
// 这个可以赋值
cc = foo1   // 只能赋值函数签名为func(int, int) int的函数
// 下面不行
// cc = 1

type myInt int
// 注意myInt和int不是同一个类型，哪怕知道它们都是int类型
var a int = 10
var b myInt = 20
```

#### 函数类型的形参、实参

```go
type calcType func(int, int) int
// 函数类型的形参
func calc(x, y int, cb calcType) int {
    return cb(x, y)
}
// 匿名函数
j := calc(3, 4, func(x, y int) int {
    return x * y
})

func add(x, y int) int {
    return x + y
}
func sub(x, y int) int {
    return x - y
}
func calc(x, y int) int {
    return x * y
}
// 函数类型的返回值
func do(o string) calcType {
    switch(o){
        case "+":
            return add
        case "-":
            return sub
        default:
            return calc
    }
}
```

#### 匿名函数

> go 语言的函数是不允许嵌套定义的，也就是函数内不能定义具名函数，但是可以定义匿名函数

```go
//定义匿名函数
func(){
    fmt.Println("test..")
}
// 自执行匿名函数
func(){
    fmt.Println("test..")
}()
// 或者下面也行
(func(){
    fmt.Println("test..")
})()
// 将匿名函数赋值给变量
var ff = func(){
    fmt.Println("test..")
}
ff()
```

#### 闭包

> 闭包的一个作用是将局部变量保存到作用域外，但同时不污染全局变量。对这个局部的修改也会被保存

```go
func adder() func(int) int {
    var x int = 9   // 局部变量
    return func(y int) int {
        x += y  // 这里会保存局部变量
        return x
    }
}

ad := adder()   // ad内部保存了局部变量，但是在外部访问不了这个变量
fmt.Println(ad(9))  // 18

// 对这个局部的修改也会被保存
func adder() func(int) int {
    var x int = 9
    return func() int {
        x ++
        return x
    }
}
ad := adder()
fmt.Println(ad(), ad()) // 10， 11
```

### defer、panic、recover 的使用

#### defer

> go 语言中的 defer 语句会将其后面跟随的语句进行延迟处理。在 defer 归属的函数即将返回时，在 return 之前才会处理。

> 对有多个 defer 的函数，defer 后面的语句会被压入一个栈中，按出栈顺序进行执行

> defer 后面的匿名函数如果有参数，则在该函数执行时参数值要明确

> 在 go 中 return 语句不是一个原子操作，defer 语句执行的时机就在返回值赋值操作后，所以在对具名返回值和不具名返回值进行 defer 时是不一样的。
> <img src="./pic/go/屏幕截图 2026-01-08 161900.png">

```go
func main() {
    // 打印顺序 3 2 1
    defer fmt.Println("1")
    defer fmt.Println("2")
    defer fmt.Println("3")
}
//在对具名返回值和不具名返回值进行 defer 时是不一样的。
// 具名返回值与匿名返回值的不同之处在于，具名返回值会直接参与函数内的相关运算，而匿名返回值是不参与的，所以在defer中的操作不会影响到匿名返回值。
func f1() int {
    a := 0
    defer func () int {
        a++
        return a
    }()
    return  a   // 这里会先将a赋值给匿名返回值，再去执行defer，此时已经不影响匿名返回值了
}

func f2() (a int) {
    defer func () int {
        a++
        return a
    }()
    return  5   // 这里会先执行a=5，然后去执行defer，defer后的匿名函数是a++，此时a=6，然后返回
}

fmt.Println(f1(), f2()) // 打印 0 6
```

#### panic、recover

> 在 go 中是没有异常机制的，使用 panic、recover 来处理错误，panic 可以在任何地方引发，但 recover 只有在 defer 调用的函数中有效。

```go
func fn1() {
    fmt.Println("fn1")
}
func fn2()  {
    defer func ()  {
        err := recover()
        if err != nil {
            fmt.Println(err)
        }
    }()
    panic("一个异常")
}

func main() {
    fn1()   // fn1
    fn2()   // 一个异常
}

// error库

```

### time 包以及日期函数

```go
func main() {
    timeObj := time.Now()
    fmt.Println(timeObj) // 2026-01-08 18:58:07.8642724 +0800 CST m=+0.000000001
    fmt.Println(timeObj.Year())
    fmt.Println(timeObj.Month())
    fmt.Println(timeObj.Day())
    fmt.Println(timeObj.Hour())
    fmt.Println(timeObj.Minute())
    fmt.Println(timeObj.Second())
    // 获取时间戳
    fmt.Println(timeObj.Unix())
    // 把时间戳转成时间对象
    timeObj := time.Unix(timeObj.Unix(), 0)

    // 把字符串转成时间对象
    str := "2026-01-08 09:11:25"
    // 第一个参数是时间模板
    timeObj2,_ := time.ParseInLocation("2006-01-02 03:04:05", str, time.Local)
    fmt.Println(timeObj2)

    // 时间常量，下面仅列出1s这个常量
    time.Second // 1s

    // 格式化输出
    // 2006 表示年
    // 01 表示月
    // 02 表示日
    // 03 表示时-12小时制    15 表示24小时制
    // 04 表示分
    // 05 表示秒
    fmt.Println(timeObj.Format("2006-01-02 03:04:05"))  // 2026-01-08 09:06:55
}
```

#### 定时器

> 使用`time.NewTicker(时间间隔)`来设置定时器

```go
ticker := time.NewTicker(time.Second)
n:=0
for t := range ticker.C {
    fmt.Println(t)
    n++
    if n > 5 {
        ticker.Stop()
        break
    }
}
```

#### sleep 休眠方法

> `time.Sleep(时间间隔)`

## 指针

> go 的指针不能指针运算

### 定义指针

> 关于指针有两个常用的操作符，一个是取地址符`&`，另一个是解引用符`*`。对一个变量进行取地址，会返回对应类型的指针。

```go
var a = 10
// nb是一个nil
var nb *int
var b *int = &a
// 解引用
fmt.Println(*b)
// 指向指针的指针
var c **int = &b    // 简写 c := &b
```

> 引用类型使用前需要用`make`或者`new`分配空间

1. 用`new`函数分配内存 --- 初始化的变量为对应类型的默认值
   
   > `new`是一个内置的函数，它的函数签名为：`func new(Type) *Type`
   > 
   > * 返回值是类型指针
   > * 接收参数是类型
   > * 专用于给指针分配内存空间
   
   ```go
   var a = new(int) // a是一个指针变量，类型是*int，*a为0（int的默认值）
   // 示例
   func main() {
      fmt.Println(*new(string))
      fmt.Println(*new(int))
      fmt.Println(*new([5]int))
      fmt.Println(*new([]float64))
   }
   // 输出
   
   0
   [0 0 0 0 0]
   []
   ```

2. 用`make`函数分配内存
   
   ```go
   func make(t Type, size ...IntegerType) Type
   ```
   
   > `make` 只用于 `slice`、`map`、`channel` 的初始化，返回的还是这三个引用类型本身
   > 
   > * 返回值是值，不是指针
   > * 接收的第一个参数是类型，不定长参数根据传入类型的不同而不同
   > * 专用于给切片，映射表，通道分配内存。

## 结构体

> 结构体是<mark>值类型</mark>

> 结构体中字段名首字母小写的是私有字段，大写的是公有字段

### 用 type 自定义类型

> 自定义类型与其它自定义类型不能相互赋值，哪怕它们底层是相同类型

```go
type myInt1 int
type myInt2 int

func main() {
    var a myInt1 = 10
    fmt.Printf("%T", a) // main.myInt
    var b myInt2 = 20
    if a == b {    // ❌，不能相互比较，不是相同类型
    //...
    }
}
```

### 结构体的定义

> go 中用`type`和`struct`关键字来定义结构体

```go
// type 类型名 struct {
//  字段名 字段类型
//  字段名 字段类型
//}
type Person struct{
    name string
    age int
    gender string
}
```

### 结构体实例化

```go
// 结构体是值类型，不需要make分配空间
var p1 Person
p1.age = 20
p1.name = "wang"
p1.gender = "male"

// 返回结构体指针
var p2 = new(Person)
p2.age = 20 // 等价于 (*p2).age = 20
p2.name = "ding"    // 等价于 (*p2).name = "ding"
p2.gender = "male"  // 等价于 (*p2).gender = "male"
fmt.Println(*p2)

// 与上面差不多
var p3 = &Person{}  // 返回结构体指针

// 实例化的同时初始化
var p4 = Person{
    name: "li",
    age: 20,
    gender: "male",
}
// 实例化的同时初始化，返回一个指针结构体
var p5 = &Person{
    name: "li",
    age: 20,
    gender: "male",
}
//不完全初始化
var p6 = Person{
    name: "li",
}
// 省略键名，必须保持顺序
var p7 = Person{
    "li",
    20,
    "male",
}
```

### 结构体方法

> go 语言中，没有类的概念但是可以给类型（<mark>结构体、自定义类型</mark>）定义方法。所谓方法就是定义了接收者的函数。
> 
> 首字母大写的结构体方法是公有的，可以在其它包中直接调用。
> 
> 首字母小写的结构体方法是私有的，仅限于同一包中调用。

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}

type Person struct{
    name string
    age int
    gender string
}
// 下面两种方式对调用者是否是指针没有要求
// 给结构体定义方法，这种方式不能修改调用结构体变量的属性
// 可以理解为，调用者将值赋值给接收者p，由p来进行方法内的操作，这些操作无法影响调用者
func (p Person) Run() {
    fmt.Println("run", p.name)
}
// 给结构体指针定义方法，可以修改调用结构体变量的属性
// 可以理解为，调用者将地址赋值给接收者p，由于p是指针，所以p进行的操作会影响到调用者
func (p *Person) SetName(name string) {
    p.name = name
}
func main() {
    var p1 Person
    p1.age = 20
    p1.name = "wang"
    p1.gender = "male"
    p1.Run()
    p1.SetName("ding")
}
```

> 给自定义类型添加方法的方式与结构体类似

### 结构体嵌套

#### 结构体匿名字段

> 结构体运行其成员字段在声明时没有字段名而只有类型，这种没有名字的字段就称为匿名字段。常用于结构体嵌套。

```go
// 匿名字段的类型不能重复
type Pp struct {
    string
    int
}
// 实例化
pp := Pp{
    "ling",
    18,
}
```

#### 结构体嵌套定义

```go
type User struct {
    Username string
    Passwd string
    Address Address
}
type Address struct {
    Name string
    Phone string
}

var u User
u.Username = "li"
u.Address.Name = "city"

//嵌套匿名结构体
type User struct {
    Username string
    Passwd string
    Address // 匿名字段
}
type Address struct {
    Name string
    Phone string
}
// 使用是一样的
var u User
u.Username = "li"
u.Address.Name = "city"
// 也可以简写
u.Phone = "234567"  // 当在结构体中找不到时相应字段，会去匿名结构体去找
```

> 当多个匿名字段(类型为结构体的)有相同字段时，不能使用简写

### 结构体继承

> go 中的结构体继承是通过结构体嵌套实现的

```go
// 定义父结构体
type Animal struct {
    Name string
}
// 定义结构体方法
func (a Animal) run() {
    fmt.Printf("%v，在运动", a.Name)
}

type Dog struct {
    Age int
    Animal  // 嵌套继承
}

func (d Dog) wang() {
    fmt.Printf("%v，在汪汪", d.Name)
    d.run()
}
```

### 结构体和 JSON 相互转换(序列化和反序列化)

> 结构体中字段名首字母小写的是私有字段，大写的是公有字段，<mark>私有属性不能被 json 包访问</mark>

```go
type Student struct {
    Id     int  // 可以通过tag实现json序列化该字段的key，如下面
    Gender string   `json:"gender"`
    Name   string
    Sno    string
}

s1 := Student{
    Id:     10,
    Gender: "male",
    Name:   "ding",
    Sno:    "90",
}
// 结构体 ==》 json
jsonByte, _  := json.Marshal(s1)
jsonStr := string(jsonByte)
fmt.Println(jsonStr)    // {"Id":10,"Gender":"male","Name":"ding","Sno":"90"}

// json ==》结构体
str := `{"Id":10, "Gender":"male", "Name":"ding", "Sno":"90"}`
var s2 Student
err := json.Unmarshal([]byte(str), &s2) // 这里必须传入结构体地址
if err != nil {
    fmt.Println(err)
}
fmt.Printf("%#v", s2)
```

## 各个类型的初始值

| 类型                                            | 零值          | 说明               |
| --------------------------------------------- | ----------- | ---------------- |
| **数值类型**                                      |             |                  |
| `int`, `int8`, `int16`, `int32`, `int64`      | `0`         | 所有整数类型           |
| `uint`, `uint8`, `uint16`, `uint32`, `uint64` | `0`         | 无符号整数            |
| `float32`, `float64`                          | `0.0`       | 浮点数              |
| `byte`                                        | `0`         | byte 是 uint8 的别名 |
| `rune`                                        | `0`         | rune 是 int32 的别名 |
| `complex64`, `complex128`                     | `(0+0i)`    | 复数类型             |
| **布尔类型**                                      |             |                  |
| `bool`                                        | `false`     | 布尔值              |
| **字符串类型**                                     |             |                  |
| `string`                                      | `""`        | 空字符串             |
| **复合类型**                                      |             |                  |
| `指针（pointer）`                                 | `nil`       | 任何类型的指针          |
| `接口（interface）`                               | `nil`       | 接口类型             |
| `切片（slice）`                                   | `nil`       | 但可以正常使用`append`  |
| `映射（map）`                                     | `nil`       | 不能直接赋值，需要`make`  |
| `通道（channel）`                                 | `nil`       | 需要`make`初始化      |
| `函数（function）`                                | `nil`       | 函数类型             |
| **结构体（struct）**                               |             |                  |
| `结构体`                                         | 所有字段对应类型的零值 | 每个字段递归地赋零值       |
| **数组（array）**                                 |             |                  |
| `数组`                                          | 每个元素对应类型的零值 | 长度固定，元素全为零值      |

## go 的包(模块化)

> 通过`go get`引用库
> 
> 通过`go intall`安装命令行程序

> go 中的包可以分为三种：
> 
> 1. 系统内置包
> 2. 自定义包
> 3. 第三方包

1. 系统内置包 -- 引入后直接使用
   
   * fmt
   * strconv
   * strings
   * sort
   * errors
   * time
   * encoding/json
   * os
   * io 等

2. 自定义包 -- 开发者自己写的包

3. 第三方包 -- 需要下载到本地才可以使用

### 编写模块

> Go Module 本质上是基于 VCS（版本控制系统），当你在下载依赖时，实际上执行的是 VCS 命令，比如`git`，所以如果你想要分享你编写的库，只需要做到以下三点：
> 
> * 源代码仓库可公开访问，且 VCS 属于以下的其中之一
>   * git
>   * hg (Mercurial)
>   * bzr (Bazaar)
>   * svn
>   * fossil
> * 是一个符合规范的 go mod 项目
> * 符合语义化版本规范

**准备**

> 启用了 Go Module，通过`go env GO111MODULE`来查看

**创建**

> 在github上创建一个仓库，创建完成后，可以看到仓库的 URL 是`https://github.com/246859/hello`，对应的 go 模块名就是`github.com/246859/hello`，克隆到本地，通过`go mod init`命令初始化模块。

**编写代码**

**编写测试**

**编写文档**

**提交代码**

### 使用模块

### 工作区

> 工作区(workspace)，是 Go 在 1.18 引入的关于多模块管理的一个新的解决方案，旨在更好的进行本地的多模块开发工作，下面将通过一个示例进行讲解。

**示例**

> 项目下有两个独立的 go 模块，分别是`auth`，`user`

```textile
LICENSE
README.md
auth
go.work
user
```

> `auth`模块依赖于`user`模块的结构体`User`，内容如下

```go
package auth

import (
  "errors"
  "github.com/246859/work/user"  // 导入user
)

// Verify user credentials if is ok
func Verify(user user.User) (bool, error) {
  password, err := query(user.Name)
  if err != nil {
    return false, err
  }
  if password != user.Password {
    return false, errors.New("authentication failed")
  }
  return true, nil
}

func query(username string) (string, error) {
  if username == "jack" {
    return "jack123456", nil
  }
  return "", errors.New("user not found")
}
```

> 在这个项目中，我们可以这样编写`go.work`文件

```go
go 1.22

use (
  ./auth
  ./user
)
```

**命令**

> 下面是一些工作区的命令

| 命令               | 介绍                  |
| ---------------- | ------------------- |
| `go work edit`   | 编辑`go.work`         |
| `go work init`   | 初始化一个新的工作区          |
| `go work sync`   | 同步工作区的模块依赖          |
| `go work use`    | 往`go.work`中添加一个新模块  |
| `go work vendor` | 将依赖按照 vendor 格式进行复制 |

**指令**

> `go.work`文件的内容很简单，只有三个指令
> 
> * `go`，指定 go 版本
> * `use`，指定使用的模块
> * `replace`，指定替换的模块



### 包管理工具 -- go mod

1. `go mod init` 初始化项目
   
   > 在项目开发中需要在项目目录中用`go mod init 项目名`命令生成一个 go.mod 文件管理项目依赖
   > 每个项目都需要一个`main.go`作为包的入口

2. 自定义包
   
   > 在初始化后的项目中，在项目文件夹中新建一个文件夹，在这个文件夹中写 go 源文件(这个源文件中的首字母大写的变量和函数是公有的，其它文件导入后可以使用，小写是私有的只能在本文件使用)
   > <img src="./pic/go/屏幕截图 2026-01-09 153049.png">

### go 包中的 init() 函数

> 在 go 中执行导入包语句会自动触发包内部`init()`函数的调用。需要注意的是：`init()`函数没有参数也没有返回值。自动执行。

> 包初始化执行的顺序如下所示：
> <img src="./pic/go/屏幕截图 2026-01-09 153608.png">
> 
> <img src="./pic/go/屏幕截图 2026-01-09 153713.png">

### 使用第三方包

> 可以在[网址](https://pkg.go.dev)查找常见的 golang 第三方包

例：安装 decimal 包(解决 float 精度损失的包)

> 查到这个包的地址为：https://github.com/shopspring/decimal

```bash
go get github.com/shopspring/decimal
# 或者
go install github.com/shopspring/decimal
```

> 包安装完毕后，可以使用`go mod tidy`命令补上依赖包(相当于 npm i)，去掉未使用的包

> go 包都安装在 `$GOPATH/pkg/mod` 里全局共享，如果一个包已经存在那么使用安装命令不会重复下载，同一个包的不同版本可以共存，不会将第三方包拷贝进项目里

> 可以用 `go clean -modcache`命令清除掉 `$GOPATH/pkg/mod` 里的全局共享包

## 测试

> 在go语言中使用`go test`作为测试命令，在包目录内，所有以`_test.go`为后缀名的源文件在执行go build时不会被构建成包的一部分，它们是go test测试的一部分。
> 
> 在`*_test.go`文件中，有三种类型的函数：
> 
> - 测试函数
> 
> - 基准测试（benchmark）函数
> 
> - 示例函数
> 
> 测试函数：是以Test为函数名前缀的函数，用于测试程序的一些逻辑行为是否正确，go test命令会调用这些测试函数并报告测试结果是PASS或FAIL。
> 
> 基准测试函数：是以Benchmark为函数名前缀的函数，它们用于衡量一些函数的性能；go test命令会多次运行基准测试函数以计算一个平均的执行时间。
> 
> 示例函数：是以Example为函数名前缀的函数，提供一个由编译器保证正确性的示例文档。

### 测试函数







## go中常见的命令

### 核心开发命令

1. **`go run`: 编译并运行Go程序**
   
   - 示例：`go run xx.go`
   - 作用：开发调试时直接运行代码，不会生成可执行文件

2. **`go build`: 编译包和依赖**
   
   - 编译项目，生成二进制可执行文件（生产部署用）

3. **`go fmt`: 格式化代码**
   
   - 自动调整代码格式（缩进、括号等），团队协作保持风格一致

4. **`go mod tidy`: 整理依赖**
   
   - 添加缺失的模块，删除未使用的模块，是解决依赖问题的首选

5. **`go install`: 编译并安装可执行文件**
   
   - 编译并将结果放到 `$GOPATH/bin` 或 `$GOBIN` 目录，全局可用。与`go get`不同，`go install`是用来安装工具的，而`go get`是用来安装依赖库的。

### 依赖管理命令（模块相关）

1. **`go mod init <module名>`: 初始化新模块**
   
   - 创建新项目时使用，生成 go.mod 文件

2. **`go get <包路径>`: 下载依赖包**
   
   - 添加或更新特定依赖

3. **`go mod vendor`: 制作依赖副本**
   
   - 将依赖复制到 vendor 目录，用于离线构建或特殊部署

4. **`go list -m all`: 查看所有依赖**
   
   - 列出当前模块的所有依赖及其版本

### 测试与质量保证

1. **`go test`: 运行测试**
   - 执行当前包的测试函数（文件名需以`_test.go`结尾）
   - `-v`参数：显示log打印
   - `-run`参数：可以运行特定的测试函数

```go

```

## 接口

> go 中接口(interface)是一种抽象类型。接口(interface)是一组函数`method`的集合，接口中不能包含变量。

> 定义格式如下

```go
type 接口名 interface {
    方法名1(参数列表1) 返回值列表
    方法名2(参数列表2) 返回值列表
}
```

> 接口名一般以 `er` 结尾

### 定义接口并实现接口

> 接口也是一种类型，实现接口的结构体和自定义类型，可以有接口类型的变量接收(多态)

```go
type Usber interface {
    start(x, y int) int
    stop(x int) int
}

// 如果接口里面有方法的话，必须要通过结构体或自定义类型实现这个接口

type Phone struct {
    Name string
}

// Phone实现Usber接口
func (p Phone) start(x, y int) int {
    return x + y
}
func (p Phone) stop(x int) int {
    return x
}
```

> <mark>官方对接口实现的定义：方法集是接口方法集的超集。</mark>
> 
> 所以除了上面寻常的实现方式，下面也是实现接口的方式，可以使用多态接收

```go
type Person interface {
    Walk()
}
// Man实现了Person接口，因为此时Man的方法集是Person的超集
type Man struct {
        Person
        age int
}

man := Man{age: 20}
tesss(man)    // √ Man结构体可以被Person接收

func tesss(p Person) {gou
  // p.Walk()    ❌，未定义方法
    fmt.Println("dd")
}
```

#### 多态

> 实现接口的结构体和自定义类型，可以有接口类型的变量接收(多态)

```go
// Phone实现了Usber，具体实现看上面一个代码块
p := Phone{
    Name: "iphone",
}
// p1是Usber类型，p是Phone类型，Phone实现了Usber
var p1 Usber
p1 = p    // 多态
```

> <mark>在结构体实现接口时，指针接收者和非指针接收者是不一样的</mark>

```go
// 接口 Usber
type Usber interface {
    start(x, y int) int
    stop(x int) int
}
// 结构体 Phone
type Phone struct {
    Name string
}
// Phone实现Usber接口，非指针接收者
func (p Phone) start(x, y int) int {
    return x + y
}
func (p Phone) stop(x int) int {
    return x
}
// 这时候 结构体指针 *Phone 和 结构体 Phone 都是Usber类型的
p := Phone{
    Name: "iphone",
}
np := &Phone{
    Name: "np",
}
var p1, p2 Usber
p1 = p
p2 = np

// 当是指针接收者时（只要有一个是指针接收者也行），Phone类型不能用Usber类型接收，*Phone可以
func (p *Phone) start(x, y int) int {
    return x + y
}
func (p *Phone) stop(x int) int {
    return x
}
p := Phone{
    Name: "iphone",
}
np := &Phone{
    Name: "np",
}
var p1, p2 Usber
// p1 = p   // 这个不行
p2 = np
```

### 空接口 --- any

> 空接口不定义任何方法，空接口表示没有任何约束，因此任何类型都可以用空接口接收。也就是空接口表示 `any` 类型（任意类型）

> **使用 `any` 类型接收的变量，想要使用操作时需要使用断言恢复原来的类型(与 ts 很像)**

### 类型断言

> 一个接口的值(简称接口值)是由一个具体类型和具体类型的值两部分组成的。这两部分分别称为接口的动态类型和动态值。

> 如果想要判断空接口中值的类型，可以用类型断言

> ==使用 `any` 类型接收的变量，想要使用操作时需要使用断言恢复原来的类型(与 ts 很像)==

`x.(T)`

- x:表示类型为 any（interface{}）的变量
- T:表示断言 x 可能的类型
- 该语法返回两个参数：第一个参数是 x 转化为 T 类型的变量，第二个值是一个布尔值，若成功是 true 反之为 false

```go
p := Phone{
    Name: "iphone",
}
var a any
a = p
v,ok := a.(Phone)
if ok {
    fmt.Printf("类型是%T\n", v) // 类型是main.Phone
}
```

> 也可以用`x.(type)`返回 x 的实际类型，这个只能用在 switch 中

```go
switch a_type := a.(type) {
    case int:
        fmt.Println("int")
        // 这里a_type是int类型
        return a_type
        break
    case Phone:
        fmt.Println("Phone")
        // 这里是Phone类型
        return a_type
    default:
        fmt.Println("default")
}
```

### 结构体实现多个接口

> 实现多个接口的结构体可以用实现的接口类型接收

```go
type A1 interface {
    SetName(name string)
}
type A2 interface {
    GetName() string
}

type Dog struct {
    Name string
}
// Dog 同时实现A1和A2
func (d *Dog) SetName(name string) {
    d.Name = name
}
func (d Dog) GetName() string {
    return d.Name
}

d := &Dog{
    Name: "dog",
}
var d1 A1
var d2 A2
d1 = d
d2 = d
```

### 接口嵌套

> go 中允许在接口中嵌套其它接口来形成新的接口

```go
// 接口B1
type B1 interface {
    Move()
}
// 接口B2
type B2 interface {
    Say()
}
// 接口B，使用B1和B2嵌套形成
type B interface {
    B1
    B2
}
// 结构体Bb
type Bb struct {
    Name string
}
// 结构体同时实现了 B1、B2、B
func (b Bb) Say() {
    fmt.Println("say")
}
func (b Bb) Move() {
    fmt.Println("Move")
}
```

## 并发

### 协程 goroutine

> go 中的主线程：（可以理解为线程/也可以理解为进程），在一个 go 程序的主线程上可以起多个协程。go 中多协程可以实现并行或者并发

> 协程：可以理解为用户级线程，对内核透明（系统不知道有协程存在），完全由用户自己的程序进行调度。协程所需资源要比线程少。

#### 协程使用

> 协程的使用比较简单，在函数调用前使用关键字 `go` 修饰就行

```go
func test() {
    for i := 0; i < 10; i++ {
        fmt.Println("test",i)
        time.Sleep(time.Millisecond * 50)
    }
}

func main() {   // 主线程
    go test()    // 开启协程
    for i := 0; i < 10; i++ {
        fmt.Println("main",i)
        // time.Sleep(time.Millisecond * 50)
    }
}
```

#### sync.WaitGroup 使用

> 上面的代码有一个 bug，主线程`main()`执行的会比协程`test()`快，主==线程不会等待协程执行完毕==，所以协程`test()`不会执行完。

> `sync.WaitGroup`方法可以线程等待协程执行完毕。原理就是对全局变量进行计数。

```go
// 定义全局变量
var wg sync.WaitGroup

func test() {
    for i := 0; i < 10; i++ {
        fmt.Println("test",i)
        time.Sleep(time.Millisecond * 50)
    }
    wg.Done()    // 协程计数器减1
}

func main() {
    wg.Add(1)    // 协程计数器加1，加多少取决于传递的参数
    go test()    // 开启协程
    for i := 0; i < 10; i++ {
        fmt.Println("main",i)
        // time.Sleep(time.Millisecond * 50)
    }
    wg.Wait()    // 等待协程计数器为0
    fmt.Println("主线程退出...")
}
```

#### 解决协程运行中出现的错误

> 解决方法与普通函数中的方式一样

#### 设置协程使用 cpu 个数

```go
func main() {
    cpuNum := runtime.NumCPU()  // 获取当前cpu个数
    runtime.GOMAXPROCS(cpuNum - 1)  // 设置使用多少cpu
}
```

### channel 管道

> 管道`channel`是 go 在语言级别上提供的协程`goroutine`间的通讯方式，可以使用`channel`在多个协程之间传递消息。

> go 的并发模型是 CSP(Communicating Sequential Processes)，提倡通过通信共享内存而不是通过共享内存而实现通信。

> go 中的管道`channel`是一种特殊的类型。管道像一个队列，遵循先进先出。

#### 管道的使用

> `channel`是一种类型，一种`引用类型`。声明管道类型的格式如下：

`var 变量名 chan 类型`

```go
var ch1 chan int
var ch2 chan string
// 管道是引用类型，必须要先分配make空间才能使用
// make的第一个变量是管道类型，第二个是管道容量大小
ch1 = make(chan int, 3)

// 管道简化定义
ch3 := make(chan int, 5)

// 给管道存数据
ch1 <- 10
ch1 <- 23
ch1 <- 34
// 从管道取值，遵循先进先出
a := <- ch1    // a = 10
b := <- ch1    // b = 23

// 获取管道的长度和容量
cap(ch1)    // 获取管道容量
len(ch1)    // 获取管道长度
```

#### 管道引起的阻塞

> 当向没有空间(没有数据)的管道写入(取出)数据会引发阻塞，直到管道有空间(数据)

```go
// 当向没有空间的管道写入数据
var ch1 chan int
ch1 = make(chan int, 1)
ch1 <- 10
ch1 <- 20    // 会引发阻塞，如果没有其它协程取出数据腾出空间，这会导致死锁
fmt.Println(len(ch1), cap(ch1), <-ch1)

// 当向没有数据的管道取出数据
var ch1 chan int
ch1 = make(chan int, 1)
ch1 <- 10
fmt.Println(len(ch1), cap(ch1), <-ch1, <-ch1)    // 会引发阻塞
```

#### 管道的遍历、解除管道阻塞

```go
var ch1 chan int
ch1 = make(chan int, 5)
for i := 0; i < cap(ch1); i++ {
    ch1 <- (i+1)
}
// 关闭管道，表示不再向管道写入数据，将会解除 所有因等待该管道写入数据引起的阻塞
close(ch1)

// 当通道被关闭时就会退出for range，如果没有关闭管道就会报一个错误fatal error
// 因为 for range读取了管道内所有数据，等待管道内的新数据引起阻塞
for v := range ch1 {
    fmt.Println(v)
}
```

#### 单向管道

> 在默认情况下，管道是双向的，即一个操作者能写入管道也能读取管道。有时我们希望限制管道，即对于操作者只能写入(或者只能读取)管道，对操作者来说这个管道是单向的。

> 双向管道能赋值给单向管道

```go
// 定义可读可写的管道
var ch1 chan int = make(chan int, 1)
ch1 <- 12
// 定义只写管道
var ch2 chan<- int = make(chan<- int, 1)
ch2 <- 1
// 下面是错的，不能读
<-ch2

// 定义只写管道
var ch3 <-chan int = make(<-chan int, 1)
// 下面是错的，不能写
ch3<-1

// 双向管道赋值给单向管道
var ch4 <-chan int = ch1
```

#### select 多路复用

> 在某些场景下我们需要同时从多个通道接收数据。这时候就可以用的`select`多路复用。

> 通常情况下通道在接收数据时，如果没有数据可以接收将会发生阻塞

> `select` 使用类似于 `switch` 语句，它有一系列 `case` 分支和一个`default`分支。每个`case`分支只能操作一个管道，且只能进行一种操作，要么读要么写，当有多个 `case` 可用时，`select` 会伪随机的选择一个 `case` 来执行。如果所有 `case` 都不可用，就会执行 `default` 分支，倘若没有 `default` 分支，将会阻塞等待，直到至少有一个 `case` 可用。格式如下:

```go
select{
    case <-ch1:
        逻辑
    case <-ch2:
        逻辑
    case <-ch3
        逻辑
    ...
    default:
        逻辑
}
```

> 使用 select 时不需要关闭管道

```go
func main() {
    ch := make(chan int)
    // 关闭通道
    defer close(ch)
    go func() {
        time.Sleep(3 * time.Second)
        ch <- 1
    }()
    for {
        select {
        case <-ch:
            fmt.Println("ch")
            return
        default:
            fmt.Println("default")
        }
        time.Sleep(100 * time.Millisecond)
    }
}
```

### Context 上下文

> `Context` 译为上下文，是 Go 提供的一种并发控制的解决方案，相比于管道和 `WaitGroup`，它可以更好的控制子孙协程以及层级更深的协程。
> 
> `Context`与`Context`之间组成了一种树的关系，上级`Context`可以取消下级`Context`或往下发送消息。
> 
> `Context` 本身是一个接口，只要实现了该接口都可以称之为上下文例如著名 Web 框架 `Gin` 中的 `gin.Context`。
> 
> `context` 标准库也提供了几个实现，分别是：
> 
> * `emptyCtx`
> * `cancelCtx`
> * `timerCtx`
> * `valueCtx`
> 
> `context`包的核心API：
> 
> - `context.WithCancel`
> 
> - `context.WithTimeout`
> 
> - `context.WithDeadline`
> 
> - `context.WithValue`

#### emptyCtx

> 顾名思义，`emptyCtx` 就是空的上下文，充当根上下文来创建其它上下文，`emptyCtx` 就可以通过 `context.Background` 和 `context.TODO` 来进行创建。两个函数如下：

```go
type emptyCtx struct{}

type backgroundCtx struct{ emptyCtx }

type todoCtx struct{ emptyCtx }

func Background() Context {
    return backgroundCtx{}
}

func TODO() Context {
    return todoCtx{}
}
```

>  `emptyCtx` 没法被取消，没有 `deadline`，也不能取值，实现的方法都是返回零值：

```go
type emptyCtx struct{}

func (emptyCtx) Deadline() (deadline time.Time, ok bool) {
    return
}

func (emptyCtx) Done() <-chan struct{} {
    return nil
}

func (emptyCtx) Err() error {
    return nil
}

func (emptyCtx) Value(key any) any {
    return nil
}

```

> `emptyCtx` 通常是用来当作最顶层的上下文，在创建其他三种上下文时作为父上下文传入。context 包中的各个实现关系如下图所示:

<img title="" src="./pic/go/context_1.png" alt="">

#### valueCtx

> `valueCtx` 多用于在多级协程中传递一些数据，无法被取消，因此 `ctx.Done` 永远会返回 `nil`
> 
> `valueCtx` 实现比较简单，其内部只包含一对键值对，和一个内嵌的 `Context` 类型的字段。

```go
type valueCtx struct {
   Context
   key, val any
}
```

> 其本身只实现了 `Value` 方法，逻辑也很简单，当前上下文找不到就去父上下文找。

```go
func (c *valueCtx) Value(key any) any {
   if c.key == key {
      return c.val
   }
   return value(c.Context, key)
}
```

**示例**

```go
var waitGroup sync.WaitGroup

func main() {
  waitGroup.Add(1)
    // 传入上下文
  go Do(context.WithValue(context.Background(), 1, 2))
  waitGroup.Wait()
}

func Do(ctx context.Context) {
    // 新建定时器
  ticker := time.NewTimer(time.Second)
  defer waitGroup.Done()
  for {
    select {
    case <-ctx.Done(): // 永远也不会执行
    case <-ticker.C:
      fmt.Println("timeout")
      return
    default:
      fmt.Println(ctx.Value(1))
    }
    time.Sleep(time.Millisecond * 100)
  }
}
```

**输出**

```textile
2
2
2
2
2
2
2
2
2
2
timeout
```

#### cancelCtx

> `cancelCtx` 以及 `timerCtx` 都实现了 `canceler` 接口，接口类型如下

```go
type canceler interface {
    // removeFromParent 表示是否从父上下文中删除自身
    // err 表示取消的原因
  cancel(removeFromParent bool, err error)
    // Done 返回一个管道，用于通知取消
  Done() <-chan struct{}
}
```

> `cancel` 方法不对外暴露，在创建上下文时通过闭包将其包装为返回值以供外界调用，就如 `context.WithCancel` 源代码中所示

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {
   if parent == nil {
      panic("cannot create context from nil parent")
   }
   c := newCancelCtx(parent)
   // 尝试将自身添加进父级的children中
   propagateCancel(parent, &c)
   // 返回context和一个函数
   return &c, func() { c.cancel(true, Canceled) }
}
```

> `cancelCtx` 译为可取消的上下文，创建时，如果父级实现了 `canceler`，就会将自身添加进父级的 `children` 中，否则就一直向上查找。如果所有的父级都没有实现 `canceler`，就会启动一个协程等待父级取消，然后当父级结束时取消当前上下文。当调用 `cancelFunc` 时，`Done` 通道将会关闭，该上下文的任何子级也会随之取消，最后会将自身从父级中删除。下面是一个简单的示例：

```go
var waitGroup sync.WaitGroup

func main() {
  bkg := context.Background()
    // 返回了一个cancelCtx和cancel函数
  cancelCtx, cancel := context.WithCancel(bkg)
  waitGroup.Add(1)
  go func(ctx context.Context) {
    defer waitGroup.Done()
    for {
      select {
      case <-ctx.Done():
        fmt.Println(ctx.Err())
        return
      default:
        fmt.Println("等待取消中...")
      }
      time.Sleep(time.Millisecond * 200)
    }

  }(cancelCtx)
  time.Sleep(time.Second)
  cancel()
  waitGroup.Wait()
}
```

**输出**

```go
等待取消中...
等待取消中...
等待取消中...
等待取消中...
等待取消中...
context canceled
```

#### timerCtx

> `timerCtx` 在 `cancelCtx` 的基础之上增加了超时机制，`context` 包下提供了两种创建的函数，分别是 `WithDeadline` 和 `WithTimeout`，两者功能类似，前者是指定一个具体的超时时间，比如指定一个具体时间 `2023/3/20 16:32:00`，后者是指定一个超时的时间间隔，比如 5 分钟后。两个函数的签名如下

```go
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc)

func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
```

> `timerCtx` 会在时间到期后自动取消当前上下文，取消的流程除了要额外的关闭 `timer` 之外，基本与 `cancelCtx` 一致。下面是一个简单的 `timerCtx` 的使用示例

```go
var wait sync.WaitGroup

func main() {
  deadline, cancel := context.WithDeadline(context.Background(), time.Now().Add(time.Second))
  defer cancel()
  wait.Add(1)
  go func(ctx context.Context) {
    defer wait.Done()
    for {
      select {
      case <-ctx.Done():
        fmt.Println("上下文取消", ctx.Err())
        return
      default:
        fmt.Println("等待取消中...")
      }
      time.Sleep(time.Millisecond * 200)
    }
  }(deadline)
  wait.Wait()
}
```

> 尽管上下文到期会自动取消，但是为了保险起见，在相关流程结束后，最好手动取消上下文。

**输出如下**

```textile
等待取消中...
等待取消中...
等待取消中...
等待取消中...
等待取消中...
上下文取消 context deadline exceeded
```

### 并发安全与锁

> 锁是为了防止多个协程竞争资源

#### 互斥锁

> 只有实施加锁操作的协程允许访问资源

```go
var mutex sync.Mutex    // 定义互斥锁对象
var wg sync.WaitGroup
var count = 0

func test()  {
    mutex.Lock()    // 加互斥锁，不允许其它协程访问下面的资源
    count++
    println("打印count", count)
    mutex.Unlock()    // 解互斥锁，允许其它资源访问
    time.Sleep(time.Millisecond * 100)
    wg.Done()
}
```

#### 读写锁

> 读写锁：允许多个读操作并发同时读取，但对于写操作是完全互斥的。
> 
> 对于一个协程而言：
> 
> * 如果获得了读锁，其他协程进行写操作时会阻塞，其他协程进行读操作时不会阻塞
> * 如果获得了写锁，其他协程进行写操作时会阻塞，其他协程进行读操作时会阻塞
> 
> Go 中读写互斥锁的实现是 `sync.RWMutex`，它也同样实现了 `Locker` 接口，但它提供了更多可用的方法，如下：

```go
var mutex sync.RWMutex    // 定义读写锁对象

// 加读锁
func (rw *RWMutex) RLock()

// 尝试加读锁
func (rw *RWMutex) TryRLock() bool

// 解读锁
func (rw *RWMutex) RUnlock()

// 加写锁
func (rw *RWMutex) Lock()

// 尝试加写锁
func (rw *RWMutex) TryLock() bool

// 解写锁
func (rw *RWMutex) Unlock()
```

#### 条件变量

> 条件变量，与互斥锁一同出现和使用，所以有些人可能会误称为条件锁，但<mark>它并不是锁</mark>，是一种通讯机制。Go 中的 `sync.Cond` 对此提供了实现，而创建条件变量的函数签名如下：

```go
func NewCond(l Locker) *Cond
```

> 可以看到创建一个条件变量前提就是需要创建一个锁，`sync.Cond` 提供了如下的方法以供使用

```go
// 阻塞等待条件生效，直到被唤醒
func (c *Cond) Wait()

// 唤醒一个因条件阻塞的协程
func (c *Cond) Signal()

// 唤醒所有因条件阻塞的协程
func (c *Cond) Broadcast()
```

**示例**

```go
var wait sync.WaitGroup
var count = 0

var rw sync.RWMutex

// 条件变量
var cond = sync.NewCond(rw.RLocker())

func main() {
  wait.Add(12)
  // 读多写少
  go func() {
    for i := 0; i < 3; i++ {
      go Write(&count)
    }
    wait.Done()
  }()
  go func() {
    for i := 0; i < 7; i++ {
      go Read(&count)
    }
    wait.Done()
  }()
  // 等待子协程结束
  wait.Wait()
  fmt.Println("最终结果", count)
}

func Read(i *int) {
  time.Sleep(time.Millisecond * time.Duration(rand.Intn(500)))
  rw.RLock()
  fmt.Println("拿到读锁")

  // 条件不满足就一直阻塞
  for *i < 3 {
    // 阻塞并释放锁
    cond.Wait()
    // 被唤醒时，自动获取锁
  }
  time.Sleep(time.Millisecond * time.Duration(rand.Intn(1000)))
  fmt.Println("释放读锁", *i)
  rw.RUnlock()
  wait.Done()
}

func Write(i *int) {
  time.Sleep(time.Millisecond * time.Duration(rand.Intn(1000)))
  rw.Lock()
  fmt.Println("拿到写锁")
  temp := *i
  time.Sleep(time.Millisecond * time.Duration(rand.Intn(1000)))
  *i = temp + 1
  fmt.Println("释放写锁", *i)
  rw.Unlock()
  // 唤醒所有因条件变量阻塞的协程
  cond.Broadcast()
  wait.Done()
}
```

**输出**

```textile
拿到读锁
拿到读锁
拿到读锁
拿到读锁
拿到写锁
释放写锁 1
拿到读锁
拿到写锁
释放写锁 2
拿到读锁
拿到读锁
拿到写锁
释放写锁 3 // 第三个写协程执行完毕
释放读锁 3
释放读锁 3
释放读锁 3
释放读锁 3
释放读锁 3
释放读锁 3
释放读锁 3
最终结果 3
```

### sync标准库

> Go 中很大一部分的并发相关的工具都是 `sync` 标准库提供的，上述已经介绍过了 `sync.WaitGroup`，`sync.Locker` 等，除此之外，`sync` 包下还有一些其他的工具可以使用。

#### Once

> 当在使用一些数据结构时，如果这些数据结构太过庞大，可以考虑采用懒加载的方式，即真正要用到它的时候才会初始化该数据结构。如下面的例子

```go
type MySlice []int

func (m *MySlice) Get(i int) (int, bool) {
   if *m == nil {
      return 0, false
   } else {
      return (*m)[i], true
   }
}

func (m *MySlice) Add(i int) {
   // 当真正用到切片的时候，才会考虑去初始化
   if *m == nil {
      *m = make([]int, 0, 10)
   }
   *m = append(*m, i)
}
```

> 果有多个协程访问的话就可能会出现问题了。可能会出现重复初始化导致数据覆盖问题。
> 
> 这就是 `sync.Once` 要解决的问题，顾名思义，`Once` 译为一次，`sync.Once` 保证了在并发条件下指定操作只会执行一次。它的使用非常简单，只对外暴露了一个 `Do` 方法，签名如下：

```go
func (o *Once) Do(f func())
```

> 在使用时，只需要将初始化操作传入 `Do` 方法即可，如下：

```go
var wait sync.WaitGroup

func main() {
  var slice MySlice
  wait.Add(4)
  for i := 0; i < 4; i++ {
    go func() {
      slice.Add(1)
      wait.Done()
    }()
  }
  wait.Wait()
  fmt.Println(slice.Len())
}

type MySlice struct {
  s []int
  o sync.Once
}

func (m *MySlice) Get(i int) (int, bool) {
  if m.s == nil {
    return 0, false
  } else {
    return m.s[i], true
  }
}

func (m *MySlice) Add(i int) {
  // 当真正用到切片的时候，才会考虑去初始化
  m.o.Do(func() {
    fmt.Println("初始化")
    if m.s == nil {
      m.s = make([]int, 0, 10)
    }
  })
  m.s = append(m.s, i)
}

func (m *MySlice) Len() int {
  return len(m.s)
}
```

#### Map

> `sync.Map` 是官方提供的一种并发安全 Map 的实现，开箱即用，使用起来十分的简单，下面是该结构体对外暴露的方法：

```go
// 根据一个key读取值，返回值会返回对应的值和该值是否存在
func (m *Map) Load(key any) (value any, ok bool)

// 存储一个键值对
func (m *Map) Store(key, value any)

// 删除一个键值对
func (m *Map) Delete(key any)

// 如果该key已存在，就返回原有的值，否则将新的值存入并返回，当成功读取到值时，loaded为true，否则为false
func (m *Map) LoadOrStore(key, value any) (actual any, loaded bool)

// 删除一个键值对，并返回其原有的值，loaded的值取决于key是否存在
func (m *Map) LoadAndDelete(key any) (value any, loaded bool)

// 遍历Map，当f()返回false时，就会停止遍历
func (m *Map) Range(f func(key, value any) bool)
```

**并发使用Map并出错的示例**

```go
func main() {
  myMap := make(map[int]int, 10)
  var wait sync.WaitGroup
  wait.Add(10)
  for i := 0; i < 10; i++ {
    go func(n int) {
      for i := 0; i < 100; i++ {
        myMap[n] = n
      }
      wait.Done()
    }(i)
  }
  wait.Wait()
}
```

> 上例中使用的普通 map，开了 10 个协程不断的存入数据，显然这很可能会触发 fatal，结果大概率会如下
> 
> `fatal error: concurrent map writes`

**解决**

> 使用 `sync.Map` 就可以避免这个问题

```go
func main() {
  var syncMap sync.Map
  var wait sync.WaitGroup
  wait.Add(10)
  for i := 0; i < 10; i++ {
    go func(n int) {
      for i := 0; i < 100; i++ {
        syncMap.Store(n, n)
      }
      wait.Done()
    }(i)
  }
  wait.Wait()
  syncMap.Range(func(key, value any) bool {
    fmt.Println(key, value)
    return true
  })
}
```

## 反射

> 反射是指在程序运行期间对程序本身进行访问和修改的能力。`正常情况`程序在编译时，变量被转换为内存地址，变量名不会被编译器写入可执行部分。在运行程序时，程序无法获取自身的信息。`支持反射的语言`可以在程序编译期将变量的反射信息，如字段名称、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期获取类型的反射信息，并且有能力修改它们。

> 可以实现的功能：
> 
> - 反射可以在程序运行期间动态的获取变量的信息，比如：变量的类型
> - 如果是结构体还可以获取结构体本身的信息，如：结构体的字段、方法
> - 通过反射，可以修改变量的值，可以调用关联的方法

> 在 go 的反射机制中，任何接口值都由是一个`具体类型`和`具体类型的值`两部分组成的。

> go 语言中反射的相关功能由内置的 `reflect` 包提供

### reflect.TypeOf() 获取任意值的类型对象

> 使用 `reflect.TypeOf()` 函数可以接受任意类型参数，返回参数的类型。

```go
// 结构体
type Dataa struct {
    Name string
}

func reflectFn(x any) {
    v := reflect.TypeOf(x)
    fmt.Println(v)    // 打印变量类型
}

func main() {
    reflectFn(45)    // int
    reflectFn(89.8)    // float64
    reflectFn("string")    // string
    d := Dataa{
        Name: "ss",
    }
    var i myInt = 90
    reflectFn(d)    // main.Dataa
    reflectFn(i)    // main.myInt
}
```

#### type Name 和 type Kind

> 在反射中关于类型还划分为两种：类型(Type)和种类(Kind)。因为在 go 中可以用`type`关键字构造很多自定义类型，而种类(Kind)就是底层的类型

```go
func reflectFn(x any) {
    v := reflect.TypeOf(x)
    fmt.Println(v, v.Name(), v.Kind())
}

type Dataa struct {
    Name string
}

type myInt int

func main() {
    reflectFn(45)    // int int int
    reflectFn(89.8)    // float64 float64 float64
    reflectFn("string")    // string string string
    d := Dataa{
        Name: "ss",
    }
    var i myInt = 90
    reflectFn(d)    // main.Dataa Dataa struct
    reflectFn(i)    // main.myInt myInt int
}
```

### reflect.ValueOf()

> `reflect.ValueOf()` 返回的是 `reflect.Value` 类型，其中包含了原始值的值类型。`reflect.Value` 与原始值之间可以相互转换。

> `reflect.Value` 类型提供的获取原始值的方法如下：

| 方法                      | 说明                                  |
| ----------------------- | ----------------------------------- |
| Interface() interface{} | 将值以`interface{}`类型返回，可以通过类型断言转为指定类型 |
| Int() int64             | 将值以`int`类型返回，所有有符号整型均可以以此方式返回       |
| Uint() int64            | 将值以`uint`类型返回，所有无符号整型均可以以此方式返回      |

`依次类推`

```go
v := reflect.ValueOf(x)
i := v.Int()    // 返回转换后的值
v.Kind()    // 返回值的类型
fmt.Println(v.Type().Name(), v)
```

#### 使用反射修改变量的值

> 要想用反射修改变量的值，要传入指针或者引用类型。传入值类型修改的只是副本，不会影响原来的值。

> 当传入指针时，要使用 `Elem`

```go
func reflectValue(x any)  {
    v := reflect.ValueOf(x)
    v.Elem().SetInt(30)    // 这里必须要用Elem
    fmt.Println(v.Elem().Kind())    // 输出int
}

var a int = 20
pi := &a
reflectValue(pi)
println(*pi)    // 30
```

### 结构体反射

> 任意值通过 `reflect.TypeOf()` 获得反射对象信息后，如果它的类型是结构体，可以通过反射类型对象(类型为`reflect.Type`)的 `NumField()` 和 `Field()` 方法获得结构体成员的详细信息。

> 如果要获取结构体对应字段的值，可以通过 `reflect.ValueOf()` 获取反射值对象(类型为`reflect.Value`)的相关属性获取。

```go
type Student struct {
    Name string
    Age int
}

func reflectStruct(x any) {
    c := reflect.TypeOf(x)
    c.NumField()    // 获取结构体字段个数
    c.Field(0)    // 获取结构体第1个字段详细信息
    c.Field(0).Name    // 获取结构体第1个字段名
    c.Field(0).Type    // 获取结构体第1个字段类型
    a,_  := c.FieldByName("Name")    // 通过字段名获取字段详细信息
}

s := Student{
    "ding",
    28,
}
reflectStruct(s)
```

#### 通过反射使用结构体的方法

```go
type Student struct {
    Name string
    Age int
}

func (s Student) Run() {
    fmt.Println("run....")
}

func reflectStruct(x any) {
    v := reflect.ValueOf(x)
    // 通过method方法获取想要调用的方法，再通过call方法调用，call中传递要调用方法的参数，没有参数传入nil
    v.Method(0).Call(nil)    // 调用 Run() 方法
}

// 定义结构体变量
s := Student{
    "ding",
    28,
}
reflectStruct(s)
```

## 文件操作

> Go 语言提供文件处理的标准库大致以下几个：
> 
> * `os`库，负责 OS 文件系统交互的具体实现。作用：打开文件，获取文件句柄
> * `io`库，读写 IO 的抽象层。作用：通过文件句柄，进行读写操作
> * `fs`库，文件系统的抽象层

### 打开文件

> 常见的两种打开文件的方式是使用`os`包提供的两个函数：
> 
> - `Open`函数
> 
> - `OpenFile`函数
> 
> <mark>注意：</mark>打开文件后，要记得关闭文件，使用`file.Close()`

```go
func Open(name string) (*File, error)
// 参数1：路径
// 参数2：模式
// 参数3：权限
func OpenFile(name string, flag int, perm FileMode) (*File, error) 
```

**使用Open打开文件**

> 使用 `os.Open()` 方法读取文件

> 这个方法有一个坑，就是最后一次读取如果不能把切片填满，那么会残留上一次读取的数据。每次获取切片中的数据，应该获取切片中前`n`(`n`是读取到的数据个数)个数据

```go
func main() {
    defer func ()  {    // 异常处理
        err := recover()
        if err != nil {
            fmt.Println(err)
        }
    }()
    // 可以传入相对路径
    txt, err := os.Open("C:\\Users\\dwl\\Desktop\\desktop\\go\\newgo\\test.txt")
    defer func ()  {    // 关闭文件流，最后关闭
        err := txt.Close()
        if err != nil {
            fmt.Println("文件关闭失败!")
            return    // 这个defer会最先执行，读取失败则退出函数
        }
    }()
    if err != nil {
        panic("读取文件出错了!")
    }
    // 创建一个容量为3的字节切片
    var tempSlice = make([]byte, 3)
    // Read() 方法需要传入一个切片来存储读取内容，
    // 返回读取的字节数和错误，读取到文件末尾时返回0和io.EOF
    n, err := txt.Read(tempSlice)
    if err != nil {
        fmt.Println("读取失败")
        return
    }
    // 切片取前n个，防止读到脏数据
    fmt.Printf("读取了%d个字符,内容为：%s", n, string(tempSlice[:n]))
}
```

> <mark>注意：</mark>`os.Open()` 打开的文件是只读的，实际上它是`OpenFile`的封装

```go
func Open(name string) (*File, error) {
	return OpenFile(name, O_RDONLY, 0)
}
```

**使用OpenFile打开文件**

> `OpenFile`函数可以控制更多细节，例如修改文件描述符和文件权限。
> 
> 对于文件描述符（模式），`os`包下提供了以下常量以供使用。

```go
const (
   // 只读，只写，读写 三种必须指定一个
   O_RDONLY int = syscall.O_RDONLY // 以只读的模式打开文件
   O_WRONLY int = syscall.O_WRONLY // 以只写的模式打开文件
   O_RDWR   int = syscall.O_RDWR   // 以读写的模式打开文件
   // 剩余的值用于控制行为
   O_APPEND int = syscall.O_APPEND // 当写入文件时，将数据添加到文件末尾
   O_CREATE int = syscall.O_CREAT  // 如果文件不存在则创建文件
   O_EXCL   int = syscall.O_EXCL   // 与O_CREATE一起使用, 文件必须不存在
   O_SYNC   int = syscall.O_SYNC   // 以同步IO的方式打开文件
   O_TRUNC  int = syscall.O_TRUNC  // 当打开的时候截断可写的文件
)
```

> 关于文件权限的则提供了以下常量。

```go
const (
   ModeDir        = fs.ModeDir        // d: 目录
   ModeAppend     = fs.ModeAppend     // a: 只能添加
   ModeExclusive  = fs.ModeExclusive  // l: 专用
   ModeTemporary  = fs.ModeTemporary  // T: 临时文件
   ModeSymlink    = fs.ModeSymlink    // L: 符号链接
   ModeDevice     = fs.ModeDevice     // D: 设备文件
   ModeNamedPipe  = fs.ModeNamedPipe  // p: 具名管道 (FIFO)
   ModeSocket     = fs.ModeSocket     // S: Unix 域套接字
   ModeSetuid     = fs.ModeSetuid     // u: setuid
   ModeSetgid     = fs.ModeSetgid     // g: setgid
   ModeCharDevice = fs.ModeCharDevice // c: Unix 字符设备, 前提是设置了 ModeDevice
   ModeSticky     = fs.ModeSticky     // t: 黏滞位
   ModeIrregular  = fs.ModeIrregular  // ?: 非常规文件

   // 类型位的掩码. 对于常规文件而言，什么都不会设置.
   ModeType = fs.ModeType

   ModePerm = fs.ModePerm // Unix 权限位, 0o777
)
```

> 下面是一个例子：以读写模式打开一个文件，权限为`0666`，表示为所有人都可以对该文件进行读写，且不存在时会自动创建。

```go
func main() {
  file, err := os.OpenFile("README.txt", os.O_RDWR|os.O_CREATE, 0666)
  if os.IsNotExist(err) {
    fmt.Println("文件不存在")
  } else if err != nil {
    fmt.Println("文件访问异常")
  } else {
    fmt.Println("文件打开成功", file.Name())
    file.Close()
  }
}
```

### 读取文件

> 打开文件后，就可以对文件进行读写了，这里介绍文件读取。
> 
> `*os.File`类型提供了以下几个公开的方法来流式读取文件。

```go
// 将文件读进传入的字节切片
func (f *File) Read(b []byte) (n int, err error)

// 相较于第一种可以从指定偏移量读取
func (f *File) ReadAt(b []byte, off int64) (n int, err error)
```

**`os.ReadFile()` 读取文件**

> 还可以使用 `os.ReadFile()` 读取整个文件，这个方法不需要`File`类型对象，这个方法在 `1.16`之前在`ioutil`这个包下，现在整合到 `os` 包下了

```go
func ioU()  {
    // 一次读取整个文件，如果文件不大的话可以用这个方法，太大的话，可以用上面的流式读取。
    byteStr, err := os.ReadFile("../test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(byteStr))
}
```

**`io.ReadAll`函数读取文件**

> `io`包下的`ReadAll`函数也可以读取整个文件，不过对于`io.ReadAll`而言，则需要提供一个`io.Reader`类型的实现，`os.File`类型实现了`io.Reader`

```go
// 函数签名
func ReadAll(r Reader) ([]byte, error)

// 使用案例
func main() {

   file, err := os.OpenFile("README.txt", os.O_RDWR|os.O_CREATE, 0666)
   if err != nil {
      fmt.Println("文件访问异常")
   } else {
      fmt.Println("文件打开成功", file.Name())
      bytes, err := io.ReadAll(file)
      if err != nil {
         fmt.Println(err)
      } else {
         fmt.Println(string(bytes))
      }
      file.Close()
   }
}
```



> 使用 `bufio` 读取文件

```go
func bufR() {
    file, err := os.Open("C:\\Users\\dwl\\Desktop\\desktop\\go\\newgo\\test.txt")
    defer func ()  {    // 关闭文件流，最后关闭
        err := file.Close()
        if err != nil {
            fmt.Println("文件关闭失败!")
            return    // 这个defer会最先执行，读取失败则退出函数
        }
    }()
    if err != nil {
        fmt.Println(err)
        return
    }
    // 获取bufio读对象
    reader := bufio.NewReader(file)
    for {
        str, err := reader.ReadString('\n')    // 读取到换行符时，停止读取，表示一次读取一行。
        if err == io.EOF {
            break
        }
        if err != nil {
            fmt.Println(err)
            return
        }
        fmt.Println(str)
    }
}
```

### 写入文件

> 打开文件后获取的`os.File`结构体提供了以下几种方法以供写入数据

```go
// 写入字节切片
func (f *File) Write(b []byte) (n int, err error)

// 写入字符串
func (f *File) WriteString(s string) (n int, err error)

// 从指定位置开始写，当以os.O_APPEND模式打开时，会返回错误
func (f *File) WriteAt(b []byte, off int64) (n int, err error)
```

> <mark>注意：</mark>如果想要对一个文件写入数据，则必须以`O_WRONLY`或`O_RDWR`的模式打开，否则无法成功写入文件。
> 
> 下面是一个以`os.O_RDWR|os.O_CREATE|os.O_APPEND|os.O_TRUNC`模式打开文件，且权限为`0666`向指定写入数据的例子

```go
func main() {
  file, err := os.OpenFile("README.txt", os.O_RDWR|os.O_CREATE|os.O_APPEND|os.O_TRUNC, 0666)
  if err != nil {
    fmt.Println("文件访问异常")
  } else {
    fmt.Println("文件打开成功", file.Name())
    for i := 0; i < 5; i++ {
      offset, err := file.WriteString("hello world!\n")
      if err != nil {
        fmt.Println(offset, err)
      }
    }
    fmt.Println(file.Close())
  }
}
```

**`os.WriteFile`写入文件**

> 函数签名如下

```go
func WriteFile(name string, data []byte, perm FileMode) error
```

> 案例

```go
func main() {
  err := os.WriteFile("README.txt", []byte("hello world!\n"), 0666)
  if err != nil {
    fmt.Println(err)
  }
}
```

**`io.WriteString`写入文件**

> `io.WriteString`需要实现`io.Writer`接口的类型，文件句柄`os.File`已经实现该类型。
> 
> 函数签名

```go
func WriteString(w Writer, s string) (n int, err error)
```

> 案例

```go
func main() {
   file, err := os.OpenFile("README.txt", os.O_RDWR|os.O_CREATE|os.O_APPEND|os.O_TRUNC, 0666)
   if err != nil {
      fmt.Println("文件访问异常")
   } else {
      fmt.Println("文件打开成功", file.Name())
      for i := 0; i < 5; i++ {
         offset, err := io.WriteString(file, "hello world!\n")
         if err != nil {
            fmt.Println(offset, err)
         }
      }
      fmt.Println(file.Close())
   }
}
```

**`bufio.NewWriter()` 方法写入文件**

> 使用 `bufio.NewWriter()` 方法写入文件，<mark>这个方法会先写入缓存，注意将缓存冲入到文件中。</mark>

```go
func bufW() {
    file, err := os.OpenFile("../test2.txt", os.O_CREATE|os.O_WRONLY|os.O_TRUNC, 0666)
    defer func ()  {    // 关闭文件
        if file.Close() != nil {
            fmt.Println("文件关闭失败!")
        }
    }()
    if err != nil {
        fmt.Println(err)
        return
    }
    iwriter := bufio.NewWriter(file)
    iwriter.WriteString("golang冲冲冲!!!")
    iwriter.Flush()    // 将缓存冲入文件
}
```

**`os.Create`函数创建文件**

> 函数`os.Create`函数用于创建文件，本质上也是对`OpenFile`的封装。

```go
func Create(name string) (*File, error) {
   return OpenFile(name, O_RDWR|O_CREATE|O_TRUNC, 0666)
}
```

> <mark>注意：</mark>在创建一个文件时，如果其父目录不存在，将创建失败并会返回错误。

### 复制文件

> 对于复制文件而言，需要同时打开两个文件，实现思路是将原文件中的数据读取出来，然后写入目标文件中，代码示例如下

```go
func main() {
    // 从原文件中读取数据
  data, err := os.ReadFile("README.txt")
  if err != nil {
    fmt.Println(err)
    return
  }
    // 写入目标文件
  err = os.WriteFile("README(1).txt", data, 0666)
  if err != nil {
    fmt.Println(err)
  } else {
    fmt.Println("复制成功")
  }
}
```

> 也可以使用标准库提供的方法，下面介绍

**`*os.File.ReadFrom`**

> 打开文件时，一个只读，一个只写。

```go
// 函数签名
func (f *File) ReadFrom(r io.Reader) (n int64, err error)

// 案例
func main() {
  // 以只读的方式打开原文件
  origin, err := os.OpenFile("README.txt", os.O_RDONLY, 0666)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer origin.Close()
  // 以只写的方式打开副本文件
  target, err := os.OpenFile("README(1).txt", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0666)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer target.Close()
  // 从原文件中读取数据，然后写入副本文件
  offset, err := target.ReadFrom(origin)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println("文件复制成功", offset)
}
```

> 这种复制方式需要先将源文件的全部内容读取到内存中，再写入目标文件，文件特别大的时候不建议这么做。

**`io.Copy`**

> 一边读一边写，先将内容读到缓冲区中，再写入到目标文件中，缓冲区默认大小为 32KB。

```go
// 函数签名
func Copy(dst Writer, src Reader) (written int64, err error)

// 案例
func main() {
  // 以只读的方式打开原文件
  origin, err := os.OpenFile("README.txt", os.O_RDONLY, 0666)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer origin.Close()
  // 以只写的方式打开副本文件
  target, err := os.OpenFile("README(1).txt", os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0666)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer target.Close()
  // 复制
  written, err := io.Copy(target, origin)
  if err != nil {
    fmt.Println(err)
  } else {
    fmt.Println(written)
  }
}
```

### 目录操作

> `os.Mkdir()` 和 `os.MkdirAll()` 用于创建目录，前者只能创建一级目录，后者可以创建多级目录。

```go
func mkDir()  {
    // 创建一级目录
    err := os.Mkdir("./abc", 0666)
    if err != nil {
        fmt.Println(err)
    }
    // 创建多级目录
    err1 := os.MkdirAll("./a/b/c", 0666)
    if err1 != nil {
        fmt.Println(err)
    }
}
```

### 删除文件和目录

> `os.Remove()` 删除一个文件或目录

> `os.RemoveAll()` 删除多个文件

```go
// 删除单个文件或者空目录，当目录不为空时会返回错误
func Remove(name string) error

// 删除指定目录的所有文件和目录包括子目录与子文件
func RemoveAll(path string) error
```

### 重命名

> `os.Rename()`，重命名也可以理解为移动文件，该函数对于文件夹也是同样的效果。

```go
// 函数签名
func Rename(oldpath, newpath string) error

// 案例
func main() {
  err := os.Rename("README.txt", "readme.txt")
  if err != nil {
    fmt.Println(err)
  } else {
    fmt.Println("重命名成功")
  }
}
```

### 文件夹

> 文件夹操作与对文件的操作类似



## 泛型

### 1. 类型参数

```go
func getData[T any](v T) T {
    return v
}

func main() {
    // 指定 string 类型，这样不需要断言就能使用 string 的方法
    str := getData[string]("hello")
    println(len(str))

    num := getData[int](23)
    println(num)
}
```

### 2. 使用接口定义类型约束

```go
type Number interface {
    int | int64 | float64
}
// 限制 T 的类型为 int | int64 | float64 ，T的类型是int或者int64或者float64
func Add[T Number](a, b T) T {
    return a + b
}

Add(2, 3)
Add(3, 4.6)    // 两个形参的类型可以不同
```

### 3. 结构体使用泛型

```go
type Container[T any] struct {
    value T
}
// 泛型结构体的方法
func (c Container[T]) Get() T {
    return c.value
}

func (c *Container[T]) Set(v T) {
    c.value = v
}
// 声明变量
c := Container[int]{
    value: 23,
}
```

## Go标准库

### 字符串工具库 --- strings

> 包 `strings` 实现了简单的函数来操作 UTF-8 编码的字符串，简单来说就是操作字符串的工具包。
> 
> 下面的介绍并不包括所有工具

#### 复制字符串

> 将会分配一个新的内存给复制的副本，如果传入一个空字符串，则不会分配内存且返回空字符串。

```go
func Clone(s string) string
```

> 案例

```go
func Learn() {
	s1 := "美丽的世界"
	s2 := strings.Clone(s1)
	fmt.Println(s1, s2)
	fmt.Println(&s1, &s2)
}
// 输出
美丽的世界 美丽的世界
0x193a56e6e070 0x193a56e6e080    // 不相等
```

#### 包含字符串

**`Contains`**

> 判断一个字符串 s 是不是包含一个子串 substr

```go
func Contains(s, substr string) bool
```

> 案例

```go
func Learn() {
	s1 := "美丽的世界"
	if strings.Contains(s1, "界") {
		fmt.Println("包含")
	}
}
// 输出
包含
```

**`ContainsAny`**

> 判断字符串 chars 内任意字符的 unicode 码是否在字符串 s 内，翻译一下就是 s 是否包含 chars 内的任意字符串

```go
func ContainsAny(s, chars string) bool
```

> 案例

```go
func Learn() {
	fmt.Println(strings.ContainsAny("abcedfg", "bac"))
	fmt.Println(strings.ContainsAny("abcedfg", "gfdecba"))
}
// 输出
true
true
```

**`ContainsRune`**

> 判断字符串 s 内是否包含字符 r

```go
func ContainsRune(s string, r rune) bool
```

> 案例

```go
func TestContainsRune(t *testing.T) {
   fmt.Println(strings.ContainsRune("abcedf", 'a'))
   fmt.Println(strings.ContainsRune("abcedf", 'b'))
   fmt.Println(strings.ContainsRune("你好世界", '你'))
}
```



### Go 模板渲染语法

> 通常我们会使用`fmt.Sprintf`函数来进行字符串格式化，但它只适用于处理小字符串的情况，而且需要使用格式化动词来指定类型，无法做到参数命名，不支持复杂情况下的处理，而这就是模板引擎所需要解决的问题。
> 
> 这里要讲述的主角是 go 内置的模板引擎库`text/template`，在实际开发中一般用的是`html/template`，后者基于前者并做了很多关于`HTML`的安全处理，一般情况使用前者即可，若是涉及到`HTML`的模板处理建议使用后者会更安全。

#### 简单案例

> 案例分析：
> 
> `template.New()`：根据给定的字符串分配一个模板，函数签名`New(name string) *Template`
> 
> `Parse()`：将字符串解析成模板，函数签名`Parse(text string) (*Template, error)`
> 
> `Execute()`：将数据应用到模板，函数签名`Execute(wr io.Writer, data any) error`

```go
package main

import (
  "fmt"
  "os"
  "text/template"
)

func main() {
  tmpl := `This is the first template string, {{ .message }}`

  te, err := template.New("texTmpl").Parse(tmpl)
  if err != nil {
    fmt.Println(err)
    return
  }

  data := map[string]any{
    "message": "hello world!",
  }
  execErr := te.Execute(os.Stdout, data)
  if execErr != nil {
    fmt.Println(err)
  }
}
```

> 输出为：

```textile
This is the first template string, hello world!
```

#### 模板语法

##### 参数

> go 通过两对花括号`{{ }}`，来在模板中表示这是一个模板参数，通过`.`来表示根对象，根对象就是传入的`data`。就像是访问一个类型的成员变量一样，通过`.`符号衔接变量名就可以在模板中访问对应的值，例如

```go
{{ .data }}
```

> 对于传入的`data`，一般是结构体或者`map`，也可以是基本类型，比如数字字符串，这时`.`所代表的根对象就是其自身。
> 
> <mark>注意：如果是结构体需要把字段大写，这样才能导出到模板中使用</mark>
> 
> 在花括号内，不一定非得去访问根对象来获取值，也可以是基本类型的字面量，例如：

```go
{{ 1 }}
{{ 3.14 }}
{{ "jack" }}
```

> 不管根对象是什么类型，`{{ . }}`效果类似`fmt.Sprintf("%s", val)`，下面的例子：

```go
func Templ2() {
    tmpl := "data -> {{ . }}\n"
    data := []any{
        1,
        "sdd",
        map[string]any{
            "msg": 345,
        },
    }
    te, err := template.New("tmpl2").Parse(tmpl)
    if err != nil {
        return
    }
    for _, val := range data {
        err = te.Execute(os.Stdout, val)
        if err != nil {
            return
        }
    }
}
```

> 输出

```textile
data -> 1
data -> sdd
data -> map[msg:345]
```

> 对于切片和`map`，并没有提供特定语法来访问某一个索引的值，但可以通过函数调用的方式来实现，如下所示：

```go
func Templ3() {
    // index 是模板函数，接受两个参数
    // 参数1：切片或map
    // 参数2：索引
    tmpl := "data -> {{ index . 1 }}\n"
    datas := []any{
        []any{"first", "second"},
        map[int]any{1: "first"},
    }
    te, err := template.New("tmpl2").Parse(tmpl)
    if err != nil {
        return
    }
    for _, val := range datas {
        err = te.Execute(os.Stdout, val)
        if err != nil {
            return
        }
    }
}
// 输出
// data -> second
// data -> first
```

> 对于嵌套的结构体或 map，可以使用`.k1.k2.k3`这种方式访问，例如：

```go
{{ .person.father.name }}
```

> 在使用模板参数时，可以在参数前后加上`-`符号来消除参数前后的空白，看个例子：

```go
func main() {
  out := os.Stdout

  tmpl := `{{ .x }} {{ - .op - }} {{ .y }}`

  datas := []any{
    map[string]any{"x": "10", "op": ">", "y": "2"},
  }

  for _, data := range datas {
    err := ExecTmpl(out, tmpl, data)
    if err != nil {
      panic(err)
    }
  }
}
```

> 正常来说输出结果应该是`10 > 2`，但由于在`op`参数前后添加了`-`符号，所以它前后的空白符都会被消除，所以实际输出为`10>2`
> 
> 注意：`-`符号需要和参数保持空格以作区分，`-`符号也会消除`\n`的作用

##### 注释

> 模板语法支持注释，注释并不会在最终的模板中生成，其语法如下：

```go
{{/* this is a comment */}}
```

##### 变量

> 在模板中也可以声明变量，通过`$`符号来表示这是一个变量，并通过`:=` 来进行赋值，就跟 go 代码一样，例子如下：

```go
{{ $name := .Name }}
{{ $val := index . 1 }}
{{ $val := index .dict key }}
// 整型赋值
{{ $numer := 1 }}
// 浮点数赋值
{{ $float := 1.234}}
// 字符串赋值
{{ $name := "jack" }}
```

> 在后续使用时，通过`$`衔接变量名来访问该变量的值，比如：

```go
func Templ4() {
    tmpl := "{{ $name := .Name }} data -> {{ $name }}\n"
    data := struct{ Name string }{Name: "ddw"}
    te, err := template.New("tmpl4").Parse(tmpl)
    if err != nil {
        fmt.Println(err)
        return
    }
    err = te.Execute(os.Stdout, data)
    if err != nil {
        fmt.Println(err)
        return
    }
}
// 输出
data -> ddw
```

##### 函数

> 模板自身的语法其实并不多，大多数功能都是通过函数来实现的，函数调用的格式为函数名后衔接参数列表，以空格为分隔符，如下所示：

```go
{{ funcname arg1 arg2 arg3 ... }}
```

> 例如之前用到的`index`函数

```go
{{ index .s 1 }}
```

> 用于比较是否相等的函数`eq`函数

```go
{{ eq 1 2 }}
```

> 每一个`*Template`都有一个`FuncsMap`，用于记录函数的映射，也可以通过这个map来挂载自己函数

```go
type FuncMap map[string]any
```

> 下面是内置的所有函数：

| 函数名        | 作用               | 示例                      |
| ---------- | ---------------- | ----------------------- |
| `and`      | 与运算              | `{{ and true false }}`  |
| `or`       | 或运算              | `{{ or true false }}`   |
| `not`      | 取反运算             | `{{ not true }}`        |
| `eq`       | 是否相等             | `{{ eq 1 2 }}`          |
| `ne`       | 是否不相等            | `{{ ne 1 2 }}`          |
| `lt`       | 小于               | `{{ lt 1 2 }}`          |
| `le`       | 小于等于             | `{{ le 1 2 }}`          |
| `gt`       | 大于               | `{{ gt 1 2 }}`          |
| `ge`       | 大于等于             | `{{ ge 1 2 }}`          |
| `len`      | 返回长度             | `{{ len .slice }}`      |
| `index`    | 获取目标指定索引的元素      | `{{ index . 0 }}`       |
| `slice`    | 切片，等价于 s[1\:2:3] | `{{ slice . 1 2 3 }}`   |
| `html`     | HTML 转义          | `{{ html .name }}`      |
| `js`       | js 转义            | `{{ js .name }}`        |
| `print`    | fmt.Sprint       | `{{ print . }}`         |
| `printf`   | fmt.Sprintf      | `{{ printf "%s" .}}`    |
| `println`  | fmt.Sprintln     | `{{ println . }}`       |
| `urlquery` | url query 转义     | `{{ urlquery .query }}` |

> 除了这些之外，还有一个比较特殊的内置函数`call`，它是用于直接调用通过在`Execute`时期传入的`data`中的函数，例如：

```go
func Templ5() {
    tmpl := "Name is: {{ call .Sayhi .Name }}"
    data := struct {
        Name  string
        Sayhi func(string) string
    }{Name: "ddw", Sayhi: func(s string) string { return s }}
    te, err := template.New("tmpl4").Parse(tmpl)
    if err != nil {
        fmt.Println(err)
        return
    }
    err = te.Execute(os.Stdout, data)
    if err != nil {
        fmt.Println(err)
        return
    }
}
// 输出
Name is: ddw
```

> 这是自定义函数的途径之一，不过通常建议使用`*Template.Funcs`方法来添加自定义函数，因为后者可以作用全局，不需要绑定到根对象中。
> 
> <mark>自定义函数的返回值一般有两个，第一个是需要用到的返回值，第二个是error。</mark>

##### pipeline 管道

> 任何能够产生数据的操作都称其为`pipeline`。下面的模板操作都属于管道操作：

```go
{{ 1 }}
{{ eq 1 2 }}
{{ $name }}
{{ .name }}
{{ $age := 20 }}
```

> 管道运算符`|`

```go
{{ $name := 1 }}{{ $name | print | printf "%s+1=?" }}
```

##### with

> 通过`with`语句可以控制变量和根对象的作用域，格式如下

```go
{{ with pipeline }}
  text
{{ end }}
```

> `with`会检查管道操作返回的值，如果值为空的话，中间的`text`模板就不会生成。如果想要处理空的情况，可以使用`with else`，格式如下

```go
{{ with pipeline }}
  text1
{{ else }}
  text2
{{ end }}
```

> 如果管道操作返回的值为空，那么就会执行`else`这块的逻辑。在`with`语句中声明的变量，其作用域仅限于`with`语句内，看下面一个例子

```go
{{ $name := "mike" }}
{{ with $name := "jack"  }}
  {{- $name -}}  // "jack"
{{ end }}
{{- $name -}}   // "mike"
```

> 通过`with`语句还可以在作用域内改写根对象，如下

```go
{{ with .name }}
  // 这里的 . 指的是 .name
  name: {{- .second }}-{{ .first -}}    
{{ end }}
age: {{ .age }}
address: {{ .address }}
```

##### 条件

> 条件语句的格式如下所示

```textile
{{ if pipeline }}
  text1
{{ else if pipeline }}
  text2
{{ else }}
  text3
{{ end }}
```

> 几个简单的例子

```textile
{{ if eq .lang "en" }}
  {{- .content.en -}}
{{ else if eq .lang "zh" }}
  {{- .content.zh -}}
{{ else }}
  {{- .content.fallback -}}
{{ end }}
```

##### 迭代

> 迭代语句的格式如下，`range`所支持的`pipeline`必须是数组，切片，`map`，以及`channel`。

```textile
{{ range pipeline }}
  loop body
{{ end }}
```

> 结合`else`使用，当长度为 0 时，就会执行`else`块的内容。

```textile
{{ range pipeline }}
  loop body
{{ else }}
  fallback
{{ end }}
```

> 除此之外，还支持`break`，`continue`这类操作，比如

```textile
{{ range pipeline }}
  {{ if pipeline }}
    {{ break }}
  {{ end }}
  {{ if pipeline }}
    {{ continue }}
  {{ end }}
  loop body
{{ end }}
```

> 简单案例

```textile
{{ range $index, $val := . }}
  {{- if eq $index 0 }}
    {{- continue -}}
  {{ end -}}
  {{- $index}}: {{ $val }}
{{ end }}
```

> 传入数据

```go
[]any{1, "2", 3.14},
// 输出
1: 2
2: 3.14
```

##### 嵌套

> 一个模板中可以定义有多个模板，比如

```textile
{{ define "t1" }} t1 {{ end }}
{{ define "t2" }} t2 {{ end }}
```

> 这些定义的模板在并不会生成在最终的模板中，除非在加载时指定了名称或者通过`ExecuteTemplate`语句手动指定。

```go
func (t *Template) ExecuteTemplate(wr io.Writer, name string, data any) error
```

**案例**

```go
func main() {
  out := os.Stdout

  tmpl :=
    `{{ define "t1" }}
    {{- with .t1 }}
      {{- .data -}}
    {{ end -}}
{{ end }}
{{ define "t2" }}
    {{- with .t2 }}
      {{- .data -}}
    {{ end}}
{{ end -}}`

  datas := []any{
    map[string]any{
      "t1": map[string]any{"data": "template body 1"},
      "t2": map[string]any{"data": "template body 2"},
    },
  }

  name := "t1"

  for _, data := range datas {
    err := ExecTmpl(out, tmpl, name, data)
    if err != nil {
      panic(err)
    }
  }
}

func ExecTmpl(writer io.Writer, tmpl string, name string, data any) error {
  t := template.New("template")
  parsedTmpl, err := t.Parse(tmpl)
  if err != nil {
    return err
  }
  return parsedTmpl.ExecuteTemplate(writer, name, data)
}
```

> 输出

```textile
template body 1
```

##### 关联

> 可以将A模板嵌套进B模板从而实现模板的模块化，这个A模板被称为子模版，通过下面模板语法实现关联：

```textile
{{ tempalte "子模版名" 传递给子模版的数据}}
```

> 父模板

**案例**

```go
func main() {
  tmpl1 := `name: {{ .name }}`

  tmpl2 := `age: {{ .age }}`

  tmpl3 := `Person Info
{{template "t1" .}}
{{template "t2" .}}`

  t1, err := template.New("t1").Parse(tmpl1)
  if err != nil {
    panic(err)
  }

  t2, err := template.New("t2").Parse(tmpl2)
  if err != nil {
    panic(err)
  }

  t3, err := template.New("t3").Parse(tmpl3)
  if err != nil {
    panic(err)
  }

  if err := associate(t3, t1, t2); err != nil {
    panic(err)
  }

  err = t3.Execute(os.Stdout, map[string]any{
    "name": "jack",
    "age":  18,
  })
  if err != nil {
    panic(err)
  }
}

func associate(t *template.Template, ts ...*template.Template) error {
  for _, tt := range ts {
    _, err := t.AddParseTree(tt.Name(), tt.Tree)
    if err != nil {
      return err
    }
  }
  return nil
}
```

##### 插槽

> 通过`block`和`define`语句，可以实现类似 vue 插槽的效果，其目的是为了复用某一个模板。
> 
> **核心思想**：父模板使用 `{{block}}` 定义一个带默认内容的“占位符”，子模板通过 `{{define}}` 来“重写”这个块。

**一个简单的案例**

**1.定义父模板（layout.html）**:使用 `block` 定义可被替换的插槽，并提供默认内容

```textile
{{define "layout"}}
<html>
<head>
    <title>{{block "title"}}默认标题{{end}}</title> <!-- 这是一个可被替换的“插槽” -->
</head>
<body>
    <div id="content">
        {{block "body"}}默认主体内容{{end}} <!-- 这是另一个“插槽” -->
    </div>
</body>
</html>
{{end}}
```

**2.定义子模板（page.html）**：使用 `define` 重新实现父模板中的同名块，实现“填充”

```textile
{{define "title"}}我的个人主页{{end}} <!-- 填充了 title 插槽 -->

{{define "body"}}
<h1>欢迎来到我的网站！</h1>
<p>这里是具体内容。</p>
{{end}}
```

**3.渲染**：在 Go 代码中，执行 `"layout"` 模板，并将子模板文件一起解析。

```go
// 同时解析父模板和子模板文件
tmpl, err := template.ParseFiles("layout.html", "page.html")
// 执行父模板
err = tmpl.ExecuteTemplate(w, "layout", nil)
```



**另一种方式**

**模板te1**

```textile
Basic Person Info
name: {{ .name }}
age: {{ .age }}
address: {{ .address }}
{{ block "slot" . }} default content body {{ end }}
```

**模板te2**

```textile
{{ template "person.txt" . }}
{{ define "slot" }}
school: {{ .school }}
{{ end }}
```

**案例**

```go
func Templ7() {
    tmpl1 := `Basic Person Info
name: {{ .name }}
age: {{ .age }}
address: {{ .address }}
{{ block "slot" . }} default content body {{ end }}`
    templ2 := `{{ template "person.txt" . }}
{{ define "slot" }}
school: {{ .school }}
{{ end }}`

    te1, err := template.New("person.txt").Parse(tmpl1)
    if err != nil {
        fmt.Println(err)
        return
    }
    te2, err := template.New("tem").Parse(templ2)
    if err != nil {
        fmt.Println(err)
        return
    }
    // 将模板te1与模板te2联系起来
    te, err := te2.AddParseTree(te1.Name(), te1.Tree)
    if err != nil {
        fmt.Println(err)
        return
    }
    data := map[string]any{
        "name":    "jack",
        "age":     18,
        "address": "usa",
        "company": "google",
        "school":  "mit",
    }
    te.Execute(os.Stdout, data)
}
```

**输出**

```textile
Basic Person Info
name: jack
age: 18
address: usa

school: mit
```

##### 模板文件

> 上面介绍语法时都是使用字符串作为模板，也可以将模板写入文件，再通过`template.ParseFS()`或`template.ParseFiles()`来读取文件获取模板











## 热重载

> 每次修改代码就要重新go run一次很麻烦，可以使用第三方工具自动重新编译运行，这里介绍air工具

1. **安装air，全局安装**

> `go install github.com/air-verse/air@latest`
> 
> 推荐使用`go install`，确保`$GOPATH/bin`在系统PATH中
> 
> 安装完成后，运行`air -v`验证是否成功

2. **初始化配置**

> 在项目根目录下运行：`air init`
> 
> 这个命令会生成一个默认的配置文件`.air.toml`，可以根据项目需要修改它

3. **启动服务**

> 在项目根目录下直接运行：`air`
> 
> Air会开始监听文件变化，一旦你保存了代码，它就会自动重启应用

# Gin -- Go 网络框架

> Gin 是一个 go 编写的轻量级网络框架。

## Gin 使用

> 安装 `go get "github.com/gin-gonic/gin"`

> `gin` 热加载工具 `fresh`
> 
> 安装：`go get github.com/pilu/fresh`

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()
    // 配置路由
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "pong",
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

## Gin 响应数据

### String() 响应一个字符串

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()
    // String() 响应一个字符串
    r.GET("/str", func(ctx *gin.Context) {
        str := "这是要响应的字符串"
        ctx.String(200, "str=%s", str)
    })
    r.Run() // listen and serve on localhost:8080
}
```

### JSON() 响应一个 json 数据( go 中的 map 类型)

> map 字段的值为结构体时，可以用 tag 来定义要展示的名字

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()
    // gin.H 是 map[string]any 类型
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "pong",
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

### JSONP() 用于解决跨域问题 -- 不推荐使用

> 推荐使用 cors 中间件

### XML() 响应 xml 数据

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()
    // gin.H 是 map[string]any 类型
    r.GET("/xml", func(ctx *gin.Context) {
        ctx.XML(200, gin.H{
            "message": "响应成功",
            "data":    23,
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

### HTML() 响应 html 数据

> 需要先导入模板文件
> 
> `ctx.HTML()`：
> 
> - 第一个参数是请求路径
> 
> - 第二个参数是模板名，会去模板文件里查找`define 模板名`，如果找到加载这个模板，模板插槽根据`blob`和`define`自动进行
> 
> - 第三个参数是传递给模板的数据

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()

    // 加载模板文件，加载./templates目录下所有模板
    r.LoadHTMLGlob("./templates/*")

    // 可以通过一定的语法将gin.H内的数据渲染到html页面中
    r.GET("/html", func(ctx *gin.Context) {
        // 第二个参数是模板名
        ctx.HTML(200, "news.html", gin.H{
            "content": "模板渲染内容",
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

## Gin 前端数据接收

### get 查询参数

`/user?uid=20&page=1`

> 上面的 get 请求传入了两个参数

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()

    // get 查询参数
    r.GET("/user", func(ctx *gin.Context) {
        uid := ctx.Query("uid")
        page := ctx.Query("page")
        // 给查询参数设置默认值，如果没有传uname参数，则uname=ddw
        uname := ctx.DefaultQuery("username", "ddw")
        // ctx.HTML(200, "display.html", gin.H{
        //    "data":   username,
        //    "passwd": passwd,
        //    "page":   page,
        //})
    })
    r.Run() // listen and serve on localhost:8080
}
```

### 路径参数(动态路由传值)

`/user/123`

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()

    // 动态路由传参
    r.GET("/user/:cid", func(ctx *gin.Context) {
        cid := ctx.Param("cid")
        ctx.JSON(200, gin.H{
            "cid": cid,
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

### post 请求 表单参数

> post表单的内容类型一般有`application/json`，`application/x-www-form-urlencoded`，`application/xml`，`multipart/form-data`。
> 
> **解析方法：**
> 
> - `PostForm`方法默认解析`application/x-www-form-urlencoded`和`multipart/form-data`类型的表单。
> 
> - `ShouldBindJSON`方法解析`application/json`和`multipart/form-data`类型绑定到结构体

**案例一：解析`application/x-www-form-urlencoded`和`multipart/form-data`类型**

```go
// 使用
package main

import "github.com/gin-gonic/gin"

func main() {
    // 创建一个默认路由
    r := gin.Default()

    // 获取post传入的参数
    r.POST("/adduser", func(ctx *gin.Context) {
        // 获取前端传入的username参数
        username := ctx.PostForm("username")
        // 获取前端传入的data参数，data参数可能有多个值，返回一个切片
        arr := ctx.PostFormArray("data")
        // 获取前端传入的data参数，data参数可能有多个键值对，返回一个map
        m := ctx.PostFormMap("data")
        ctx.JSON(200, gin.H{
            "display": username,
            "data":    arr,
            "map":     m,
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

**案例二：解析`application/json`类型**

> 将前端传入的 `json` 数据绑定到结构体上，通过`tag` 和 `ctx.ShouldBindJSON` 实现

```go
type User struct {
    // json:"name" 会在结构体转成json时会将字段名Name转成name
    // 也会将json数据中name字段绑定到Name
    Name string `json:"name"`
    Age  int    `json:"age"`
}

user := User{}
err := ctx.ShouldBindJSON(&user)
if err != nil {
    ctx.JSON(500, gin.H{
        "error": "绑定出错",
    })
}
```

> 将 `post` 传递的 `form` 表单数据绑定到结构体上，通过`tag` 和 `ctx.ShouldBind` 实现

```go
type User struct {
    // json:"name" 会在结构体转成json时会将字段名Name转成name
    Name string `json:"name" form:"name"`
    // form:"age" 会将form中数据age字段绑定到 Age上
    Age  int    `json:"age" form:"age"`
}

err := ctx.ShouldBind(&user)
if err != nil {
    ctx.JSON(500, gin.H{
        "error": "绑定出错",
    })
}
```

## Gin 前端数据校验

> `gin`内置的校验工具其实是`github.com/go-playground/validator/v10`，使用方法也几乎没有什么差别。
> 
> 这里只介绍一下在gin中如何进行数据校验和一些`validator`库中一些tag

**简单示例**

```go
type LoginUser struct {
    Name  string `json:"name" binding:"required,min=2,max=50"`
    Email string `json:"email" binding:"required,email"`
    Age   int    `json:"age" binding:"gte=18,lte=120"`
    Phone string `json:"phone" binding:"required,len=11"`
}

func main() {
   e := gin.Default()
   e.POST("/register", Register)
   log.Fatalln(e.Run(":8080"))
}

func Register(ctx *gin.Context) {
   newUser := &LoginUser{}
   if err := ctx.ShouldBind(newUser); err == nil {
      ctx.String(http.StatusOK, "user%+v", *newUser)
   } else {
      ctx.String(http.StatusBadRequest, "invalid user,%v", err)
   }
}
```

> 上面示例中，`binding`标签后面跟的内容就是验证，当gin内对这个结构体进行绑定时会自动验证，如果验证不通过会报错。

### 验证标签

**特殊符号**

| Tag       | Description           |
| --------- | --------------------- |
| 逗号( ,)    | 把多个验证标记隔开，隔开逗号之间不能有空格 |
| 横线( - )   | 跳过该字段不验证              |
| 竖线( \| )  | 使用多个验证标记，但是只需满足其中一个即可 |
| required  | 表示该字段值必输设置，且不能为默认值    |
| omitempty | 如果字段未设置，则忽略它          |

**比较**

| Tag   | Description |
| ----- | ----------- |
| `eq`  | 等于          |
| `gt`  | 大于          |
| `gte` | 大于等于        |
| `lt`  | 小于          |
| `lte` | 小于等于        |
| `ne`  | 不等于         |

**字段**

| Tag             | Description                           |
| --------------- | ------------------------------------- |
| `eqcsfield`     | 在一个单独的结构中，验证当前字段的值是否等于由 param 的值指定的字段 |
| `eqfield`       | 验证当前字段的值是否等于参数值指定的字段                  |
| `fieldcontains` | 验证当前字段的值是否包含由参数值指定的字段                 |
| `fieldexcludes` | 验证当前字段的值是否不包含由参数值指定的字段                |
| `gtcsfield`     | 在一个单独的结构中，验证当前字段的值是否大于由参数的值指定的字段      |
| `gtecsfield`    | 在一个单独的结构中，验证当前字段的值是否大于或等于由参数的值指定的字段   |
| `gtefield`      | 验证当前字段的值是否大于或等于由参数值指定的字段              |
| `gtfield`       | 验证当前字段的值是否大于由参数值指定的字段                 |
| `ltcsfield`     | 在一个单独的结构中，验证当前字段的值是否小于由参数的值指定的字段      |
| `ltecsfield`    | 在一个单独的结构中，验证当前字段的值是否小于等于由参数的值指定的字段    |
| `ltefield`      | 验证当前字段的值是否小于或等于由参数值指定的字段              |
| `ltfield`       | 验证当前字段的值是否小于由参数值指定的字段                 |
| `necsfield`     | 验证当前字段的值不等于由参数的值指定的单独结构中的字段           |
| `nefield`       | 验证当前字段的值是否不等于参数值指定的字段                 |

**字符串**

| Tag               | Description                     |
| ----------------- | ------------------------------- |
| `alpha`           | 验证当前字段的值是否是有效的字母                |
| `alphanum`        | 验证当前字段的值是否是有效的字母数字              |
| `alphanumunicode` | 验证当前字段的值是否是有效的字母数字 unicode 值    |
| `alphaunicode`    | 验证当前字段的值是否是有效的字母 unicode 值      |
| `ascii`           | 验证字段的值是否为有效的 ASCII 字符           |
| `boolean`         | 验证当前字段的值是否为有效的布尔值或是否可以安全地转换为布尔值 |
| `contains`        | 验证字段的值是否包含参数中指定的文本              |
| `containsany`     | 验证字段的值是否包含参数中指定的任何字符            |
| `containsrune`    | 验证字段的值是否包含参数中指定的符文              |
| `endsnotwith`     | 验证字段的值不以参数中指定的文本结束              |
| `endswith`        | 验证字段的值以参数中指定的文本结束               |
| `excludes`        | 验证字段的值不包含参数中指定的文本               |
| `excludesall`     | 验证字段的值不包含参数中指定的任何字符             |
| `excludesrune`    | 验证字段的值不包含参数中指定的字符               |
| `lowercase`       | 验证当前字段的值是否为小写字符串                |
| `multibyte`       | 验证字段的值是否具有多字节字符                 |
| `number`          | 验证当前字段的值是否为有效数字                 |
| `numeric`         | 验证当前字段的值是否是有效的数值                |
| `printascii`      | 验证字段的值是否是有效的可打印 ASCII 字符        |
| `startsnotwith`   | 验证字段的值不是以参数中指定的文本开始             |
| `startswith`      | 验证字段的值是否以参数中指定的文本开始             |
| `uppercase`       | 验证当前字段的值是否为大写字符串                |

## 路由分组及路由抽离

> 将路由放在一个页面中很难管理，所以需要将路由分组抽离到其它文件中，这些文件存放再 routers 中。

```go
// 在其它文件中定义函数，与java中的controller比较相似
// r是路由对象
func AdminRouter(r *gin.Engine) {
    // 路由分组
    adminRouters := r.Group("/admin")
    {
        // 实际地址为 /admin/dashboard
        adminRouters.GET("/dashboard", func(ctx *gin.Context) {
            ctx.JSON(200, gin.H{
                "message": "欢迎来到管理员后台",
            })
        })
        //
    }
}
// 在main中注册路由
func main() {
    r = gin.Default()
    // 调用上面的函数，传入路由对象
    AdminRouter(r)
}
```

## 自定义控制器 -- 抽离业务逻辑

> 将上面的路由进一步分解，把路由中的回调函数抽取到 controller 中，再挂载到一个结构体上。

> 在项目文件夹中建立 controller 文件夹，在这个文件夹中再创建不同路由的文件夹(例如：新建 admin 文件夹存放 admin 路由的处理函数)

```go
// 在controller中的admin文件夹中
package admin

// 把业务逻辑挂载到结构体上
type AdminController struct {
    // 空结构体
}
// 挂载
func (ad AdminController) AdminRouterSayHello(c *gin.Context) {
    c.String(200, "say hello")
}

// 在admin的路由中
func AdminRouter(r *gin.Engine) {
    // 声明业务逻辑结构体
    adminController := AdminController{}
    // 路由分组
    adminRouters := r.Group("/admin")
    {
        // 实际地址为 /admin/hello
        // 传入AdminRouterSayHello函数
        adminRouters.GET("/hello", adminController.AdminRouterSayHello)
    }
}
```

## Gin 中间件

> Gin 允许开发者在处理请求的过程中，加入用户自己的钩子(Hook)函数。这个钩子函数就叫中间件，中间件适合处理一些公共的业务逻辑，比如登录认证、权限校验、数据分页、记录日志、耗时统计等。

> 在最后一个函数之前的都叫中间件，中间件可以有多个，响应处理函数是主处理函数（类似一种中间件）

### 局部中间件

```go
r.GET("/user/", func(ctx *gin.Context){
    // 中间件
    fmt.Println("这是中间件")
    // 调用该请求的剩余处理程序
    // 中间件只执行一次，用Next调用过的，不再执行了
    ctx.Next()    // 执行紧接着的下一个中间件，这个例子中没有其它中间件了，所以执行响应函数
    // 上面其它中间件执行完后继续执行下面的代码
    fmt.Println("继续执行这个中间件")
    // 终止调用该中间件的下一个中间件的执行
    ctx.Abort()
    fmt.Println("这个还继续执行")
} ,func(ctx *gin.Context) {
    cid := ctx.Param("cid")
    ctx.JSON(200, gin.H{
        "cid": cid,
    })
})
```

**另一个案例**

```go
func MiddlewareA() gin.HandlerFunc {
    return func(c *gin.Context) {
        fmt.Println("A 前")
        c.Next()
        fmt.Println("A 后")
    }
}

func MiddlewareB() gin.HandlerFunc {
    return func(c *gin.Context) {
        fmt.Println("B 前")
        c.Next()
        fmt.Println("B 后")
    }
}

func MainHandler(c *gin.Context) {
    fmt.Println("主处理函数")
    c.JSON(200, gin.H{"msg": "ok"})
}

// 输出顺序：
// A 前 -> B 前 -> 主处理函数 -> B 后 -> A 后
```

### 全局中间件

> `Use`方法可以设置全局中间件

```go
// 定义中间件
func initMiddle(c *gin.Context) {
    fmt.Println("这是一个中间件")
}

// 注册全局中间件，对所有的路由都生效
r.Use(initMiddle, middle2/*可以传入多个*/)
```

### 中间件之间的通信

> 可以用 `Set` 来设置全局信息让其它中间件（<mark>只能是在同一个 url 路由的中间件来使用</mark>）来使用， `Get` 获取其它中间件设置的信息，抽离到其它文件也能用。

```go
r.GET("/set", func(ctx *gin.Context) {
        // 设置数据
        ctx.Set("set", "这是set设置的")
    }, func(ctx *gin.Context) {
        // 获取数据
        v, f := ctx.Get("set")
        if !f {
            ctx.JSON(200, gin.H{
                "msg": "数据不存在",
            })
            return
        }
        ctx.JSON(200, gin.H{
            "msg":  "success",
            "data": v,
        })
    })
```

### 在中间件中使用 goroutine

> 在中间件中使用 `goroutine` 时，<mark>不能使用</mark>原始的上下文(`ctx *gin.Context`)，必须使用其只读副本

```go
// 获取只读副本
cCp := ctx.Copy()
```

## 自定义 Model -- 抽取公共代码

> 如果项目比较复杂，可能不同的 Controller 中存在相同的业务逻辑，可以将不同的 Controller 中的相同业务逻辑抽离到 Model，以减少重复代码。

## 文件传输

> 文件传输分为上传和下载

**单文件上传**

> go 接收文件上传，使用 `file, err := ctx.FormFile()` 方法接收，使用 `ctx.SaveUploadedFile(file, "目录")` 来保存。

```go
func main() {
  e := gin.Default()
  e.POST("/upload", uploadFile)
  log.Fatalln(e.Run(":8080"))
}

func uploadFile(ctx *gin.Context) {
  // 获取文件
  file, err := ctx.FormFile("file")
  if err != nil {
    ctx.String(http.StatusBadRequest, "%+v", err)
    return
  }
  // 保存在本地
  err = ctx.SaveUploadedFile(file, "./"+file.Filename)
  if err != nil {
    ctx.String(http.StatusBadRequest, "%+v", err)
    return
  }
  // 返回结果
  ctx.String(http.StatusOK, "upload %s size:%d byte successfully!", file.Filename, file.Size)
}
```

**多文件上传**

> 使用`ctx.MultipartForm()`来解析多文件表单

```go
func main() {
   e := gin.Default()
   e.POST("/upload", uploadFile)
   e.POST("/uploadFiles", uploadFiles)
   log.Fatalln(e.Run(":8080"))
}

func uploadFiles(ctx *gin.Context) {
  // 获取gin解析好的multipart表单
  form, _ := ctx.MultipartForm()
  // 根据键值取得对应的文件列表
  files := form.File["files"]
  // 遍历文件列表，保存到本地
  for _, file := range files {
    err := ctx.SaveUploadedFile(file, "./"+file.Filename)
    if err != nil {
      ctx.String(http.StatusBadRequest, "upload failed")
      return
    }
  }
  // 返回结果
  ctx.String(http.StatusOK, "upload %d files successfully!", len(files))
}
```

**文件下载**

> 文件下载使用`FileAttachment()`函数：
> 
> - 第一个参数，文件路径
> 
> - 第二个参数，用户保存文件时的文件名

```go
func (c *Context) FileAttachment(filepath, filename string)

// 使用
func main() {
  e := gin.Default()
  e.POST("/upload", uploadFile)
  e.POST("/uploadFiles", uploadFiles)
  e.GET("/download/:filename", download)
  log.Fatalln(e.Run(":8080"))
}

func download(ctx *gin.Context) {
    // 获取文件名
  filename := ctx.Param("filename")
    // 返回对应文件
  ctx.FileAttachment(filename, filename)
}
```



## Gin 中的 Cookie

> Cookie: 是保存在用户设备上的，可以让用户同一个设备访问同一个网站时共享数据。

**设置 cookie**

```go
ctx.SetCookie(name, value string, maxAge int, path, domain string, secure, httpOnly bool)

// 示例
ctx.SetCookie("gin_cookie", "test", 3600, "/", "localhost", false, true)
```

> * 第一个参数是： cookie 的 key
> * 第二个参数是：设置 key 的 value
> * 第三个参数是：过期时间。如果只想设置 cookie 的保存路径而不像设置存活时间，可以传入 nil
> * 第四个参数是：cookie 的路径
> * 第五个参数是：cookie 的路径 Domain 作用域，本地调试配置成 localhost，正式上线配置成域名
> * 第六个参数是：secure，当 secure 值为 true 时，cookie 在 http 中是无效的，在 https 中才有效
> * 第七个参数是：httpOnly，是微软对 cookie 做的扩展。如果在 cookie 中设置了"httpOnly"属性，则通过程序(js 等)将无法读取到 cookie 信息，防止 xss 攻击

**获取 cookie**

> `ctx.Cookie("cookie键名")`

**删除 cookie**

> 与设置 cookie 相同，只是把时间设置为-1 或者把值设置为空字符串

## Gin 中的 Session

> session 是另一种记录客户状态的机制，不同的是 cookie 保存在客服端浏览器中，而 session 保存在服务器上。

**Session 的工作流程**

> 当客户端浏览器第一次访问服务器并发送请求时，服务器端会创建一个 `session` 对象，生成一个类似于 `key`，`value` 的键值对，然后将 `value` 保存到服务器，将 `key(cookie)`返回到浏览器。浏览器下次访问时会携带 `key(cookie)`，找到对应的 `session(value)`

**Gin 中使用 Session**

> Gin 官方没有 session，所以需要使用第三方模块。

## JWT

> JWT是服务端发完客户端的令牌，JWT 已经成为了现代服务端通信认证的主流方式之一，具有轻量，无状态的特点。

### 工作原理

> 在身份验证中，当用户使用凭据成功登录时，将返回一个 JSON Web 令牌。由于令牌是凭证，因此必须非常小心地防止出现安全问题。一般来说，令牌的保存时间不应超过所需的时间。然后无论何时用户想要访问受保护的路由和资源，在发起请求时就必须携带上 token，通常都是在请求头中的`Authorization` header 中的`Bearer schema`，例如下方：

```textile
Authorization: Bearer <token>
```

> 服务器在收到 JWT 后，会对其进行有效性验证，例如内容有篡改，token 已过期等等，如果验证通过就可以顺利的访问资源。虽然 JWT 中可以携带一些基本信息，但是依旧建议信息不要太大。

### 结构

> 在 RFC 标准中，JWT 由以下三个部分组成：
> 
> * Header 头部
> * Payload 载荷
> * Signature 签名
> 
> 然后每一个部分用一个点`.`来分隔，最后组成一个字符串，格式就是`header.payload.signature`吗，这就是一个 JWT 令牌的标准结构，接下来就一个个讲解每个结构的作用。

#### 头部

> 头部只是声明一些基本信息，通常由两部分组成，令牌的类型，和签名所使用的加密算法，例如下方：

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

> 以上的信息大致就是，令牌的类型为 JWT，签名部分所使用的加密算法为 HS256，最后再将 JSON 对象通过`Base64Url`编码成字符串，该字符串就是 JWT 的头部。

#### 载荷

> JWT 的第二部分是载荷部分，主要包含声明(`claims`)部分，声明部分通常是关于一个实体的数据，比如一个`用户id`。
> 
> 关于声明的类型总共有三种：
> 
> * `reigstered`注册声明：`Registered claims`代表着 一些预定义的声明，一些并不强制使用但是仍然推荐使用，例如：`iss`(issuer 签发者)，`exp`(expiration time 过期时间) ，`iat`(签发时间)，`sud`(用户主体)。
> * `public`公共声明：`Public claims`是可以由使用 JWT 的人随意定义的，最好要避免和其他声明部分冲突。
> * `private claims`私有声明：这部分的声明同样也是自定义的，通常用于在服务双方共享一些信息，如：{"user_id": 1001, "role":"vip"}。

```json
{
  // 标准声明
  "iss": "auth.myapp.com",
  "sub": "user-1001",
  "aud": "myapp-api",
  "exp": 1735689600,
  "iat": 1735686000,
  "jti": "550e8400-e29b-41d4-a716-446655440000",

  // 自定义声明
  "user_id": 1001,
  "username": "张三",
  "email": "zhangsan@example.com",
  "role": "admin",
  "avatar_url": "https://cdn.myapp.com/avatars/1001.jpg"
}
```

> 该 JSON 对象将会通过`Base64Url`被编码成字符串，从而组成 JWT 的第二部分。
> 
> <mark>注意：该部分不会被加密，不能放敏感信息</mark>

#### 签名

> 在获得了编码的头部和编码的载荷部分后，就可以通过头部所指明的签名算法根据前两个部分的内容再加上密钥进行加密签名，所以一旦 JWT 的内容有任何变化，解密时得到的签名都会不一样，同时如果是使用私钥，也可以对 JWT 的签发者进行验证。

```textile
sign = HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

> 最后得到的输出就是一个由三个`base64Url`字符串组成且由`.`分隔的字符串，大概长下面这样

```textile
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
cThIIoDvwdueQB468K5xDc5633seEFoqwxjF_xSJyQQ
```

### 选择签名算法

> 可用的签名算法有好几种，在使用之前应该先了解下它们之间的区别以便更好的去选择签名算法，它们之间最大的不同就是**对称加密**和**非对称加密**
> 
> 不同的签名算法所需要的密钥的类型也不同，下面给出一些常见签名算法的类型：
> 
> * `HMAC`：对称加密，需要类型`[]byte`的值用于签名和验证。 (`HS256`,`HS384`,`HS512`)
> * `RSA`：非对称加密，需要`*rsa.PrivateKey`类型的值用于签名，和`*rsa.PublicKey`类型的值用于验证。(`RS256`,`RS384`,`RS512`)
> * `ECDSA`：非对称加密，需要`*ecdsa.PrivateKey`类型的值用于签名，和`*ecdsa.PublicKey`类型的值用于验证。(`ES256`,`ES384`,`ES512`)
> * `EdDSA`：非对称加密，需要`ed25519.PrivateKey`类型的值用于签名和`ed25519.PublicKey` 类型的值用于验证。(`Ed25519`)

### JWT使用示例

**HMAC 的创建与签名**

```go
// token结构体，JWT库中定义，无需自己再定义
type Token struct {
  Raw       string                 // 原始Token字符串，当开始解析时填充此字段
  Method    SigningMethod          // 签名使用的方法
  Header    map[string]interface{} // JWT的header部分
  Claims    Claims                 // JWT的payload部分
  Signature string                 // JWT的签名部分，当开始解析时填充此字段
  Valid     bool                   // JWT是否合法有效
}

func TestHmac(t *testing.T) {
   // hmac的密钥类型是字节数组
   secret := []byte("my secret")
   // 头部是签名算法，可以根据选择自动确定
   // 使用HS256算法，jwt.MapClaims是payload
   token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
      "id":   123456,
      "name": "jack",
   })
   fmt.Printf("%+v\n", *token)
   // 签名
   signedString, err := token.SignedString(secret)
   fmt.Println(signedString, err)
}
```

**HMAC 解析验证 Token**

```go
func TestParse(t *testing.T) {
   secret := []byte("my secret")
   // 假设通过HS256算法创建并签名生成了一个token
   tokenString := "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTIzNDU2LCJuYW1lIjoiamFjayJ9.QxLw9NkFgZW3BluyXIofe4efp1IAy61s8b2fe3Eo86M"

   // 传入token字符串和验证钩子函数，返回值就是一个Token结构体
   token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
      // 验证签名算法是否匹配
      if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
         return nil, fmt.Errorf("不匹配的签名算法: %s", token.Header["alg"])
      }

      // 返回验证密钥
      return secret, nil
   })
   if err != nil {
      fmt.Println(token, err)
   }

   if claims, ok := token.Claims.(jwt.MapClaims); ok && token.Valid {
      fmt.Println(claims)
   } else {
      fmt.Println(err)
   }

}
```

## context 上下文

# GORM -- Go 的 orm 框架

> `orm`: `orm` 是通过实例对象的语法，完成关系型数据库的操作的技术，是对象-关系映射 Object/Relational Mapping 的缩写。

> `GORM` 官方支持都数据库类型有：`MySQL`，`PostgreSQL`，`SQLite`，`SQL Server`

## GORM 安装

```shell
# 安装gorm框架
go get -u gorm.io/gorm
# 安装对应数据库驱动
go get -u gorm.io/driver/mysql
```

## 定义数据库模型

需注意以下几点：

1. 结构体的名称必须首字母大写，并和数据库表名对应
   
   > 例如：表名`user`结构体名称定义成`User`，表名称为`article_cate`结构体名称定义成`ArticleCate`

2. 结构体中的字段名首字母必须大写，并和数据库表中的字段一一对应
   
   > 例如：结构体中的字段名`Id`和数据库表中的`id`对应，`Username`和数据库中的`username`对应，`AddTime`和数据库中的`add_time`对应

3. 默认情况下表名是结构体名称的复数形式。
   
   > 例如：如果结构体名称定义为`User`，表示这个模型默认操作的是`users`表。

4. 可以使用结构体中的自定义方法`TableName`改变结构体的默认表名称
   
   ```go
   // 表示User这个结构体操作user表
   func (User) TableName() string {
       return "user"
   }
   ```

### 定义一个完整的表结构的结构体

> 下面就像是 `spring` 中的 `dto`

```go
type Guser struct {
    Id       int    `gorm:"column:id;primaryKey" json:"id"`
    Username string `gorm:"column:username" json:"username"`
    Age      int    `gorm:"column:age" json:"age"`
    Email    string `gorm:"column:email" json:"email"`
    AddTime  int    `gorm:"column:add_time" json:"add_time"`
}

func (Guser) TableName() string {
    return "guser"
}
```

### 定义一个不完全的表结构的结构体

> 通常是传递给前端的结构体，传递完整表结构的结构体会向用户展示不必要的数据，下面的例子中，只展示两个字段

```go
type Suser struct {
    Username string `gorm:"column:username" json:"username"`
    Age      int    `gorm:"column:age" json:"age"`
}
// 表示这个结构体操作guser数据表
func (Suser) TableName() string {
    return "guser"
}
```

### 一个自动生成表结构的工具

> 如果数据库表的项比较多的话，手动创建表结构的结构体很麻烦，所以下面介绍一个gorm官方提供的自动化工具`Gorm Gen`

1. 安装`Gorm Gen`

```shell
go get -u gorm.io/gen
```

2. 编写生成器脚本

```go
// 建议写在数据库初始化函数里
func InitDB() {
    var err error
    dsn := "root:123456@tcp(127.0.0.1:3309)/sw?charset=utf8mb4&parseTime=True&loc=Local"
    DB, err = gorm.Open(mysql.Open(dsn), &gorm.Config{})
    if err != nil {
        slog.Error("数据库连接失败")
        return
    }
    g := gen.NewGenerator(gen.Config{
        // 生成文件的输出路径
        OutPath: "./dal/query",
        // 生成的 Model 文件路径 (如果不指定，会和查询代码在同一级)
        ModelPkgPath: "./po",

        // 生成模式: 这里选择不要生成查询函数
        Mode: gen.WithoutContext | gen.WithQueryInterface,

        // 以下配置控制生成 Model 的字段属性 (可选)
        FieldNullable:     true,  // 数据库字段可空时，生成指针类型
        FieldCoverable:    false, // 字段有默认值时，生成指针类型以处理零值问题
        FieldSignable:     false, // 检测整数字段是否为无符号类型
        FieldWithIndexTag: true,  // 生成 gorm index 标签
        FieldWithTypeTag:  true,  // 生成 gorm type 标签
    })
    // 3. 使用数据库连接
    g.UseDB(DB)

    // 4. 指定要生成代码的表
    // 方式A: 生成所有表
    //g.ApplyBasic(g.GenerateAllTable()...)

    // 方式B: 生成指定的表，并可以自定义模型 (推荐)
    // 后面写哪个表
    userModel := g.GenerateModel("swuser")

    // 方式C: 生成表时进行更精细的控制
    // g.GenerateModel("products",
    //     gen.FieldType("price", "decimal.Decimal"), // 自定义字段类型
    //     gen.FieldIgnore("deleted_at"),             // 忽略某个字段
    // )

    // 将生成的结构体写入文件
    g.ApplyBasic(userModel)

    // 执行生成
    g.Execute()
    slog.Info("初始化完成")
}
```

## 初始化数据库

> 以 mysql 数据库为例，下面连接 gin 表

```go
// 在models目录中定义一个core.go
// 在core.go中定义一个全局变量，方面其它文件使用
var DB *gorm.DB

// 这个初始化方法要在main方法中调用，要在路由初始化方法之前
func InitDB() {
    var err error
    // root：是这里的用户名
    // 123456：是密码
    // 127.0.0.1:3306：是数据库地址
    // gin：是数据库
    dsn := "root:123456@tcp(127.0.0.1:3306)/gin?charset=utf8mb4&parseTime=True&loc=Local"
    DB, err = gorm.Open(mysql.Open(dsn), &gorm.Config{})
    if err != nil {
        fmt.Println("连接数据库出错了", err)
    }
}
```

## gorm 的 crud

> 下面例子中的数据库 `DB` 放在了 `models` 中 `core.go`

### 1. 查

```go
// model.Guser 是数据库表的结构体
// 查询id为2的数据，查询到的数据存在 gu 中
gu := models.Guser{Id: 2}
model.DB.Find(&gu)

// 定义表结构体切片
guserList := []models.Guser{}
// 查询数据，age字段为的数据，查询到的数据存放在切片中
model.DB.Where("age=?", 26).Find(&guserList)
```

### 2. 增

```go
// 添加一条数据
guser := models.Guser{
    Id:       3,
    Username: "yang",
    Age:      22,
    Email:    "yang@qq.com",
    AddTime:  300,
}
model.DB.Create(&guser)
```

### 3. 改

```go
// 查询id为2的数据
u := models.Guser{Id: 6}
models.DB.Find(&u)
// 更改数据
u.Username = "哈哈"
// 保存修改的数据
models.DB.Save(&u)


// 或者用下面
u := models.Guser{}
models.DB.Model(&u).Where("id=?", 2).Update("username", "哈哈")

// 使用多个占位符
models.DB.Model(&u).Where("id>? AND id<?", 2, 9).Update("username", "哈哈")
```

### 4. 删

```go
// 删除id为2的数据
u := models.Guser{Id: 2}
models.DB.Delete(&u)

// 或者
u := models.Guser{}
models.DB.Where("id = ?", 2).Delete(&u)
```

## gorm 中使用原生 sql 语句

```go
// 没有查询结果的
models.DB.Exec("delete from user where id=?", 5)

// 有查询结果的，把查到的数据存到u中
u := []models.User{}
models.DB.Raw("select * from user").Scan(&u)
```

## grom 多表查询

### 一对一 关联关系

> 先定义多表结构体

```go
type ClassInfo struct {
    Id        int
    ClassName string
}

type User struct {
    Id       int    `json:"id"`
    Username string `json:"username"`
    Age      int    `json:"age"`
    Email    string `json:"email"`
    AddTime  int    `json:"add_time"`
    // 外键
    ClassId  int    `json:"classid"`
    // 通过tag来表示哪个是外键
    ClassInfo ClassInfo    `gorm:"foreignKey:ClassId;references:Id"`
}

// 查询
user := []models.User{}
models.DB.Preload("ClassInfo").Find(&user)

```

### 一对多 关联关系

### 多对多 关联关系

# Go的日志技术

> 这里介绍的官方提供的slog

# Go-Zero --- 微服务框架

> `go-zero` 是一个集成了各种工程实践的 `web` 和 `rpc` 框架。通俗的讲：`go-zero` 既是一个 web 框架也是一个微服务框架。
> 
> `go-zero` 提供了极简的 API 定义和强大的代码生成工具 `goctl`，可以大幅提升开发效率。定义好 API 后通过 `goctl` 可以快速生成工程代码。
> 
> `go-zero` 中还内置了丰富的微服务治理能力，如：限流、熔断、降载、服务发现、负载均衡、链路追踪等。

## 搭建 go-zero 环境

1. 首先要安装 golang

2. 安装 goctl
   
   > goctl 是 go-zero 的内置脚手架，是提升开发效率的一大利器，可以一键生成代码、文档、部署 k8s yaml、dockerfile 等。
   
   ```bash
   # 安装
   go install github.com/zeromicro/go-zero/tools/goctl@latest
   # 验证
   goctl --version
   ```

3. 安装 protoc
   
   > protoc 是一个用于生成代码的工具，它可以根据 proto 文件生成 C++、Java、Python、Go、PHP 等多重语言的代码，而 gRPC 的代码生成还依赖 protoc-gen-go，protoc-gen-go-grpc 插件来配合生成 Go 语言的 gRPC 代码。

> 通过 goctl 可以一键安装 protoc，protoc-gen-go，protoc-gen-go-grpc 相关组件，你可以执行如下命令：

```bash
goctl env check --install --verbose --force
```

4. go-zero 安装
   
   > 在 Golang 中，推荐使用 go module 来管理。即在项目中用`go mod tidy`安装。

## 创建并初始化项目

1. 创建项目

```bash
# 创建工作空间并进入该目录
mkdir -p ~/workspace/项目名 && cd ~/workspace/项目名
# 执行指令生成demo服务
goctl api new firstdemo
```

2. 下载依赖

```bash
go mod tidy
```

### 目录结构介绍

<img src="./pic/go/屏幕截图 2026-01-14 133812.png">

## goctl 常用命令

1. `goctl api new` 快速创建 API 服务（重点）

```bash
# firstdemo 是服务名
goctl api new firstdemo
```

2. `goctl api --o 文件名.api` 快速生成一个 API 文件

```bash
# shop是api文件名
goctl api --o shop.api
```

3. `goctl api format` 格式化 API 文件

```bash
goctl api format --dir .
```

4. `goctl api go`为 API 文件生成 Go 代码（重点）

```bash
# --api 后面指定api文件位置
# --dir 后面指定生成Go代码位置
goctl api go --api shop.api --dir .
```

5. `goctl api doc`生成 doc 文档

```bash
# --dir 后面指定项目位置
# --o 后面指定生成文档位置
goctl api doc --dir . --o ./doc
```

## api 文件语法详解

> `api` 是 `go-zero` 自研的领域特性语言（下文称 `api` 语言 或 `api` 描述语言），旨在实现人性化的基础描述语言，作为生成 `HTTP` 服务最基本的描述语言。

```go
// 版本号，固定值
syntax = "v1"

// 描述shop微服务的信息
// info (
//  title: // add title
//  desc: // add description
//  author: "dugufeng"
//  email: "dugufeng@tutamail.com"
// )
/*
下面定义结构体
*/
// 公共的响应结构体
type CommonResponse {
    Code    int         `json:"code"`
    Message string      `json:"message"`
    Data    interface{} `json:"data"`
    Success bool        `json:"success"`
}

// 定义轮播图的请求和响应结构体
type (
    // 这是请求结构体，只有一个id字段
    FocusImageRequest {
        // tag的意思是接收get传值
        Id string `form:"id"`
    }
    // 响应结构体，包含轮播图的详细信息
    FocusImageResponse {
        Id       string `json:"id"`
        Title    string `json:"title"`
        ImageUrl string `json:"image_url"`
        LinkUrl  string `json:"link_url"`
    }
)

@server (
    // 定义一个鉴权控制的中间件，多个中间件以英文逗号,分割，如 Middleware1,Middleware2,中间件按声明顺序执行
    middleware: UserAgentMiddleware
)
//
service shop-api {
    // GetFocus 是处理函数的名称
    @handler GetFocus
    // /api/focus 是请求的路径，后面的括号中是请求的结构体类型，这里没有请求体所以为空
    // CommonResponse 是响应的结构体类型
    get /api/focus returns (CommonResponse)

    // GetFocusById 是处理函数的名称
    @handler GetFocusById
    //
    post /api/onefocus (FocusImageRequest) returns (CommonResponse)
}

// 可以有多个@server，每个@server对应紧接着的service
@server(
    // 路由分组
    group: Foo
)
service shop-foo-api {
    @handler Test
    get /foo/test returns (CommonResponse)

}
```

## go-zero 中使用gorm

> 具体使用参考前面的 `gorm` 使用

1. 安装依赖

```bash
go get -u gorm.io/gorm
go get -u gorm.io/driver/mysql

go mod tidy
```

2. 创建数据库模型

> 在 `model/gorm/文件名.go` 中创建模型

```go
// 这个结构体与表结构相同
type Focus struct {
    ID int64
    Title string
}
// 指定这个结构体操作哪个表
func (Focus) TableName() string {
    // 指定哪个表
    return "focus"
}
```

3. 配置 `Gorm` 数据库连接

> 在 `etc/zerogorm-api.yaml` 中

```yaml
MySQL:
    DataSource: root:123456@tcp(127.0.0.1:3306)/gin?charset=utf8mb4&parseTime=True&loc=Local
```

> 在 `internal/config/config.go` 中

```go
// 这个结构体对应etc目录下的yaml配置项
type Config struct {
    rest.RestConf
    // 定义配置 Mysql
    Mysql struct {
        DataSource string
    }
}
```

4. 创建上下文

> 在 `svc/servicecontext.go` 中

```go
type ServiceContext struct {
    Config config.Config
    DB     *gorm.DB
}

func NewServiceContext(c config.Config) *ServiceContext {
    // 初始化数据库连接
    db, err := gorm.Open(mysql.Open(c.Mysql.DataSource), &gorm.Config{})
    if err != nil {
        // panic("数据库连接出错了")
        logx.Error(err)
    }
    return &ServiceContext{
        Config: c,
        // 在逻辑处理函数中使用DB来操作数据库
        DB: db,
    }
}
```

## rpc -- 远程调用

> go 语言内置了 rpc 功能，以下演示 go 标准库 rpc 的用法

> ==注意：== 标准库 rpc 默认采用 go 语言特有的 gob 编码，没法实现跨语言调用。golang 官方还提供了 net/rpc/jsonrpc 库实现 rpc 方法，jsonrpc 采用 json 进行数据传输，因此支持跨语言调用。但目前的 josnrpc 库是基于 tcp 协议实现的，暂时不支持使用 http 进行数据传输。

1. 服务端

```go
// 定义一个远程调用的方法

type Hello struct {
}

// 方法只能有两个可序列化的参数，其中第二个参数是指针类型的
// req是客户端传入的参数，res是返回给客户端的参数
func (this Hello) SayHello(req string, res *string) error {
    *res = "Hello, " + req
    return nil
}

func main() {
    // 注册一个RPC服务,服务名为"aaa"，类型为Hello
    err1 := rpc.RegisterName("aaa", new(Hello))
    if err1 != nil {
        println(err1)
    }
    // 监听端口
    listener, err2 := net.Listen("tcp", "127.0.0.1:8080")
    if err2 != nil {
        println(err2)
    }
    // 退出时关闭监听端口
    defer listener.Close()

    for { // 不用for循环的话只能处理一个请求，处理完后就退出了
        println("开始建立连接")
        // 建立链接,阻塞等待客户端连接,返回连接对象
        conn, err3 := listener.Accept()
        if err3 != nil {
            println(err3)
        }
        // 绑定服务
        rpc.ServeConn(conn)
    }
}
```

2. 客户端

```go
func main() {
    // 用rpc.Dial连接RPC服务器
    conn, err := rpc.Dial("tcp", "127.0.0.1:8080")
    if err != nil {
        println(err)
    }
    // 退出时关闭连接
    defer conn.Close()
    // 定义一个变量用于接收返回结果
    var reply string
    // 调用远程方法，第一个参数是服务名+方法名，第二个参数是传入的参数，第三个参数是返回的结果
    err2 := conn.Call("aaa.SayHello", "我是客户端", &reply)
    if err2 != nil {
        println(err2)
    }
    println(reply)
}
```

## protobuf 认识与使用

### 1. protobuf 简介

> Protobuf 是一种结构化数据序列化机制，核心优势是高性能、跨平台、可扩展。在 go-zero 中，它用于定义 RPC 服务和消息格式。

### 2. 安装使用

> 好像跟着安装 go-zero 后就不需要安装下面的了

```bash
# 1. 安装 protoc 编译器（必需）
# 访问 https://github.com/protocolbuffers/protobuf/releases 下载对应系统的 protoc

# 2. 安装 Go 插件
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

# 3. 安装 go-zero 的 zRPC 插件
go install github.com/zeromicro/go-zero/tools/goctl@latest
# goctl 已包含 protoc-gen-goctl 插件
```

### 3. protobuf 语法

1. 简单语法演示

> 在一个 `.proto` 文件中

```go
// 声明 proto 语法版本，固定值
syntax = "proto3";

// proto 包名，用于在 .proto 文件之间避免命名冲突
package greet;

// 生成 golang 代码后的包名
// 表示在执行生成命令时所在目录下新建example/proto/greet，在greet中存放生成的go代码
// 例如：在proto中执行 protoc 命令，则生成代码在proto/example/proto/greet
option go_package = "example/proto/greet";


// message定义一种消息类型，关键字message定义结构，并且结构中可以嵌套定义结构体
// 定义请求体
message SayHelloReq {
    // int32是类型符号 id变量名 1是标号
    int32 id = 1;
    string name = 2;
    // 表示string类型的切片
    repeated string hobbies = 3;
}
// 定义响应体
message SayHelloResp {}

// 定义order结构体，使用嵌套定义
message Order {
  int64 id = 1;
  double price = 2;
  string name = 3;
  string tel = 4;
  string address = 5;
  // add_time 变量名生成的是AddTime
  string add_time = 6;
  // 声明要嵌套定义的结构体，也可以定义在外面
  message OrderItem {
    int64 goods_id = 1;
    string title = 2;
    double price = 3;
    int32 num = 4;
  }
  // 声明上面结构体的变量
  OrderItem order_item = 7;
}

// 声明枚举类型，枚举类型是有默认值的
enum PhoneType {
  MOBILE = 0;
  HOME = 1;
  WORK = 2;
}
message Phone{
    // 这有一个默认值，就是枚举的第一个值
    PhoneType phone_type = 1;
}

// 定义 Greet 服务
service Greet {
  // 定义一个 SayHello 一元 rpc 方法，请求体和响应体必填。
  rpc SayHello(SayHelloReq) returns (SayHelloResp);
}
```

> 将上面的 `.proto` 文件编译成 go

```bash
protoc --proto_path
```

> ==在 go-zero 中使用命令生成 rpc==

```bash
# 这是 go-zero 中最常用、最完整的命令
goctl rpc protoc your_service.proto --go_out=. --go-grpc_out=. --zrpc_out=.
```

2. protobuf 类型

> ==标量类型对照表：==
> | .proto | Go 类型 | 默认值 | 说明 |
> | ------ | ------- | ------ | ------------------ |
> | double | float64 | 0.0 |
> | float | float32 | 0.0 |
> | int32 | int32 | 0 | 变长编码，负效率低 |
> | int64 | int64 | 0 | 变长编码 |
> | uint32 | uint32 | 0 | |
> | uint64 | uint64 | 0 | |
> | sint32 | int32 | 0 | 适合负数 |
> | sint64 | int64 | 0 | 适合负数 |
> | bool | bool | false | |
> | string | string | "" | UTF-8 |
> | bytes | []byte | nil | 二进制数据 |

> ==字段规则:==
> | 字段规则 | 说明 | Go 对应类型 |
> | ----------------------------- | --------- | ----------------- |
> | string user_id = 1; | 单值字段 | string |
> | optional string email = 2; | 可选字段 | \*string |
> | repeated string tags = 3; | 数组/切片 | []string |
> | map<string, string> data = 4; | 映射 | map[string]string |

### 4. protobuf 序列化与反序列化

> 假设上面用 proto 文件生成的 message 结构体是 Userinfo
> 下面将 Userinfo 进行序列化

```go
// 定义结构体
u := userinfo.Userinfo{
    Username: "ding",
    Age:      20,
    Hobbies:  []string{"sing", "dance"},
}
// protobuf的序列化，data是序列化后的字节切片
data, _ := proto.Marshal(&u)
// 反序列化
var nu userinfo.Userinfo
err := proto.Unmarshal(data, &nu)
```
