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
     - 1. 直接打印字符会输出对应 ASCII 码
     - 2. byte 型字符(uint8 类型)是指 ASCII 表中一个字符，rune 类型代表一个 utf-8 字符
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

```go
var a = 10
// nb是一个nil
var nb *int
var b *int = &a
// 指向指针的指针
var c **int = &b    // 简写 c := &b
```

> 引用类型使用前需要用`make`或者`new`分配空间

1. 用`new`函数分配内存 --- 初始化的变量为对应类型的默认值
   
   > `new`是一个内置的函数，它的函数签名为：`func new(Type) *Type`
   
   ```go
   var a = new(int) // a是一个指针变量，类型是*int，*a为0（int的默认值）
   ```

2. 用`make`函数分配内存
   
   > `make` 只用于 `slice`、`map`、`channel` 的初始化，返回的还是这三个引用类型本身
   > `new` 用于类型的内存分配，返回的是指向类型的指针

## 结构体

> 结构体是<mark>值类型</mark>

> 结构体中字段名首字母小写的是私有字段，大写的是公有字段

### 用 type 自定义类型

```go
type myInt int

func main() {
    var a myInt = 10
    fmt.Printf("%T", a) // main.myInt
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

> go 语言中，没有类的概念但是可以给类型（结构体、自定义类型）定义方法。所谓方法就是定义了接收者的函数。
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

> 结构体中字段名首字母小写的是私有字段，大写的是公有字段，==私有属性不能被 json 包访问==

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

> go 中的包可以分为三种：
> 
> 1. 系统内置包
> 2. 自定义包
> 3. 第三方包

1. 系统内置包 -- 引入后直接使用
   
   - fmt
   - strconv
   - strings
   - sort
   - errors
   - time
   - encoding/json
   - os
   - io 等

2. 自定义包 -- 开发者自己写的包

3. 第三方包 -- 需要下载到本地才可以使用

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

> ==使用 `any` 类型接收的变量，想要使用操作时需要使用断言恢复原来的类型(与 ts 很像)==

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
switch a.(type) {
    case int:
        fmt.Println("int")
        break
    case Phone:
        fmt.Println("Phone")
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

> `select` 使用类似于 `switch` 语句，它有一系列 `case` 分支和一个默认分支。每个分支对应一个管道的通信(接收或发送)过程。`select` 会一直等待，直到某个 `case` 的通信操作完成时，就会执行 `case` 分支对应的语句。所有管道读取完毕后执行 default。格式如下:

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
ch1 := make(chan int, 5)
for i := 0; i < cap(ch1); i++ {
    ch1 <- i
}
ch2 := make(chan string, 5)
for i := 0; i < cap(ch2); i++ {
    ch2 <- fmt.Sprintf("元素%d", i)
}

for {    // 这是一个死循环

}

for flag := 0; flag == 0; {    // 当flag不为0时跳出循环
    select {
    case v := <-ch1:
        fmt.Println("case 1 打印:", v)
    case v := <-ch2:
        fmt.Println("case 2 打印:", v)
    default:
        fmt.Println("都读取完了，要跳出for循环")
        // return    // 通过retrun来跳出循环,如果代码在一个函数中
        flag = 1
    }
}
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

```go
var mutex sync.RWMutex    // 定义读写锁对象
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

### 读取文件

#### 1. 读取文件方式一

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

> 可以使用 `os.ReadFile()` 读取整个文件，这个方法在 `1.16`之前在`ioutil`这个包下，现在整合到 `os` 包下了

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

#### 2. 读取文件方式二

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

#### 1. 写入文件方式一

> `os.OpenFile()` 方法参数：
> 
> - 第一个参数：是要写入文件地址，可以是绝对地址也可以是相对地址
> - 第二个参数：是写入的模式
> - 第三个参数：`perm` 是文件权限，具体参考 linux 文件权限，这里使用 0666

| 模式          | 说明   |
| ----------- | ---- |
| os.O_WRONLY | 只写   |
| os.O_CREATE | 创建文件 |
| os.O_RDONLY | 只读   |
| os.O_RDWR   | 读写   |
| os.O_TRUNC  | 清空   |
| os.O_APPEND | 追加   |

> 不同模式可以组合，例如 `os.O_CREATE|os.O_WRONLY|os.O_TRUNC`

```go
func osW() {
    // 创建这个文件、只对这个文件写入、清空这个文件
    file, err := os.OpenFile("../test1.txt", os.O_CREATE|os.O_WRONLY|os.O_TRUNC, 0666)
    defer func ()  {    // 关闭文件
        if file.Close() != nil {
            fmt.Println("文件关闭失败!")
        }
    }()
    if err != nil {
        fmt.Println("文件打开失败!")
        return
    }
    // 写入文件，写入字符串
    file.WriteString("这是写入的数据!\n")
    for i := 0; i < 10; i++ {
        file.WriteString(fmt.Sprintf("用for循环写入的数据%d\n", i))
    }
    // 写入文件，写入字符切片
    byteStr := make([]byte, 0 , 1)
    byteStr = append(byteStr, 'a', 'd', 'd')
    file.Write(byteStr)
}
```

> 可以使用 `os.WriteFile()` 写入文件，这个方法在 `1.16`之前在`ioutil`这个包下，现在整合到 `os` 包下了

> 第一个参数是：文件地址
> 第二个参数是：要写入的字节切片
> 第三个参数是：文件权限，参考 Linux 文件权限，这里用 0666

```go
func ioW() {
    str := "golang冲冲冲!!!"
    err := os.WriteFile("../test3.txt", []byte(str), 0666)
    if err != nil {
        fmt.Println(err)
        return
    }
}
```

#### 2. 写入文件方式二

> 使用 `bufio.NewWriter()` 方法写入文件，==这个方法会先写入缓存，注意将缓存冲入到文件中。==

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

### 重命名

> `os.Rename()`

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

## Gin -- Go 网络框架

> Gin 是一个 go 编写的轻量级网络框架。

### Gin 使用

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

### Gin 响应数据

#### String() 响应一个字符串

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

#### JSON() 响应一个 json 数据( go 中的 map 类型)

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

#### JSONP() 用于解决跨域问题 -- 不推荐使用

> 推荐使用 cors 中间件

#### XML() 响应 xml 数据

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

#### HTML() 响应 html 数据

> 需要先导入模板文件

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

==对于 go 模板渲染语法，我跳过了==

### Gin 接收前端传值

#### 1. get 查询参数

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
        username := ctx.Query("username")
        passwd := ctx.Query("passwd")
        // 给查询参数设置默认值，如果没有传page参数，则page=1
        page := ctx.DefaultQuery("page", "1")
        ctx.HTML(200, "display.html", gin.H{
            "data":   username,
            "passwd": passwd,
            "page":   page,
        })
    })
    r.Run() // listen and serve on localhost:8080
}
```

#### 2. 路径参数(动态路由传值)

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

#### 3. post 请求传值

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

#### 将前端提交的数据绑定到结构体上

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

### 路由分组及路由抽离

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

### 自定义控制器 -- 抽离业务逻辑

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

### Gin 中间件

> Gin 允许开发者在处理请求的过程中，加入用户自己的钩子(Hook)函数。这个钩子函数就叫中间件，中间件适合处理一些公共的业务逻辑，比如登录认证、权限校验、数据分页、记录日志、耗时统计等。

> 在最后一个函数之前的都叫中间件，中间件可以有多个，响应处理函数也是一种中间件

#### 局部中间件

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

#### 全局中间件

> `Use`方法可以设置全局中间件

```go
// 定义中间件
func initMiddle(c *gin.Context) {
    fmt.Println("这是一个中间件")
}

// 注册全局中间件，对所有的路由都生效
r.Use(initMiddle, middle2/*可以传入多个*/)
```

#### 中间件之间的通信

> 可以用 `Set` 来设置全局信息让其它中间件（==只能是在同一个 url 路由的中间件来使用==）来使用， `Get` 获取其它中间件设置的信息，抽离到其它文件也能用。

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

#### 在中间件中使用 goroutine

> 在中间件中使用 `goroutine` 时，==不能使用==原始的上下文(`ctx *gin.Context`)，必须使用其只读副本

```go
// 获取只读副本
cCp := ctx.Copy()
```

### 自定义 Model -- 抽取公共代码

> 如果项目比较复杂，可能不同的 Controller 中存在相同的业务逻辑，可以将不同的 Controller 中的相同业务逻辑抽离到 Model，以减少重复代码。

### 文件上传

> go 接收文件上传，使用 `file, err := ctx.FormFile()` 方法接收，使用 `ctx.SaveUploadedFile(file, "目录")` 来保存。

### Gin 中的 Cookie

> Cookie: 是保存在用户设备上的，可以让用户同一个设备访问同一个网站时共享数据。

#### 设置 cookie

> `ctx.SetCookie(name, value string, maxAge int, path, domain string, secure, httpOnly bool)`

- 第一个参数是： cookie 的 key
- 第二个参数是：设置 key 的 value
- 第三个参数是：过期时间。如果只想设置 cookie 的保存路径而不像设置存活时间，可以传入 nil
- 第四个参数是：cookie 的路径
- 第五个参数是：cookie 的路径 Domain 作用域，本地调试配置成 localhost，正式上线配置成域名
- 第六个参数是：secure，当 secure 值为 true 时，cookie 在 http 中是无效的，在 https 中才有效
- 第七个参数是：httpOnly，是微软对 cookie 做的扩展。如果在 cookie 中设置了"httpOnly"属性，则通过程序(js 等)将无法读取到 cookie 信息，防止 xss 攻击

#### 获取 cookie

> `ctx.Cookie("cookie键名")`

#### 删除 cookie

> 与设置 cookie 相同，只是把时间设置为-1 或者把值设置为空字符串

### Gin 中的 Session

> session 是另一种记录客户状态的机制，不同的是 cookie 保存在客服端浏览器中，而 session 保存在服务器上。

#### Session 的工作流程

> 当客户端浏览器第一次访问服务器并发送请求时，服务器端会创建一个 `session` 对象，生成一个类似于 `key`，`value` 的键值对，然后将 `value` 保存到服务器，将 `key(cookie)`返回到浏览器。浏览器下次访问时会携带 `key(cookie)`，找到对应的 `session(value)`

#### Gin 中使用 Session

> Gin 官方没有 session，所以需要使用第三方模块。

### context 上下文

## GORM -- Go 的 orm 框架

> `orm`: `orm` 是通过实例对象的语法，完成关系型数据库的操作的技术，是对象-关系映射 Object/Relational Mapping 的缩写。

> `GORM` 官方支持都数据库类型有：`MySQL`，`PostgreSQL`，`SQLite`，`SQL Server`

### GORM 安装

```shell
# 安装gorm框架
go get -u gorm.io/gorm
# 安装对应数据库驱动
go get -u gorm.io/driver/mysql
```

### 定义数据库模型

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

#### 定义一个完整的表结构的结构体

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

#### 定义一个不完全的表结构的结构体

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

#### 一个自动生成表结构的工具

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

### 初始化数据库

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

### gorm 的 crud

> 下面例子中的数据库 `DB` 放在了 `models` 中 `core.go`

#### 1. 查

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

#### 2. 增

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

#### 3. 改

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

#### 4. 删

```go
// 删除id为2的数据
u := models.Guser{Id: 2}
models.DB.Delete(&u)

// 或者
u := models.Guser{}
models.DB.Where("id = ?", 2).Delete(&u)
```

### gorm 中使用原生 sql 语句

```go
// 没有查询结果的
models.DB.Exec("delete from user where id=?", 5)

// 有查询结果的，把查到的数据存到u中
u := []models.User{}
models.DB.Raw("select * from user").Scan(&u)
```

### grom 多表查询

#### 一对一 关联关系

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

#### 一对多 关联关系

#### 多对多 关联关系

## Go的日志技术

> 这里介绍的官方提供的slog

## Go-Zero --- 微服务框架

> `go-zero` 是一个集成了各种工程实践的 `web` 和 `rpc` 框架。通俗的讲：`go-zero` 既是一个 web 框架也是一个微服务框架。
> 
> `go-zero` 提供了极简的 API 定义和强大的代码生成工具 `goctl`，可以大幅提升开发效率。定义好 API 后通过 `goctl` 可以快速生成工程代码。
> 
> `go-zero` 中还内置了丰富的微服务治理能力，如：限流、熔断、降载、服务发现、负载均衡、链路追踪等。

### 搭建 go-zero 环境

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

### 创建并初始化项目

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

### goctl 常用命令

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

### api 文件语法详解

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

### go-zero 中操作数据库

#### 使用 go-zero 内置的工具 sqlx --- 不推荐

#### 在 go-zero 中使用 gorm

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

### rpc -- 远程调用

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

#### protobuf 认识与使用

##### 1. protobuf 简介

> Protobuf 是一种结构化数据序列化机制，核心优势是高性能、跨平台、可扩展。在 go-zero 中，它用于定义 RPC 服务和消息格式。

##### 2. 安装使用

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

##### 3. protobuf 语法

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

##### 4. protobuf 序列化与反序列化

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
