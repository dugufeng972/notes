# C语言

## printf 标准输出

1. printf() 的标准格式

```c
//printf(格式控制符，输出列表);
//格式控制字符串中占位符的个数与输出列表中变量或常量的个数相同
printf("num is %d\n"，5);
int num = 5;
printf("num is %d\n"，num);
```

2. 常见占位符

```c
/**
 * %c: 字符类型
 * %d：十进制的int类型
 * %ld：十进制long类型
 * %f：浮点类型float
 * %lf：浮点类型double
 * %s：字符串
 * %u：十进制的无符号的整数
 * %p：指针
 * /
```

3. 输出格式说明

```c
//限定宽度,默认右对齐，就是位数不够空格加在左边
printf("num is %5d\n", 123);
//限定宽度左对齐
printf("num is %-5d\n", 123);
//显示正负号
printf("num is %+d\n", 123);
//限定小数位数
printf("num is %.2f\n", 12.3);
//同时限定宽度和小数位数（.之前是宽度，之后是小数位数）
printf("num is %5.2f\n", 12.3);
```

## 数据类型

bit
byte(字节) = 8 bit

1. 基本数据类型
   - 整型
     - 短整型 short 4byte
     - 整型 int 不同计算机长度不一样
     - 长整型 long
   - 浮点型
     - 单精度型 float 4byte
     - 双精度型 double 8byte
     - 长双精度型 long double 12byte
   - 字符型 char 1byte
2. 构造类型
   - 数组类型
   - 结构体类型 struct
   - 共用体类型 union
   - 枚举类型 enum

### 整数类型

整数类型都可以被 signed(有符号)和 unsigned(无符号)修饰
整数类型不同计算机长度不一样(以 64 位编译器为例)
整型：

- 短整型 short 2byte
- 整型 int 4byte
- 长整型 long 8byte
  声明 long 类型时，可以添加'l'后缀

### 浮点类型

声明 float 类型时，可以添加'f'后缀
声明 double 类型时，可以添加'd'后缀

### 字符类型

1. 表示方式 - 使用一对'',括起来一个单独的字符 - 使用具体的字符对应的 ASCII 码表示
   char 类型中的每个字符对应一个 ASCII 码，ASCII 码是一个数值，因此 char 类型可以进行+ - / \* 运算

### 布尔类型

在 c89 标准中没有布尔类型，用非 0 为真，0 表示假
在 c99 标准中添加了类型\_Bool，表示布尔类型，true 为 1，false 为 0
在 c99 标准中提供了一个头文件 stdbool.h，文件中定义了 bool 代表\_Bool

### 常量

常量分为：

- #define 定义的标识符常量
- const 修饰的常变量
- 枚举常量
1. 使用#define--宏定义
   所谓宏定义，就是用一个标识符来表示一个常量值，如果后面的代码中出现了该标识符，那么编译时就全部替换成指定的常量值。即用宏体替换所有宏名，简称宏替换
   定义格式：#define 符号名(宏名) 常量值(宏体)
2. const 修饰的常变量

```c
const int i = 9;
```

3. 枚举常量

```c
enum Gender{
    male,
    female,
    secret
};
//枚举中变量默认从0开始
```

4. 字面量也是常量

### 变量间运算规则

自动类型转换：窄类型 -> 宽类型
强制类型转换：宽类型 -> 窄类型

## 输入/输出函数

输入函数：

- scanf(): 是格式输入函数，可以接受任意类型的数据
- getchar(): 是字符输入函数，只能接收单个字符
- gets(): 是字符串输入函数
  输出函数：
- printf(): 是格式输出函数，可以按指定的格式显示任意类型的数据
- putchar(): 字符显示函数，只能显示单个字符
- puts(): 是字符串输出函数

### scanf()

使用格式：scanf("格式控制字符串", 参数*地址*列表);
例：

```c
scanf("%d%d%d", &a,&b,&c);
```

"格式控制字符串" 个数必须与 "参数地址列表" 一致

## 运算符

### 位运算符

- `<<` 左移：二进制数向左一位，右边补 0
- `>>` 右移：二进制数向右一位，正数左边补 0，负数左边补 1
- `&` 按位与：对应位都是 1 结果为 1，否则为 0 `1 & 1 = 1   1 & 0 = 0`
- `|` 按位或
- `^` 按位异或
- `~` 按位取反
  结合赋值运算，有：`<<=` `>>=` `&=`

### 条件运算符

例：
`(条件表达式1) ? 表达式1 : 表达式2`

### sizeof 运算符

使用 sizeof(参数)

- 参数：可以是数据类型的关键字，也可以是变量名或某个具体的值
- 返回某种数据类型或某个值占用的字节数量

==`sizeof不用于计算字符串长度`==
sizeof 返回类型在不同系统是不一样的，有的是 unsigned int 有的是 unsigned long 。。。
c 语言统一用`size_t`表示 sizeof 的返回值类型
用 `%zd` 或 `%zu` 表示 sizeof 返回值类型的占位符

## 分支结构

### switch-case

基本语法：

```c
//表达式可以是整型或字符类型，不能是浮点类型
switch(表达式) {
    //表达式=常量1 执行
    case 常量1:
        语句块1;
        //break;
    case 常量2:
        语句块2;
        //break;
    case 常量3:
        语句块3;
        //break;
    //default是可选的
    default:
        语句块n;
}
```

### goto 关键字

使用 goto，可以实现无条件的语句转移。
一般格式：
`goto 标号;`

> 其中，标号，属于标识符，以":"为标记，位于某语句前面。
> goto 只能在同一个函数中跳转
> 例：

```c
int main() {
    //loop_label是标号
    loop_label:printf("loop");
    goto loop_label;
    return 0;
}
```

## 数组

==数组名是一个常量，里面存放着地址，不允许重新赋值地址==

### 一维数组

1. 定义方式

```c
int arr1[10];
arr1[0] = 0;
int arr2[3] = {1, 2, 3};
//等价上面
int arr3[] = {1, 2, 3};
//可以缺省(剩余的值都初始化为0)
int arr4[5] = {1, 2, 3};
//赋值指定位置上的成员(剩余的值都初始化为0)
int arr5[20] = {[2] = 23, [10] = 90, [15] = 99};
```

2. 数组的字节长度
   使用`sizeof(arr)`返回数组的字节长度
3. 数组的内存分析
- 数组名，记录该数组的首地址，即 a[0]的地址。
- 数组的各个元素是连续分布的。
  注意：
  数组变量一旦声明，数组名指向的地址就不可更改。编译器会自动为数组赋值一个地址
  所以下面是错误的：

```c
int num[10];
//企图改变数组名指向的地址
num = {1,23};
// 不能将数组赋值给数组
int num1[10] = num;
```

4. 变长数组
   变长数组的根本特征是数组长度只有运行时才能确定。

```c
int i = 10;
int a2[i];
int a3[i + 5];
int a4[i + k];
```

### char 型数组与字符串

一方面，可以看作普通数组，初始化、常用操作如前所述。比如：

```c
char arr[] = {'a', 'c'};
```

另一方面，字符型数组可以用于存储字符串。
c 语言中，在字符串结尾会自动添加一个'\0'，所以表示字符串的字符数组也必须以'\0'结尾。
例：

```c
char s1[] = {'a','b','c','\0'};
//简化写法,等同上面
char s2[] = "abc";
```

#### 字符串数组的长度

```c
char s2[] = "abc";
sizeof(s2); //返回值为4
```

"x" 与 'x' 是不一样的
"x"：占两个字符，'x' 和 '\0'
'x'：占一个字符

### 多维数组--二维数组

1. 定义

```c
//相当于将array1[10]作为元素存放在array2[8]中
int arr[8][10];
//定义同时进行初始化
int a[2][3] = {{1, 2, 3}, {4, 5, 6}};
//部分初始化,其余全为0
int a[2][3] = {[0][1] = 9, [1][2] = 7};
//不用{},自动匹配
int a[2][2] = {1, 2, 3, 4};
//上面的简写
int a[][3] = {1, 2, 3, 4, 5, 6};
//访问
//第一行第二列
arr[0][2];
//第二行第四列
arr[1][4];
```

二维数组在内存中是连续存放的
二维数组必须指定列数

## 指针

int \*p 的写法更符合指针的底层含义：

- \*p 是一个 int（通过解引用 p 得到整数），因此 p 是一个指向 int 的指针。

- 这种写法强调 \* 是变量 p 的修饰符，而非类型的一部分。
1. 定义指针变量
   不要把非地址值赋值给指针变量

```c
//用于存储指定int类型变量的地址
int a = 9;
int *p = &a;
int *p1, *p2;
//定义二级指针
int **pp = &p;
```

### 指针运算

1. 取地址运算符，使用"&"符合来表示。
   作用：取出指定变量在内存中的地址，用法：
   `&变量;`
   c 语言中的地址不仅包括位置信息，也包括它所指向的数据的类型信息，即它是带`类型的地址`
   对于暂时未赋值的指针变量为它赋值 NULL
2. 取值运算符，使用"*"
   作用：取出存放在指针变量里的地址 里的值
   用法:
   `*指针变量;`

#### 指针运算--指针与整数值的加减运算

格式：`指针+-整数`
指针与整数值的加减运算，表示指针所指向的内存地址的移动(加，向后移动；减，向后移动)。指针移动的单位，与指针指向的数据类型有关。数据类型占据多少个字节，每单位就移动多少个字节。

#### 指针运算--同类指针相减运算

格式：`指针-指针`
相同类型的指针允许进行`减法`运算，返回它们之间的距离，即`相隔多少个数据单位(注意：非字节数)`。
高位地址减去低位地址，返回的是正值；反之，为负值。
返回值属于`ptrdiff_t`类型，这是一个带符号的整数类型别名，具体类型根据系统有所不同，定义在头文件 stddef.h 里面。

### 野指针

野指针：就是指向的位置不可知的指针
野指针成因：

1. 指针未初始化
2. 指针越界访问
3. 指针指向已释放空间

### 指针与数组

指针 p[i] 会被当做 \*(p+i) 处理

```c
int *abc = malloc(sizeof(int) * 5);
printf("请输入%d个整数:\n", 5);
for (int i = 0; i < 5; i++)
{
    scanf("%d", abc + i);
}
printf("===========\n");
for (int *p = abc; p < (abc + 5); p++)
{
    printf("%d\n", *p);
}
```

1. a 与 &a 是一样的

```c
int a = {1, 2, 4};
a == &a //true
//a表示a[0]元素的地址
//&a表示a数组的地址
(a+1) == (&a + 1) //false
//a+1向后移一个int大小
//&a + 1向后移一个int数组大小
```

2. 二维数组
   二维数组虽然是连续存放的，但是通过 a，a[0]等方式访问到的是地址，把它当作二级指针创建的二维数组(两者是不一样的，二级指针可能不是连续存放的)

```c
//下面数组的值虽然是连续存放的，但编译器认为是将b[4]作为元素放在a[3]里面，可以尝试int *pa = a，会报错
int a[3][4] = {{1,2,3,4},{1,2,3,4},{1,2,3,4}};
a[0][0] // 1，等价于 *(a[0]), *(*a)
&a[0][0]    //获取a[0][0]的地址
a   //表示a[0]地址
a[0]    //表示a[0][0]的地址
&a  //表示二维数组的地址，与a的值是一样的，差异参见1
//讨论：a[1][0]相关
a[1]的地址：&a[1]  a+1

a[1][0]的地址：&a[1][0] a[1] *(a+1)

a[1][2]的地址：&a[1][2] a[1]+2  *(a+1)+2
```

用指针访问二维数组

```c
int *pa = a[0]; // 表示pa指向a[0][0]地址，可以用pa++来获取所有二维数组的元素
```

#### 指针数组 与 数组指针

> 数组指针：指向数组首地址的指针
> 指针数组：是存放指针的数组

#### 字符指针 与 字符指针变量

一个字符串，可以使用一维字符数组表示，也可以使用字符指针来表示。

- 字符数组由若干个元素组成，每个元素放在一个字符
- 字符指针变量中存放的是地址（字符串的首地址），绝不是将字符串放到字符指针变量中
  举例：

```c
char str[] = "hello tom";
char *pStr = "hello tom";   // 这是一个常量
//打印字符串，直接将地址传过去就行
printf("%s\n%s\n", pStr, str);
//重新赋值，但不允许修改其中的字符
pStr = "nihao";
//
```

#### 字符串数组的表示

两种表示方式：

1. 二维字符数组
2. 字符指针数组

#### 拓展--指向固定长度数组的指针变量

定义一个整型指针变量指向一维数组，一维数组的每个元素包含 m 个元素。(通常用于二维数组)
定义格式：
`(标识符)[一维数组元素个数];`
例如：定义一个指针变量 p，它指向包含有 4 个元素的一维数组。
`int (*p)[4];`
例：

```c
int a[2][3] = {{1,2,3},{4,5,6}};
int (*p)[3] = a;
/**
 * p内存放的是`int(*)[4]`（这是指一个|类型int的数组|的地址）的首地址
 * 所有p+1会直接跳到下一行数组
 * *p是一个指向数组首地址的，它是int类型的，所以*p+1会跳到下一个元素
 * */
//
**(p+1) // 4
*(*(p+1)+1) //5，即a[1][1]
```

> 说明：p 先和*结合，说明 p 是一个指针变量，指向一个大小为 4 的整型数组。
> 注意：此时定义的是一个指针变量，并非是一个指针数组。(*p 必须放在括弧里)
> 访问：p 内存放的是`int(*)[4]`（这是指一个|类型 int 的数组|的地址）的首地址，但是用`*p`得到的依然是首地址（数组首元素退化成地址）需要用`**p`访问

## 函数

分类：

1. 主函数：main()函数
2. 子函数：非 main()函数
   每一个 c 应用程序有且仅有一个 main()主函数，c 程序总是从 main()开始执行

是否允许源文件外调用：

1. 内部函数
2. 外部函数

### 函数的声明

c 语言的函数不能嵌套定义
c 语言不支持函数重载，函数名要唯一
可以用`return;`来结束函数

### main()函数的理解

c 语言会自动为 main 函数添加返回值，但建议自己要添加
main 函数的其它写法

```c
//argv是字符串数组，argc是字符串数组的个数
int main(int argc, char *argv[]) {
    //函数体
}
```

### exit()函数

该函数的原型定义在`stdlib.h`

- EXIT_SUCCESS 相当于 0 表示程序运行成功
- EXIT_FAILURE 相当于 1 表示程序异常中止

```c
//等同于exit(0);
exit(EXIT_SUCCESS);
//等同于exit(1);
exit(EXIT_FAILURE);
```

在 main()函数结束时也会隐私的调用 exit()函数，==exit()等价于 return 语句==

> c 语言还提供了一个 atexit()函数，用来登记 exit()执行时额外执行的函数，用来做一些退出程序时的收尾工作，该文件也定义在`stdlib.h`

### 函数原型

函数必须先声明后使用，为了先使用后定义需要先声明原型
例：

```c
void func1();
void func2(int, int);
int main() {
    func1();
    func2(0, 1);
    return 0;
}
void func1() {
    //...
}
void func2(int i, int j) {
    //...
}
```

### 参数传递机制

普通变量是值传递
函数传参：

1. 值传递--基本数据类型(整型、浮点型、字符型)、结构体、共用体、枚举类型
   - 值传递不影响原值
2. 地址传递--数组、指针
   - 地址传递双向影响

```c
int add(int i, int j)
{
    return i + j;
}

int tt(int *i, int *j)
{
    return *i + *j;
}

int main() {
    //值传递
    add(1, 2);
    //地址传递，将地址传递给形参
    tt(&i, &j)
}

```

==注意：函数不要返回内部变量的指针==

3. 数组的传递

4. 字符串(字符指针)作为形参

### 扩展--C++中的引用传递

### 可变参数

有些函数的参数数量是不确定的，此时可以使用 c 语言提供的`可变参数函数`
声明可变参数函数的时候，使用省略号`...`表示可变数量的参数。
例：

```c
int printf(const char* format, ...);
```

注意：`...`符号必须放在参数序列的结尾

1. 使用可变参数需要引入<stdarg.h>头文件
2. 在可变参数前，声明一个 va_list 类型的变量来存储可变参数，它必须在操作可变参数是，首先使用
3. 使用 va_start 函数初始化 va_list 类型的变量。他接受两个参数：
   - 参数 1：可变参数对象
   - 参数 2：原始函数里，可变参数之前的那个参数，用来为可变参数定位（在例子中是 count）
4. 使用 va_arg 函数来逐个获取可变参数的值。每次调用后，内部指针就会指向下一个可变参数。它接收两个参数：
   - 参数 1：可变参数对象
   - 参数 2：当前可变参数的类型
5. 使用 va_end 函数来结束可变参数的处理

```c
double average(int count, ...) {
    va_list args;   //args来存储可变参数
    va_start(args, count);  //用来初始化va_list变量,count是用来定位参数

    double sum = 0.0;

    for(int i = 0; i < count; i++) {
        int num = va_arg(args, int);
        sum += num;
    }

    va_end(args);   //结束可变参数的处理
    return sum / count;
}
```

### 指针函数(返回值是指针)

c 语言允许函数的返回值是一个指针(地址)，这样的函数称为`指针函数`
指针函数的定义的一般格式

```c
返回值类型 *函数名(形参列表) {
    函数体
}
```

### 函数指针(指向函数的指针)

> 一个函数本身就是一段内存里面的代码，总是占用一段连续的内存区域。这段内存区域也有首地址，把函数的这个首地址(或称入口地址)赋予一个指针变量，使指针变量指向函数所在的内存区域，然后通过指针变量就可以找到并调用该函数。这种指针就是`函数指针`。
> 简单的说，函数指针就是指向函数的指针。
> 格式：
> `返回值类型 (*指针变量名)(参数列表);`

```c
void print(int a) {
    printf("%d\n", a);
}
// 函数名也会退化成一个地址
int main() {
    void (*print_ptr)(int); //定义函数指针
    print_ptr = print; //等价于 print_ptr = &print; 为指针赋值，指向函数
    print_ptr(6);   //调用
    return 0;
}
```

==拓展：函数名也会退化成一个地址==
注意：

1. 指向函数的指针不能进行运算，+ -

### 回调函数

把函数指针作为参数传递给其它函数 b 中，此时的函数 b 就称为`回调函数`

### 函数的说明符(修饰符)

c 语言可以把函数分为两类--`内部函数`和`外部函数`

1. 内部函数(静态函数)
   只能在定义的文件中进行调用，不能被同一程序的其它文件中的函数调用，这种函数称为内部函数。
   此时，内部函数需要使用`static`修饰。
   定义内部函数的一般形式是：

```c
/**
*static 类型说明符 函数名(形参列表){
*    函数体
*}
*
*/
static int f(int a, int b) {
    //函数体
}
```

2. 外部函数--不加修饰符，则默认是外部函数
   外部函数在整个源程序中都有效，只要定义函数时，在前面加上 extern 关键字就行
   格式：
   `extern 类型说明符 函数名(形参列表)`
   例：

```c
extern int f(int a, int b) {
    // 函数体
}
//上面等价下面
int f(int a, int b) {
    // 函数体
}
```

## 变量--延申

### 变量分类--外部变量 与 内部变量

定义在函数的变量是全局变量，反之是局部变量。
注意：

1. 全局变量未显示初始化，会自动、默认初始化为 0 或空值，具体取决于数据类型。
2. 全局变量保存在内存的全局存储区中，占用静态的存储单元。
3. 局部变量保存在栈中
4. 尽量不用使用全局变量

### 变量存储方式--动态存储 与 静态存储

在 c 语言中，每一个变量都有两个属性：`数据类型`和`数据的存储类别`。

1. 动态(自动)存储方式
   
   > 动态存储方式：在程序运行期间根据需要进行动态的分配存储空间的方式，数据存放在动态存储区。

一般都是局部变量

在动态存储区中存放以下数据：

- 函数形参
- 函数中定义的局部变量且没有用 static 修饰的变量
- 函数调用时的返回地址

自动(动态)变量用`auto`作为存储类别的声明，关键字`auto`可以不写，不写则隐含指定为自动存储类别。

2. 静态存储方式
   
   > 静态存储方式：数据存放在`静态存储区`，它们在程序整个运行期间都不释放，所以生命周期为程序运行周期。

==局部变量使用`static`修饰后，则使用静态存储方式。(可以把局部变量生命周期变为程序运行周期)==

```c
static int a = 9;
```

全局变量大多存放在`静态存储区中`，==使用`static`修饰的全局变量称为`静态全局变量`,该变量只对当前文件可见==

### 变量的其它修饰符--register、extern、const

1. 寄存器变量(register 变量)
   
   > 一般情况下，变量(静态存储方式和动态存储方式)的值是存放在内存中的。用 register 修饰的变量是存放在寄存器中的。

```c
register int f; //定义f为寄存器变量
```

注：一般不自己指定

2. extern 修饰符

3. const 修饰符
- 常量
  `const int i = 10;`
- 常量数组

```c
const int arr[] = {1,2,3};
```

- 指针常量--指向常量的指针

```c
int num = 2;
const int *ptr = &num;
//下面是错误的
*ptr = 20;
```

- 常量指针--不能重新为指针赋值

```c
int *const ptr = &num1;
//下面是错误的
ptr = &num2;
```

- 结合以上两者

```c
const int *const ptr = &num1;
```

## 结构体

### 声明结构体

结构体在使用前要初始化

```c
struct 结构体名 {
    数据类型1 成员名1;
    数据类型2 成员名2;
    数据类型3 成员名3;
    数据类型4 成员名4;
    ...
};
//声明结构体变量
struct 结构体名 变量名;
//使用结构体成员
变量名.成员名;
//初始化(可以一个一个初始化)
struct Cat {
    char *name;
    int age;
};
struct Cat cat1 = {"akl", 3};
//部分初始化,其余值都是默认值
struct Cat cat2 = {"小黑"};
//初始化--显示初始化
struct Cat cat3 = {.age=3};
//声明结构体同时声明变量
struct Student {
    char *name;
    int age;
} stu1, stu2;
//匿名结构体--定义匿名结构体的同时定义变量
struct {
    char *name;
    int age;
} t1, t2;   //定义匿名结构体的同时定义变量

//为结构体起别名
typedef struct Employee {
    char *name;
    int age;
} emp;
//定义Employee结构体变量
emp emp1;
```

### 结构体嵌套使用

例：

```c
struct Name {
    char *firstName;
    char *lastName;
};
struct Student {
    //结构体嵌套使用
    struct Name name;
    int age;
};
//使用
struct Student stu1 = {.name={.firstName="sdji", .lastName="sdfef"}, .age=23};
struct Name name = {"dfe", "fe"};
stu1.name = name;
```

### 结构体占用的空间

结构体所占用的空间，不是各个属性存储空间的总和。为了计算效率，c 语言内存占用空间一般来说都是 int 类型的整数倍，`struct类型所占用的存储空间也是int类型的整数倍`
例：

```c
struct A {  //占用八个字节(两倍的int类型大小)
    char a; //一个字节
    int b;  //四个字节
}
```

### 结构体的赋值操作

同类型的结构体可以相互赋值(==这是值传递==)
例：

<!-- 这是值传递 -->

`stu1 = stu2;`

### 结构体数组

```c
struct Student stuArr[3];
//
struct Student {
    char *name;
    int age;
} stuArr[2];
//可以定义时同时初始化
struct Student {
    char *name;
    int age;
} stuArr[2] = {...};
```

### 结构体指针

结构体指针：指向结构体变量的指针(将结构体变量的首地址放在指针里)
定义格式：
`struct 结构体名 *结构体指针变量名;`
例：

```c
struct Student *stu;
//使用
(*stu).name;
//只有stu是结构体指针才能用->符号
stu->name;
```

### 指向结构体数组的指针

```c
struct Person arr[2]={{"ding", 10}, {"wang", 12}};
struct Person *p;
p = arr;    //指向数组
//调用
p->name;
(p+1)->name;
```

## 共用体

> c 语言提供了共用体类型(Union 结构)，用来自定义可以灵活变更的数据结构。它内部可以包含各种属性，但同一时间只能有一个属性，因为所有属性都保存在同一个内存地址，后面写入的属性会覆盖前面的属性。这样最大的好处是节省内存空间。
> 共用体与结构体的定义形式相似，但它们的含义是不同的。

- 结构体变量长度，是成员长度之和(一般会凑成 int 类型的整数倍)
- 共用体长度是最长的成员的长度

### 声明共用体

格式：

```c
union 共用体类型名称 {
    数据类型 成员1;
    数据类型 成员1;
    数据类型 成员1;
    ...
};

union Data
{
    short a;
    float b;
    char c;
};
//定义变量
union Data d1;
d1.a = 23;
//会擦除d1.a，一时间只能有一个值
d1.b = 10.9;
//共用体也支持->符号
```

共用体操作与结构体类似

## typedef

c 语言允许使用 typedef 为一个数据类型起一个别名，方便编码

### typedef 的使用

1. 场景 1--给基本数据类型起别名
   例：

```c
typedef int Integer;
Integer i = 10;
```

2. 场景 2--给结构体共用体起别名

```c
//写法1
struct Student
{
    char *name;
    int id;
    char gender;
    char profession;
    double score;
};
typedef struct Student Stu;
//写法2
typedef struct Student
{
    char *name;
    int id;
    char gender;
    char profession;
    double score;
} Student;
//写法3---匿名结构体起别名
typedef struct
{
    char *name;
    int id;
    char gender;
    char profession;
    double score;
} Student;
```

3. 场景 3--给指针类型起别名

```c
typedef int * INT_PTR;
//这是一个指针类型
INT_PTR ptr;
```

4. 场景 4--给数组类型起别名(==给数组起别名有点不一样==)

```c
typedef int FIVE_INTS[5];   //FIVE_INTS为 int [5]的别名
typedef char * PTR_CHAR[5]; //PTR_CHAR为char *[5]的别名
```

5. 场景 5--给函数指针起别名

```c
int (*ptr_compare)(int, int);   //这里ptr_compare是变量
ptr_compare = max;  //
typedef int (*PTR_FUNC)(int, int);  //PTR_FUNC就是别名
PTR_FUNC ptr_func = min;   //min是符合格式的函数名
```

## 常用函数

### 字符串相关函数

相关函数都在`string.h`头文件中

1. 返回字符串长度(不包含'\0')--strlen()
2. 字符串复制--strcpy()
3. 将字符串前 n 个字符复制到字符数组中去--strncpy()
4. 将两个字符串连接在一起--strcat()
5. 字符串比较--strcmp()
6. 改变字符串的大小写--strlwr()/strupr()

### 基本数据类型和字符串的转换

1. 基本数据类型--->字符串
   `sprintf()`函数可以将其它数据类型转换成字符串类型。此函数存放在`stdio.h`中

### 日期和时间相关函数

头文件为`time.h`

### Math 相关函数

头文件为`math.h`

## C 语言内存管理

C 语言中，不同数据在内存中分配说明：

1. 全局变量和静态局部变量--内存中的静态存储区/全局区
2. 非静态的局部变量--内存中的动态存储区：stack 栈
3. 临时使用的数据--建立动态内存分配区域，需要时随时开辟，不需要时及时释放--heap 堆
4. 根据需要向系统申请所需大小的空间，由于未在声明定义其为变量或者数组，不能通过变量名或者数组名来引用这些数据，只能通过指针来引用

C 程序内存分为以下几个部分：

1. stack 栈---存放普通局部变量
2. 堆(heap)空间---malloc 等函数动态分配的数据
3. 全局区/静态区/常量区
4. 代码区---存放自己编写的代码、指令等

### void 指针(无类型指针)

1. 每一块内存都有地址，通过指针变量可以获取指定地址的内存块。
2. 指针变量必须有类型，否则编译器无法知道如何解读内存块保存的二进制数据。

有时向系统请求内存的时候，不确定会有什么样的数据写入内存，需要先获取内存块，稍后再确定写入的数据类型。C 语言提供了一种不定类型的指针，叫 void 指针。

> void 指针：只有内存块的地址信息，没有类型信息，等到使用该块内存时，再向编译器补充说明，里面的数据类型是什么。(比如：malloc 函数，在使用 malloc 函数时才能知道是什么类型的指针)

void 指针等同于无类型指针，可以`指向任意类型`的数据，但是不能解读数据。

> 任何类型的指针都可以转为 void 指针，void 指针也可以转为任意类型的指针

#### void 指针的使用

在使用前应该显示转成目标类型

```c
void *r;
char c = 'c';
r = &c;
//使用前强转----(char *)r ：转成字符指针类型(char *)，使用*是获取指针(前面是转成指针)内的数据
char cc = *(char *)r;
//void可以指向不同类型
double d = 2.3;
r = &d;
```

### 内存分配函数

主要是：`malloc()` `calloc()` `realloc()` `free()`
头文件: `stdlib.h`

1. malloc()----掌握
   函数原型：

```c
void *malloc(unsigned int size);    //size为无符号整型
```

作用：在内存的堆区分配一个长度为 size 的连续空间，并返回该空间的首地址

==malloc 分配地址失败时，返回 NULL==

2. free()----掌握
   函数原型：

```c
void free(void *p);
```

3. calloc()----了解
   函数原型：

```c
void *calloc(unsigned int n, unsigned int size);
```

作用：在内存的堆区分配一个单位长度为 size，总长度为`n*size`的连续空间，并返回该空间的首地址

==与 malloc 差异:==calloc 分配空间后会自动进行初始化(初始化为 0，字符为'\0')

4. realloc()----掌握
   函数原型：

```c
void *realloc(void *p, unsigned int size);
```

作用：调整内存空间大小（原空间内的值会复制到新空间中）。将指针 p 所指向的动态空间大小改变为 size，单位为字节。返回值是一个新地址(也可能与原地址一样)。

分配失败返回 NULL

## 文件和流

C 程序中，对于文件中数据的输入/输出操作以`流(stream)`的方式进行，可以看做是一种数据的流动。

输入流：数据从数据源(比如：文件)到程序(或内存)的流动过程
输出流：数据从程序(或内存)到数据源(比如：文件)的流动过程

### C 输入&输出

输入源：可以是键盘、文件或者网络
输出源：可以是屏幕、打印机或者任意文件

> 键盘输入称为标准输入
> 屏幕输出称为标准输出

#### 标准文件 I/O

C 语言把所有的设备都当做文件。所有设备的处理方式与文件相同。
以下三个文件会在程序执行时自动打开，以便访问键盘和屏幕。

| 标准文件 | 文件指针 | 设备 |
| -------- | -------- | ---- |
| 标准输入 | stdin    | 键盘 |
| 标准输出 | stdout   | 屏幕 |
| 标准错误 | stderr   | 屏幕 |

文件指针是访问文件的方式

#### scanf()和 printf()函数

scanf()从 stdin 读取输入
printf()向 stdout 输出

#### C 语言文件读写

##### 创建/打开文件

使用 fopen()函数来创建一个新的文件或者打开一个已有的文件

函数原型：

```c
//filename是字符串，用来命名文件;访问模式mode的值可以下面表中的一个
FILE *fopen(const char *filename, const char *mode);
```

| 模式 | 描述                                                                                                                       |
| ---- | -------------------------------------------------------------------------------------------------------------------------- |
| r    | 只读。打开一个已有的文本文件，只允许读取                                                                                   |
| w    | 只写。打开一个文件，只允许写入文件。如果文件不存在，则会创建一个新文件。如果文件存在，则以覆盖原文件方式写入               |
| a    | 追加。如果文件不存在，则会创建一个新文件。如果文件存在，则以追加方式写入                                                   |
| rb   | 只读(二进制)。打开一个二进制文件，只允许读取                                                                               |
| wb   | 只写(二进制)。打开一个二进制文件，只允许写入文件。如果文件不存在，则会创建一个新文件。如果文件存在，则以覆盖原文件方式写入 |
| ab   | 追加(二进制)。如果文件不存在，则会创建一个新文件。如果文件存在，则以追加方式写入                                           |

| 模式 | 描述                                                                                                |
| ---- | --------------------------------------------------------------------------------------------------- |
| r+   | 读写。打开一个文本，允许读取和写入                                                                  |
| w+   | 读写。打开一个文本，允许读取和写入。如果文件不存在，则创建一个新文件。如果存在，覆盖写入。          |
| a+   | 读写。打开一个文本，允许读取和写入。如果文件不存在，则创建一个新文件。如果存在，追加写入。          |
| r+b  | 或"rb+"：读写(二进制)。打开一个二进制文件，允许读取和写入。                                         |
| w+b  | 或"wb+"：打开一个二进制文本，允许读取和写入。如果文件不存在，则创建一个新文件。如果存在，覆盖写入。 |
| a+b  | 或"ab+"：打开一个二进制文本，允许读取和写入。如果文件不存在，则创建一个新文件。如果存在，追加写入。 |

##### 关闭文件

使用完文件后，一定要用`fclose()`将该文件关闭。

函数原型：

```c
int fclose(FILE *fp);
```

- 如果成功关闭，`fclose()`返回`0`。此时，会清空缓冲区的数据，关闭文件，并释放用于该文件的所有内存。
- 如果关闭失败，`fclose()`返回`EOF`。`EOF`是定义在头文件`stdio.h`中的常量。

##### 写入文件

1. fputc 函数

函数原型：

```c
int fputc(int c, FILE *fp);
```

> 说明：函数 fputc()把参数 c 的`字符值`写入到 fp 所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回`EOF`。

2. fputs 函数

函数原型：

```c
int fputs(const char *str, FILE *fp);
```

> 说明：函数 fputs()把参数`str`的`字符串`写入到 fp 所指向的输出流中。如果写入成功，它会返回一个`非负值`，如果发生错误，则会返回`EOF`。

3. fprintf 函数

函数原型：

```c
int fprintf(FILE *fp, const char *format, ...);
```

##### 读取文件到内存

1. fgetc 函数

函数原型：

```c
int fgetc(FILE *fp);
```

> 说明：fgetc()函数从 fp 所指向的输入文件中读取一个字符。返回值是读取的字符，如果发送错误则返回`EOF`。

2. fscanf 函数

函数原型：

```c
int fscanf(FILE *fp, const *format, ...);
```

> 说明：使用`fscanf()`函数从文件中读取格式化的数据，比如整型、浮点等各种类型的数据。format 参数指定了数据格式，后面的参数是用于存储读取数据的变量。

> 如果使用`fscanf()`函数从文件中读取字符串，会在遇到第一个空白字符(空格、制表符、换行符等)时，停止读取，之后的内容会被忽略。

> 如果函数成功读取了一些数据并按照指定格式存储到相应的变量中，它会返回成功读取的参数个数。或者在读取失败或文件结束时返回`EOF`。

3. fgets 函数

函数原型：

```c
char *fgets(char *buf, int n, FILE *fp);
```

> 说明：此函数按行读取数据，它从文件中读取一行数据(包括换行符'\n')，并将这一行的内容存储到指定的缓冲区。参数中 buf 是用于存储读取的文本的缓冲区，n 是缓冲区的大小，fp 是文件指针。

> 如果成功读取，返回参数 buf，即读取到的字符串首地址。如果到达文件末尾或者读取失败，返回`NULL`

> 如果成功读取一行，下次读取时会从下一行读取。

## gcc常用命令

### 基础编译命令

| 命令                           | 说明                                                          |
| ------------------------------ | ------------------------------------------------------------- |
| gcc hello.c                    | 编译生成默认的可执行文件 a.exe (Windows) 或 a.out (Linux/Mac) |
| gcc hello.c -o hello           | 编译并指定输出文件名为 hello.exe 或 hello                     |
| gcc file1.c file2.c -o program | 编译多个源文件并链接成可执行文件                              |

### 常用编译选项

#### 1. 控制编译过程

```bash
# 只预处理（展开宏、头文件），生成 .i 文件
gcc -E hello.c -o hello.i

# 编译到汇编代码，生成 .s 文件
gcc -S hello.c -o hello.s

# 编译到目标文件（不链接），生成 .o 文件
gcc -c hello.c -o hello.o

# 链接目标文件生成可执行文件
gcc hello.o -o hello
```

#### 2. 优化选项

```bash
# 基本优化（减少代码大小和执行时间）
gcc -O1 hello.c -o hello

# 更多优化（推荐用于生产环境）
gcc -O2 hello.c -o hello

# 最高级别优化（可能增加编译时间）
gcc -O3 hello.c -o hello

# 优化代码大小
gcc -Os hello.c -o hello

# 无优化（默认，便于调试）
gcc -O0 hello.c -o hello
```

#### 3. 调试信息

```bash
# 生成调试信息（供 GDB 使用）
gcc -g hello.c -o hello

# 生成更详细的调试信息
gcc -ggdb hello.c -o hello

# 调试信息 + 优化（调试优化后的代码）
gcc -g -O1 hello.c -o hello
```

#### 4. 警告信息

```bash
# 开启常用警告
gcc -Wall hello.c -o hello

# 开启更多警告
gcc -Wextra hello.c -o hello

# 将警告当作错误处理
gcc -Werror hello.c -o hello

# 开启所有警告
gcc -Wall -Wextra -Wpedantic hello.c -o hello
```

#### 5.指定语言标准

```bash
# C语言标准
gcc -std=c99 hello.c -o hello   # C99 标准
gcc -std=c11 hello.c -o hello   # C11 标准
gcc -std=c17 hello.c -o hello   # C17 标准
gcc -std=c2x hello.c -o hello   # C23 实验性支持

# C++语言标准（使用 g++）
g++ -std=c++11 hello.cpp -o hello  # C++11
g++ -std=c++14 hello.cpp -o hello  # C++14
g++ -std=c++17 hello.cpp -o hello  # C++17
g++ -std=c++20 hello.cpp -o hello  # C++20
```

## C 语言多文件编程

### 预处理指令

#### 1.文件包含指令

```c
#include <stdio.h>    // 包含系统头文件
#include "myheader.h"  // 包含用户头文件
```

#### 2.宏定义指令

```c
#define PI 3.14159           // 定义常量宏
#define MAX(a,b) ((a)>(b)?(a):(b))  // 定义函数宏
#undef PI                   // 取消宏定义
```

#### 3.条件编译指令

```c
#ifdef DEBUG               // 如果DEBUG已定义
    // 调试代码
#endif

#ifndef HEADER_H          // 如果HEADER_H未定义
#define HEADER_H
    // 头文件内容
#endif

#if OS == 1              // 条件判断
    // Windows代码
#elif OS == 2
    // Linux代码
#else
    // 其他系统代码
#endif

// 特殊操作符
#if defined(DEBUG) && defined(VERBOSE)
// defined() 操作符检查宏是否定义
```

#### 4.编译控制指令

```c
#pragma once             // 防止头文件重复包含
#pragma pack(1)          // 设置结构体对齐方式
#pragma warning(disable: 4996) // 禁用特定警告
#pragma message("编译信息") // 输出编译信息
```

### 多文件编程

> c 语言的多文件编程，是通过头文件来进行的

```c
// 文件结构
project/
├── main.c
├── math_utils.h
└── math_utils.c


// math_utils.h 头文件定义
#ifndef MATH_UTILS_H
#define MATH_UTILS_H
// 函数声明
int add(int a, int b);
int multiply(int a, int b);

#endif


// math_utils.c 头文件中声明的函数实现
// 必须引入头文件
#include "math_utils.h"
// 函数定义
int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}



// main.c
#include <stdio.h>
#include "math_utils.h"  // 包含头文件

int main() {
    int sum = add(5, 3);      // 使用声明
    int prod = multiply(5, 3); // 使用声明
    printf("Sum: %d, Product: %d\n", sum, prod);
    return 0;
}
```



# C++

## 基础类型变量

## 类型转换
> C++支持C风格的类型转换，但也定义了一些自己的类型转换方法
> 1. static_cast    - 静态转换（编译时检查）
> 2. dynamic_cast   - 动态转换（运行时检查）
> 3. const_cast     - 常量转换（移除 const 属性）
> 4. reinterpret_cast - 重新解释转换（位级转换）

**1. static_cast 静态类型转换** 
> static_cast 是编译时检查，编译器根据已知类型信息进行转换，即使转换错误，编译器也不会报错，无运行时开销。
```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default;
    void baseFunc() { std::cout << "Base function\n"; }
};

class Derived : public Base {
int data = 100;
public:
    void derivedFunc() { std::cout << data << "Derived function\n"; }
};

int main() {
    int a = 10;
    double b = (double)a;
    double b2 = static_cast<double>(a);

    Derived derived;
    Base* bptr = &derived;
    Derived* d = (Derived*)&(*bptr);    // C风格转换
    Derived* d2 = static_cast<Derived*>(bptr);  // C++风格转换

    // void*转换
    int value = 100;
    void* vptr = &value;
    int* intptr = static_cast<int*>(vptr);

    d->derivedFunc();
    d2->derivedFunc();

    // 向下转换（危险！）
    Base* b3 = new Base();
    Derived* d3 = static_cast<Derived*>(b3);  // 编译通过，但运行时会出错！
    d3->derivedFunc();
}
```

**2. dynamic_cast 动态转换**
> 运行时检查，使用RTTI（Run-Time Type Information）进行类型检查，转换失败时返回`nullptr`（指针）或抛出异常（引用）,有运行时成本,要求：必须用于多态类型（至少有一个虚函数）

```cpp
class Base1 {
    public:
        virtual ~Base1(){}
};

class Derived1 : public Base1 {};
class Derived2 : public Base1 {};

int main() {
    Base1* b11 = new Base1();
    Derived1* d11 = dynamic_cast<Derived1*>(b11);
    if (d11) {
        // 转换成功
        std::cout<<"success!\n";
    } else {
        std::cout<<"error!";
    }
}
```

## 数组

```cpp
// 传统c数组
// c数组就是一个指针常量
int a[8];
int* ap = a;
for (int i = 0; i < 8; ++i) {
    a[i] = i;
}
// 使用new声明一个长度为8的int数组，返回一个int指针
int* aptr = new int[8];

// c++11中的数组，相较于上面两种，提供了边界检测和数组长度
std::array<int, 8> na = {1,3};
std::cout<<"数组长度："<<na.size()<<"\n";
```

### 数组初始化
> 以上说明的三种方式定义的数组初始化行为是一致的，以下只介绍传统数组初始化行为，其余两种一样

```cpp
int arr1[5];                    // 未初始化，包含随机值
int arr2[5] = {1, 2, 3, 4, 5};  // 完全初始化
int arr3[] = {1, 2, 3, 4, 5};   // 自动推断大小，这个其余两个没有
int arr4[5] = {1, 2};           // 部分初始化，其余为0

// 使用new方法
int* aptr = new int[8]{1,3};    // 部分初始化，其余为0

// 使用c++11中的数组
std::array<int, 8> na = {1,3};  // 部分初始化，其余为0
```

### 多维数组
> 二维数组的性能比较差，因为内存是分散开的，最佳实践是将二维数组存储为一维数组

```cpp
// 二维数组
int** a2d = new int*[10];
// 为二维数组的每位元素分配空间
for(int i = 0; i < 10; ++i) {
    a2d[i] = new int[10];
}

// 释放空间
for(int i = 0; i < 10; ++i) {
    delete[] a2d[i];
}
```


## 标准数组（std::array）
> C++中提供了一个标准数组它与C风格的数组使用完全一致，但是它额外提供了数组长度和边界检查，所以更推荐使用标准数组而不是C风格数组

```cpp
#include <array>
#include <iostream>

template<typename T, int N>
void PrintArray(const std::array<T, N>& data) {
    // 通过size方法获取长度
    for (int i = 0; i < data.size(); ++i) {
        std::cout<<data[i]<<std::endl;
    }
}

int main() {
    // 标准数组是在栈上分配的与C风格数组一致，动态数组是在堆上分配的
    std::array<int, 5> a = {1,2,3,4,5};
    PrintArray<int, a.size()>(a);
}
```

## 字符串
> c++是兼容c语言类型的字符串的，下面介绍c++的字符串

### 定义并构造字符串
```cpp
#include <string>
// c++中用双引号的默认是const char*类型，双引号的是c类型字符串
const char* ss = "string";

// string类型底层储存字符的是char数组
// string构造字符串
std::string s1;     // 空字符串
std::string s2("hello");     // 从c字符串构造, 等同于std::string s2 = "hello"
std::string s3 = "World";          // 直接赋值
std::string s4(5, 'A');            // "AAAAA"
std::string s5(s2);                // 拷贝构造
std::string s6 = s2 + " " + s3;    // 连接 "Hello World"

// std::string s7 = "hello " + "strig";    错误！因为c字符串没有重载加号
// 在c++14中可以使用下面的方法进行字符串拼接
using namespace std::literals;
std::string s3 = "ddw"s + "yyds";
```

### 字符串常用操作
```cpp
string str = "Hello C++ World";
    
// 1. 拼接
str += "!";                    // "Hello C++ World!"
str.append("!!!");             // "Hello C++ World!!!!"
str.push_back('?');            // 添加单个字符

// 2. 插入
str.insert(5, " Awesome");     // 在位置5插入

// 3. 替换
str.replace(6, 3, "CPP");      // 替换从位置6开始的3个字符

// 4. 删除
str.erase(5, 8);               // 从位置5删除8个字符
str.pop_back();                // 删除最后一个字符

// 5. 提取子串
string sub = str.substr(0, 5);  // 从位置0开始的5个字符

// 6. 查找
size_t pos = str.find("C++");   // 查找第一次出现的位置
if (pos != string::npos) {
    cout << "找到C++，位置: " << pos << endl;
}
    
pos = str.rfind("o");           // 从后往前查找
pos = str.find_first_of("aeiou"); // 查找元音字母第一次出现
pos = str.find_last_of("aeiou");  // 查找元音字母最后一次出现
pos = str.find_first_not_of(" "); // 查找第一个非空格字符

// 7. 比较
string a = "apple", b = "banana";
if (a < b) {                    // 字典序比较
    cout << a << " 小于 " << b << endl;
}

// 8. 转换C字符串
const char* cstr = str.c_str();  // 获取C字符串（只读）
char* data = str.data();         // 获取数据指针（C++11后可修改）
```

### 字符串格式化(c++20)
```cpp
#include <format>  // C++20
// 基本格式化
string s1 = format("Hello {}!", "World");
```

## 函数


## 头文件

> 用来存储函数声明的文件，告诉c++某个源文件中存在哪些函数

```cpp
// #pragma once    // 只允许头文件被包含一次，与下面的ifnedf意义一样

#ifndef _LOG_H
#define _LOG_H

void Log(const char* msg);

#endif
```

> 事实上头文件也可以用于区分c标准库和c++标准库，c标准库有`.h` c++没有
> 
> 例如：c标准库 `#include<stdio.h>` c++标准库 `#include<iostream>`

## 分支语句

> 与c语言一样



## 循环语句

> 与c语言一样



## 指针

### 指针基础概念

> 指针是一个变量，存储的是另一个变量的内存地址。

```cpp
int main() {
    int var = 8;
    int* p = &var;
    // 定义一个长度为8的字节指针
    char* c = new char[8];
}




```

### 指针的类型

#### 空指针

```cpp
// NULL(c风格)，其指针变量存储的地址为0，指针变量指向的值为空（没有东西）
// nullptr，其指针变量存储的地址为空（没有东西），指针变量指向的值为空（没有东西）


int* p1 = nullptr;  // C++11推荐方式
int* p2 = NULL;     // C风格（不推荐）
int* p3 = 0;        // 也可以

if (p1 == nullptr) {
    // 指针为空时的处理
}
```

#### void指针

> 可以指向任何类型，但不能直接解引用

```cpp
int num = 10;
double d = 3.14;
void* ptr;

ptr = &num;     // ✅ 可以指向int
ptr = &d;       // ✅ 也可以指向double

// cout << *ptr; // ❌ 错误：不能解引用void指针
int* intPtr = static_cast<int*>(ptr);  // 需要转换后才能使用 100
```

#### 指针的指针

```cpp
int value = 100;
int* ptr = &value;      // 一级指针
int** pptr = &ptr;      // 二级指针

cout << **pptr;  // 输出 100
```

### 指针与数组

#### 数组名是指针常量

```cpp
int arr[5] = {1, 2, 3, 4, 5};
int* p = arr;  // 等价于 &arr[0]

// 指针算术
cout << *(p + 2);  // 输出 3（arr[2]）
p++;               // 指向下一个元素（arr[1]）
```

#### 遍历数组的多种方式

```cpp
int arr[5] = {10, 20, 30, 40, 50};

// 方法1：下标
for(int i = 0; i < 5; i++) {
    cout << arr[i] << " ";
}

// 方法2：指针
for(int* p = arr; p < arr + 5; p++) {
    cout << *p << " ";
}

// 方法3：指针+偏移
for(int i = 0; i < 5; i++) {
    cout << *(arr + i) << " ";
}
```

### 指针与函数

#### 指针作为函数参数

```cpp
// 通过指针修改外部变量
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int x = 5, y = 10;
swap(&x, &y);  // x=10, y=5
```

#### 指针作为函数返回值

```cpp
int* findMax(int* arr, int size) {
    int* max = arr;
    for(int i = 1; i < size; i++) {
        if(arr[i] > *max) {
            max = &arr[i];
        }
    }
    return max;  // 返回指向最大元素的指针
}
```

#### 函数指针

```cpp
// 指向函数的指针
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }

int (*funcPtr)(int, int);  // 声明函数指针
funcPtr = add;             // 指向add函数
cout << funcPtr(5, 3);     // 输出 8

funcPtr = subtract;        // 指向subtract函数
cout << funcPtr(5, 3);     // 输出 2
```

### 动态内存分配

#### new 和 delete
> new会在堆上分配一块空间并将该空间的首地址返回，分配的空间不会自动释放，必须使用delete释放空间

```cpp
// 单个对象
int* p = new int(42);
*p = 100;
delete p;  // 释放内存
p = nullptr;  // 避免悬空指针

// 数组
int* arr = new int[10];
for(int i = 0; i < 10; i++) {
    arr[i] = i * 10;
}
delete[] arr;  // 释放数组内存智能指针（C++11起）智能指针（C++11起）**
```

注意：在局部作用域中声明的变量（不用new）是在栈上分配空间的，离开局部作用域之后就会释放空间，所以返回这块空间的地址会导致空指针异常。

```cpp
int* CreateArray() {
    // 下面的数组在函数运行结束后会被释放，返回的地址指向的空间已经被回收，会导致异常
    // int a[3] = {2,3,4};  错误！
    // 正确的做法是使用new在堆上分配空间，如果不主动使用delete释放空间，不会释放分配的空间
    int* a = new int[3]{2,3,4};
    return a;
}
```

### 智能指针（C++11起）

> 使用new分配空间得到的指针是不会自动释放的可能会导致内存泄漏，更安全的指针管理，自动释放内存

#### unique_ptr 智能指针
> unique_ptr 是一种作用域指针，当该指针超出作用域时会自动销毁
> 
> unique_ptr 是独占的，因为当有第二个指针指向同一个地址时，第一个指针被自动销毁时会导致第二个指针指向空

```cpp
#include <memory>

class Entity {
public:
    int x;
    Entity(int x) : x(x) {
        std::cout<<"Created Entity\n";
    }
    ~Entity() {
        std::cout<<"Destroyed Entity\n";
    }
};

int main() {
    // unique_ptr：独占所有权
    std::unique_ptr<int> uptr = std::make_unique<int>(100);
    {
        // std::make_unique<Entity>(3) 参数是构造函数所需要的参数
        std::unique_ptr<Entity> e = std::make_unique<Entity>(3);
        e->print();
    }   // 离开这个作用域会自动销毁e

    // 像一个普通指针一样使用智能指针
    std::unique_ptr<int> iptr = std::make_unique<int>(100);
    int a = *iptr;
}
```

#### shared_ptr 智能指针
> `shared_ptr指针` 是一种可以被重复指向的智能指针，它底层采用引用计数的方式，每当有一个`shared_ptr指针`指向同一块地址计数都会加一，如果指向一块地址的计数为0时会自动释放这块地址。
>
> `shared_ptr指针`在超出作用域时，也会自动销毁（但不一定释放空间，因为可能还有其它指针指向它）

```cpp
#include <memory>

class Entity {
public:
    int x;
    Entity(int x) : x(x) {
        std::cout<<"Created Entity\n";
    }
    ~Entity() {
        std::cout<<"Destroyed Entity\n";
    }
};

int main() {
    std::shared_ptr<Entity> sptr2;
    {
        // shared_ptr：共享所有权（引用计数）
        std::shared_ptr<Entity> sptr1 = std::make_shared<Entity>(200);
        sptr2 = sptr1;  // 引用计数+1
    }   // 这里不会释放sptr1所指向的那块空间，因为还有sptr2指向它
}
```

#### weak_ptr
> `weak_ptr指针`指向`shared_ptr指针`所指向的空间时不会增加`shared_ptr指针`的引用计数

```cpp
int main() {    
    {
        // shared_ptr：共享所有权（引用计数）
        std::shared_ptr<Entity> sptr1 = std::make_shared<Entity>(200);
        // weak_ptr：弱引用，不增加引用计数
        std::weak_ptr<int> wptr = sptr1;
    }  
}
```

## 引用

> 引用就是一个变量的别名，操作引用就是操作原变量本身（创建一个指向原变量的指针）

```cpp
int value = 42;
int& ref = value;  // ref是value的引用

ref = 100;         // 修改ref等同于修改value
cout << value;     // 输出 100
cout << ref;       // 输出 100
cout << &value;    // 输出value的地址
cout << &ref;      // 输出相同的地址

// 引用在函数中使用
void swap(int& a, int& b) {
    int t = a;
    a = b;
    b = t;
}
```

### 引用的特性

* **必须初始化**：定义时必须指定引用的对象

* **不能重新绑定**：一旦初始化，永远引用同一个对象

* **没有空引用**：必须引用有效的对象

* **语法简洁**：使用时不需特殊操作符

```cpp
int& ref2;    // ❌  引用必须要指定一个被引用对象

int a = 6;
int b = 8;
int& ref = a;
ref = b;    // 这里实际上是将b赋值给a
```



## 枚举

### 传统枚举的使用（C风格枚举）
> 传统类型的枚举不构成一个命名空间，也就是说如果两个枚举中的变量重名了会产生冲突。

```cpp
// 定义的的一个普通枚举
enum Num {
    a, b, c
};
// 使用，只能赋值为a b c
Num n = a;


// 指定枚举的默认值,全部指定
enum Num {
    a=9, b=1, c=5
};
// 如果部分指定
// a = 0 , b = 90 , c = 91
enum Num {
    a, b=90, c
};

// 枚举都是整数类型，但是可以选择指定的整数类型
// 例如选择unsigned short
enum Num : unsigned short {
    a, b=8, c
};

// 枚举变量重名冲突
enum Color { RED, GREEN, BLUE };
enum TrafficLight { RED, YELLOW, GREEN };  // 编译错误！RED、GREEN重复定义
```

### c++11 强类型枚举（enum class）
> 相比于传统枚举，自带命名空间，不会冲突

```cpp
enum class Color {
    RED,
    GREEN,
    BLUE
};
// 下面的不会冲突
enum class TrafficLight {
    RED,
    YELLOW,
    GREEN
};

// 简单使用
Color c = Color::RED;      // 必须使用作用域
TrafficLight t = TrafficLight::RED;  // 不会冲突

// 不会隐式转换为整数
// int x = Color::RED;      // 编译错误！
int x = static_cast<int>(Color::RED);  // 必须显式转换

// 可以比较同类型
if (c == Color::RED) {
    cout << "是红色" << endl;
}

// 不同类型的枚举不能直接比较
// if (c == t) { }  // 编译错误！
```

## const关键字

### const和指针
> const修饰指针的记忆技巧：const 在 * 左边修饰指向的内容，右边修饰指针本身

```cpp
int main() {
    // const变量必须初始化
    const int MAX_AGE = 90;

    // const指针
    int n1 = 10;
    int n2 = 11;
    
    // 1. 指向常量的指针（指针指向的内容不可修改）
    const int* p1 = &n1;
    //*p1 = 90; // 错误：不能修改指向的内容
    p1 = &n2;   // 正确：可以改变指向

    // 2. 常量指针（指针本身不可修改）
    int* const p2 = &n1;
    *p2 = 90;   // 正确：可以修改指向的内容
    //p2 = &n2; // 错误：不能改变指向

    // 3. 指向常量的常量指针（都不能修改）
    const int* const p3 = &n1;
    // p3 = &n2;    // 错误：不能修改内容
    // *p3 = 90;    // 错误：不能改变指向

    // 记忆技巧：const 在 * 左边修饰指向的内容，右边修饰指针本身
}
```

### const和引用
> 不能通过const修饰的引用去修改被引用变量的值
> 其实也容易理解，引用时指针的一个语法糖，const在前的指针是不能修饰指向地址内的值的

```cpp
int a = 10;
// 1. const 引用
const int& ref1 = a;
// ref1 = 90;   // 错误：不能通过const修饰的引用修改变量值

// 2. const 引用可以绑定到临时对象
const int& ref2 = 100;           // 正确：临时对象
const int& ref3 = a + 20;        // 正确：临时对象

// 3. 非常量引用不能绑定到临时对象
// int& ref4 = 100;  // 错误：不能绑定到临时对象
```

### const在类中的应用
> 在上面可以看到const修饰的引用是不能修改值的
> 当引用指向对象中的函数可能会修改类中的成员变量时会违背上面的规定，所以可以用const修饰成员函数

```cpp
class Base {
int a = 0;
public:
    void SetA() {
        int a = 90;
        this->a = a;
    }
    // const修饰表示这个函数不会修改类中成员变量的值
    int SetA() const {
        return a;
    }
};

void test(const Base& b) {
    // b不能修饰Base中成员变量
    // 所以不能调用第一个SetA函数，这里会自动调用第二个
    std::cout<<b.SetA();
}
```

## mutable关键字
> 关键字用于修饰类的成员变量，允许它在 const 成员函数 中被修改

```cpp
class Base {
int a = 0;
// 可以被const修饰的成员函数修改
mutable int b = 1;
public:
    void SetA() {
        int a = 90;
        this->a = a;
    }
    int SetA() const {
        return a;
    }

    int SetB() const {
        // 可以修改b
        return ++b;
    }
};
```

## 类

```cpp
// 声明一个类
class Player {
// c++中，类的成员默认是私有的
int p;  // 这个是私有的

public: // 这里之后在没有遇见新的访问修饰符之前，都是这个修饰符起作用
    int x, y;
    int speed;

    void Move(Player player, int xa, int xy) {
        player.x += xa * player.speed;
        player.y += xy * player.speed;
    }
};
// 定义这个类的对象
Player player;
player.x = 5;
player.Move(player, 9, 8);
```
### 类的可见性
> c++中可见性修饰符只有三种：`private`、`protected`、`public`
> `private`：只有类内部可以访问，派生类和外部都不能访问
> `protected`：可以在类内部访问、派生类可以访问、类外部不能访问
> `public`：任何地方都可以访问，类内外、派生类、友元都可以访问


### 构造函数
> 在c++中如果不定义构造函数，会自动提供一个无参构造函数

```cpp
class Entity {
    public:
        float x, y;
        // 定义无参构造函数
        Entity() {
            x = 9.8f;
            y = 29.3f;
        }
        Entity(float x, float y) {
            this->x = x;
            this->y = y;
        }
        void print() {
            std::cout<<x<<", "<<y<<"\n";
        }
};
// 使用
// 默认使用无参构造函数
Entity e;
// 调用无参构造函数
Entity e = Entity();
// 调用有参构造函数
Entity e = Entity(3,4);
```

### 成员初始化列表
> 用于初始化类的成员变量，比在构造函数中初始化性能要好。

```cpp
class E {
public:
    int a;
    std::string m_str;
    E(int a, std::string str) : a(a), m_str(str) {}
    E() {
        a = 90;
        m_str = "ddd";
    }
};

class Example {
private:
    int a;
    double b;
    E e;
public:
    // e是一个对象，直接在里面写构造函数所需要的参数个数
    Example(): a(9), b(9.3), e(3, "www") {

    }
    // 上面与下面效果等同，但是上面的性能更好
    // Example() {
    //     a = 9;
    //     b = 9.3;
    // }
    Example(int a, double b, E e): a(a), b(b), e(e){}
    
};
```

### 析构函数
> c++中初始化对象会调用类的构造函数，销毁对象时会调用析构函数
>
> 调用时机：
> - 当对象定义于一个局部作用域中时，离开作用域时会自动调用析构函数
> - 当使用new定义的一个对象时，使用delete时会调用析构函数

```cpp
class Entity {
    public:
        float x, y;

        Entity() {
            x = 9.8f;
            y = 29.3f;
        }
        Entity(float x, float y) {
            this->x = x;
            this->y = y;
        }
        // 析构函数
        ~Entity() {
            std::cout<<"执行析构函数";
        }
        void print() {
            std::cout<<x<<", "<<y<<"\n";
        }
};
// 自动调用
int main() {
    Entity e = Entity(3,4);
    e.print();
}   // 在离开这个作用域时会自动调用析构函数
// 手动调用
Entity e = Entity(3,4);
e.~Entity();
```

### 拷贝构造函数
**1. 浅拷贝**
> 只复制指针而不复制指针所指向的内容。在浅拷贝中，当一个对象被复制到另一个对象时，只是简单地将对象的非指针成员变量的值进行复制。对于指针成员变量，只是复制指针的值，而不是指针所指向的数据。

**2. 深拷贝**
> 为每个数据成员分配新的内存并复制内容，适用于包含动态分配资源的对象，以确保每个对象独立管理自己的资源。
> 在深拷贝中，对于对象中的指针成员变量，不仅复制指针的值，还会为副本对象的指针重新分配内存，并将源对象指针所指向的数据完整地复制到新分配的内存中。这样，源对象和副本对象的指针成员分别指向不同的内存区域，它们的数据相互独立。

> 在没有拷贝构造函数时，C++对象拷贝默认是浅拷贝，这可能会导致重复回收地址的异常

```cpp
#include <cstring>
#include <iostream>

class String{
public:
    char* m_buffer;
    int m_size;
    String(const char* string) {
        m_size = strlen(string);
        m_buffer = new char[m_size + 1];
        memcpy(m_buffer, string, m_size + 1);
        m_buffer[m_size] = 0;
    }
    ~String() {
        delete[] m_buffer;
    }
};

int main() {
    {
        String s1 = "s1";
        // 这里是浅拷贝，所以s2中的m_buffer会指向s1中的m_buffer地址
        String s2 = s1; 
    } // 这里s1和s2都会销毁调用析构函数，因为两个m_buffer所指向的地址是相同的，所以这里调用两次析构函数会导致重复释放内存异常 
    std::cout<<"nn\n";
}
```

> 添加一个拷贝构造函数会解决上述问题，会让对象进行深拷贝

```cpp
#include <cstring>
#include <iostream>

class String{
public:
    char* m_buffer;
    int m_size;
    String(const char* string) {
        m_size = strlen(string);
        m_buffer = new char[m_size + 1];
        memcpy(m_buffer, string, m_size + 1);
        m_buffer[m_size] = 0;
    }
    // 拷贝构造函数，实际上就是一个接收本类型参数的构造函数
    String(const String& other) {
        m_size = other.m_size;
        m_buffer = new char[m_size + 1];
        memcpy(m_buffer, other.m_buffer, m_size + 1);
        m_buffer[m_size] = 0;
    }
    ~String() {
        delete[] m_buffer;
    }
};

int main() {
    {
        String s1 = "s1";
        String s2 = s1;
    }
    std::cout<<"nn\n";
}
```


### 类的实例化
> c++中有两种实例化方式
> 方式一：
>   在栈上创建对象，超出作用域自动销毁，开销小性能高
>   Entity e = Entity();    
>
> 方式二：
>   在堆上创建对象，new总是返回一个指针，需要使用delete释放空间
>   Entity* e = new Entity(8);

```cpp
class Entity {
public:
    int a;
    Entity(): a(3) {}
    Entity(int a): a(a) {}
};

int main() {
    // 容易理解的实例化
    Entity e = Entity();

    // 会自动寻找合适的构造函数，生成实例对象e3
    Entity e3(3);

    // 在堆上分配空间创建对象
    Entity* e1 = new Entity(9); // 有参构造
    Entity* e2 = new Entity();  // 无参构造
}
```

### 类的隐式构造与explicit关键字
> c++除了上面的实例对象方法，还可以采用下面的方法

```cpp
class Entity {
private:
    int age = 0;
    std::string str = "entity";
public:
    Entity(int age): age(age) {}
    Entity(std::string str): str(str) {}

    void print() {
        std::cout<<age<<" "<<str<<std::endl;
    }
};

// 隐式调用第一个构造函数
Entity e2 = 3;
// 会隐式调用第二个构造函数,字符串字面量不是std::string类型，所以需要转换一下
Entity e3 = std::string("str");
```

> 也可以使用`explicit`关键字阻止隐式构造

```cpp
class Entity {
private:
    int age = 0;
    std::string str = "entity";
public:
    Entity(int age): age(age) {}
    explicit Entity(std::string str): str(str) {}

    void print() {
        std::cout<<age<<" "<<str<<std::endl;
    }
};

// 错误！因为Entity(std::string str): str(str) {}构造函数被explicit修饰
// Entity e3 = std::string("str");
```

### this指针
> this实际上是指向该方法所属的当前对象实例的指针

### 类的继承

#### 继承方式
> C++中有三种继承方式：public继承、protected继承和private继承。它们控制基类成员在派生类中的访问权限。
> public继承不会改变父类成员的访问权限。（父类中的private在子类中不可见）
> protected继承会将父类成员的public改为protected，protected不变，（父类中的private在子类中不可见）
> private继承会将父类成员的访问权限改为private（父类中的private在子类中不可见）

```cpp
class Base {
private:
    int private_member = 1;
protected:
    int protected_member = 2;
public:
    int public_member = 3;
    
    void showBase() {
        cout << "在Base内部：可以访问所有成员" << endl;
        cout << "private: " << private_member << endl;
        cout << "protected: " << protected_member << endl;
        cout << "public: " << public_member << endl;
    }
};

// 1. public 继承
class PublicDerived : public Base {
public:
    void show() {
        // cout << private_member;  // 错误！不能访问基类的private成员
        cout << "public继承 - protected成员: " << protected_member << endl;  // ✓ 可以访问
        cout << "public继承 - public成员: " << public_member << endl;        // ✓ 可以访问
    }
};

// 2. protected 继承
class ProtectedDerived : protected Base {
public:
    void show() {
        // cout << private_member;  // 错误！不能访问基类的private成员
        cout << "protected继承 - protected成员: " << protected_member << endl;  // ✓
        cout << "protected继承 - public成员: " << public_member << endl;        // ✓
    }
};

// 3. private 继承
class PrivateDerived : private Base {
public:
    void show() {
        // cout << private_member;  // 错误！
        cout << "private继承 - protected成员: " << protected_member << endl;  // ✓
        cout << "private继承 - public成员: " << public_member << endl;        // ✓
    }
};

// 多层继承示例
class GrandChild : public PublicDerived {
public:
    void show() {
        // cout << private_member;  // 错误！
        cout << "孙子类 - protected成员: " << protected_member << endl;  // ✓（从PublicDerived继承的protected）
        cout << "孙子类 - public成员: " << public_member << endl;        // ✓
    }
};

class GrandChildFromProtected : public ProtectedDerived {
public:
    void show() {
        // cout << private_member;  // 错误！
        cout << "从protected继承来的孙子: " << protected_member << endl;  // ✓（仍然是protected）
        cout << "从protected继承来的孙子: " << public_member << endl;     // ✓（现在是protected）
    }
};

class GrandChildFromPrivate : public PrivateDerived {
public:
    void show() {
        // cout << private_member;     // 错误！
        // cout << protected_member;   // 错误！在PrivateDerived中已经是private了
        // cout << public_member;      // 错误！在PrivateDerived中已经是private了
    }
};
```
#### 多继承
> c++是允许多继承的

```cpp
class Printer {
public:
    void print(const string& text) {
        cout << "打印: " << text << endl;
    }
};

class Scanner {
public:
    void scan() {
        cout << "扫描中..." << endl;
    }
};

// 多继承
class AllInOne : public Printer, public Scanner {
public:
    void copy() {
        scan();
        print("扫描的内容");
    }
};
```

#### 构造函数和析构函数的继承
> c++中的继承会默认继承父类中的无参构造函数
> 
> 内存布局：子类对象包含父类部分的完整拷贝
> 继承关系：子类继承了父类的成员变量，这些变量存储在子类对象的内存中
>
> 也就是说父类的构造函数初始化对应变量后，子类会直接将初始化后的变量复制过来（如果可见的话）
> 
> // 内存结构示意
> class Player 对象的内存布局：
> ┌─────────────────────┐
> │  Entity 部分         │
> │  ├─ float x (9.8f)  │  ← 由 Entity 构造函数初始化
> │  └─ float y (29.3f) │
> ├─────────────────────┤
> │  Player 特有部分     │
> │  └─ 成员函数等        │
> └─────────────────────┘

```cpp
class Base {
public:
    Base() {
        cout << "Base默认构造函数" << endl;
    }
    
    Base(int x) {
        cout << "Base参数构造函数: " << x << endl;
    }
    
    ~Base() {
        cout << "Base析构函数" << endl;
    }
};

class Derived : public Base {
public:
    // 调用基类默认构造函数
    // 等同于Derived() : Base()
    Derived() {
        cout << "Derived默认构造函数" << endl;
    }
    
    // 显式调用基类参数构造函数
    Derived(int x, int y) : Base(x) {
        cout << "Derived参数构造函数: " << y << endl;
    }
    
    // 拷贝构造函数
    Derived(const Derived& other) : Base(other) {
        cout << "Derived拷贝构造函数" << endl;
    }
    
    ~Derived() {
        cout << "Derived析构函数" << endl;
    }
};

// 构造顺序：Base → Derived
// 析构顺序：Derived → Base
```

### static关键字

#### 静态全局变量和函数

> 在c++中普通全局变量和函数具有外链性（全局变量和函数全文件可见，用extern即可以导入--函数不能导入）
> 而用`static`修饰的变量和函数只有本文件可见性

```cpp
// 没有static修饰的普通变量和函数
// 在static.cpp中
int var = 900;
void func() {
    std::cout<<"func";
}
// 在Main.cpp中
extern int var; // extern不需要头文件也能使用其它文件中的全局变量
// 这里编译时会提示重复定义，因为这里可见staic中的func
void func() {

}

int main() {
    std::cout<<var; // 打印900
}

// 有static修饰的
// 在static.cpp中
static int var = 900;   // 无法使用extern导入
static void func() {    // 不会在其它文件可见，除非定义到头文件中
    //...
}
```

#### 静态static在类中使用

静态成员函数特点：

- 没有 `this` 指针
- 只能访问静态成员变量
- 不能是 `const` 或 `virtual`
- 可以通过类名直接调用

```cpp
class Entity {
    public:
        static int x, y;
        void print() {
            std::cout<<"x:"<<x<<" y:"<<y<<std::endl;
        }

        static int getX() {
            return x;
        }
};
// 使用类中的静态变量，需要在类外重新声明一下并初始化（或许可以不初始化）
int Entity::x = 0;
int Entity::y = 0;

int main() {
    Entity e1;
    e1.x = 9;
    e1.y = 10;
    e1.print();
    // 使用静态方法
    Entity::getX();
}
```

#### static 局部静态变量

> 在局部块中声明的静态变量会一直驻留在内存（生命周期从程序开始到结束），但只能在本作用域内访问到，访问的总是同一个变量
> 
> 特点：
> 
> - 生命周期：程序开始到结束
> 
> - 作用域：仅在函数内部
> 
> - 只初始化一次，保持上次的值

```cpp
void counter() {
    static int count = 0;  // 只初始化一次
    count++;
    cout << "调用次数: " << count << endl;
}

int main() {
    counter();  // 输出: 调用次数: 1
    counter();  // 输出: 调用次数: 2
    counter();  // 输出: 调用次数: 3
    return 0;
}
```

### 虚函数
> 虚函数是C++实现多态的核心机制，允许通过基类指针或引用调用派生类的函数。
> 主要是为了正确运行重写的函数，在使用多态时，如果不使用虚函数无法正确运行重写的子类函数

```cpp
class Base {
public:
    virtual string GetName() {return "Base";}
    string getName() {return "Base";}
};

class Derived : public Base {
public:
    string d_Name = "Derived";
    string GetName() override {
        return d_Name;
    }
    string getName() {return d_Name;}
};

int main() {
    Derived* d = new Derived();
    Base* b = d;
    // 第一个虚函数可以正常打印
    // 第二个会运行父类函数
    cout<<b->GetName()<<endl<<b->getName()<<endl;
    // Derived
    // Base
}
```

#### 工作原理（虚函数表）

### 虚析构函数
> 虚析构函数的作用是：以多态的形式赋值派生类对象后，释放该变量能正常调用派生类的析构函数

**问题展示**
```cpp
#include <iostream>
class Base {
public:
    Base() {
        std::cout<<"Base created!\n";
    }
    ~Base() {
        std::cout<<"Base destroy!\n";
    }
};

class Entity : public Base {
int* m_arr;
public:
    Entity() {
        m_arr = new int[10]{1,2,3,4,5,6,7,8,9,10};
        std::cout<<"Entity created!\n";
    }
    ~Entity() {
        delete [] m_arr;
        std::cout<<"Entity destroy!\n";
    }
};

int main() {
    Base* b1 = new Entity();
    // 下面只会调用Base的析构函数
    // 而在Entity中分配了一块内存，因为没有调用Entity的析构函数，所以这块内存不会被回收
    delete b1;
}
```

**问题解决**
> ==结论：在任何可能会被继承的类中，都要使用virtual关键字来修饰析构函数==
```cpp
#include <iostream>
class Base {
public:
    Base() {
        std::cout<<"Base created!\n";
    }
    // 在基类的析构函数添加 virtual 关键字
    virtual ~Base() {
        std::cout<<"Base destroy!\n";
    }
};

class Entity : public Base {
int* m_arr;
public:
    Entity() {
        m_arr = new int[10]{1,2,3,4,5,6,7,8,9,10};
        std::cout<<"Entity created!\n";
    }
    ~Entity() {
        delete [] m_arr;
        std::cout<<"Entity destroy!\n";
    }
};

int main() {
    Base* b1 = new Entity();
    // 会先调用Entity的析构函数，再调用Base的析构行数
    delete b1;
}
```

### c++接口（纯虚函数）
> 在C++中，接口通常通过抽象类来实现，即包含纯虚函数的类。
> 接口不能被初始化，只能由完全重写的类进行初始化

```cpp
// 不能被初始化
class Printable {
public:
    // 可以有成员变量
    int age = 90;
    // 纯虚函数，定义接口
    virtual string GetName() = 0;
    virtual int GetAge() = 0;
    // 虚析构函数
    virtual ~Printable() = 0;
    // 也可以有非虚函数
    void print() {
        cout<<"Printable\n";
    }
};

class A : public Printable {
public:
    string GetName() override {return "A";}
    int GetAge() override {return age;}
    ~A() override {}
};
// Printable p; x 错误！
A a;    // 可以初始化
```

### 结构体与类

> c++中，结构体与类的差异主要在于：结构体成员默认是public的，而类中是private的。
> 但是如果要使用继承的话，最好使用类

## 运算符重载
> 运算符重载语法

```cpp
// 返回类型 operator@ (参数列表)
// @ 是要重载的运算符

class ClassName {
public:
    // 1. 作为成员函数
    返回类型 operator运算符(参数列表) {
        // 实现
    }
};

// 2. 作为全局函数
返回类型 operator运算符(参数列表) {
    // 实现
}
```

> 下面是运算符重载案例
```cpp
class Vector2 {
public:
    int x, y;
    Vector2(int x, int y): x(x), y(y) {}
    Vector2 Add(Vector2 other) {
        return Vector2(x + other.x, y + other.y);
    }
    // 在类中重载
    Vector2 operator+(Vector2 other){
        return Add(other);
    }
    void print() {
        std::cout<<"x:"<<x<<std::endl<<"y:"<<y<<std::endl;
    }
};

class Vector3 {
public:
    int x, y, z;
    Vector3(int x, int y, int z): x(x), y(y), z(z) {}
    void print() {
        std::cout<<"x:"<<x<<std::endl<<"y:"<<y<<std::endl<<"z:"<<z<<std::endl;
    }
};

// 在全局中重载
Vector3 operator+(Vector3 a, Vector3 b) {
    return Vector3(a.x + b.x, a.y + b.y, a.z + b.z);
}

int main() {
    Vector3 v1(1,2,3);
    Vector3 v2(1,2,3);
    Vector3 v3 = v1 + v2;
    Vector2 v4(1,2);
    Vector2 v5(1,2);
    Vector2 v6 = v4 + v5;
    v3.print();
    v6.print();
}
```

## 动态数组
> 动态数组底层仍然还是一个数组只是会自动扩容且拥有一些其它功能

### 动态数组的简单使用

```cpp
#include <iostream>
#include<vector>

class Vertex
{
public:
    int x, y, z;
    Vertex(int x, int y, int z):x(x), y(y), z(z) {}
    void print() {
        std::cout<<x<<std::endl;
    }
};

int main() {
    // 动态数组可以是任意c++类型（可以是基本类型，如：int）
    std::vector<Vertex> vertexs;
    // 添加元素的方式，在数组最后面添加一个元素
    vertexs.push_back({1,2,3});
    vertexs.push_back(Vertex(4,5,6));

    for (int i = 0; i < vertexs.size(); ++i) {
        // 动态数组重载了[]运算符
        vertexs[i].print();
    }
    // 删除最后一个元素
    vertexs.pop_back();
    vertexs.push_back(Vertex(5,5,6));
    // 删除指定位置的元素
    vertexs.erase(vertexs.begin() + 1);
    // 动态数组重载了for循环
    for (Vertex v : vertexs) {
        v.print();
    }
    return 0;
}
```

### 动态数组的使用优化
> 动态数组使用优化主要是减少复制

> 默认情况下，动态数组的容量为1，当存入数量大于1时就要进行扩容，扩容就是重新创建一个数组然后将当前内容复制到新数组中去，这就会产生复制性能消耗

> 在使用`push_back函数`向动态数组中添加对象时，会先在当前环境创建一个对象，然后将它复制到动态数组内存中，这里也会产生复制性能消耗。

```cpp
#include <iostream>
#include<vector>

class Vertex
{
public:
    int x, y, z;
    Vertex(int x, int y, int z):x(x), y(y), z(z) {}
    void print() {
        std::cout<<x<<std::endl;
    }
};

int main() {
    std::vector<Vertex> v1;
    // 预先设置动态数组的最小容量，减少因扩容导致的复制性能损失
    v1.reserve(3);
    // push_back添加元素，会首先在main函数栈内创建对象，然后复制到动态数组内存中，导致额外的复制性能消耗
    v1.push_back(Vertex(1,2,3));
    // 会直接在动态数组所在内存区域内创建对象
    // 参数是构造函数所需的参数
    v1.emplace_back(1,2,3);

    return 0;
}
```

## C++中使用库（第三方包）

静态链接库是包含在可执行文件里的

动态链接库是不在可执行文件里的，例如：Windows上dll文件

## C++处理多返回值
> C++中有多种返回多个返回值的方法（类型可以不同），比较好的方式是使用结构体返回结果

### 使用结构体作为返回值

```cpp
#include <string>

struct Res {
    int num;
    std::string str;

    Res(int num, std::string str): num(num), str(str) {}
};

Res f1() {
    return Res(1,"str");
}

int main() {
    Res r = f1();
}
```

### 元组（tuple）
> 元组可以存储多个不同类型的变量

```cpp
#include <string>
#include <tuple>
#include <utility>

std::tuple<std::string, int> f() {
    return std::make_pair("ss", 12);
}

int main() {
    // 使用auto来接收元组类型的变量
    auto res = f();
    // 使用get来获取元组内的元素
    std::string fs = std::get<0>(res);
    int si = std::get<1>(res);
}
```

## 模板
> 模板有点像其它语言中的泛型，但是它要比泛型更强大，下面做一些简单的示例

```cpp
#include <iostream>

template<typename T>
void Print(T value) {
    std::cout<<value<<std::endl;
}

int main() {
    // 在使用时指定类型
    Print<int>(9);
    // 也可以不指定
    Print("sss");
}
```
> ==实际上，模板的作用是让编译器帮我们写代码，上面的代码中传入两个不同的类型在编译时，编译器会自动生成函数重载，如下面。==

```cpp
#include <iostream>

// 编译器自动生成
void Print(int value) {
    std::cout<<value<<std::endl;
}

void Print(std::string value) {
    std::cout<<value<<std::endl;
}

int main() {
    // 在使用时指定类型
    Print<int>(9);
    // 也可以不指定
    Print("sss");
}
```

> 模板在类上的使用

```cpp
#include <string>

template <typename T, int N>
class Entity {
public:
    T m_array[N];
    int GetSize() const {
        return N;
    }
};

int main() {
    // 生成一个数组元素类型为string，长度为8的对象
    Entity<std::string, 8> arr;
    // 生成另一个不同的对象
    Entity<int, 10> a;
}
```

## C++中的宏
> C++中的宏指的是编译前的预处理，在预处理阶段可以将代码当成文本来处理（替换和更改某些内容），`#`开头的那些命令就是预处理命令

```cpp
#include <iostream>

#define WAIT std::cin.get()

// 可以通过编译命令定义DEBUG
// g++ -o va -DDEBUG .\m_hong.cpp
// -D后面接参数名，这里是DEBUG
#ifdef DEBUG   // 如果定义了DEBUG，则执行下面代码
#define LOG(x) std::cout<<x<<std::endl
#define ADD(x, y) Add(x, y)
#else // 否则执行下面代码
#define LOG(x) 
#endif

int Add(int a, int b) {
    return a + b;
}

int main() {
    std::cout<<"ddd\n";
    #ifdef DEBUG    // 如果定义了DEBUG，则运行下面的代码，否则不运行
    LOG("ddd"); // 在预处理时会被替换为：std::cout<<"ddd"<<std::endl
    LOG(ADD(2, 4));
    #endif
    WAIT;   // 会替换为：std::cin.get()
}
```

## auto关键字
> auto可以自动推导类型，下面是auto使用的简单案例

```cpp
#include <iostream>

class Entity {
    public:
        int x;
        Entity():x(90){}
};

// 1. auto 作为返回值（C++14）
auto add(int a, int b) {
    return a + b;  // 返回类型推导为 int
}

// 2. 尾置返回类型（C++11）
auto multiply(int a, int b) -> int {
    return a * b;
}

int main() {
    auto a = 0; // 自动推导为int类型
    // a = "sss"; 错误！不能再传入其它类型

    auto e = Entity();
    // auto可以自动推导类型，但是不能包括引用
    auto& ee = e;   // ee为Entity&类型

    std::cout<<a<<std::endl;
}
```

> auto关键字在`函数指针`方面的应用

```cpp
int p(int a) {
    return a + 1;
}

int main() {
    // 可以自动推导为：int (*)(int) 类型，即上面的的函数指针
    auto func = p;
    func(); // 调用
    // 上面等同于
    int (*p_func)(int) = p;
    p_func();   // 调用
}
```

## Lambda表达式

### Lambda简单使用
```cpp
#include <iostream>

int main() {
    // 最简单的lambda
    auto a1 = [](){std::cout<<"最简单的lambda"<<std::endl;};
    a1();
    // 带参数并且无返回值的lambda
    auto a2 = [](int a){std::cout<<a<<std::endl;};
    a2(5);
    // 带参数并且返回值的lambda
    auto a3 = [](int a){return a;};
    std::cout<<a3(8)<<std::endl;
    // 带参数且指定返回值类型的lambda
    auto a4 = [](int a) -> int {return a;};
    std::cout<<a4(7)<<std::endl;
    // 无参数时可以省略参数列表
    auto a5 = []{std::cout<<"no params"<<std::endl;};

    return 0;
}
```

### Lambda可以作为参数
> 当参数类型是函数指针时，可以使用lambda

```cpp
#include <array>
#include <iostream>

// 第二个参数是一个函数指针
void print(const std::array<int, 5>& arr, void(*p)(int)) {
    for (int i = 0; i < arr.size(); ++i) {
        p(arr[i]);
    }
}

int main() {
    std::array<int, 5> arr = {1,2,3,4};
    // lambda作为参数
    print(arr, [](int a){std::cout<<a<<std::endl;});

    return 0;
}
```

### Lambda的捕获列表

```cpp
#include <iostream>
#include <memory>

int main() {
    int x = 10;
    int y = 20;
    std::string str = "hahaha";

    // 值捕获
    auto byValue = [x](){
        // 捕获的是x的副本
        // x是只读的
        std::cout<<x<<std::endl;
    };

    // 引用捕获
    auto byReference = [&x, &y]() {
        x = 100;    // 可以修改原变量
        y = 200;
    };

    // 混合捕获
    auto mixed = [x, &y](){
        // x = 200; 错误！x是值捕获，不能修改
        y = 200;    // 可以修改
    };

    // 隐式捕获
    auto implicit = [=](){
        // 值捕获所有使用到的变量
        std::cout << "x = " << x << ", y = " << y << std::endl;
    };
    auto implicitRef = [&]() {  
        // 引用捕获所有使用到的变量
        x = 1000;
        y = 2000;
    };

    // 初始化捕获（C++14）
    auto initCapture = [z = x + y]() {  // 用表达式初始化捕获
        std::cout << "z = " << z << std::endl;  // z = 30
    };
    
    auto moveCapture = [ptr = std::make_unique<int>(42)]() {
        std::cout << *ptr << std::endl;
    };  // unique_ptr 被移动到 lambda 中

    // 捕获 this
    class MyClass {
        int value = 100;
    public:
        void func() {
            auto lambda = [this]() {  // 捕获当前对象
                cout << value << endl;
            };
            lambda();
            
            auto lambda2 = [*this]() {  // C++17：捕获对象的副本
                cout << value << endl;
            };
        }
    };
    return 0;
}
```

## C++命名空间
> 命名空间存在的目的是区分同名的标识符，在不同命名空间下的标识符可以相同

### 简单使用
```cpp
#include <iostream>
#include <string>
// 声明一个命名空间
namespace apple {
    void print(const char* str) {
        std::cout<<"apple:"<<str<<std::endl;
    }
    // 命名空间嵌套
    namespace apple_inside {
        void print() {
            std::cout<<"inside"<<std::endl;
        }
    }
}

namespace orange {
    void print(std::string str) {
        std::cout<<"orange:"<<str<<std::endl;
    }
}
// 给命名空间起别名
namespace inside = apple::apple_inside;

int main() {
    // 在使用如下命令的作用域内，可以省略apple前缀
    using namespace apple;
    print("apple"); // 使用的是apple空间内的print
    apple::print("ddd");
    orange::print("dsds");
    // 嵌套命名空间内函数使用
    apple::apple_inside::print();
    // 使用别名
    inside::print();
}
```

### 在头文件内使用命名空间
```cpp
// 源文件
#include <iostream>
#include <string>

namespace Na {
    void print() {
        std::cout<<"Na"<<std::endl;
    }

    void SayHello(std::string str) {
        std::cout<<"hello "<<str<<std::endl;
    }
}

// 头文件
#include <string>

namespace Na {
    void print();
    void SayHello(std::string str);
}

// 使用
#include "./na.h"

int main() {
    Na::SayHello("na");
}
```

## 线程

### 简单使用

```cpp
#include <chrono>
#include <iostream>
#include <string>
#include <thread>

static bool flag = false;

void DoWork(std::string str) {
    while (!flag) {
        std::cout<<str<<std::endl;
        // 让当前线程睡眠1s
        std::this_thread::sleep_for(std::chrono::seconds(1));   
    }
}

void IDoWork(int& str) {
    while (!flag) {
        std::cout<<str<<std::endl;
        // 让当前线程睡眠1s
        std::this_thread::sleep_for(std::chrono::seconds(1));   
    }
}

int main() {
    std::string kl = "ff";
    // 创建新线程
    std::thread worker(DoWork, kl);
    std::cin.get();
    flag = true;
    worker.join();  // 阻止当前线程，等待worker完成
    std::cout<<"main finshed.."<<std::endl;
}
```

### 线程生命周期管理
```cpp
int main() {
    // 1. join() - 等待线程完成
    thread t1(worker, 1);
    t1.join();  // 阻塞直到 t1 完成
    cout << "t1 joined" << endl;
    
    // 2. detach() - 分离线程
    // 因为父线程执行结束后中止使用detach()的子线程不会崩溃
    thread t2(worker, 2);
    t2.detach();  // 线程在后台运行，主线程不等待
    cout << "t2 detached" << endl;
    
    // 3. joinable() - 检查线程是否可 join
    thread t3(worker, 3);
    if (t3.joinable()) {
        cout << "t3 is joinable" << endl;
        t3.join();
    }
    
    // 4. 移动线程（转移所有权）
    thread t4(worker, 4);
    thread t5 = move(t4);  // t4 不再拥有线程
    if (!t4.joinable()) {
        cout << "t4 no longer joinable" << endl;
    }
    t5.join();
    
    // 5. 获取线程 ID
    thread t6(worker, 6);
    cout << "Thread ID: " << t6.get_id() << endl;
    cout << "Main thread ID: " << this_thread::get_id() << endl;
    t6.join();
    
    // 等待一段时间让分离的线程执行
    this_thread::sleep_for(chrono::seconds(3));
    
    return 0;
}
```

## 时间库

### 时间测量（std::chrono）

**1. 简单使用**
```cpp
#include <chrono>
#include <iostream>
#include <thread>

int main() {
    // 获取当前时间点
    auto start = std::chrono::high_resolution_clock::now();
    // 当前时间10s后的时间点
    auto later = start + std::chrono::seconds(10);
    std::this_thread::sleep_for(std::chrono::seconds(1));
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<float> duration = end - start;
    std::cout<<duration.count();
}
```

**2. 时钟**
> 时钟提供当前时间点

```cpp
#include <chrono>
#include <iostream>

int main() {
    // 1. system_clock：系统时钟（可以调整，对应系统时间）
    auto now = std::chrono::system_clock::now();
    
    // 2. steady_clock：单调时钟（不可调整，适合测量时间间隔）
    auto start = std::chrono::steady_clock::now();
    
    // 3. high_resolution_clock：高精度时钟（通常是 steady_clock 的别名）
    auto precise = std::chrono::high_resolution_clock::now();
    
    return 0;
}
```

**3. 时间间隔**
```cpp
#include <chrono>
#include <iostream>

int main() {
    // 预定义的时间间隔类型
    std::chrono::nanoseconds   ns(100);           // 纳秒
    std::chrono::microseconds  us(100);           // 微秒
    std::chrono::milliseconds  ms(100);           // 毫秒
    std::chrono::seconds       s(10);             // 秒
    std::chrono::minutes       min(5);            // 分钟
    std::chrono::hours         h(2);              // 小时
    
    // 时间间隔转换
    auto ms_from_sec = std::chrono::duration_cast<std::chrono::milliseconds>(s);
    std::cout << ms_from_sec.count() << " ms" << std::endl;  // 10000 ms
    
    // 浮点数时间间隔
    std::chrono::duration<double> dsec(1.5);       // 1.5秒
    std::chrono::duration<double, std::milli> dms(1500.5); // 1500.5毫秒
    
    return 0;
}
```

## 排序
> `std::sort`是C++标准库中最常用的排序算法，位于`<algorithm>`头文件中

**1. 默认升序排序**
```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v = {5, 2, 8, 1, 9, 3};
    
    // 默认升序排序
    std::sort(v.begin(), v.end());
    
    // 输出：1 2 3 5 8 9
    for (int x : v) std::cout << x << " ";
    
    return 0;
}
```
**2. 自定义比较函数（降序）**
```cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v = {5, 2, 8, 1, 9, 3};
    
    // 降序排序
    std::sort(v.begin(), v.end(), std::greater<int>());
    // 或使用 lambda
    // lambda函数返回true，则a排在b前面
    // lambda函数返回false，则a排在b后面
    std::sort(v.begin(), v.end(), [](int a, int b) { return a > b; });
    
    // 输出：9 8 5 3 2 1
    for (int x : v) std::cout << x << " ";
    
    return 0;
}
```

## 类型双关
> 类型双关是一种编程技术，允许将一种类型的数据当作另一种类型来解释和使用。本质上是在不转换数据的情况下，重新解释内存中的二进制表示。

```cpp
// 示例：将 float 的二进制表示当作 int 来读取
float f = 3.14f;
// 先通过&f获得float指针，再通过(int*)转换成int指针，最后*解指针
int i = *(int*)&f;  // 类型双关：将 float 的内存解释为 int
```

## 联合体
> 联合体是一种特殊的数据结构，它允许在同一块内存空间中存储不同类型的数据，但同一时间只能使用其中一个成员。内存大小取成员中所需内存最大者的容量。

**1. 基本使用**
```cpp
#include <iostream>
#include <string>

union Value {   // 所需内存为4字节
    int int_val;
    float float_val;
    const char* str_val;
};

int main() {
    Value v;
    
    // 存储整数
    v.int_val = 42;
    std::cout << "整数: " << v.int_val << std::endl;
    
    // 存储浮点数（覆盖之前的值）
    v.float_val = 3.14f;
    std::cout << "浮点数: " << v.float_val << std::endl;
    
    // 存储字符串指针
    v.str_val = "Hello";
    std::cout << "字符串: " << v.str_val << std::endl;
    
    return 0;
}
```

**2. 匿名联合体**
```cpp
#include <iostream>

struct Vector3 {
    union { // 这个联合体内存大小为：3 * 4 字节
        struct {
            float x, y, z;
        };
        // 这个数组的内存与上面的结构体是重合的，所以能通过数组访问结构体内容
        float data[3];
    };
    
    Vector3(float x, float y, float z) : x(x), y(y), z(z) {}
};

int main() {
    Vector3 v(1.0f, 2.0f, 3.0f);
    
    // 通过成员名访问
    std::cout << "x: " << v.x << ", y: " << v.y << ", z: " << v.z << std::endl;
    
    // 通过数组访问
    for (int i = 0; i < 3; ++i) {
        std::cout << "data[" << i << "]: " << v.data[i] << std::endl;
    }
    
    return 0;
}
```


## 迭代器
> 迭代器用于遍历集合的

## 预编译头文件