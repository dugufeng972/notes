# 多语言模块集成

> c#可以调用 vb，因为 c#和 vb 都会被编译为微软中间语言

## 托管代码与非托管代码

托管代码：类似于 java 字节码，在 CLR 管理下运行的程序指令代码（MSIL 指令）称为托管代码，运行在 c#虚拟机上
非托管代码：类似于机器码，仅针对于特定操作系统而生的 CPU 机器指令

## 公共语言运行库（CLR）

CLS：公共语言规范，解决不同开发语言之间的语法问题
CTS：通用类型，解决不同开发语言之间的数据类型差异

# 编程要素

> 命名空间和类 数据类型 变量和代码规范

## 命名空间

> 像是 java 中的模块，里面包含着一个或多个类

## 类

类是在命名空间下的，不同命名空间是要引用的

## 数据类型

## 输入输出

> cw + tab 键 可以快速生成 Console.WriteLine

### 数据格式化

> `string str = string.Format("姓名：{0}，成绩：{1}",name,score);`其中`name`和`score`是变量

简化

> `string str = $"姓名：{name}，成绩：{score}"`

### 类型转换

自动类型转换
强制类型转换
使用`Convert`
例：`int a = Convert.ToInt32("3.4");`

## 字符串

### 字符串常用的重要方法

1. `indexof`方法和`Length`属性

```csharp
string str = "afeofejow@gmail.com";
int i1 = str.IndexOf("@g"); // 返回匹配的字符串第一个位置，如果不存在匹配字符串则返回-1
Console.WriteLine(i1.ToString(), str.Length);
```

2. 字符串截取`Substring`方法

```csharp
// 第一个参数是开始截取位置，第二个参数是截取的长度
// 如果只有一个参数，则截取该参数之后包括该参数的所有内容
string str2 = str.Substring(0, 2);
```

3. 字符串的比较
   使用 == 和 Equals 比较
   == 和 Equals 默认只能比较值类型，对象要重写 Equals 才能用 Equals 比较

### 字符串格式化的扩展

<img src="./pic/csharp/1.png">

### 字符串空字符串

判断字符串是否为""而不是 null
c#中"" 与 string.Empty 等价的

### 字符串其它方法

1. 去空格
   `str.Trim()` 前后都去空格

2. 转大小写
   `str.ToUpper()`转大写
   `str.ToLower()`转小写

3. 获取字符串条件的最后一个索引位置
   `str.LastIndexOf("asa")`

### 字符串拼接

`StringBuilder`类的使用
<img src="./pic/csharp/屏幕截图 2025-10-21 103057.png">

```csharp
StringBuilder sb = new StringBuilder();
sb.Append(str);
sb.Append("添加");
sb.Insert(2, "-insert-");
sb.AppendFormat($"-{"aaa"}-"); // 输出：af-insert-eofejow@gmail.com添加-aaa-
sb.Remove(0, 2); // 输出：-insert-eofejow@gmail.com添加-aaa-

```

特点：可以定义可变字符串，实现字符串的追加
使用：

- 高频率的字符串拼接中首选
- 普通的简单拼接依然可以使用 + 方法

### c#常见转义字符

\

## 数组

```csharp
// 定义方式与java一样
int[] scores;   //声明数组，此时未分配空间
scores = new int[5];    // 分配空间
// 等价定义
int[] ss = new int[3] {1, 2, 3};
int[] ss = new int[] {1, 2, 3};
```

### 数组循环 -- foreach

```csharp
foreach(int i in scores)
{
    Console.WriteLine(i.ToString());
}
```

### 字符串分割与连接

1. `Split()`方法：返回字符串数组，默认以空格分隔
2. `string.Join()`方法：组合字符串

==注：字符串属于引用类型，但它的行为像值类型==

### String 与 string

1. String 是内置的类
2. string 是 C#的关键字，表示一种数据类型，会映射为 String
3. 使用 string 时编译器会把它编译成 String
4. string 是关键字，而 String 不是关键字

# 常量 与 枚举

1. 常量用 const 修饰
   例：`const int ABC = 10;`

2. 枚举
   定义枚举：
   ```csharp
   // 定义枚举
    public enum Gender
    {
        Male,
        Female
    }
   // 使用
    Gender gender = Gender.Male;
   ```

# C# 类

## 类的组成

```
访问修饰符 class 类的名称 {
    类的属性;
    类的方法;
}
```

## 类的访问修饰符

1. internal

   > 类的默认修饰符是 internal，只能在当前模块（也叫类库或者程序集）使用这个类

2. public

   > 没用访问限制

3. readonly
   > 只读修饰

## 类的属性 get 和 set 方法

```csharp
public string EquipName { get; set; }
```

> 上面的 get 和 set 是给`EquipName`变量提供了 get 和 set 方法简便写法
> 属性本身不存储数据，数据存在`_EquipName`中
>
> **上面的写法与下面相同**

```csharp
private int _id = 0;
public int EquipID
{
    get
    {
        return _id;
    }
    set
    {
        _id = value;
    }
}
```

**如果没有逻辑需求，则可以采用简写，有逻辑需要采用下面写法**

```csharp
private int _id = 0;    // 用于存储EquipID内值的私有变量
public int EquipID
{
    get
    {
        // 如果大于返回99，也就是EquipID最多只到99
        if(_id >= 100) {
            return 99;
        }
        return _id;
    }
    set
    {
        if(value >= 100) {
            _id = 99;
        }
        else{
            _id = value;
        }
    }
}
```

==注意：C#的属性只有在有 set 和 get 时才可以设置和获取，只有 set 或 get 只能设置或者只能获取，如果没有则不能获取或设置==

### 属性修饰符

1. private

   > 只能内部访问

2. public

   > 没用访问限制

3. readonly
   > 只读修饰

### 属性使用和新特性扩展

1. 属性直接初始化

```csharp
public string EquipName { get; set; } = "nihao";
```

2. 属性表达式

## 类的字段

> 没有 get 和 set 的属性就是字段，只能在类中使用，但是可以通过类中的方法来操作字段，一般用`private`修饰

## 类的初始化方法

### 构造方法

**\*构造方法复用**

```csharp
public Eq1(int eqNo) { this._eqNo = eqNo; }
// 把要复用的参数写到this中
public Eq1(int eqNo, int id) : this(eqNo)
{
    this._eqNo = eqNo;
}
```

### 对象初始化器

> 对象初始化器也是对象初始化的一种方法
> 特点：只能初始化属性，并且不需要做任何额外的预制工作

```csharp
Sensor sensor = new Sensor
{
    Temperature = 25.6
};
```

## 对象的实例方法

1. 方法调用方式 1 --- 命名参数

```csharp
public int TestFunc(int i, string s)
{
    Console.WriteLine(s);
    return i + 1;
}
TestFunc(i: 1, s: "sss");
```

2. 其它与 java 类似

### 可变参数 -- params 修饰

语法：

```
修饰符 返回类型 方法名 (params 类型[] 变量名) {
    // 函数体
}
```

要求：

- 该参数必须放到方法参数最后，并且只能用一次
- 参数数组必须是一维数组
- params 不能与 ref 和 out 组合使用

```csharp
public void TestParams(string str, params int[] param)
{
    Console.WriteLine(str);
    for (int i = 0; i < param.Length; i++)
    {
        Console.WriteLine(param[i]);
    }
}
// 调用
TestParams("abc", 1,2,3,4);
```

# 垃圾回收机制

> C#有自动垃圾回收机制

# 集合与泛型

## List<T>

```csharp
// 创建
List<string> ts = new List<string>();
// 添加
ts.Add("abc");
// 删除
ts.Remove("abc");
// 遍历
foreach (var item in ts)
{
    Console.WriteLine(item);
}
// 初始化器
List<string> ts = new List<string>() { "你好", "大家好" };
// 集合复制
List<string> ts2 = new List<string>(ts);
// 等价上面的复制
List<string> ts2 = [.. ts];
// 将集合转成数组
string[] tsArray = ts2.ToArray();   //string[] tsArray = [.. ts2];
// 将数组转成集合
List<string> ts3 = tsArray.ToList();    // List<string> ts3 = [.. tsArray];
// 确认ts3中是否包含abc
ts3.Contains("abc");
// 指定位置插入内容
ts3.Insert(1, "2");
// 删除指定位置内容
ts3.RemoveAt(1);
```

## Dictionary<k,v>

```csharp
Teacher teacher = new Teacher() { age=20, name="yang"};
// 制造字典
Dictionary<string, Teacher> teacherDic1 = new Dictionary<string, Teacher>();
// 字典初始化器
Dictionary<string, Teacher> teacherDic = new Dictionary<string, Teacher>()
{
    ["a"] = teacher,
};
// 为字典添加元素
teacherDic.Add("b", teacher);
// 根据key获取value
Console.WriteLine(teacherDic["a"]);
// 遍历key
foreach(var key in teacherDic.Keys)
{
    Console.WriteLine(key);
}
// 遍历value
foreach (var value in teacherDic.Values)
{
    Console.WriteLine(value);
}
// 判断是否包含某个key
// 移除键值对
```

# 窗体和事件

1. windows 程序（winform 程序、桌面程序、c/s（client/server））
2. windows 窗体继承 Form
3. 控件都是窗体对象的成员变量

> 使用 vs 时双击控件就可以直接到控件的响应函数里

```csharp
// 添加事件
this.button1.Click += new System.EventHandler(this.button1_Click);
// 移除事件
this.button1.Click -= new System.EventHandler(this.button1_Click);
// 定义事件处理函数
private void button1_Click(object sender, EventArgs e)
{
    MessageBox.Show("你好！");
}
```

> sender 是触发事件的控件，如 button
> 控件有一个 tag 属性，可以在属性中设置，这是一个对象可以用来绑定一些数据
> 自动生成的控件都会自动添加到所在页面类中的`Controls`属性中，这是一个集合

### Windows 窗体对象创建和显示的两种模式

1. 创建并显示新窗体

```csharp
// 创建子窗体
private void button4_Click(object sender, EventArgs e)
{
    FormMain formMain = new FormMain(); // 这里可以用有参构造函数
    formMain.Show();
}
```

2. 关闭窗体

```csharp
private void button1_Click(object sender, EventArgs e)
{
    this.Close();
}
```

3. 窗体的显示方式

```csharp
private void button4_Click(object sender, EventArgs e)
{
    FormMain formMain = new FormMain(); // 这里可以用有参构造函数
    formMain.Show();
}

// 模态方式显示，不关闭不能操作其它窗体
private void button4_Click(object sender, EventArgs e)
{
    FormMain formMain = new FormMain(); // 这里可以用有参构造函数
    formMain.ShowDialog();
}
```

# 控件综合

## 常用控件

### 常用容器

> 窗体也是一种容器

1. GroupBox

   > 主要是用来对控件进行分组的
   > 优点：统一对控件进行布局调整。还可以对控件分组管理

2. panel
   > 与 GroupBox 类似，但是没有标题
3. splitcontainer

   > 用于将容器分成两栏的容器

4. TabControl
   > 选项卡控件。适合于在一个窗体中集中多个 panel 的形式

### 常用控件

> 控件都在工具箱中

1. 标签：`Lable`，为控件提供运行时说明性文字
   <img src="./pic/csharp/屏幕截图 2026-01-05 113705.png">
2. 文本框：`TextBox`，用于内容输入
3. 单选框：`RadioButton`，用于选择
4. 下拉框：`ComboBox`
5. 日期控件：`DateTimePicker`，自定义日期格式，例：yyyy-MM-dd => 2026-01-05
6. 复选框：`CheckBox`
7. 图片控件：`PictureBox`
8. 超链接：`LinkLable`

### DataGridView 与 List 集合数据展示

1. DataGridView 控件属性使用

   - BackGroundColor：设置背景颜色
   - 编辑列：设置列标题、样式等属性
   - 特别注意：EnablHeadersVisualStyles 设置为 false，否则相关样式不起作用
   - GridColor：单元格边框颜色
   - RowsDefaultCellStyle：数据行默认的单元格样式（重点）
   - SelectionMode：选中行的时，是选中全部列，还是当前列。

2. List 集合数据源

   > 目的：让 DataGridView 显示数据

   步骤：

   - 编写实体类：只有属性的类
   - 设置 dgv 绑定属性为“实体类的属性”（列标题的三大属性：Name、DataPropertyName、HeaderText）
   - 组织数据集合
   - 设置 DataSource：这是 DataGridView 控件的一个属性
   - 禁止自动生成列：this.dgvList.AutoGenerateColumns = false; //dgvList 是 DataGridView 控件名，这段代码放到对应窗体的构造函数中

   - 如果数据不显示，检查 DataPropertyName 是否设置，实体类是否对应属性是否有{get;set;}

### 控件命名、下拉框绑定与文件对话框

1. 控件命名：

   | 标签           | 命名                         |
   | -------------- | ---------------------------- |
   | Lable          | lbl\_名称，例：lbl_EquipName |
   | TextBox        | txt\_名称                    |
   | RadioButton    | rdb\_名称                    |
   | ComboBox       | cbb\_名称                    |
   | DateTimePicker | dtp\_名称                    |
   | CheckBox       | ckb\_名称                    |
   | LinkLable      | llbl\_名称                   |
   | PictureBox     | pb\_名称                     |
   | GroupBox       | gb\_名称                     |
   | Panel          | panel\_名称                  |
   | TabControl     | tc\_名称                     |
   | DataGridView   | dgv\_名称                    |

2. 下拉框动态绑定

```cs
// 这个函数应该写在窗口构造函数里的组件初始化函数的后面
private void InitData()
{
    List<Department> depList = new List<Department>()
    {
        new Department{DepartmentId=10, DepartmentName="设计"},
        new Department{DepartmentId=11, DepartmentName="工程"}
    };
    // 指定数据源
    this.comboBox1.DataSource = depList;
    // 指定展示的数据属性
    this.comboBox1.DisplayMember = "DepartmentName";
    // 指定绑定的对应值
    this.comboBox1.ValueMember = "DepartmentId";
}
```

3. 文件对话框

> 双击超链接添加事件

```cs
private void lbll_selectpic_LinkClicked(object sender, LinkLabelLinkClickedEventArgs e)
{
    // 创建文件打开对话框对象
    OpenFileDialog openFileDialog = new OpenFileDialog();
    // 以模态对话框方式打开
    DialogResult result = openFileDialog.ShowDialog();
    if (result == DialogResult.OK) {
        string path = openFileDialog.FileName;  // 图片完整路径
        this.pictureBox1.Image = Image.FromFile(path);  //显示图片
    }
}
```

### 主窗体退出的正确逻辑设计与对话框重载

### Button 按钮扁平化设计与 ListView 专题

1. 按钮扁平化设计要求：

   - 为按钮添加图片
   - 调整样式

2. ListView 列表视图控件
   > 有点像 ul li

属性说明：

- 添加列标题：设置文本 Text、Name、Width 三个属性
- BorderStyle：FixedSingle
- FullRowSelect:设置是否选择模式。默认（false）提示：只有在 Details 视图该属性才有意义。
- GridLine：True
- View：Details

### Menu 菜单使用专题
