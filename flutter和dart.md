# Dart

## Dart安装

> 使用下面命令安装

Windows：`choco install dart-sdk`

## 变量

> `dart`所有类型都继承于`Object`，除了`null`

### 变量声明

> `dart` 是强类型的，第一次赋值后类型不能再变了

```dart
// 可以自动推导类型，建议加上类型。
var age = 20
```

### 常量

1. 编译时常量声明 -- const

> 关键字 `const`，`const`表达式中不允许有变量值

> `const` 是编译时确定，不可修改。

```dart
const num = 23.4;
var num1 = 90;
// 下面不合法
// const num2 = num1;
```

2. 运行时常量声明 -- final

> 在 Dart 中，final 修饰符用于声明一个变量，它保证这个变量在初始化后不能再被重新赋值。

> 关键字 `final`，运行时被初始化，其值设置后不可以修改

> 特定：
> 
> - 运行时确定：值在运行时第一次赋值后确定
> - 可以延迟初始化：可以在声明时不初始化，在构造函数或第一次使用时初始化
> - 每个实例可以有不同的值：对于类的实例变量很有用
> - 内存分配：每个实例有自己的 final 变量存储

```dart
// 这个每次运行都是一个
final time = DateTime.now()
```

3. 两者之间的异同
   
   > 1. 赋值时机（最核心区别）
   >    
   >    * **`final`**：在**运行时**第一次被访问时赋值，或者实例化时在构造函数中赋值。
   >    
   >    * **`const`**：在**编译时**就必须确定值。编译器会把 `const` 值直接硬编码到二进制文件中。
   > 
   > 2. 内存与实例化（“常量折叠”）
   >    
   >    * **`final`**：每次执行到该代码行，**可能会创建新对象**（除非编译器优化）。相同内容的两个 `final` 对象在内存中不是同一个实例。
   >    
   >    * **`const`**：具有**常量规范化**（Canonicalization）特性。无论你在代码中写多少次同样的 `const` 值，**Dart 在内存中只会保留一份**，所有引用都指向同一个对象，极大节省内存。
   > 
   > 3. 对象内部的修改权限（可变性）
   >    
   >    * **`final`**：只保证**变量引用不可变**，但**对象内部可以是可变的**。
   >    
   >    * **`const`**：保证**变量引用不可变**，并且**对象内部也是完全不可变**（深度不可变）。

### 类型

#### String 类型

`String 属性名 = "文本内容";`

```dart
// 声明字符串变量
String str = "ddd";
str = "aaa"

// 模板字符串，与js很像
String content = "我要在${DateTime.now()}吃饭";
```

#### 数值类型 -- int/num/double

> int：整型数字
> num：可以整型，可以浮点数
> double：浮点数

```dart
int cc = 3;
double fc = 4.5;
num sdd = 9.2;
```

> double和int不能直接相互赋值，num也不能赋值给double，double可以直接赋值给num，数字类型转换。

```dart
int ic = 8;
double fc = 9.3;
// double转成int
ic = fc.toInt();
// int转成double
fc = ic.toDouble();
// double赋值给num
num nc = 0;
nc = fc;
// num赋值给double要转换
fc = nc.toDouble();
```

#### bool 类型

`bool 变量名 = false;`

```dart
bool fb = false;
```

#### 动态类型 -- dynamic

> Dart中，dynamic用来声明动态类型
> 允许变量运行时自由改变类型，同时绕过编译时的静态检查
> 
> 定义：`dynamic 变量名 = 值;`

```dart
dynamic free = "sd";
free = 23;
```

> `dynamic`:运行时可自由改变类型，无编译检查，方法和属性直接调用
> `var`:根据初始值进行推断类型，确定类型后类型确定，有编译检查，仅限推断的属性和方法

#### 列表类型 -- List

> 可以存储多个内容，可以存储不同类型的元素

```dart
List students = ["a", 2];
```

##### 列表操作

1. 添加
- `add(内容)`：在尾部添加

- `addAll(列表)`：在尾部添加一个列表
  
  ```dart
  // 在尾部添加
  List students = ["a", 2];
  students.add("fv");
  
  //在尾部添加一个列表
  students.addAll(["b", "c", false, 3, 4]);
  ```
2. 删除
- `remove(内容)`：删除满足内容的第一个
- `removeLast()`：删除最后一个，不需要参数
- `removeRange(start, end)`：删除索引范围内数据

```dart
List students = ["a", 2, "b", "b", "c", 2.3];
// 删除第一个b
students.remove("b");
students.removeLast();
```

3. 列表遍历

> 遍历：`forEach((item){});`

```dart
List students = ["a", 2, "b", "b", "c", 2.3];
students.forEach((element) => print(element));
```

> 是否满足条件：`every((item){return 布尔值})`

```dart
// 返回布尔值，如果不是所有元素都符合条件，就返回false
students.every((element) {
  return element == 2.3;
})
```

4. 筛选列表

> 筛选列表满足条件的数据：`where((item) {return 布尔值});`

```dart
// 返回一个迭代器，符合条件的元素进入新列表，用toList方法将迭代器转成列表
students.where((item) {
  return item == "b";
}).toList()
```

5. 列表常用属性
- 长度属性：`length`
- 最后一个元素：`last`
- 第一个元素：`first`
- 是否为空：`isEmpty`

```dart
List students = ["a", 2, "b", "b", "c", 2.3];
print({students.length, students.last, students.first, students.isEmpty});
```

#### 字典类型 -- Map

> 定义：`Map 变量名 = {key: value};`

```dart
Map tmap = {"aa": 23};
// 获取值
tmap["aa"];
// 设置值
tmap["aa"] = "sd";
// 指定键值的类型
Map<String, int> mmm = {"a": 1};
```

##### 字典的遍历

> 使用 `forEach`

```dart
Map<String, int> m = {"a": 1};
m.forEach((key, value) {
  print({key, value});
});
```

##### 再添加一个字典

> 使用方法`addAll`

```dart
Map<String, int> m = {"a": 1};
m.addAll({"cc": 34});
```

##### 是否包含某个key

> `containsKey`，返回`bool`值

```dart
Map<String, int> m = {"a": 1};
m.containsKey("cc")
```

##### 移除键值对

> `remove(key)`，根据key删除对应键值对

```dart
m.remove("cc");
```

##### 清空字典

> `clear`，

```dart
m.clear();
```

## Dart中的空安全机制

> 定义：在Dart语言中，通过编译静态检查将运行时空指针提前暴露。
> 特点：将空指针异常从运行时提前至写代码时

> 常用空安全操作符

<img src="./pic/dart/屏幕截图 2026-01-17 154045.png">

```dart
// 不能设置为null，下面不合法
//String aa = null;

// 可以设置为null
String? username = null;
username = "jk";
// username为null时跳过操作
username?.startsWith("j");

//开发者认为变量非空，编译不检查，如果为空可能会导致异常
username!.startsWith("j");

// 左侧为null时返回右侧默认值，username可能为null
String n = username ?? "as";
```

## Dart运算符

### 算术运算符

<img src="./pic/dart/屏幕截图 2026-01-17 155235.png">

### 赋值运算符

<img src="./pic/dart/屏幕截图 2026-01-17 155411.png">

### 比较运算符

<img src="./pic/dart/屏幕截图 2026-01-17 155550.png">

### 逻辑运算符

<img src="./pic/dart/屏幕截图 2026-01-17 155644.png">

## Dart流程控制

### if分支

> 与js一样

### 三元运算符

> 与js一样

### switch/case

> 与js一样

### while循环

> 与js一样

### for循环

> 与js一样

## 函数

### 函数定义

> 返回类型 函数名(形参类型 形参) {
>    // 函数体
> }

例如：

```dart
// 有返回值
int add(int a, int b) {
    return a + b;
}

// 无返回值
void test() {
    print("a");
}

// 省略返回值类型，返回值类型推断为dynamic，不推荐
getValue() {
    return 1;
}
```

### 函数的参数

> 分类：分为必传参数、可选参数、
> 特点：必传参数不能为空

> <mark>可选位置参数</mark>: `函数名(String a, [String? b])`

```dart
// 可选位置参数
String combine(String a, [String? b, String? c, int? d]) {
    // 可选参数可能为空
  return a + (b ?? "") + (c ?? "") + (d ?? 0).toString();
}
// 可选位置参数，但是有默认值
String combine2(String a, [String? b = "b", String? c = "c", int? d = 0]) {
  return a + (b ?? "") + (c ?? "") + (d ?? 0).toString();
}
void main() {
  print(combine("a"));  // a0
  print(combine2("q", "s", "dd")); // qsdd0
}
```

> <mark>可选命名参数</mark>: `(String a, {String? b, ...})`, 传递时按照`参数名:值` 的方式进行传递

```dart
void showPerson(String uname, {int? age, String? gender}) {
  print("uname: $uname, age: $age, gender: $gender");
}
showPerson("ding", age: 26, gender: "male");
```

### 匿名函数和箭头函数

> 匿名函数语法：`Function 变量名 = () {}`

```dart
void main() {
  // 声明匿名函数类型的变量
  Function test = (int el) {
    return el * 2;
  };
  print(test(2));
}

// 匿名函数类型的参数
void onTest(Function callback) {
  callback();
}
onTest(() => print("dd"));
```

> 当函数体只有一行代码时可以用箭头函数
> 箭头函数语法：`函数名() => 代码逻辑`
> 注意：使用箭头函数可以省略`return`关键字和分号

```dart
// 函数体只能由一行，省略return和分号
Function t = (int a, int b) => a + b;
```

## 类

### 定义类

> 非空类型的属性必须要初始化
> 给类的非空类型属性添加`late`修饰符可以不用初始化，表示之后会初始化但这里展示不初始化。

```dart
class Person {
  int age = -1;
  String uname = "default";
  String gender = "";
  // 构造函数
  Person({int? age, String? uname, String? gender}) {
    this.age = age ?? -1;
  }
  void study() {
    print("${this.uname} study");
  }
}

// 声明可空属性
class Student {
  int? age;
  String? uname;
  String? gender;
  // 构造函数
  Student(int? age, String? uname, String? gender) {
    this.age = age;
    this.uname = uname;
    this.gender = gender;
  }
}

// 初始化类，下面两个方法都可以
Person p1 = new Person();
Person p2 = Person();
```

#### 构造函数

1. 默认构造函数

```dart
class Student {
  int? age;
  String? uname;
  String? gender;
  // 默认构造函数
  Student(int? age, String? uname, String? gender) {
    this.age = age;
    this.uname = uname;
    this.gender = gender;
  }
}
// 实例化
Student p = new Student(age: 25, uname: "ss");
```

2. 命名构造函数

> 定义：构造函数可以采用命名的方式，返回一个实例化对象，一个类可以有多个命名构造函数

> 定义语法：

```dart
class 类名 {
    类名.构造函数名(可选命名参数) {
    }

    类名.构造函数名2(可选命名参数) {
        // ...
    }
 }
// 实例化
Pseron p = Person.构造函数名(属性名:值);
```

```dart
class Person {
  String? uname;
  int? age;
  // 命名构造函数
  Person.createPerson({String? uname, int? age}) {
    this.uname = uname;
    this.age = age;
  }
}
// 实例化
Person p = Person.createPerson(uname: "ding", age: 25);
```

3. 构造函数语法糖

> 这种语法糖，有点像js的对象赋值

> 语法：

```dart
class 类名 {
    类名({this.属性1, this.属性2});
    // 或
    类名.构造函数名({this.属性1, this.属性2});
}
```

```dart
class Student {
  String? uname;
  int? age;
  Student({this.age, this.uname});
}
// 或
class Student {
  String? uname;
  int? age;
  Student.createPerson({this.age, this.uname});
}

// 实例化与正常的构造函数一样
```

#### 对象的比较

> 在 Dart 中，对象比较主要分为两种情况：**值比较**和**引用比较**。

**对于普通类的默认行为**

> 自己创建的普通类，如果没有重写 `==` 运算符，比较的是**引用**（即内存地址）。

```dart
class Person {
  final String name;
  Person(this.name);
}

void main() {
  var p1 = Person('Alice');
  var p2 = Person('Alice');

  print(identical(p1, p2)); // false，因为是两个不同的实例
  print(p1 == p2);          // false，因为默认也是比较引用
}
```

**实现值比较（重写 ==）**

> 要让自定义类的实例能进行值比较，你需要重写 `==` 运算符，并同时重写 `hashCode`。

```dart
class Person {
  final String name;
  Person(this.name);

  @override
  bool operator ==(Object other) {
    // 1. 检查是否为同一个对象（优化）
    if (identical(this, other)) return true;
    // 2. 检查类型是否相同
    if (other is! Person) return false;
    // 3. 比较关键属性
    return other.name == name;
  }

  @override
  int get hashCode => name.hashCode; // 必须与 == 保持一致
}

void main() {
  var p1 = Person('Alice');
  var p2 = Person('Alice');

  print(p1 == p2); // true，因为重写了 ==，比较的是 name 属性
}
```

**identical 函数**

> **`identical(a, b)`**：这是 Dart 中最严格的比较。它检查两个对象是否是**同一个实例**，即它们在内存中的地址是否相同。与`==`的默认行为一致。

### 公有属性和私有属性

> 语法：私有属性以下划线开头，如：`_name` ，其余均为公有属性。

> <mark>私有属性不能用构造函数语法糖进行初始化</mark>

### 类的继承

> Dart中类的继承属于单继承，一个类只能拥有一个直接父类，子类拥有父类所有的属性和方法。

> 语法：`class 类名 extends 父类`
> 重写：子类可以通过 `@override` 注解重写父类方法，扩展其行为。
> 注意：子类不会继承父类构造函数，子类必须通过`super`关键字调用父类构造函数确保父类正确初始化
> super语法：`子类构造函数(可选命名参数):super({参数})`

```dart
// 父类
class Parent {
  String? uname;
  int? age;
  Parent({this.uname, this.age});
  void study() {
    print("${uname}在学习");
  }
}
// 子类
class Child extends Parent {
  String? gender;
  Child(String? name, int? age, {this.gender}) : super(uname: name, age: age);
  @override
  void study() {
    // super.study(); // 调用父类方法
    print("child");
  }
}

void main() {
    // 子类实例化
  Child child = Child("ding", 20, gender: "male");
  print({child.gender, child.age, child.uname});
}
```

### 类的多态

> 可以用父类变量来接收子类实例化的对象

```dart
abstract class PayBase {
  void pay();
}

class WxPay extends PayBase {
  @override
  void pay() {
    print("WxPay");
  }
}

class AliPay extends PayBase {
  @override
  void pay() {
    print("AliPay");
  }
}

PayBase wx = WxPay();
wx.pay();
PayBase ali = AliPay();
ali.pay();
```

#### 抽象类和接口实现

> 使用 `abstract` 关键字声明一个抽象类
> 使用 `implements` 关键字继承并实现抽象类

<mark>代码参考上面一个代码块</mark>

### 类的混入

> Dart允许在不使用传统继承的情况下，向类中添加新的功能

步骤：

- 使用 `mixin` 关键字定义一个对象
- 使用 `with` 关键字将定义的对象混入到当前对象 

特点：

- 一个类支持 `with` 多个 `mixin`，调用优先级遵循后来居上原则，即后混入的会覆盖先混入的同名方法。

```dart
// 定义混入类
mixin Base {
  void song(String name) {
    print("$name在唱歌");
  }
}

// 将Base混入Student
class Student with Base {
  String? sname;
  int? age;
  Student({this.sname, this.age});
}

class Teacher with Base {
  String? tname;
  int? age;
  Teacher({this.tname, this.age});
}

void main() {
  Student student = new Student(sname: "ding", age: 20);
  student.song(student.sname ?? "default");
}
```

## 泛型

1. 列表泛型

```dart
List<String> list = ["a", "b"];
```

2. 函数泛型

```dart
T getValue<T>(T value) {
  return value;
}

void setValue<T>(T value) {
  print(value);
}

// 调用
String str = getValue<String>("a");
```

3. 类中的泛型

```dart
class Person<T> {
  T? pname;
  K getValue<K>(K value) {
    return value;
  }
}
```

## 异步

> Dart是单线程语言，即同时只能做一件事，遇到耗时任务就会造成程序阻塞，此时需要异步编程。
> 
> Dart采用单线程+事件循环机制

> 事件循环：

<img src="./pic/dart/屏幕截图 2026-01-17 224255.png">

> 微任务队列：`Future.microtask()`
> 事件队列：`Future`、`Future.delayed()`、`I/O操作`等

### Future

> `Future` 代表一个异步操作的最终结果
> 状态：`Uncompleted(等待)`、`Completed with a value(成功)`、`Completed with a error(失败)`
> 创建：`Future((){})`
> 执行成功：不抛出异常-成功状态-`then((){})`
> 执行失败：抛出异常-失败状态-`catchError((){})`

```dart
Future f = Future(() {
  return "future";
});
// value是在Future中传入匿名函数的返回值
f
    .then((value) {
      print(value);
    })
    .catchError((err) {
      print(err);
    });
```

> 链式调用

<img src="./pic/dart/屏幕截图 2026-01-17 230006.png">

#### async/await

特点：

- `await` 总是等到后面的 `Future` 执行成功，才执行下方逻辑，`async` 必须配套`await` 出现

### stream --- 多次的异步结果

> `Future`是一次性的异步结果，而`Stream`是多次的异步结果。
> 
> `Stream`的特点：
> 
> - **异步性**：`Stream`中的数据是异步产生和消费的
> 
> - **惰性**: `Stream` 只有在被监听时才开始产生数据
> 
> - **可组合**: 可以通过各种操作符组合和变换 `Stream`
> 
> - **背压处理**: 可以处理生产者和消费者速度不匹配的情况
> 
> - **发布订阅式**：
> 
> 单订阅流 和 广播流：
> 
> - 单订阅流：
>   
>   - 只能被订阅一次，事件会缓冲
> 
> - 广播流：
>   
>   - 可被多个订阅者同时监听
> 
> <mark>注意</mark>：流在不使用的时候应该被关闭

#### 创建Stream

```dart
// 从迭代器中创建
Stream<int> s1 = Stream<int>.fromIterable([1, 2, 3, 4]);
// 从Future中创建
Stream<int> s2 = Stream<int>.fromFuture(Future.value(42))；
// 从周期性中创建
Stream<int> s3 = Stream.periodic(const Duration(seconds: 1), (t) => t);

// 使用 async* 和 yield（最常用）
// async* 表示一个Stream，yield表示发出一个数据
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // 每 1 秒产出一个
  }
}

// 使用 StreamController 手动控制
StreamController<int> controller = StreamController<int>();
  // 生成stream流，并监听
  controller.stream.listen(
    (data) {
      print('controller data: $data');
    },
    onDone: () => print('结束'),
    onError: (err) => print(err),
  );
  // stream中添加数据
  controller.add(1);
  controller.add(10);
  // 向stream中写入err数据
  controller.addError('err');
  // 关闭流
  controller.close();r.close();
```

#### 订阅消费Stream

```dart
// await for 顺序处理
// await for会阻塞当前程序执行
var s1 = Stream.periodic(const Duration(seconds: 1), (t) => t);
await for (var v in s1) {
  print('await for: $v');
}
// listen消费
final sub = countStream(3).listen(
  (value) => print('数据: $value'),
  onError: (e, st) => print('错误: $e'),
  onDone: () => print('完成'),
  cancelOnError: false, // 出错是否自动取消订阅
);

// 取消订阅
await sub.cancel();
```

#### 链式处理Stream

```dart
var s1 = Stream.periodic(const Duration(seconds: 1), (t) => t);
final subscription = s1
    .where((n) => n % 2 == 0)
    .map((n) => n * 10)
    .listen((value) => print('value: $value'));
// 关闭流
Future.delayed(Duration(seconds: 20), () => subscription.cancel());
```

#### 广播流

```dart
final bc = StreamController<int>.broadcast();

bc.stream.listen((v) => print('A: $v'));
bc.stream.listen((v) => print('B: $v'));

bc.add(1); // A: 1  B: 1
bc.add(2); // A: 2  B: 2
```



# Flutter

## 配置Flutter环境

1. 下载Flutter SDK
2. 配置环境变量
3. 诊断flutter环境

## 创建Flutter项目

> 使用命令创建 `Flutter` 工程

```bash
# --platforms 后面是平台名称
flutter create --platforms web 项目名
```

## Flutter目录结构

<img src="./pic/dart/屏幕截图 2026-01-18 101929.png">

## runApp和Widget

- `runApp` 函数是Flutter内部提供的一个函数，启动一个Flutter应用。
- `Widget` 表示控件、组件、部件的含义，`Flutter` 中万物皆`Widget`

<img src="./pic/dart/屏幕截图 2026-01-18 102857.png">

## Flutter的默认Material库

- `Material` 是google公司推行的一套设计风格，有很多的设计规范，如颜色、文字排版、动画等。
- 目的：`Material`为Android、Web、iOS、HarmonyOS多个平台提供统一的交互和视觉体验。

## 组件

### 基础组件 --- MaterialApp

> 特性：整个应用被`MaterialApp`包裹，方便我们对整个应用的属性进行整体设计。

常见属性：

- title：用来展示窗口的标题内容（可以不设置）
- theme：用来设置整个应用的主题
- home：用来展示窗口的主体内容

```dart
void main() {
  runApp(
    MaterialApp(
      title: "flutter组件初体验",
      theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
      home: Scaffold(),
    ),
  );
}
```

### 基础组件 --- Scaffold组件

> `Scaffold` 组件：用于构建 `Material Design` 风格页面的核心布局组件，提供标准、灵活配置的页面骨架。

<img src="./pic/dart/屏幕截图 2026-01-18 105421.png">

案例:

```dart
void main() {
  runApp(
    MaterialApp(
      title: "Scaffold组件初体验",
      theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
      home: Scaffold(
        appBar: AppBar(title: Text("头部区域")),
        body: Container(child: Center(child: Text("中部区域"))),
        bottomNavigationBar: Container(
          height: 80,
          color: Colors.amber,
          child: Center(child: Text("底部区域")),
        ),
      ),
    ),
  );
}
```

案例总结：

- `MaterialApp`包裹整个应用形成统一的`Material Design`风格，相当于`html`中的`body`
- `Scoffold`组件可快速搭建页面骨架，如`appBar`、`body`、`bottomNavigationBar`等。
- `Container`用来作为容器，设置高度(`height`)，`child`用来存放子组件
- `Text`是用来显示文本的组件

### 布局组件

> <mark>布局约束：</mark>在flutter中，上层 widget 向下层 widget 传递约束条件；  然后，下层 widget 向上层 widget 传递大小信息。  最后，上层 widget 决定下层 widget 的位置。

#### 布局组件介绍

| 组件类别 | 核心组件                           | 主要特点/使用场景                           |
| ---- | ------------------------------ | ----------------------------------- |
| 基础容器 | Container、Center、Align、Padding | 提供装饰、对齐、边距等基础样式和布局控制，是使用频率极高的组件     |
| 线性布局 | Row、Column                     | 在水平或垂直方向线性排列子组件，是构建界面的基础            |
| 弹性布局 | Flex、Expanded、Flexible         | 按照比例分配剩余空间，实现自适应布局，常与Row和Column配合使用 |
| 层叠布局 | Stack、Positioned               | 让子组件重叠堆叠，用于实现如图片上叠加文字、悬浮按钮等效果       |
| 流式布局 | Wrap、Flow                      | 当主轴空间不足时自动换行或换列，常用于标签、滤镜等动态宽高内容的排列  |
| 滚动布局 | ListView、GridView              | 提供可滚动的列表或网格视图，高效展示大量数据              |

#### 基础容器 --- Container

> 定义：Container是功能丰富的布局组件，是一个多功能组合容器
> 
> 尺寸控制：可通过多种方式定义大小，有明确优先级规则
> 
> 优先级：明确宽高 > constraints约束（最大宽高）> 父组件约束 > 自适应组件大小
> 
> 装饰系统：通过decoration属性实现视觉效果，但和color属性互斥
> 
> 布局控制：提供内外边距和对齐方式
> 
> 可选变化：支持绘制时进行矩阵变换，如旋转、倾斜、平移等

<mark>常见属性</mark>
<img src="./pic/dart/屏幕截图 2026-01-18 204209.png">

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Container组件",
      home: Scaffold(
        body: Container(
          // 设置容器内child对齐方式
          alignment: Alignment.center,
          // 旋转
          transform: Matrix4.rotationZ(0.05),
          // 设置外边距，左和上外边距
          margin: EdgeInsets.only(left: 20, top: 20),
          width: 200,
          height: 200,
          // color: Colors.blue, // 设置简单的背景颜色
          // 这个不仅能设置颜色，也能设置其它属性，color属性会冲突报错
          decoration: BoxDecoration(
            color: Colors.blue,
            // 设置圆角
            borderRadius: BorderRadius.circular(15),
            // 设置边框
            border: Border.all(width: 3, color: Colors.yellow),
          ),
          // 子组件
          child: Text(
            "hello Container组件",
            // 设置文本样式
            style: TextStyle(color: Colors.amber, fontSize: 20),
          ),
        ),
      ),
    );
  }
}
```

#### 基础容器 --- SizedBox

> 功能与Container相似，但是SizedBox没有装饰功能。SizedBox性能优于Container

#### 基础容器 --- Center 居中组件

> Center：将其子组件在父容器的空间进行`水平和垂直方向上的居中`排列
> 
> 实现固定宽高且居中的组件：用Center包裹
> 
> <mark>注意事项：</mark>Center不能设置宽高，Center的最终大小取决于其父组件传递给它的约束，center会向它的父组件申请尽可能大的空间。

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        // 会在body中居中显示
        body: Center(
          child: Container(
            alignment: Alignment.center,
            width: 100,
            height: 100,
            decoration: BoxDecoration(color: Colors.blue),
            child: Text("center"),
          ),
        ),
      ),
    );
  }
}
```

#### 基础容器 --- Align 对齐组件

> 作用：精确控制其子组件在父容器空间内的对齐位置
> 
> `alignment(对齐方式)`：子组件在父容器内的对齐方式
> 
> `widthFactor(宽度因子)`：`Align的宽度` 将是子组件宽度乘以该因子
> 
> `heightFactor(高度因子)`：`Align的高度` 将是子组件高度乘以该因子

> <mark>使用场景：</mark>当需要将一个组件放置在父容器的特定角落时，Align是理想选择。

```dart
void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Container(
          color: Colors.blue,
          // Align组件是特殊的center组件，Align组件没有decoration属性，不能设置背景，可以给父容器设置decoration属性
          child: Align(
            widthFactor: 3,
            heightFactor: 8,
            child: Icon(Icons.star, size: 50, color: Colors.amberAccent),
          ),
        ),
      ),
    );
  }
}
```

#### 基础容器 --- Padding 内边距组件

> 作用：为其子组件添加`内边距`

| 属性      | 类型                 | 作用说明                                |
| ------- | ------------------ | ----------------------------------- |
| padding | EdgeInsetsGeometry | 必需。定义内边距的大小和方向，通常使用`EdgeInsets`类来设置 |
| child   | Widget             | 需要被添加内边距的子组件                        |

> `EdgeInsets`的属性如下：

| 属性        | 作用                  | 示例代码                                                    |
| --------- | ------------------- | ------------------------------------------------------- |
| all       | 设置所有方向的空白           | `EdgeInsets.all(8.0)`                                   |
| symmetric | 设置对称方向的空白，包括垂直和水平方向 | `EdgeInsets.symmetric(vertical: 10.0, horizontal: 5.0)` |
| fromLTRB  | 分别设置左、上、右、下四个方向的空白  | `EdgeInsets.fromLTRB(5.0, 10.0, 15.0, 20.0)`            |
| only      | 设置某个方向的空白，其他方向默认为 0 | `EdgeInsets.only(left: 10.0)`                           |

```dart
void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("padding代码实例")),
        body: Container(
          color: Colors.blue,
          child: Padding(
            padding: EdgeInsets.all(10),
            // 占据除了内边距的整个Padding
            child: Container(color: Colors.amber),
          ),
        ),
      ),
    );
  }
}
```

#### 线性布局 --- Column

> 作用：用于垂直排列其子组件的核心布局容器，`Column`继承于`Flex`

> <mark>注意：</mark> `Column`没有宽高，如果没有给父元素设置宽高的话，会默认撑满整个父元素高度，宽度是其子元素最大的宽度。`Column`在宽度上是收缩，如果设置了父元素宽高，会撑满整个父元素。

**常见属性**<img src="./pic/dart/屏幕截图 2026-01-19 102129.png">

**主轴排列方式**

<img src="./pic/dart/屏幕截图 2026-01-19 102254.png">

```dart
// 省略主函数，column使用案例
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("column代码实例")),
        body: Container(
          width: 100,
          height: 600,
          color: Colors.blue,
          child: Column(
            // 设置主轴对齐方式
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [Text("data1"), Text("data2"), Text("data3")],
          ),
        ),
      ),
    );
  }
}
```

**交叉轴排列方式**

<img src="./pic/dart/屏幕截图 2026-01-19 103844.png">

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("column代码实例")),
        body: Container(
          width: 100,
          height: 600,
          color: Colors.blue,
          child: Column(
            // 设置主轴对齐方式
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            // 设置交叉轴对齐方式
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [Text("data1"), Text("data2"), Text("data3")],
          ),
        ),
      ),
    );
  }
}
```

#### 线性布局 --- Row

> 作用：用于水平排列其子组件的核心布局容器，`Row`继承于`Flex`

> <mark>注意：</mark>  `Row`本身不支持滚动，如果内容超出，需要使用`ListView`或者`SingleChildScrollView`包裹，需要<mark>父元素明确告知宽度的尺寸约束</mark>，`Row`默认情况下不会给父组件的大小直接影响Row的最终大小和子组件的布局行为。

<img src="./pic/dart/屏幕截图 2026-01-19 104618.png">

**主轴排列方式**

<img src="./pic/dart/屏幕截图 2026-01-19 104717.png">

**交叉轴**

<img src="./pic/dart/屏幕截图 2026-01-19 105335.png">

**使用与column基本一样**

#### 弹性布局 --- Flex

> 作用：允许`沿一个主轴（水平或垂直）`排列其子组件，灵活的控制这些子组件在主轴上的`尺寸比例`和`空间分配`。
> 
> `Flex`很像`Column`和`Row`的结合体，`Column`和`Row`继承于`Flex`

**常用属性**

<img src="./pic/dart/屏幕截图 2026-01-19 110010.png">

> 子组件：`Flex`的子组件常使用`Expanded`或`Flexible`来控制空间分配。
> 
> `Expanded/Flexible`作为`Flex`的子组件通过`Flex`属性来分配`Flex`组件空间

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Center(child: Text("flex布局"))),
        body: Container(
          width: double.maxFinite,
          height: double.maxFinite,
          decoration: BoxDecoration(color: Colors.amber),
          child: Flex(
            // 设置主轴方向
            direction: Axis.horizontal,
            children: [
              Expanded(
                // 设置占比
                // 这个案例中有两个expanded，flex分别是10和2，所以一个占10/(10+2) 另一个是2/(10+2)
                flex: 10,
                // 这里的宽度受flex，可以不用设置
                child: Container(color: Colors.red, height: 200),
              ),
              Expanded(
                flex: 5,
                child: Container(color: Colors.green, height: 200, width: 100),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

> flex布局受父组件的约束影响。确保父组件提供了适当的布局约束
> expanded与flexible的区别：expanded强制子组件填满所以剩余空间，flexible根据自身大小调整，不强制占满空间。

##### 弹性布局容器 ---- Expanded（强制占用）

> `Expanded` 弹性布局组件只能用于`Flex`及其继承者（`Column`和`Row`）的内部。
> 
> 作用：`Expanded`的子组件会**强制占满**主轴上的剩余空间

```dart
class _TitleBarState extends State<TitleBar> {
  bool _is_window_max = false;
  @override
  Widget build(BuildContext context) {
    return Container(
      height: 36,
      color: Colors.blueGrey,
      child: Row(
        // mainAxisAlignment: MainAxisAlignment.,
        children: [
          Text(
            "left AAA",
            style: TextStyle(
              fontSize: 18,
              color: Colors.white,
              decoration: TextDecoration.none,
            ),
          ),
          // 占位的区域，会填充Text和Row之间的所有空间
          Expanded(
            child: Container(
              height: double.infinity,
              color: Colors.amberAccent,
            ),
          ),
          // 三个窗口控制按钮
          Row(
            children: [
              GestureDetector(
                onTap: () {
                  WindowUtil.setMinimize();
                },
                child: SvgPicture.asset(
                  "assets/icons/generic_minimize.svg",
                  width: 36,
                  height: 36,
                  color: Colors.white,
                ),
              ),
              //...
            ],
          ),
        ],
      ),
    );
  }
}
```

##### 弹性布局容器 ---- Flexible（可选占用）

> `Flexible` 弹性布局组件只能用于`Flex`及其继承者（`Column`和`Row`）的内部。
> 
> 作用：`Flexible`子组件可以占满剩余空间，也可以不占

```dart
Row(
  children: [
    Container(width: 80, color: Colors.red),
    Flexible(
      fit: FlexFit.loose,  // 不强制占满（默认）
      child: Container(color: Colors.blue),
    ),
  ],
)
```



#### 流式布局 --- Wrap

> 作用：流式布局组件，当子组件在主轴方向上排列不下时，它会自动换行（或换列）

**常用属性**

<img src="./pic/dart/屏幕截图 2026-01-19 130606.png">

> <mark>注意：</mark> `Column`/`Row`/`Flex`内容超出均不会换行。
> 
> `Wrap`组件更像是`Flex组件加了换行特性`

> <mark>使用场景：</mark>
> 
> - 当子组件内容是根据数据动态生成时，使用Wrap可以确保布局始终适配。
>   <img src="./pic/dart/屏幕截图 2026-01-19 131247.png" style="height:100px; width:100px">

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  // 生成随机个数的container
  List<Widget> getList() {
    return List.generate(Random().nextInt(20), (int index) {
      return Container(
        width: 100,
        height: 100,
        // margin: EdgeInsets.all(10),
        color: Colors.blue,
      );
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("flex代码实例")),
        body: Container(
          width: double.infinity,
          height: double.infinity,
          color: Colors.amber,
          child: Wrap(
            direction: Axis.horizontal,
            spacing: 10,
            runSpacing: 10,
            children: getList(),
          ),
        ),
      ),
    );
  }
}
```

#### 层叠布局 --- Stack/Positioned

> 作用：层叠布局组件，允许你将多个子组件按照Z轴（深度）进行叠加排列。

**Stack 常用属性**

<img src="./pic/dart/屏幕截图 2026-01-19 133812.png">

> 搭档：`Positioned`组件是`Stack`的黄金搭档，对子组件进行精确定位控制。`Positioned`必须作为`Stack`的直接子组件。`Positioned`通过`left`、`right`、`top`、`bottom`属性来将子组件“钉”在`Stack`的某个角落或边缘。

> <mark>注意：</mark> `Positioned`的位置定位，是相对于`Stack`父元素的边界，`Positioned`定位的优先级比子元素的宽高优先级高，先满足定位需求。

**没有positioned的案例**

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("Stack/Positioned代码实例")),
        body: Stack(
          // stack不能设置宽高
          // 设置对齐方式（后面一个容器堆叠在前一个容器的位置），这里是居中
          alignment: Alignment.center,
          children: [
            Container(width: 200, height: 200, color: Colors.blue),
            // 这个会堆叠在上面的容器上，后面的会堆叠在前面上。
            Container(width: 100, height: 100, color: Colors.red),
          ],
        ),
      ),
    );
  }
}
```

**有positioned的案例**

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("Stack/Positioned代码实例")),
        body: Container(
          width: double.infinity,
          height: double.infinity,
          color: Colors.grey,
          child: Stack(
            children: [
              Container(width: 200, height: 200, color: Colors.blue),
              Positioned(
                left: 10,
                top: 20,
                child: Container(width: 100, height: 100, color: Colors.red),
              ),
              Positioned(
                // positioned的位置定位，是相对于stack父元素的边界
                right: 10,
                bottom: 10,
                child: Container(width: 100, height: 100, color: Colors.amber),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

#### 裁剪组件 --- Clip



#### 文本组件 --- Text

> 作用：在用户界面中显示文本的基础组件。

**常用属性**

<img src="./pic/dart/屏幕截图 2026-01-19 150325.png">

```dart
// 基本功能演示
Text(
  "Stack/Positioned代码实例。。。。。。。。。Stack/Positio例。。。",
  style: TextStyle(
    // 设置颜色
    color: Colors.blue,
    // 加粗
    fontWeight: FontWeight.w800,
    // 下划线
    decoration: TextDecoration.underline,
    // 文本样式 -- 斜体
    fontStyle: FontStyle.italic,
  ),
)

// 设置超出显示省略号
Text(
  "Stack/Positioned代码实例,Stack/Positioned代码实例,Stack/Positioned代码实例,Stack/Positioned代码实例",
  style: TextStyle(color: Colors.blue, fontSize: 20),
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
),
```

==文本组件 --- Text/TextSpan==

> 需要在同一段文本中显示不同样式，可以用`Text.rich`构造函数配合`TextSpan`来实现

```dart
Text.rich(
  TextSpan(
    text: "hello ",
    style: TextStyle(color: Colors.red),
    children: [
      TextSpan(
        text: "flutter ",
        style: TextStyle(color: Colors.green),
      ),
      TextSpan(
        text: "!",
        style: TextStyle(color: Colors.red),
      ),
    ],
  ),
),
```

> <mark>注意事项：</mark> `Text`组件本身和其`TextStyle`中都有可能有`ovreflow`等属性，`Text`组件属性优先级更高，假如文本过长最好设置`maxLines`和`overflow`。大量重复使用的文本样式，建议统一定义，有助于保持一致性并提升性能。

#### 图片组件 --- Image

> 作用：在用户界面中显示图片的核心部件

**图片分类**

<img src="./pic/dart/屏幕截图 2026-01-19 155910.png">

**常见属性**

<img src="./pic/dart/屏幕截图 2026-01-19 160042.png">

```dart
// 在yaml中配置
assets:
  - lib/images/
//使用
Image.asset("lib/images/R.jpg")
```

### 表单组件

#### 文本输入组件 --- TextField

> 作用：实现文本输入功能的核心组件

> <mark>注意：</mark> 使用`TextField`必须使用`有状态组件`

<img src="./pic/dart/屏幕截图 2026-01-19 171903.png">

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  final TextEditingController _accountController = TextEditingController();
  final TextEditingController _passwdController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        body: Container(
          // color: Colors.amber,
          padding: EdgeInsets.all(10),
          child: Column(
            children: [
              TextField(
                // 控制器
                controller: _accountController,
                decoration: InputDecoration(
                  contentPadding: EdgeInsets.only(left: 20),
                  filled: true,
                  fillColor: Colors.white,
                  hintText: "请输入账号",
                  hoverColor: Colors.white,
                  border: OutlineInputBorder(
                    borderSide: BorderSide.none,
                    borderRadius: BorderRadius.circular(25),
                  ),
                ),
              ),
              // 这个SizedBox作用是让两个输入框有10个单位的距离
              SizedBox(height: 10),
              TextField(
                // 控制器
                controller: _passwdController,
                // 输入内容发生变化时执行该函数
                onChanged: (value) {
                  print(value);
                },
                // 用户输入回车时执行一次(只执行一次，除非后面又有其它输入再次输入回车)
                onSubmitted: (value) {
                  print(value);
                },
                obscureText: true, // 表示这个是个密码框，不显示明文
                decoration: InputDecoration(
                  contentPadding: EdgeInsets.only(left: 20), //设置文本框内边距
                  filled: true, // 允许输入框有背景颜色
                  fillColor: Colors.white, // 输入框背景颜色
                  hintText: "请输入密码", // 输入框提示语
                  hoverColor: Colors.white, // 设置鼠标经过时的文本框的颜色
                  border: OutlineInputBorder(
                    borderSide: BorderSide.none, // 取消文本框外边框
                    borderRadius: BorderRadius.circular(25), // 设置文本框圆角
                  ),
                ),
              ),

              Container(
                margin: EdgeInsets.only(top: 40),
                width: double.infinity,
                height: 50,
                decoration: BoxDecoration(
                  color: Colors.black,
                  borderRadius: BorderRadius.circular(25),
                ),
                // TextButton不能设置背景颜色和宽高，所以把它放入一个容器中
                child: TextButton(
                  onPressed: () {
                    // 通过控制器获取输入框文本
                    print("账号-${_accountController.text}");
                  },
                  child: Text("登录", style: TextStyle(color: Colors.white)),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

#### 表单组件 --- Form





### 滚动组件

#### 常用滚动组件

<img src="./pic/dart/屏幕截图 2026-01-19 200632.png">

#### 常用滚动组件 --- SingleChildScrollView

> 用法：包裹一个子组件，让`单个子组件`具备滚动能力。

> 子组件：只能包含一个子组件，如果滚动多个组件，通常将其嵌套在column或row组件中。

> 滚动方向：通过scrollDirection属性控制，默认垂直方向，也可以设置为水平方向。

> <mark>注意：</mark> `SingleChildScrollView`会一次性构建所有子组件，如果嵌套的`Column`或`Row`中包含大量子项，可能会导致性能问题，建议使用`ListView`

> 控制滚动：绑定一个`ScrollController`对象给controller对象，可以使用跳转功能

> 控制滚动：controller：给组件的controller绑定ScrollController对象
> 一个比较常见的案例是，去底部和去顶部

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 可以滚动的body
        body: SingleChildScrollView(
          child: Column(
            children: List.generate(100, (index) {
              return Container(
                width: double.infinity,
                height: 60,
                color: Colors.blue,
                margin: EdgeInsets.only(bottom: 10),
                child: Center(
                  child: Text(
                    "第${index + 1}个container",
                    style: TextStyle(color: Colors.white),
                  ),
                ),
              );
            }),
          ),
        ),
      ),
    );
  }
}
```

**有跳转的功能**

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  final ScrollController _scrollController = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 可以滚动的body
        body: Container(
          child: Stack(
            children: [
              SingleChildScrollView(
                controller: _scrollController,
                child: Column(
                  children: List.generate(100, (index) {
                    return Container(
                      width: double.infinity,
                      height: 60,
                      color: Colors.blue,
                      margin: EdgeInsets.only(bottom: 10),
                      child: Center(
                        child: Text(
                          "第${index + 1}个container",
                          style: TextStyle(color: Colors.white),
                        ),
                      ),
                    );
                  }),
                ),
              ),
              Positioned(
                top: 20,
                right: 20,
                child: GestureDetector(
                  onTap: () {
                    // 跳转到指定位置，没有动画效果
                    // 滚动条的最大值_scrollController.position.maxScrollExtent
                    // _scrollController.jumpTo(
                    //   _scrollController.position.maxScrollExtent,
                    // );

                    // 有动画效果的跳转
                    _scrollController.animateTo(
                      // 跳转位置
                      _scrollController.position.maxScrollExtent,
                      // 整个动画时间
                      duration: Duration(seconds: 1),
                      // 动画曲线
                      curve: Curves.easeIn,
                    );
                  },
                  child: Container(
                    width: 60,
                    height: 60,
                    decoration: BoxDecoration(
                      borderRadius: BorderRadius.circular(50),
                      color: Colors.amberAccent,
                    ),
                    child: Center(
                      child: Text(
                        "去底部",
                        style: TextStyle(color: Colors.blueGrey),
                      ),
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

#### 常用滚动组件  --- ListView

> 作用：用于`构建可滚动列表`的核心部件，并提供流畅滚动体验。
> 
> 方式：提供多种构造函数，如默认构造函数（这个不会懒加载，后面三种都会懒加载）、`ListView.builder`、`ListView.separated`、`ListView.custom`
> 
> 机制：采用按需渲染（懒加载），只构建当前可见区域的列表项，极大提升长列表性能

<mark>默认构造函数来构造滚动列表</mark>

> 这种方式不会懒加载数据，使用方式与`SingleChildScrollView`基本一样

```dart
class _MainPageState extends State<MainPage> {
  final ScrollController _scrollController = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用默认构造函数来创建滚动列表
        body: ListView(
          controller: _scrollController,
          children: List.generate(100, (index) {
            return Container(
              width: double.infinity,
              height: 50,
              decoration: BoxDecoration(color: Colors.blue),
              margin: EdgeInsets.only(bottom: 10),
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个container",
                style: TextStyle(color: Colors.white),
              ),
            );
          }),
        ),
      ),
    );
  }
}
```

<mark>使用ListView.builder创建懒加载滚动列表</mark>

> 作用：处理长列表或动态数据的首选和推荐方式
> 方式：接收一个itemBuilder回调函数来按需构建列表项，通过itemCount控制列表长度。

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用builder来创建滚动列表
        body: ListView.builder(
          // 要渲染的列表项长度
          itemCount: 100,
          // itemBuilder的意思是每构建一个列表项就会执行一次itemBuilder
          itemBuilder: (BuildContext context, int index) {
            // 这个函数需要返回一个组件，就是生成的列表项
            return Container(
              width: double.infinity,
              height: 50,
              decoration: BoxDecoration(color: Colors.blue),
              margin: EdgeInsets.only(bottom: 10),
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个container",
                style: TextStyle(color: Colors.white),
              ),
            );
          },
        ),
      ),
    );
  }
}
```

<mark>使用ListView.separated模式</mark>

> 作用：在`ListView.builder`的基础上，额外提供了`构建分割线`的能力(在两个渲染容器的之间插入分隔容器)
> 
> 方式：需要同时提供`itemBuilder`、`separatorBuilder`、`itemCount`三个属性

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用builder来创建滚动列表
        body: ListView.separated(
          // 要渲染的列表项长度
          itemCount: 100,
          // itemBuilder的意思是每构建一个列表项就会执行一次itemBuilder
          itemBuilder: (BuildContext context, int index) {
            // 这个函数需要返回一个组件，就是生成的列表项
            return Container(
              width: double.infinity,
              height: 50,
              decoration: BoxDecoration(color: Colors.blue),
              margin: EdgeInsets.only(bottom: 10),
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个container",
                style: TextStyle(color: Colors.white),
              ),
            );
          },
          // 这个回调函数会构建列表项中间的分隔容器
          separatorBuilder: (BuildContext context, int index) {
            return Container(
              height: 10,
              width: double.infinity,
              color: Colors.amber,
            );
          },
        ),
      ),
    );
  }
}
```

#### 常用滚动组件 --- GridView

> 作用：用于`创建二维可滚动网格布局`的核心组件
> 
> 方式：提供多种构建方式，`GridView.count`、`GridView.extent`、`GridView.builder`等。`GridView`的默认构造方式（写起来很繁琐，很少使用）

> `GridView.count`：基于固定列数的网格布局（最常用之一）
> `GridView.extent`：基于固定子项最大宽度/高度的网格布局（最常用之一）
> `GridView.builder`：用于`网格项数量巨大`或`动态生成`的情况，需要接收`gridDelegate`布局委托属性。
> `gridDelegate`：`SliverGridDelegateWithFixedCrossAxisCount`：固定列数。`mainAxisSpacing`：主轴间距。`SliverGridDelegateWithMaxCrossAxisExtent`：最大宽度。`crossAxisSpacing`：交叉轴间距。`scrollDirection`：设置滚动方向。

<mark>GridView.count构造</mark>

> `GridView.count`：基于固定列数的网格布局（最常用之一）

<img src="./pic/dart/屏幕截图 2026-01-20 101227.png">

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用builder来创建滚动列表
        body: GridView.count(
          // 设置主轴方向，默认纵向
          // scrollDirection: Axis.horizontal,

          //设置交叉轴的元素个数
          crossAxisCount: 3,
          // 设置主轴方向元素间距
          mainAxisSpacing: 10,
          // 设置交叉轴方向元素间距
          crossAxisSpacing: 10,
          children: List.generate(100, (index) {
            return Container(
              color: Colors.blue,
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个",
                style: TextStyle(color: Colors.white, fontSize: 20),
              ),
            );
          }),
        ),
      ),
    );
  }
}
```

==GridView.extent构造==

> 作用：使用`GridView.extent`指定子项在交叉轴上最大排列数量

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用extent来创建滚动列表
        body: GridView.extent(
          // 控制子项在交叉轴方向上的最大数量
          // 如果屏幕在交叉轴方向放不下会换行放，但交叉轴最大只能放100个子项（这里的设置）
          maxCrossAxisExtent: 100,
          children: List.generate(100, (index) {
            return Container(
              color: Colors.blue,
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个",
                style: TextStyle(color: Colors.white, fontSize: 20),
              ),
            );
          }),
        ),
      ),
    );
  }
}
```

==使用`GridView.builder`构造==

> 作用：使用`GridView.builder`实现动态渲染网格（懒加载，只渲染可见区域）

> ==注意：== 接收`gridDelegate`布局委托、`itemBuilder`构建函数、`itemCount`构建数量

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用extent来创建滚动列表
        body: GridView.builder(
          // 布局委托
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            // 设置交叉轴子项个数，不论屏幕如何变换，这个个数都不变
            crossAxisCount: 4,
            // 设置间距
            mainAxisSpacing: 10,
            crossAxisSpacing: 10,
          ),
          // 子项个数
          itemCount: 100,
          //子项生成回调函数
          itemBuilder: (context, index) {
            return Container(
              color: Colors.blue,
              alignment: Alignment.center,
              child: Text(
                "第${index + 1}个",
                style: TextStyle(color: Colors.white, fontSize: 20),
              ),
            );
          },
        ),
      ),
    );
  }
}
```

#### 自定义滚动容器 --- CustomScrollView

> 作用：用于`组合多个可滚动组件`（如列表、网格），实现`统一协调`的滚动效果
> 
> `Sliver`：`Flutter`中描述可滚动视图内部一部分内容的组件，它是滚动视图的“切片”
> 
> 用法：通过`slivers`属性接收一个`sliver组件列表`，这个列表元素需要用`SliverToBoxAdapter`（下面的各种的Sliver组件）等包裹

`Sliver`组件对应关系：

- `SliverList` => `ListView`
- `SliverGrid` => `GridView`
- `SliverAppBar` => `AppBar`
- `SliverPadding` => `Padding`
- `SliverToBoxAdapter` => `ToBoxAdapter`(用于包裹普通 `Widget`)
- `SliverPersistentHeader(粘性吸顶)` 

可以实现如下效果
<img src="./pic/dart/屏幕截图 2026-01-20 115718.png">

==案例--粘性吸顶分类SliverPersistentHeader==

> SliverPersistentHeader：给delegate属性赋值一个继承SliverPersistentHeaderDelegate的对象实例，设置固定属性pinned为true

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("文本框")),
        // 用extent来创建滚动列表
        body: CustomScrollView(
          // 这里存放要渲染的组件，必须用SliverToBoxAdapter包裹
          slivers: [
            SliverToBoxAdapter(
              child: Container(
                color: Colors.blue,
                height: 260,
                alignment: Alignment.center,
                child: Text(
                  "轮播图1",
                  style: TextStyle(color: Colors.white, fontSize: 30),
                ),
              ),
            ),
            // 粘性吸顶，
            SliverPersistentHeader(delegate: _StickyCategory(), pinned: true),
            SliverList.separated(
              itemCount: 100,
              itemBuilder: (BuildContext context, int index) {
                return Container(
                  color: Colors.blue,
                  height: 100,
                  child: Text("列表项${index + 1}"),
                );
              },
              // 分隔容器
              separatorBuilder: (BuildContext context, int index) {
                return SizedBox(height: 20);
              },
            ),
            SliverToBoxAdapter(
              child: Container(
                color: Colors.blue,
                height: 260,
                alignment: Alignment.center,
                child: Text(
                  "轮播图2",
                  style: TextStyle(color: Colors.white, fontSize: 30),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class _StickyCategory extends SliverPersistentHeaderDelegate {
  @override
  Widget build(
    BuildContext context,
    double shrinkOffset,
    bool overlapsContent,
  ) {
    //
    return Container(
      // color: Colors.red,
      margin: EdgeInsets.only(top: 10, bottom: 10),
      child: ListView.builder(
        itemCount: 30,
        scrollDirection: Axis.horizontal,
        itemBuilder: (context, index) {
          return Container(
            width: 100,
            height: 60,
            margin: EdgeInsets.only(left: 10, right: 10),
            color: Colors.blue,
            alignment: Alignment.center,
            child: Text(
              "分类${index + 1}",
              style: TextStyle(color: Colors.white),
            ),
          );
        },
      ),
    );
  }

  @override
  // 最大展开高度
  double get maxExtent => 100;

  @override
  // 最小折叠高度
  double get minExtent => 60;

  @override
  bool shouldRebuild(covariant SliverPersistentHeaderDelegate oldDelegate) {
    // 当视口大小变化时，是否重新渲染。
    return false;
  }
}
```

#### 整页滚动容器 --- PageView

> 作用：用于实现分页滚动视图(就是一滑就是一整个屏幕的滚动)的核心组件
> 
> 方式：提供多种构建方式，`默认构造方式`、`PageView.builder`等。
> 
> 优势：支持懒加载（按需渲染）

```dart
SliverToBoxAdapter(
  child: Container(
  color: Colors.blue,
  height: 260,
  alignment: Alignment.center,
  child: PageView.builder(
    itemCount: 10,
    itemBuilder: (context, index) {
      return Container(
        width: 100,
        height: 60,
        alignment: Alignment.center,
        child: Text(
          "轮播图${index + 1}",
          style: TextStyle(color: Colors.white, fontSize: 30),
        ),
      );
    },
  ),
),
),
```

==PageView跳转控制==

> PageView绑定controller属性，对象类型为PageController
> 
> 切换方法：controller.jumpPage/animateToPage

```dart
Container(
                    color: Colors.blue,
                    height: 260,
                    alignment: Alignment.center,
                    child: PageView.builder(
                      // 绑定控制器
                      controller: _pageController,
                      itemCount: 10,
                      itemBuilder: (context, index) {
                        return Container(
                          width: 100,
                          height: 60,
                          alignment: Alignment.center,
                          child: Text(
                            "轮播图${index + 1}",
                            style: TextStyle(color: Colors.white, fontSize: 30),
                          ),
                        );
                      },
                    ),
                  ),
                  Positioned(
                    bottom: 0,
                    left: 0,
                    right: 0,
                    height: 20,
                    child: Container(
                      width: double.infinity,
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: List.generate(10, (index) {
                          return GestureDetector(
                            onTap: () {
                              // 点击跳转到指定轮播图
                              _pageController.jumpToPage(index);
                              setState(() {
                                // 改变状态
                                currentIndex = index;
                              });
                            },
                            child: Container(
                              width: 10,
                              height: 10,
                              margin: EdgeInsets.only(left: 10, right: 10),
                              decoration: BoxDecoration(
                                borderRadius: BorderRadius.circular(50),
                                // 这是闭包，index会一直存在这里
                                color: currentIndex == index
                                    ? Colors.red
                                    : Colors.white,
                              ),
                            ),
                          );
                        }),
                      ),
                    ),
                  ),
```

### 悬浮组件



### 自定义组件 --- 无状态组件 和 有状态组件

> 定义：根据自己特定的需求创建自己的`Widget`
> 分类：`Flutter`分为`无状态组件` 和 `有状态组件`

| 特征   | StatelessWidget（无状态） | StatefulWidget（有状态）       |
| ---- | -------------------- | ------------------------- |
| 核心特征 | 一旦创建，内部状态不可变         | 持有可在其生命周期内改变的状态           |
| 使用场景 | 静态内容展示，外观仅由配置参数决定    | 交互式组件，如计数器、可切换开关、表单输入框    |
| 生命周期 | 相对简单，主要是构建（build）    | 更为复杂，包含状态创建、更新和销毁         |
| 代码结构 | 单个类                  | 两个关联的类：Widget本身和单独的State类 |

#### 无状态组件 --- StatelessWidget

> 定义：创建一个新的类，继承 `StatelessWidget` 类并实现 `build` 方法
> 要点：`build` 返回一个 `Widget`
> 场景：纯展示型组件，没有用户交互操作

**案例：**

```dart

void main() {
  // 在入口处注册这个组件，(首页就是这个注册的组件)
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 整个build方法返回一个widget组件
    return MaterialApp(
      title: "无状态自定义组件",
      home: Scaffold(
        appBar: AppBar(title: Text("头部区域")),
        body: Center(child: Text("中部区域")),
        bottomNavigationBar: SizedBox(
          height: 80,
          child: Center(child: Text("底部区域")),
        ),
      ),
    );
  }
}
```

#### 有状态组件 --- StatefulWidget

> 定义：有状态组件是构建动态交互界面的核心，能够管理变化的内部状态，当状态改变时，组件会更新显示内容
> 
> 实现：
> 
> - 创建两个类，第一个类继承`StatefulWidget`类，主要接收和定义最终参数，核心作用时创建`State对象`
> - 第二个类继承`State<第一个类名>`，负责管理所有可变的数据和业务逻辑，并实现`build`构建方法
> 
> 要点：`build`方法需要返回一个`组件Widget`

<mark>案例：</mark>

```dart
void main() {
  runApp(MainPage());
}

// 自定义有状态widget

// 创建第一个类，state类
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<StatefulWidget> createState() {
    // implement createState
    // 返回第二个类的对象
    return _MainPageState();
  }
}

// 创建第二个类，内部类 负责管理数据 处理业务逻辑 并且渲染视图
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    //  implement build
    return MaterialApp(
      title: "有状态自定义组件",
      home: Scaffold(
        appBar: AppBar(title: Text("头部区域")),
        body: Center(child: Text("中部区域")),
        bottomNavigationBar: SizedBox(
          height: 80,
          child: Center(child: Text("底部区域")),
        ),
      ),
    );
  }
}
```

#### 使用快捷键创建组件

> 需要在vscode中安装 `Awesome Flutter Snippets`
> 
> 无状态组件快捷键：`statelessW`
> 有状态组件快捷键：`statefulW`



### 生命周期 --- 无状态组件 和 有状态组件

#### 无状态组件生命周期

> 无状态组件唯一阶段：`build方法`
> 
> 当组件被创建或父组件状态变化导致其需要重新构建时，`build方法`会被调用

#### 有状态组件生命周期

<img src="./pic/dart/屏幕截图 2026-01-18 140348.png">

<img src="./pic/dart/屏幕截图 2026-01-18 140541.png">

```dart
// 创建第一个类，state类
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<StatefulWidget> createState() {
    // 创建阶段的createState()
    print("创建阶段的createState()");

    // 返回第二个类的对象
    return _MainPageState();
  }
}

// 创建第二个类，内部类 负责管理数据 处理业务逻辑 并且渲染视图
class _MainPageState extends State<MainPage> {

  // 创建阶段的initState()，只执行一次
  @override
  void initState() {
    print("创建阶段的initState()");
    super.initState();
  }

  // 创建阶段didChangeDependencies()
  @override
  void didChangeDependencies() {
    print("创建阶段didChangeDependencies(), 可能执行多次")
    super.didChangeDependencies();
  }

  @override
  Widget build(BuildContext context) {
    //  implement build
    return MaterialApp(
      title: "有状态自定义组件",
      home: Scaffold(
        appBar: AppBar(title: Text("头部区域")),
        body: Center(child: Text("中部区域")),
        bottomNavigationBar: SizedBox(
          height: 80,
          child: Center(child: Text("底部区域")),
        ),
      ),
    );
  }
}
```

#### 总结

- 无状态组件：build
- 有状态组件（创建阶段）：`createState -> initState -> didChangeDependencies -> build`
- 有状态组件（更新阶段）：`didUpdateWidget -> build`
- 有状态组件（销毁阶段）：`deactivate -> dispose`
- 执行一次函数：`createState、initState、dispose`
- InheritedWidget：专门用于在Widget树种自顶向下高效的共享数据，顶层组件提供数据，子孙节点直接获取。

## 组件状态管理

### 单组件 --- setState

> 数据的变化要更新UI视图，需要执行setState方法，setState方法会造成build的重新执行。
> 
> setState方法会造成受影响的组件重新渲染

```dart
// 有状态组件
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  // 定义状态，也就是要操作的数据
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              TextButton(
                onPressed: () {
                  // 在setState中操作的数据是响应的
                  setState(() {
                    count--;
                  });
                },
                child: Text("减"),
              ),
              Text(count.toString()),
              TextButton(
                onPressed: () {
                  count++;
                  // 只要调用setState就会更新视图，所以在setState之前调用也行
                  setState(() {});
                },
                child: Text("加"),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

### 跨组件 --- ValueNotifier 和 ValueListenableBuilder

> **优点**：无需手动setState，自动刷新依赖的Widget。  
> 
> **缺点**：需要创建一个相关组件都能访问的状态
> 
> **适用**：少量全局状态（如主题切换、登录状态）。

```dart
// 创建全局可监听对象
final counterNotifier = ValueNotifier<int>(0);

// 在页面A读取
ValueListenableBuilder(
  valueListenable: counterNotifier,
  builder: (context, value, child) => Text('$value'),
)

// 在页面B修改,修改后会通知页面A修改状态
ElevatedButton(
  onPressed: () => counterNotifier.value++,
  child: Text('+1'),
)
```

### 官方推荐 --- Provider

> 使用：
> 
> - **添加依赖**：`flutter pub add provider`
> 
> - **创建状态类**：
>   
>   ```dart
>   class Counter with ChangeNotifier {
>     int _count = 0;
>     int get count => _count;
>   
>     void increment() {
>       _count++;
>       notifyListeners(); // 关键：通知所有监听者刷新
>     }
>   }
>   ```
> 
> - 在顶层提供，在子组件消费
>   
>   ```dart
>   // 顶层（main.dart）
>   return MultiProvider(
>     providers: [ChangeNotifierProvider(create: (_) => Counter())],
>     child: MyApp(),
>   );
>   
>   // 子组件A：读取
>   final counter = context.watch<Counter>();
>   Text('${counter.count}');
>   
>   // 子组件B：修改
>   final counter = context.read<Counter>();
>   ElevatedButton(onPressed: counter.increment, child:Text('+1'))
>   
>   // watch默认监听整个状态类，如果修改一个组件没有使用的属性组件也会更新
>   // 使用select精确监听
>   // ComponentA：只监听count
>   class ComponentA extends StatelessWidget {
>     @override
>     Widget build(BuildContext context) {
>       final count = context.select<MyState, int>((state) => state.count);
>       print('ComponentA rebuilt');
>       return Text('Count: $count');
>     }
>   }
>   
>   // ComponentB：只监听name
>   class ComponentB extends StatelessWidget {
>     @override
>     Widget build(BuildContext context) {
>       final name = context.select<MyState, String>((state) => state.name);
>       print('ComponentB rebuilt');
>       return Text('Name: $name');
>     }
>   }
>   ```

> **注意**：
> 
> * `context.watch`：数据变化时**刷新**当前Widget。
> 
> * `context.read`：只获取对象，**不刷新**（用于按钮点击等事件）。
> 
> * 需要跨页面共享时，把`Provider`放在`MaterialApp`上层即可。

### 更现代 --- Riverpod

> Provider的升级版，**编译安全、无需BuildContext**

```dart
// 1. 创建状态
final counterProvider = StateProvider<int>((ref) => 0);

// 2. 读取（任意地方）
final count = ref.watch(counterProvider);
Text('$count');

// 3. 修改（任意地方）
ref.read(counterProvider.notifier).state++;
```

> **优势**：
> 
> * 不用传`context`，可在任何类/函数中使用。
> 
> * 依赖关系清晰，测试更友好。

## 事件处理与通知

> Flutter中的手势系统有两个独立的层。
> 
> - 第一层为原始指针(pointer)事件，它描述了屏幕上指针（例如，触摸、鼠标和触控笔）的位置和移动。 
> 
> - 第二层为手势，描述由一个或多个指针移动组成的语义动作，如拖动、缩放、双击等。

### 原始指针事件处理

> 原始指针事件(Pointer Event，在移动设备上通常为触摸事件)

#### 命中测试简介

> 在移动端，各个平台或UI系统的原始指针事件模型基本都是一致，即：一次完整的事件分为三个阶段：手指按下、手指移动、和手指抬起，而更高级别的手势（如点击、双击、拖动等）都是基于这些原始事件的。
> 
> 当指针按下时，Flutter会对应用程序执行**命中测试(Hit Test)**，以确定指针与屏幕接触的位置存在哪些组件（widget）， 指针按下事件（以及该指针的后续事件）然后被分发到由命中测试发现的最内部的组件，然后从那里开始，事件会在组件树中向上冒泡，这些事件会从最内部的组件被分发到组件树根的路径上的所有组件，这和Web开发中浏览器的**事件冒泡**机制相似， 但是Flutter中没有机制取消或停止“冒泡”过程，而浏览器的冒泡是可以停止的。
> 
> 注意，只有通过命中测试的组件才能触发事件。

#### Listener 组件

> `Listener`用于监听原始触摸事件，**监听的是它整个 `child` 子树（包括所有后代）中发生的事件**。
> 
> 下面是`Listener`的构造函数定义：

```dart
Listener({
  Key key,
  this.onPointerDown, //手指按下回调
  this.onPointerMove, //手指移动回调
  this.onPointerUp,//手指抬起回调
  this.onPointerCancel,//触摸事件取消回调
  this.behavior = HitTestBehavior.deferToChild, //决定子组件如何响应命中测试节会专门介绍
  Widget child
})
```

**示例**

> 手指在一个容器上移动时显示手指相对于容器的位置。

```dart
class _PointerMoveIndicatorState extends State<PointerMoveIndicator> {
  PointerEvent? _event;

  @override
  Widget build(BuildContext context) {
    return Listener(
      child: Container(
        alignment: Alignment.center,
        color: Colors.blue,
        width: 300.0,
        height: 150.0,
        child: Text(
          '${_event?.localPosition ?? ''}',
          style: TextStyle(color: Colors.white),
        ),
      ),
      onPointerDown: (PointerDownEvent event) => setState(() => _event = event),
      onPointerMove: (PointerMoveEvent event) => setState(() => _event = event),
      onPointerUp: (PointerUpEvent event) => setState(() => _event = event),
    );
  }
}
```

> 参数 `PointerDownEvent`、 `PointerMoveEvent`、 `PointerUpEvent` 都是`PointerEvent`的子类，注意 Pointer，即“指针”， 指事件的触发者，可以是鼠标、触摸板、手指。
> 
> `PointerEvent`类中包括当前指针的一些信息：
> 
> * `position`：它是指针相对于当对于全局坐标的偏移。
> * `localPosition`: 它是指针相对于当对于`Listener`本身布局坐标的偏移。
> * `delta`：两次指针移动事件（`PointerMoveEvent`）的距离。
> * `pressure`：按压力度，如果手机屏幕支持压力传感器(如iPhone的3D Touch)，此属性会更有意义，如果手机不支持，则始终为1。
> * `orientation`：指针移动方向，是一个角度值。
> * 等等...

#### 忽略指针事件

> 假如不想让某个子树响应`PointerEvent`的话，可以使用`IgnorePointer`和`AbsorbPointer`，这两个组件都能阻止子树接收指针事件，即阻断命中测试。
> 
> 不同之处在于`AbsorbPointer`本身会参与命中测试，而`IgnorePointer`本身不会参与，这就意味着`AbsorbPointer`本身是可以接收指针事件的(但其子树不行)，而`IgnorePointer`不可以。

**案例**

```dart
Listener(
  child: AbsorbPointer(
    child: Listener(
      child: Container(
        color: Colors.red,
        width: 200.0,
        height: 100.0,
      ),
      onPointerDown: (event)=>print("in"),
    ),
  ),
  onPointerDown: (event)=>print("up"),
)
```

> 点击`Container`时，由于它在`AbsorbPointer`的子树上，被阻断了命中测试，所以`Container`的`Listenser`接收不到指针事件，所以日志不会输出"in"，但`AbsorbPointer`本身是可以接收指针事件的，所以会输出"up"。
> 
> 如果将`AbsorbPointer`换成`IgnorePointer`，那么两个都不会输出。

### 手势识别

#### 点击事件 --- GestureDetector

> `GestureDetector`是 Flutter 中最常用、功能最丰富的手势检测组件
> 用法：使用`GestureDetector`包裹被点击的元素，传入`onTap`方法
> 
> <mark>注意：</mark> flutter存在手势竞争问题。如，`GestureDetector`的`onTap`和`onDoubleTap`同时存在时，触发`onTap`会有明显的延迟，因为flutter需要等待一下以确认是`onTap`还是`onDoubleTap`事件

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    //  implement build
    return MaterialApp(
      title: "有状态自定义组件",
      home: Scaffold(
        appBar: AppBar(title: Text("头部区域"), centerTitle: true),
        body: Center(child: Text("中部区域")),
        bottomNavigationBar: SizedBox(
          height: 80,
          child: Center(
            // 使用 GestureDetector
            child: GestureDetector(
              // 单击作用的元素，执行
              onTap: () {
                print("点击了一次。");
              },
              // 双击作用的元素，执行
              onDoubleTap: () => print("双击了"),
              // 作用的元素
              child: Text("底部区域"),
            ),
          ),
        ),
      ),
    );
  }
}
```

### 组件点击事件

> 除了 GestureDetector ，flutter还提供了其它事件绑定

<img src="./pic/dart/屏幕截图 2026-01-18 150831.png">

## Flutter动画

> 动画实现的原理：在一段时间内，快速地多次改变UI外观；由于人眼会产生视觉暂留，所以最终看到的就是一个“连续”的动画。
> 
> 将UI的一次改变称为一个动画帧，对应一次屏幕刷新。决定动画流畅度的一个重要指标就是帧率FPS（Frame Per Second），即每秒的动画帧数。
> 
> 一般情况下，对于人眼来说，动画帧率超过16 FPS，就基本能看了，超过 32 FPS就会感觉相对平滑，而超过 32 FPS，大多数人基本上就感受不到差别了。

### 隐式动画

> 由于隐式动画背后的实现原理和繁琐的操作细节被隐去了，可以通过几行代码就能实现动画，所以被称为隐式动画，Flutter中提供的`AnimatedContainer`、`AnimatedPadding`、`AnimatePositioned`、`AnimatedOpacity`、`AnimatedDefaultTextStyle`、`AnimatedSwitecher`都属于隐式动画。

#### AnimatedContainer

> `AnimatedContainer`的属性和`Container`属性基本是一样的，当`AnimatedContainer`属性改变的时候就会触发动画。

**示例**

> 必须要有duration参数

```dart
int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          AnimatedContainer(
            width: _counter % 2 == 0 ? 200 : 300,
            height: _counter % 2 == 0 ? 200 : 300,
            color: Colors.amber,
            // 必需的参数，动画时间
            duration: const Duration(seconds: 1),
          ),
          TextButton(onPressed: () => _incrementCounter(), child: Text("加一")),
        ],
      ),
    );
  }
```





## Flutter样式

### 长度单位

> flutter中使用的是逻辑像素，在不同的分辨率的屏幕上显示大小基本一致
> 
> 在160dpi上，1逻辑像素 = 1物理像素
> 
> 在320dpi上，1逻辑像素 = 2物理像素
> 
> 物理尺寸（英寸）= 逻辑像素长度 / 160
> 
> 例如：200 逻辑像素的宽度
> 
> double inches = 200 / 160;  // = 1.25 英寸

## 组件通信

<img src="./pic/dart/屏幕截图 2026-01-22 135321.png">

### 组件通信 --- 父传子（构造函数传参数）

> 步骤：
> 
> - `子组件定义接收属性`
> - `子组件在构造函数中接收参数`
> - `父组件传递属性给子组件`
> - 有状态组件在`对外的类`接收属性，`对内的类`通过`widget对象`获取对应属性
> - 注意：子组件定义接收属性需要使用`final关键字`-因为属性由`父组件决定`，子组件不能随意更改。

<mark>无状态组件传递消息</mark>

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Container(
          child: Column(
            children: [
              Text("父组件", style: TextStyle(color: Colors.blue, fontSize: 20)),
              // 父组件向子组件传递消息
              child(msg: "张三"),
            ],
          ),
        ),
      ),
    );
  }
}

class child extends StatelessWidget {
  final String? msg; // 定义子组件接收的属性
  const child({super.key, this.msg});

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text("子组件$msg", style: TextStyle(color: Colors.red, fontSize: 20)),
    );
  }
}
```

<mark>有状态组件的通信</mark>

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  int count = 0;
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Container(
          child: Column(
            children: [
              GestureDetector(
                // 使用setState，会让Child这个组件重新渲染
                onTap: () => setState(() {
                  count++;
                }),
                child: Text(
                  "父组件",
                  style: TextStyle(color: Colors.blue, fontSize: 20),
                ),
              ),
              // 父组件向子组件传递消息
              Child(msg: "张三$count"),
            ],
          ),
        ),
      ),
    );
  }
}

// class MainPage extends StatelessWidget {
//   const MainPage({super.key});
//   @override
//   Widget build(BuildContext context) {
//     return MaterialApp(
//       home: Scaffold(
//         body: Container(
//           child: Column(
//             children: [
//               Text("父组件", style: TextStyle(color: Colors.blue, fontSize: 20)),
//               // 父组件向子组件传递消息
//               Child(msg: "张三"),
//             ],
//           ),
//         ),
//       ),
//     );
//   }
// }

class Child extends StatefulWidget {
  // 这个对外的类接收属性
  final String? msg;
  const Child({super.key, this.msg});

  @override
  State<Child> createState() => _ChildState();
}

class _ChildState extends State<Child> {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text(
        // 通过widget接收属性
        "子组件${widget.msg}",
        style: TextStyle(color: Colors.red, fontSize: 20),
      ),
    );
  }
}
```

### 组件通信 --- 子传父（回调函数）

> 步骤：
> 
> - 父组件`传递一个函数`给子组件
> - 子组件`调用该函数`
> - 父组件通过`回调函数获取参数`

```dart
class MainPage extends StatefulWidget {
  const MainPage({super.key});

  @override
  State<MainPage> createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  final List<String> _list = ["鱼香肉丝", "宫保鸡丁", "麻婆豆腐", "京酱肉丝", "熘肉片"];
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: GridView.count(
          crossAxisCount: 2,
          mainAxisSpacing: 10,
          crossAxisSpacing: 10,
          children: List.generate(_list.length, (index) {
            return Child(
              msg: _list[index],
              index: index,
              // 父组件传入函数
              delFood: (index) {
                // 闭包
                _list.removeAt(index);
                // 更新状态
                setState(() {});
              },
            );
          }),
        ),
      ),
    );
  }
}

class Child extends StatefulWidget {
  final String? msg;
  final int index;
  // 接收一个回调函数
  final Function(int index) delFood;
  const Child({
    super.key,
    this.msg,
    required this.index,
    required this.delFood,
  });

  @override
  State<Child> createState() => _ChildState();
}

class _ChildState extends State<Child> {
  @override
  Widget build(BuildContext context) {
    return Stack(
      children: [
        Container(
          color: Colors.blue,
          alignment: Alignment.center,
          child: Text(
            "${widget.msg}",
            style: TextStyle(color: Colors.white, fontSize: 20),
          ),
        ),
        Positioned(
          right: 10,
          top: 10,
          child: TextButton.icon(
            onPressed: () {
              // 调用父组件传递的函数
              widget.delFood(widget.index);
            },
            icon: Icon(Icons.delete, color: Colors.white),
            label: Text("删除", style: TextStyle(color: Colors.white)),
          ),
        ),
      ],
    );
  }
}
```

## 网络请求

### 网络请求 --- Dio插件使用

> Dio是一个第三方库，需要安装

安装命令：`flutter pub add dio`

> 基本使用：`Dio().get(地址).then().catchError()`

#### Dio工具的封装

```dart
//封装一个工具类
class DioUtils {
 final Dio _dio = Dio();
 DioUtils() {
   // 配置基础地址和超时时间
   // _dio.options.baseUrl = "https://geek.itheima.net/v1_0/";
   // _dio.options.connectTimeout = Duration(seconds: 10);
   // _dio.options.sendTimeout = Duration(seconds: 10);
   // _dio.options.receiveTimeout = Duration(seconds: 10);

   // 像上面那样对同一个对象的属性进行操作可以使用如下语法糖
   _dio.options
     ..baseUrl = "https://geek.itheima.net/v1_0/"
     ..connectTimeout = Duration(seconds: 10)
     ..sendTimeout = Duration(seconds: 10)
     ..receiveTimeout = Duration(seconds: 10);

   // 拦截器
   // 注册拦截器
   _addInterceptor();
 }
 // 添加拦截器
 void _addInterceptor() {
   _dio.interceptors.add(
     InterceptorsWrapper(
       // 请求拦截器
       onRequest: (options, handler) {
         // 放过请求，如果有其它拦截器，去往下一个拦截器
         // handler.next(options);

         // 拒绝请求，请求中止，不会执行后续拦截器
         // handler.reject(DioException(requestOptions: options));
       },
       // 响应拦截器
       onResponse: (response, handler) {
         // 根据http状态码来判断是否拒绝
         if (response.statusCode! >= 200 && response.statusCode! < 300) {
           handler.next(response);
           return;
         } else {
           // 否则拒绝
           handler.reject(
             DioException(requestOptions: response.requestOptions),
           );
         }
       },
       // 错误拦截器
       onError: (error, handler) {
         // 在这里直接拒绝
         handler.reject(error);
       },
     ),
   );
 }

 // 向外暴露一个get方法
 Future<Response<dynamic>> get(String url, {Map<String, dynamic>? params}) {
   // 这里的url会与baseUrl进行拼接组合成完整的url
   return _dio.get(url, queryParameters: params);
 }
}
```

#### 解决web端跨域问题

> 默认情况下，flutter运行web端加载网站资源会报跨域提示错误

1. 在`flutter/packages/flutter_tools/lib/src/web/chrome.dart`添加`--disable-web-security`
2. 删除`flutter/bin/cache/`下`flutter_tools.snaphot`和`flutter_tools.stamp`
3. 执行`flutter doctor -v`然后重新运行项目

## 路由

> 定义：路由管理是构建多页面应用的核心，它通过`Navigator`和`Route`来管理页面栈，实现页面跳转和返回

<img src="./pic/dart/屏幕截图 2026-01-24 140148.png">

### 路由管理 -- 基本路由

> 场景：基本路由适合页面不多、跳转逻辑简单的场景
> 
> 用法：无需提前注册路由，跳转时创建`MaterialPageRoute实例`即可
> 
> 跳转新页面：`Navigator.push(BuildContext context, Route route)`
> 
> 返回上一页：`Navigator.pop(buildContext context)`

```dart
// 路由跳转-Material风格，只能有一个MaterialApp
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(home: ListPage());
  }
}

// 列表页
class ListPage extends StatefulWidget {
  const ListPage({super.key});

  @override
  State<ListPage> createState() => _ListPageState();
}

class _ListPageState extends State<ListPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("列表页")),
      body: ListView.builder(
        itemBuilder: (context, index) {
          return GestureDetector(
            onTap: (){
              // 路由跳转，从列表页跳转到详情页
              Navigator.push(context, MaterialPageRoute(
                  builder: (context){
                    // 要去往的页面
                    return DetailPage();
                  })
              );
            },
            child: Container(
              color: Colors.blue,
              height: 100,
              alignment: Alignment.center,
              margin: EdgeInsets.only(bottom: 5),
              child: Text(
                style: TextStyle(color: Colors.white, fontSize: 20),
                "列表项${index + 1}",
              ),
            ),
          );
        },
        itemCount: 100,
      ),
    );
  }
}

class DetailPage extends StatefulWidget {
  const DetailPage({super.key});

  @override
  State<DetailPage> createState() => _DetailPageState();
}

class _DetailPageState extends State<DetailPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("详情页"),),
      body: Center(
        child: TextButton(
            onPressed: (){
              Navigator.pop(context);
            }, 
            child: Text("返回上一页")
        ),
      ),
    );
  }
}
```

### 路由管理 -- 命名路由

> 场景：应用页面增多后，使用命名路由提升代码可维护性
> 
> 用法：需要先在`MaterialApp`中注册一个路由表并设置`initialRoute`（首页）

```dart
class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      // 注册路由表
      routes: {
        "/list": (context) => ListPage(),
        "/detail": (context) => DetailPage(),
      },
      home: ListPage(),
    );
  }
}

// 列表页
class ListPage extends StatefulWidget {
  const ListPage({super.key});

  @override
  State<ListPage> createState() => _ListPageState();
}

class _ListPageState extends State<ListPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("列表页")),
      body: ListView.builder(
        itemBuilder: (context, index) {
          return GestureDetector(
            onTap: () {
              // 路由跳转，从列表页跳转到详情页
              // Navigator.push(
              //   context,
              //   MaterialPageRoute(
              //     builder: (context) {
              //       // 要去往的页面
              //       return DetailPage();
              //     },
              //   ),
              // );

              // 用路由名称来跳转
              Navigator.pushNamed(context, "/detail");
            },
            child: Container(
              color: Colors.blue,
              height: 100,
              alignment: Alignment.center,
              margin: EdgeInsets.only(bottom: 5),
              child: Text(
                style: TextStyle(color: Colors.white, fontSize: 20),
                "列表项${index + 1}",
              ),
            ),
          );
        },
        itemCount: 100,
      ),
    );
  }
}
```

### 路由管理 -- 跳转方法

<img src="./pic/dart/屏幕截图 2026-01-24 155815.png">

### 路由管理 -- 传递参数

#### 命名路由传参

> 作用：通过`路由传递参数`是实现`页面间数据通信`的常用方式
> 
> 传递参数（命名路由）：`Navigator.pushNamed(context, 地址, arguments:{参数})`
> 
> 接收参数（命名路由）：`ModalRoute.of(context)?.settings.arguments` 
> 
> 接收时机：initState获取不到上下文参数context，需要放置在`Future.microtask(异步微任务)中`

==传递参数==

```dart
// 用路由名称来跳转
Navigator.pushNamed(
  context,
  "/detail",
  // 传递参数
  arguments: {"id": index + 1},
);
```

==获取路由参数==

```dart
// 在目标页面的initState中
void initState() {
  super.initState();
  // 获取路由参数
  Future.microtask(() {
    // 需要放在microtask中，因为在initState中可能还没有context
    if (ModalRoute.of(context) != null) {
      // 能获取路由参数
      var params =
          ModalRoute.of(context)!.settings.arguments as Map<String, dynamic>;
      _id = params["id"].toString();
      setState(() {});
    } else {
      // 不能够获取路由参数
      print("不能够获取路由参数");
    }
  });
}
```

#### 基础路由传参

> 传递参数(基础路由)：通过`组件构造函数`传递参数-（父传子）
> 
> 接收参数(基础路由)：通过`组件构造函数`接收参数--（父传子）
> 
> 接收时机：`initState可获取`到基础路由的`构造函数传参`

==传递参数==

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) {
      // 要去往的页面，通过构造函数传递参数
      return DetailPage(id: index + 1);
    },
  ),
);
```

==获取参数==

```dart
class DetailPage extends StatefulWidget {
  final int? id;
  const DetailPage({super.key, this.id});

  @override
  State<DetailPage> createState() => _DetailPageState();
}

class _DetailPageState extends State<DetailPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("详情页")),
      body: Center(
        child: Column(
          children: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              // 通过widget获取参数
              child: Text("去列表页${widget.id}"),
            ),
            TextButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text("返回上一页"),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 路由管理 -- 动态路由与高级控制

> 场景：更复杂的场景，如需根据参数动态生成页面，或实现路由拦截，可以使用`onGenerateRoute`和`onUnknownRoute`

<img src="./pic/dart/屏幕截图 2026-01-25 130303.png">

> `onGenerateRoute`：允许你根据`RouteSettings`（包含路由名称和参数）动态`创建不同的Route`

==onGenerateRoute使用案例==

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: "/goodsList",
      routes: {
        "/goodsList": (context) => GoodsListState(),
        // "/cartList": (context) => CartList(),
      },
      // 在上面routes中没有的路由，会到下面来找
      onGenerateRoute: (settings) {
        // 判断是不是去往需要的页面
        if (settings.name == "/cartList") {
          bool isLogin = false;
          // 判断是否登录
          if (isLogin) {
            // 登录前往购物车页面
            return MaterialPageRoute(builder: (context) => CartList());
          } else {
            // 未登录前往登录页面
            return MaterialPageRoute(builder: (context) => LoginPage());
          }
        }
      },
    );
  }
}
```

> `onUnknownRoute`：跳转一个`未在路由表中注册`、也未在`onGenerateRoute`中处理的路由，会调用此回调。通常显示"404"页面

==onUnknownRoute使用案例==

```dart
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: "/goodsList",
      routes: {
        "/goodsList": (context) => GoodsListState(),
        // "/cartList": (context) => CartList(),
      },
      // 在上面routes中没有的路由，会到下面来找
      onGenerateRoute: (settings) {
        // 判断是不是去往需要的页面
        if (settings.name == "/cartList") {
          bool isLogin = false;
          // 判断是否登录
          if (isLogin) {
            // 登录前往购物车页面
            return MaterialPageRoute(builder: (context) => CartList());
          } else {
            // 未登录前往登录页面
            return MaterialPageRoute(builder: (context) => LoginPage());
          }
        }
      },
      // 处理显示404页面
      onUnknownRoute: (settings) {
        return MaterialPageRoute(builder: (context) => NotFound());
      },
    );
  }
}
```

## 项目

### flutter常用命令



### 目录结构

<img src="./pic/dart/屏幕截图 2026-01-27 131144.png">

### 项目实战中遇到的内容

SafeArea(避开安全区组件)

IndexedStack(堆叠组件，根据索引显示对应组件)

BottomNavigationBar(上图下字组件，可切换索引)
