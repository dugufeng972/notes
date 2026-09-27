# 平时补充

## 注解

1. `@PostConstruct`: Java的标准注解，在 Spring 完成依赖注入后，自动执行初始化方法
2. ## Function接口

# 面向对象基础

## 方法

### 可变参数

可变参数用类型...定义，可变参数相当于<mark>数组类型</mark>：

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

### 构造方法

可是使用this(...)语法来调用其它构造方法

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }

    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```

## 继承

### 阻止继承

正常情况下，只要某个class没有final修饰符，那么任何类都可以从该class继承。

从Java 15开始，允许使用sealed修饰class，并通过permits明确写出能够从该class继承的子类名称。

```java
//只允许Rect, Circle, Triangle继承Shape
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

## 多态

### final

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为final。用final修饰的类不能被继承。

如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为final。用final修饰的方法不能被Override。

对于一个类的实例字段，同样可以用final修饰。用final修饰的字段在初始化后不能被修改。可以在构造方法中初始化final字段。

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

### 接口

#### default方法

`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是default方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

```java
// interface
public class Main {
    public static void main(String[] args) {
        Person p = new Student("Xiao Ming");
        p.run();
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

//Student没有覆写run方法没有报错
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}

```

#### 接口的静态字段

因为interface是一个纯抽象类，所以它不能定义实例字段。但是，interface是可以有静态字段的，并且静态字段必须为final类型：

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

实际上，因为interface的字段只能是public static final类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public static final:
    int MALE = 1;
    int FEMALE = 2;
}
```

## classpath和jar

### classpath

> `classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`.class`文件。
> 
> 因为Java是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正可以被JVM执行的字节码。因此，当`.class`文件不在当前目录时，jvm需要知道去哪里寻找。
> 
> 所以，`classpath`就是一组目录的集合，它设置的搜索路径与操作系统相关。

```textile
例如，在Windows系统上，用;分隔，带空格的目录用""括起来，可能长这样：
C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"

在Linux系统上，用:分隔，可能长这样:
/usr/shared:/usr/local/bin:/home/liaoxuefeng/bin
```

> 现在我们假设`classpath`是`.;C:\work\project1\bin;C:\shared`，当JVM在加载`abc.xyz.Hello`这个类时，会依次查找：
> 
> * <当前目录>\abc\xyz\Hello.class
> * C:\work\project1\bin\abc\xyz\Hello.class
> * C:\shared\abc\xyz\Hello.class
> 
> 注意到`.`代表当前目录。如果JVM在某个路径下找到了对应的`class`文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。

> `classpath`的设定方法有两种：
> 
> - 在系统环境变量中设置`classpath`环境变量，不推荐；
> 
> - 在启动JVM时设置`classpath`变量，推荐。
>   
>   ```shell
>   # 前面那个是classpath，后面那个是字节码文件名
>   java -classpath .;C:\work\project1\bin;C:\shared abc.xyz.Hello
>   
>   # 简写使用 -cp 
>   java -cp .;C:\work\project1\bin;C:\shared abc.xyz.Hello
>   ```

> 没有设置系统环境变量，也没有传入`-cp`参数，那么JVM默认的`classpath`为`.`
> 
> ```shell
> java abc.xyz.Hello
> ```

> <mark>不要把任何Java核心库添加到classpath中！JVM根本不依赖classpath加载核心库！</mark>

### jar包

> jar包就是用来干这个事的，它可以把`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其他文件）都打成一个jar文件。
> 
> jar包实际上就是一个zip格式的压缩文件，而jar包相当于目录。如果我们要执行一个jar包的`class`，就可以把jar包放到`classpath`中：
> 
> ```shell
> java -cp ./hello.jar abc.xyz.Hello
> ```
> 
> 这样JVM会自动在`hello.jar`文件里去搜索某个类。

> 如何创建jar包？
> 
> 因为jar包就是zip包，所以，直接在资源管理器中，找到正确的目录，点击右键，在弹出的快捷菜单中选择“发送到”，“压缩(zipped)文件夹”，就制作了一个zip文件。然后，把后缀从`.zip`改为`.jar`，一个jar包就创建成功。

```textile
在下面的目录结构上单击bin右键创建jar包
bin
├─ hong
│  └─ Person.class
│  ming
│  └─ Person.class
└─ mr
   └─ jun
      └─ Arrays.class
```



> jar包还可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本，可以指定`Main-Class`和其它信息。JVM会自动读取这个`MANIFEST.MF`文件，如果存在`Main-Class`，我们就不必在命令行指定启动的类名，而是用更方便的命令：
> 
> ```shell
> java -jar hello.jar
> ```
> 
> 大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建jar包。Java社区提供了大量的开源构建工具，例如Maven，可以非常方便地创建jar包。







# 异常处理

### 断言

断言（Assertion）是一种调试程序的方式。

```java
public static void main(String[] args) {
    double x = Math.abs(-123.45);
    assert x >= 0;
    System.out.println(x);
}
```

语句assert x >= 0;即为断言，断言条件x >= 0预期为true。如果计算结果为false，则断言失败，抛出AssertionError。

使用assert语句时，还可以添加一个可选的断言消息：

```java
assert x >= 0 : "x must >= 0";
```

这样，断言失败的时候，AssertionError会带上消息x must >= 0，更加便于调试。

Java断言的特点是：断言失败时会抛出AssertionError，导致程序结束退出。<mark>因此，断言不能用于可恢复的程序错误，只应该用于开发和测试阶段。</mark>

# 反射

> 反射就是Reflection，Java的反射是指程序在运行期可以拿到一个对象的所有信息。
> 
> 反射是为了解决在运行期，对某个实例一无所知的情况下，如何调用其方法。

## Class类

> 除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。例如：
> 
> * `String`
> * `Object`
> * `Runnable`
> * `Exception`
> * ...
> 
> 仔细思考，我们可以得出结论：`class`（包括`interface`）的本质是数据类型（`Type`）。无继承关系的数据类型无法赋值：

```java
Number n = new Double(123.456); // OK
String s = new Double(123.456); // compile error!
```

> 而`class`是由JVM在执行过程中动态加载的。JVM在第一次读取到一种`class`类型时，将其加载进内存。
> 
> 每加载一种`class`，JVM就为其创建一个`Class`类型的实例，并关联起来。注意：这里的`Class`类型是一个名叫`Class`的`class`。它长这样：

```java
public final class Class {
    private Class() {}
}
```

> 以`String`类为例，当JVM加载`String`类时，它首先读取`String.class`文件到内存，然后，为`String`类创建一个`Class`实例并关联起来：

```java
Class cls = new Class(String);
```

> 这个`Class`实例是JVM内部创建的，如果我们查看JDK源码，可以发现`Class`类的构造方法是`private`，只有JVM能创建`Class`实例。
> 
> 所以，JVM持有的每个`Class`实例都指向一个数据类型（`class`或`interface`）
> 
> 一个`Class`实例包含了该`class`的所有完整信息：

```textile
┌───────────────────────────┐
│      Class Instance       │────▶ String
├───────────────────────────┤
│name = "java.lang.String"  │
├───────────────────────────┤
│package = "java.lang"      │
├───────────────────────────┤
│super = "java.lang.Object" │
├───────────────────────────┤
│interface = CharSequence...│
├───────────────────────────┤
│field = value[],hash,...   │
├───────────────────────────┤
│method = indexOf()...      │
└───────────────────────────┘
```

> 由于JVM为每个加载的`class`创建了对应的`Class`实例，并在实例中保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等，因此，如果获取了某个`Class`实例，我们就可以通过这个`Class`实例获取到该实例对应的`class`的所有信息。
> 
> <mark>这种通过Class实例获取class信息的方法称为反射（Reflection）。</mark>

**获取Class实例的方法**

> 方法一：直接通过一个`class`的静态变量`class`获取：

```java
Class cls = String.class;
```

> 方法二：如果我们有一个实例变量，可以通过该实例变量提供的`getClass()`方法获取：

```java
String s = "Hello";
Class cls = s.getClass();
```

> 方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

```java
Class cls = Class.forName("java.lang.String");
```

> 因为`Class`实例在JVM中是唯一的，所以，上述方法获取的`Class`实例是同一个实例。可以用`==`比较两个`Class`实例：

```java
Class cls1 = String.class;

String s = "Hello";
Class cls2 = s.getClass();

boolean sameClass = cls1 == cls2; // true
```

> 如果获取到了一个`Class`实例，我们就可以通过该`Class`实例来创建对应类型的实例：

```java
// 获取String的Class实例:
Class cls = String.class;
// 创建一个String实例:
String s = (String) cls.newInstance();
```

> 上述代码相当于`new String()`。通过`Class.newInstance()`可以创建类实例，它的局限是：只能调用`public`的无参数构造方法。带参数的构造方法，或者非`public`的构造方法都无法通过`Class.newInstance()`被调用。

**动态加载**

> JVM在执行Java程序的时候，并不是一次性把所有用到的class全部加载到内存，而是第一次需要用到class时才加载。例如：

```java
// Main.java
public class Main {
    public static void main(String[] args) {
        if (args.length > 0) {
            create(args[0]);
        }
    }

    static void create(String name) {
        Person p = new Person(name);
    }
}
```

> 当执行`Main.java`时，由于用到了`Main`，因此，JVM首先会把`Main.class`加载到内存。然而，并不会加载`Person.class`，除非程序执行到`create()`方法，JVM发现需要加载`Person`类时，才会首次加载`Person.class`。如果没有执行`create()`方法，那么`Person.class`根本就不会被加载。

> 动态加载`class`的特性对于Java程序非常重要。利用JVM动态加载`class`的特性，我们才能在运行期根据条件加载不同的实现类。例如，Commons Logging总是优先使用Log4j，只有当Log4j不存在时，才使用JDK的logging。利用JVM动态加载特性，大致的实现代码如下：

```java
// Commons Logging优先使用Log4j:
LogFactory factory = null;
if (isClassPresent("org.apache.logging.log4j.Logger")) {
    factory = createLog4j();
} else {
    factory = createJdkLog();
}

boolean isClassPresent(String name) {
    try {
        Class.forName(name);
        return true;
    } catch (Exception e) {
        return false;
    }
}
```

## 访问字段(Field)

> 对任意的一个`Object`实例，只要我们获取了它的`Class`，就可以获取它的一切信息。
> 
> 如何通过`Class`实例获取字段信息。`Class`类提供了以下几个方法来获取字段：
> 
> * Field getField(name)：根据字段名获取某个public的field（包括父类）
> * Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
> * Field[] getFields()：获取所有public的field（包括父类）
> * Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）

**示例代码**

```java
// reflection
public class Main {
    public static void main(String[] args) throws Exception {
        Class stdClass = Student.class;
        // 获取public字段"score":
        System.out.println(stdClass.getField("score"));
        // 获取继承的public字段"name":
        System.out.println(stdClass.getField("name"));
        // 获取private字段"grade":
        System.out.println(stdClass.getDeclaredField("grade"));
    }
}

class Student extends Person {
    public int score;
    private int grade;
}

class Person {
    public String name;
}
```

### Field对象

> 一个`Field`对象包含了一个字段的所有信息：
> 
> * `getName()`：返回字段名称，例如，`"name"`；
> * `getType()`：返回字段类型，也是一个`Class`实例，例如，`String.class`；
> * `getModifiers()`：返回字段的修饰符，它是一个`int`，不同的bit表示不同的含义。

> 以`String`类的`value`字段为例，它的定义是：

```java
public final class String {
    private final byte[] value;
}
```

> 用反射获取该字段的信息，代码如下：

```java
Field f = String.class.getDeclaredField("value");
f.getName(); // "value"
f.getType(); // class [B 表示byte[]类型
int m = f.getModifiers();
Modifier.isFinal(m); // true
Modifier.isPublic(m); // false
Modifier.isProtected(m); // false
Modifier.isPrivate(m); // true
Modifier.isStatic(m); // false
```

### 获取字段值

> 获取字段值是通过`Field.get(实例对象)`方法获取的
> 
> 对于一个`Person`实例，我们可以先拿到`name`字段对应的`Field`，再获取这个实例的`name`字段的值：

```java
// reflection
import java.lang.reflect.Field;
public class Main {

    public static void main(String[] args) throws Exception {
        Object p = new Person("Xiao Ming");
        Class c = p.getClass();
        Field f = c.getDeclaredField("name");
        // 需要把实例对象传递进去，以确定是哪个对象
        Object value = f.get(p);    // ❌，字段为private
        System.out.println(value); // "Xiao Ming"
    }
}

class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }
}
```

> 上述代码先获取`Class`实例，再获取`Field`实例，然后，用`Field.get(Object)`获取指定实例的指定字段的值。
> 
> 运行代码，如果不出意外，会得到一个`IllegalAccessException`，这是因为`name`被定义为一个`private`字段，正常情况下，`Main`类无法访问`Person`类的`private`字段。要修复错误，可以将`private`改为`public`，或者，在调用`Object value = f.get(p);`前，先写一句：

```java
f.setAccessible(true);
```

> 调用`Field.setAccessible(true)`的意思是，别管这个字段是不是`public`，一律允许访问。

### 设置字段值

> 设置字段值是通过`Field.set(Object, Object)`实现的，其中第一个`Object`参数是指定的实例，第二个`Object`参数是待修改的值。示例代码如下：

```java
// reflection
import java.lang.reflect.Field;

public class Main {

    public static void main(String[] args) throws Exception {
        Person p = new Person("Xiao Ming");
        System.out.println(p.getName()); // "Xiao Ming"
        Class c = p.getClass();
        Field f = c.getDeclaredField("name");
        f.setAccessible(true);
        f.set(p, "Xiao Hong");
        System.out.println(p.getName()); // "Xiao Hong"
    }
}

class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

## 调用方法

> 能通过`Class`实例获取所有`Field`对象，同样的，可以通过`Class`实例获取所有`Method`信息。
> 
> `Class`类提供了以下几个方法来获取`Method`：
> 
> * `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
> * `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
> * `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
> * `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

**示例代码**

```java
// reflection
public class Main {
    public static void main(String[] args) throws Exception {
        Class stdClass = Student.class;
        // 获取public方法getScore，参数为String:
        System.out.println(stdClass.getMethod("getScore", String.class));
        // 获取继承的public方法getName，无参数:
        System.out.println(stdClass.getMethod("getName"));
        // 获取private方法getGrade，参数为int:
        System.out.println(stdClass.getDeclaredMethod("getGrade", int.class));
    }
}

class Student extends Person {
    public int getScore(String type) {
        return 99;
    }
    private int getGrade(int year) {
        return 1;
    }
}

class Person {
    public String getName() {
        return "Person";
    }
}
```

### Method对象

> 一个`Method`对象包含一个方法的所有信息：
> 
> * `getName()`：返回方法名称，例如：`"getScore"`；
> * `getReturnType()`：返回方法返回值类型，也是一个Class实例，例如：`String.class`；
> * `getParameterTypes()`：返回方法的参数类型，是一个Class数组，例如：`{String.class, int.class}`；
> * `getModifiers()`：返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义。

### 调用方法

> 对`Method`实例调用`invoke`就相当于调用该方法，`invoke`的第一个参数是对象实例，即在哪个实例上调用该方法，后面的可变参数要与方法参数一致，否则将报错。

> 用反射来调用`String`的`substring`方法，需要以下代码：

```java
// reflection
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        // String对象:
        String s = "Hello world";
        // 获取String substring(int)方法，参数为int:
        Method m = String.class.getMethod("substring", int.class);
        // 在s对象上调用该方法并获取结果:
        String r = (String) m.invoke(s, 6);
        // 打印调用结果:
        System.out.println(r); // "world"
    }
}
```

### 调用静态方法

> 如果获取到的Method表示一个静态方法，调用静态方法时，由于无需指定实例对象，所以`invoke`方法传入的第一个参数永远为`null`。我们以`Integer.parseInt(String)`为例：

```java
// reflection
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        // 获取Integer.parseInt(String)方法，参数为String:
        Method m = Integer.class.getMethod("parseInt", String.class);
        // 调用该静态方法并获取结果:
        Integer n = (Integer) m.invoke(null, "12345");
        // 打印调用结果:
        System.out.println(n);
    }
}

```

### 调用非public方法

> 和Field类似，对于非public方法，我们虽然可以通过`Class.getDeclaredMethod()`获取该方法实例，但直接对其调用将得到一个`IllegalAccessException`。为了调用非public方法，我们通过`Method.setAccessible(true)`允许其调用：

```java
// reflection
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        Person p = new Person();
        Method m = p.getClass().getDeclaredMethod("setName", String.class);
        m.setAccessible(true);
        m.invoke(p, "Bob");
        System.out.println(p.name);
    }
}

class Person {
    String name;
    private void setName(String name) {
        this.name = name;
    }
}

```

> 此外，`setAccessible(true)`可能会失败。如果JVM运行期存在`SecurityManager`，那么它会根据规则进行检查，有可能阻止`setAccessible(true)`。

### 反射调用方法符合多态

> 一个`Person`类定义了`hello()`方法，并且它的子类`Student`也覆写了`hello()`方法，那么，从`Person.class`获取的`Method`，作用于`Student`实例时，调用的方法到底是哪个？

```java
// reflection
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        // 获取Person的hello方法:
        Method h = Person.class.getMethod("hello");
        // 对Student实例调用hello方法:
        h.invoke(new Student());
    }
}

class Person {
    public void hello() {
        System.out.println("Person:hello");
    }
}

class Student extends Person {
    public void hello() {
        System.out.println("Student:hello");
    }
}

```

> 运行上述代码，发现打印出的是`Student:hello`

> <mark>使用反射调用方法时，仍然遵循多态原则：即总是调用实际类型的覆写方法（如果存在）。</mark>

## 调用构造方法

> 如果通过反射来创建新的实例，可以调用Class提供的newInstance()方法：

```java
Person p = Person.class.newInstance();
```

> 调用`Class.newInstance()`的局限是，它只能调用该类的public无参数构造方法。如果构造方法带有参数，或者不是public，就无法直接通过`Class.newInstance()`来调用。

### Constructor对象

> 为了调用任意的构造方法，Java的反射API提供了`Constructor`对象，它包含一个构造方法的所有信息，可以创建一个实例。
> 
> 通过Class实例获取Constructor的方法如下：
> 
> * `getConstructor(Class...)`：获取某个`public`的`Constructor`；
> * `getDeclaredConstructor(Class...)`：获取某个`Constructor`；
> * `getConstructors()`：获取所有`public`的`Constructor`；
> * `getDeclaredConstructors()`：获取所有`Constructor`。

```java
import java.lang.reflect.Constructor;

public class Main {
    public static void main(String[] args) throws Exception {
        // 获取构造方法Integer(int):
        Constructor cons1 = Integer.class.getConstructor(int.class);
        // 调用构造方法:
        Integer n1 = (Integer) cons1.newInstance(123);
        System.out.println(n1);

        // 获取构造方法Integer(String)
        Constructor cons2 = Integer.class.getConstructor(String.class);
        Integer n2 = (Integer) cons2.newInstance("456");
        System.out.println(n2);
    }
}
```

> <mark>注意Constructor总是当前类定义的构造方法，和父类无关，因此不存在多态的问题。</mark>

> 调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。`setAccessible(true)`可能会失败。

## 获取继承关系

### 获取父类的Class

> 有了`Class`实例，还可以通过`getSuperclass`来获取它的父类的`Class`：

```java
// reflection
public class Main {
    public static void main(String[] args) throws Exception {
        Class i = Integer.class;
        Class n = i.getSuperclass();
        System.out.println(n);
        Class o = n.getSuperclass();
        System.out.println(o);
        System.out.println(o.getSuperclass());
    }
}
```

> 可以看到，`Integer`的父类类型是`Number`，`Number`的父类是`Object`，`Object`的父类是`null`。除`Object`外，其他任何非`interface`的`Class`都必定存在一个父类类型。

### 获取interface

> 由于一个类可能实现一个或多个接口，通过`Class`可以用`getInterfaces()`来查询到实现的接口类型。
> 
> 例如，查询`Integer`实现的接口：

```java
// reflection
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        Class s = Integer.class;
        Class[] is = s.getInterfaces();
        for (Class i : is) {
            System.out.println(i);
        }
    }
}
```

> <mark>要特别注意：getInterfaces()只返回当前类直接实现的接口类型，并不包括其父类实现的接口类型</mark>
> 
> 如果一个类没有实现任何`interface`，那么`getInterfaces()`返回空数组。

### 继承关系

> 当我们判断一个实例是否是某个类型时，正常情况下，使用`instanceof`操作符：

```java
Object n = Integer.valueOf(123);
boolean isDouble = n instanceof Double; // false
boolean isInteger = n instanceof Integer; // true
boolean isNumber = n instanceof Number; // true
boolean isSerializable = n instanceof java.io.Serializable; // true
```

> 如果是两个`Class`实例，要判断一个向上转型是否成立，可以调用`isAssignableFrom()`：

```java
// Integer i = ?
Integer.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Integer
// Number n = ?
Number.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Number
// Object o = ?
Object.class.isAssignableFrom(Integer.class); // true，因为Integer可以赋值给Object
// Integer i = ?
Integer.class.isAssignableFrom(Number.class); // false，因为Number不能赋值给Integer
```

## 动态代理

> 来比较Java的`class`和`interface`的区别：
> 
> * 可以实例化`class`（非`abstract`）；
> * 不能实例化`interface`。

> 有没有可能不编写实现类，直接在运行期创建某个`interface`的实例呢？
> 
> 这是可能的，因为Java标准库提供了一种动态代理（Dynamic Proxy）的机制：可以在运行期动态创建某个`interface`的实例。

**正常情况：实现接口并调用实现类**

```java
// 定义接口
public interface Hello {
    void morning(String name);
}

// 实现类
public class HelloWorld implements Hello {
    public void morning(String name) {
        System.out.println("Good morning, " + name);
    }
}

// 创建实例并调用
Hello hello = new HelloWorld();
hello.morning("Bob");
```

**动态代理创建实现类**

> 在运行期动态创建一个`interface`实例的方法如下：
> 
> 1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；
> 2. 通过`Proxy.newProxyInstance()`创建`interface`实例，它需要3个参数：
>    1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
>    2. 需要实现的接口数组，至少需要传入一个接口进去；
>    3. 用来处理接口方法调用的`InvocationHandler`实例。
> 3. 将返回的`Object`强制转型为接口。

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class Main {
    public static void main(String[] args) {
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method);
                if (method.getName().equals("morning")) {
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }
        };
        Hello hello = (Hello) Proxy.newProxyInstance(
            Hello.class.getClassLoader(), // 传入ClassLoader
            new Class[] { Hello.class }, // 传入要实现的接口
            handler); // 传入处理调用方法的InvocationHandler
        hello.morning("Bob");
    }
}

interface Hello {
    void morning(String name);
}
```

# 注解

> 注解（Annotation）是放在Java源码的类、方法、字段、参数前的一种特殊“注释”。

**例子**

```java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
```

> 注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

**注解的作用**

> Java的注解可以分为三类：
> 
> - 第一类是由编译器使用的注解，例如：
>   
>   - `@Override`：让编译器检查该方法是否正确地实现了覆写；
>   
>   - `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。
>   
>   这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。
> 
> - 第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。
> 
> - 第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

**注解的参数**

> 定义一个注解时，还可以定义配置参数。配置参数可以包括：
> 
> * 所有基本类型；
> * String；
> * 枚举类型；
> * 基本类型、String、Class以及枚举的数组。
> 
> 因为<mark>配置参数必须是常量</mark>，所以，上述限制保证了注解在定义时就已经确定了每个参数的值。
> 
> 注解的配置参数可以有默认值，缺少某个配置参数时将使用默认值。
> 
> 此外，大部分注解会有一个名为`value`的配置参数，对此参数赋值，可以只写常量，相当于省略了value参数。
> 
> 如果只写注解，相当于全部使用默认值。

```java
public class Hello {
    @Check(min=0, max=100, value=55)
    public int n;

    @Check(value=99)
    public int p;

    @Check(99) // @Check(value=99)
    public int x;

    @Check
    public int y;
}
```

## 定义注解

> Java语言使用`@interface`语法来定义注解（`Annotation`），它的格式如下：

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

> 注解的参数类似无参数方法，可以用`default`设定一个默认值（强烈推荐）。最常用的参数应当命名为`value`。

### 元注解

> 有一些注解可以修饰其他注解，这些注解就称为元注解（meta annotation）。Java标准库已经定义了一些元注解，我们只需要使用元注解，通常不需要自己去编写元注解。
> 
> - @Target
> 
> - @Retention
> 
> - @Repeatable
> 
> - @Inherited

#### @Target

> 最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：
> 
> * 类或接口：`ElementType.TYPE`；
> * 字段：`ElementType.FIELD`；
> * 方法：`ElementType.METHOD`；
> * 构造方法：`ElementType.CONSTRUCTOR`；
> * 方法参数：`ElementType.PARAMETER`
> 
> 例如，定义注解`@Report`可用在方法上，我们必须添加一个`@Target(ElementType.METHOD)`：

```java
@Target(ElementType.METHOD)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

> 定义注解`@Report`可用在方法或字段上，可以把`@Target`注解参数变为数组`{ ElementType.METHOD, ElementType.FIELD }`：

```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```

> 实际上`@Target`定义的`value`是`ElementType[]`数组，只有一个元素时，可以省略数组的写法。

#### @Retention

> 元注解`@Retention`定义了`Annotation`的生命周期：
> 
> * 仅编译期：`RetentionPolicy.SOURCE`；
> * 仅class文件：`RetentionPolicy.CLASS`；
> * 运行期：`RetentionPolicy.RUNTIME`。
> 
> 如果`@Retention`不存在，则该`Annotation`默认为`CLASS`。因为通常我们自定义的`Annotation`都是`RUNTIME`，所以，务必要加上`@Retention(RetentionPolicy.RUNTIME)`这个元注解：

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

#### @Repeatable

> 使用`@Repeatable`这个元注解可以定义`Annotation`是否可重复。
> 
> 经过`@Repeatable`修饰后，在某个类型声明处，就可以添加多个`@Report`注解。

```java
@Repeatable(Reports.class)
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}

@Target(ElementType.TYPE)
public @interface Reports {
    Report[] value();
}
// 重复使用
@Report(type=1, level="debug")
@Report(type=2, level="warning")
public class Hello {
}
```

#### @Inherited

> 使用`@Inherited`定义子类是否可继承父类定义的`Annotation`。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效：

```java
@Inherited
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

### 如何定义Annotation

**第一步，用`@interface`定义注解：**

```java
public @interface Report {
}
```

**第二步，添加参数、默认值：**

> 把最常用的参数定义为`value()`，推荐所有参数都尽量设置默认值。

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

**第三步，用元注解配置注解：**

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

## 处理注解

> Java的注解本身对代码逻辑没有任何影响。所以我们需要编写一个段处理代码，根据注解的参数或注解本身的不同来进行特定的操作。
> 
> 下面只介绍如何读取`RUNTIME`类型的注解。

> 因为注解定义后也是一种`class`，所有的注解都继承自`java.lang.annotation.Annotation`，因此，读取注解，需要使用反射API。
> 
> Java提供的使用反射API读取`Annotation`的方法包括：
> 
> 判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：
> 
> * Class.isAnnotationPresent(注解Class)
> * Field.isAnnotationPresent(注解Class)
> * Method.isAnnotationPresent(注解Class)
> * Constructor.isAnnotationPresent(注解Class)

**例子**

```java
// 判断@Report是否存在于Person类:
Person.class.isAnnotationPresent(Report.class);
```

> 使用反射API读取Annotation：
> 
> * Class.getAnnotation(Class)
> * Field.getAnnotation(Class)
> * Method.getAnnotation(Class)
> * Constructor.getAnnotation(Class)

**例子**

```java
// 获取Person定义的@Report注解:
Report report = Person.class.getAnnotation(Report.class);
int type = report.type();
String level = report.level();
```

> 使用反射API读取`Annotation`有两种方法。
> 
> 方法一是先判断`Annotation`是否存在，如果存在，就直接读取：

```java
Class cls = Person.class;
if (cls.isAnnotationPresent(Report.class)) {
    Report report = cls.getAnnotation(Report.class);
    ...
}
```

> 第二种方法是直接读取`Annotation`，如果`Annotation`不存在，将返回`null`：

```java
Class cls = Person.class;
Report report = cls.getAnnotation(Report.class);
if (report != null) {
   ...
}
```

> 读取方法、字段和构造方法的`Annotation`和Class类似。
> 
> 但要读取方法参数的`Annotation`就比较麻烦一点，因为方法参数本身可以看成一个数组，而每个参数又可以定义多个注解，所以，一次获取方法参数的所有注解就必须用一个二维数组来表示。例如，对于以下方法定义的注解：

```java
public void hello(@NotNull @Range(max=5) String name, @NotNull String prefix) {
}
```

> 要读取方法参数的注解，我们先用反射获取`Method`实例，然后读取方法参数的所有注解：

```java
// 获取Method实例:
Method m = ...
// 获取所有参数的Annotation:
Annotation[][] annos = m.getParameterAnnotations();
// 第一个参数（索引为0）的所有Annotation:
Annotation[] annosOfName = annos[0];
for (Annotation anno : annosOfName) {
    if (anno instanceof Range r) { // @Range注解
        r.max();
    }
    if (anno instanceof NotNull n) { // @NotNull注解
        //
    }
}
```

**注解案例**

> 看一个`@Range`注解的定义和处理

> `@Range`定义

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Range {
    int min() default 0;
    int max() default 255;
}
```

> 在某个类中使用：

```java
public class Person {
    @Range(min=1, max=20)
    public String name;

    @Range(max=10)
    public String city;
}
```

> 编写代码来处理注解，这里编写一个`Person`实例的检查方法，它可以检查`Person`实例的`String`字段长度是否满足`@Range`的定义：

```java
void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
    // 遍历所有Field:
    for (Field field : person.getClass().getFields()) {
        // 获取Field定义的@Range:
        Range range = field.getAnnotation(Range.class);
        // 如果@Range存在:
        if (range != null) {
            // 获取Field的值:
            Object value = field.get(person);
            // 如果值是String:
            if (value instanceof String s) {
                // 判断值是否满足@Range的min/max:
                if (s.length() < range.min() || s.length() > range.max()) {
                    throw new IllegalArgumentException("Invalid field: " + field.getName());
                }
            }
        }
    }
}
```

# 集合

> 集合像是数组的进阶版。
> 
> 数组有如下限制：
> 
> * 数组初始化后大小不可变；
> * 数组只能按索引顺序存取。
> 
> 集合有如下特点：
> 
> * 可变大小的顺序链表；
> * 保证无重复元素的集合；
> * ...

> Java标准库自带的`java.util`包提供了集合类：`Collection`，它是除`Map`外所有其他集合类的根接口。
> 
> Java的`java.util`包主要提供了以下三种类型的集合：
> 
> * `List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；
> * `Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；
> * `Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。

## List

> 在集合类中，`List`是最基础的一种集合：它是一种有序列表。
> 
> `List`的行为和数组几乎完全相同：`List`内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，`List`的索引和数组一样，从`0`开始。



> `List`是一个接口，它有几个主要的接口方法：
> 
> * 在末尾添加一个元素：`boolean add(E e)`
> * 在指定索引添加一个元素：`boolean add(int index, E e)`
> * 删除指定索引的元素：`E remove(int index)`
> * 删除某个元素：`boolean remove(Object e)`
> * 获取指定索引的元素：`E get(int index)`
> * 获取链表大小（包含元素的个数）：`int size()`
> 
> `List`有两个实现：`ArrayList`和`LinkedList`

| 操作         | ArrayList | LinkedList |
| ---------- | --------- | ---------- |
| 获取指定元素     | 速度很快      | 需要从头开始查找元素 |
| 添加元素到末尾    | 速度很快      | 速度很快       |
| 在指定位置添加/删除 | 需要移动元素    | 不需要移动元素    |
| 内存占用       | 少         | 较大         |

### ArrayList

> `ArrayList`是`List`接口一个实现。`ArrayList`在内部使用了数组来存储所有元素。

### LinkedList

> 通过“链表”实现了List接口。在`LinkedList`中，它的内部每个元素都指向下一个元素：

### List的特点

> `List`接口允许我们添加重复的元素，即`List`内部的元素可以重复：

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add("pear"); // size=2
        list.add("apple"); // 允许重复添加元素，size=3
        System.out.println(list.size());
    }
}
```

> `List`还允许添加`null`：

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3
        String second = list.get(1); // null
        System.out.println(second);
    }
}
```











## Collections工具类

> `Collections`是JDK提供的工具类，同样位于`java.util`包中。它提供了一系列静态方法，能更方便地操作各种集合。

### 创建空集合

> 对于旧版的JDK，可以使用`Collections`提供的一系列方法来创建空集合：
> 
> * 创建空List：`List<T> emptyList()`
> * 创建空Map：`Map<K, V> emptyMap()`
> * 创建空Set：`Set<T> emptySet()`
> 
> <mark>要注意到返回的空集合是不可变集合，无法向其中添加或删除元素。</mark>
> 
> 新版的JDK≥9可以直接使用`List.of()`、`Map.of()`、`Set.of()`来创建空集合。

### 创建单元素集合

> 对于旧版的JDK，`Collections`提供了一系列方法来创建一个单元素集合：
> 
> * 创建一个元素的List：`List<T> singletonList(T o)`
> * 创建一个元素的Map：`Map<K, V> singletonMap(K key, V value)`
> * 创建一个元素的Set：`Set<T> singleton(T o)`
> 
> <mark>要注意到返回的单元素集合也是不可变集合，无法向其中添加或删除元素。</mark>
> 
> 新版的JDK≥9可以直接使用`List.of(T...)`、`Map.of(T...)`、`Set.of(T...)`来创建任意个元素的集合。

### 排序

> `Collections`可以使用`sort`方法对`List`进行排序。因为排序会直接修改`List`元素的位置，因此必须传入可变`List`：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple");
        list.add("pear");
        list.add("orange");
        // 排序前:
        System.out.println(list);
        Collections.sort(list);
        // 排序后:
        System.out.println(list);
    }
}
```

### 洗牌

> `Collections`提供了洗牌算法，即传入一个有序的`List`，可以随机打乱`List`内部元素的顺序，效果相当于让计算机洗牌：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        for (int i=0; i<10; i++) {
            list.add(i);
        }
        // 洗牌前:
        System.out.println(list);
        // 洗牌:
        Collections.shuffle(list);
        // 洗牌后:
        System.out.println(list);
    }
}
```

### 不可变集合

> `Collections`还提供了一组方法把可变集合封装成不可变集合：
> 
> * 封装成不可变List：`List<T> unmodifiableList(List<? extends T> list)`
> * 封装成不可变Set：`Set<T> unmodifiableSet(Set<? extends T> set)`
> * 封装成不可变Map：`Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m)`
> 
> 这种封装实际上是通过创建一个代理对象，拦截掉所有修改方法实现的。

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> mutable = new ArrayList<>();
        mutable.add("apple");
        mutable.add("pear");
        // 变为不可变集合:
        List<String> immutable = Collections.unmodifiableList(mutable);
        immutable.add("orange"); // UnsupportedOperationException!
    }
}
```

> 然而，继续对原始的可变`List`进行增删是可以的，并且，会直接影响到封装后的“不可变”`List`：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> mutable = new ArrayList<>();
        mutable.add("apple");
        mutable.add("pear");
        // 变为不可变集合:
        List<String> immutable = Collections.unmodifiableList(mutable);
        mutable.add("orange");
        System.out.println(immutable);    // ["apple", "pear", "orange"]
    }
}

```

# IO

> IO是指Input/Output，即输入和输出。以内存为中心：
> 
> * Input指从外部读入数据到内存，例如，把文件从磁盘读取到内存，从网络读取数据到内存等等。
> * Output指把数据从内存输出到外部，例如，把数据从内存写入到文件，把数据从内存输出到网络等等。
> 
> IO流是一种顺序读写数据的模式，它的特点是单向流动。数据类似自来水一样在水管中流动，所以我们把它称为IO流。

**InputStream / OutputStream**

> IO流以`byte`（字节）为最小单位，因此也称为**字节流**。
> 
> 在Java中，`InputStream`代表输入字节流，`OuputStream`代表输出字节流，这是最基本的两种IO流。

**Reader / Writer**

> 如果我们需要读写的是字符，并且字符不全是单字节表示的ASCII字符，那么，按照`char`来读写显然更方便，这种流称为_字符流_。
> 
> Java提供了`Reader`和`Writer`表示字符流，字符流传输的最小数据单位是`char`。

> 例如，我们把`char[]`数组`Hi你好`这4个字符用`Writer`字符流写入文件，并且使用UTF-8编码，得到的最终文件内容是8个字节，英文字符`H`和`i`各占一个字节，中文字符`你好`各占3个字节：
> 
> ```textile
> 0x48
> 0x69
> 0xe4bda0
> 0xe5a5bd
> ```
> 
> 反过来，我们用`Reader`读取以UTF-8编码的这8个字节，会从`Reader`中得到`Hi你好`这4个字符。
> 
> 因此，`Reader`和`Writer`本质上是一个能自动编解码的`InputStream`和`OutputStream`。

**同步和异步**

> 同步IO是指，读写IO时代码必须等待数据返回后才继续执行后续代码，它的优点是代码编写简单，缺点是CPU执行效率低。
> 
> 而异步IO是指，读写IO时仅发出请求，然后立刻执行后续代码，它的优点是CPU执行效率高，缺点是代码编写复杂。
> 
> Java标准库的包`java.io`提供了同步IO，而`java.nio`则是异步IO。上面我们讨论的`InputStream`、`OutputStream`、`Reader`和`Writer`都是同步IO的抽象类。

## File对象

> 文件是非常重要的存储方式。Java的标准库`java.io`提供了`File`对象来操作文件和目录。
> 
> 要构造一个`File`对象，需要传入文件路径（既可以传入绝对路径，也可以传入相对路径）：

```java
import java.io.File;

public class Main {
    public static void main(String[] args) {
        File file = new File("C:\\Users\\dwl\\Factorial.java");
        System.out.println("=======");
        System.out.println(file);
    }
}
```

> File对象有3种形式表示的路径，一种是`getPath()`，返回构造方法传入的路径，一种是`getAbsolutePath()`，返回绝对路径，一种是`getCanonicalPath`，它和绝对路径类似，但是返回的是规范路径。

```java
public class Main {
    public static void main(String[] args) throws IOException {
        File file = new File("..");
        System.out.println("=======");
        System.out.println(file.getPath());
        System.out.println(file.getAbsolutePath());
        System.out.println(file.getCanonicalPath());
    }
}
// 输出
// ..
// C:\Users\dwl\Desktop\desktop\java\javatest\..
// C:\Users\dwl\Desktop\desktop\java
```

### 文件和目录

> `File`对象既可以表示文件，也可以表示目录。特别要注意的是，构造一个`File`对象，即使传入的文件或目录不存在，代码也不会出错，因为构造一个`File`对象，并不会导致任何磁盘操作。只有当我们调用`File`对象的某些方法的时候，才真正进行磁盘操作。
> 
> 例如，调用`isFile()`，判断该`File`对象是否是一个已存在的文件，调用`isDirectory()`，判断该`File`对象是否是一个已存在的目录：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        File f1 = new File("C:\\Windows");
        File f2 = new File("C:\\Windows\\notepad.exe");
        File f3 = new File("C:\\Windows\\nothing");
        System.out.println(f1.isFile());
        System.out.println(f1.isDirectory());
        System.out.println(f2.isFile());
        System.out.println(f2.isDirectory());
        System.out.println(f3.isFile());
        System.out.println(f3.isDirectory());
    }
}
```

> 用`File`对象获取到一个文件时，还可以进一步判断文件的权限和大小：
> 
> * `boolean canRead()`：是否可读；
> * `boolean canWrite()`：是否可写；
> * `boolean canExecute()`：是否可执行；
> * `long length()`：文件字节大小。
> 
> 对目录而言，是否可执行表示能否列出它包含的文件和子目录。

### 创建和删除文件

> 当File对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除该文件：

```java
File file = new File("/path/to/file");
if (file.createNewFile()) {
    // 文件创建成功:
    // TODO:
    if (file.delete()) {
        // 删除文件成功:
    }
}
```

> 有些时候，程序需要读写一些临时文件，File对象提供了`createTempFile()`来创建一个临时文件，以及`deleteOnExit()`在JVM退出时自动删除该文件。

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        File f = File.createTempFile("tmp-", ".txt"); // 提供临时文件的前缀和后缀
        f.deleteOnExit(); // JVM退出时自动删除
        System.out.println(f.isFile());
        System.out.println(f.getAbsolutePath());
    }
}
```

### 遍历文件和目录

> 当File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。

| 方法                | 返回类型       | 返回内容                          |
| ----------------- | ---------- | ----------------------------- |
| **`list()`**      | `String[]` | 返回**文件名/目录名**的字符串数组           |
| **`listFiles()`** | `File[]`   | 返回**文件对象**的数组，可以继续调用 File 的方法 |

> `listFiles()`提供了一系列重载方法，可以过滤不想要的文件和目录：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("C:\\Windows");
        File[] fs1 = f.listFiles(); // 列出所有文件和子目录
        printFiles(fs1);
        File[] fs2 = f.listFiles(new FilenameFilter() { // 仅列出.exe文件
            public boolean accept(File dir, String name) {
                return name.endsWith(".exe"); // 返回true表示接受该文件
            }
        });
        printFiles(fs2);
    }

    static void printFiles(File[] files) {
        System.out.println("==========");
        if (files != null) {
            for (File f : files) {
                System.out.println(f);
            }
        }
        System.out.println("==========");
    }
}
```

> 和文件操作类似，File对象如果表示一个目录，可以通过以下方法创建和删除目录：
> 
> * `boolean mkdir()`：创建当前File对象表示的目录；
> * `boolean mkdirs()`：创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
> * `boolean delete()`：删除当前File对象表示的目录，当前目录必须为空才能删除成功。

### Path

> Java标准库还提供了一个`Path`对象，它位于`java.nio.file`包。`Path`对象和`File`对象类似，但操作更加简单。
> 
> 如果需要对目录进行复杂的拼接、遍历等操作，使用`Path`对象更方便。

```java
import java.io.*;
import java.nio.file.*;

public class Main {
    public static void main(String[] args) throws IOException {
        Path p1 = Paths.get(".", "project", "study"); // 构造一个Path对象
        System.out.println(p1);
        Path p2 = p1.toAbsolutePath(); // 转换为绝对路径
        System.out.println(p2);
        Path p3 = p2.normalize(); // 转换为规范路径
        System.out.println(p3);
        File f = p3.toFile(); // 转换为File对象
        System.out.println(f);
        for (Path p : Paths.get("..").toAbsolutePath()) { // 可以直接遍历Path
            System.out.println("  " + p);
        }
    }
}
```

## InputStream

> `InputStream`就是Java标准库提供的最基本的输入流。它位于`java.io`这个包里。`java.io`包提供了所有同步IO的功能。
> 
> 要特别注意的一点是，`InputStream`并不是一个接口，而是一个抽象类，它是所有输入流的超类。这个抽象类定义的一个最重要的方法就是`int read()`，签名如下：

```java
public abstract int read() throws IOException;
```

> 这个方法会读取输入流的下一个字节，并返回字节表示的`int`值（0~255）。如果已读到末尾，返回`-1`表示不能继续读取了。

> `FileInputStream`是`InputStream`的一个子类。顾名思义，`FileInputStream`就是从文件流中读取数据。下面的代码演示了如何完整地读取一个`FileInputStream`的所有字节：

```java
public void readFile() throws IOException {
    // 创建一个FileInputStream对象:
    InputStream input = new FileInputStream("src/readme.txt");
    for (;;) {
        int n = input.read(); // 反复调用read()方法，直到返回-1
        if (n == -1) {
            break;
        }
        System.out.println(n); // 打印byte的值
    }
    input.close(); // 关闭流
}
```

> 如果读取过程中发生了IO错误，`InputStream`就没法正确地关闭，资源也就没法及时释放。
> 
> 因此，我们需要用`try ... finally`来保证`InputStream`在无论是否发生IO错误的时候都能够正确地关闭：

```java
public void readFile() throws IOException {
    InputStream input = null;
    try {
        input = new FileInputStream("src/readme.txt");
        int n;
        while ((n = input.read()) != -1) { // 利用while同时读取并判断
            System.out.println(n);
        }
    } finally {
        if (input != null) { input.close(); }
    }
}
```

> 用`try ... finally`来编写上述代码会感觉比较复杂，更好的写法是利用Java 7引入的新的`try(resource)`的语法，只需要编写`try`语句，让编译器自动为我们关闭资源。推荐的写法如下：

```java
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        int n;
        while ((n = input.read()) != -1) {
            System.out.println(n);
        }
    } // 编译器在此自动为我们写入finally并调用close()
}
```

### 缓冲

> 在读取流的时候，一次读取一个字节并不是最高效的方法。很多流支持一次性读取多个字节到缓冲区，对于文件和网络流来说，利用缓冲区一次性读取多个字节效率往往要高很多。`InputStream`提供了两个重载方法来支持读取多个字节：
> 
> * `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
> * `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

```java
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        // 定义1000个字节大小的缓冲区:
        byte[] buffer = new byte[1000];
        int n;
        while ((n = input.read(buffer)) != -1) { // 读取到缓冲区
            System.out.println("read " + n + " bytes.");
        }
    }
}
```

### 阻塞

> 在调用`InputStream`的`read()`方法读取数据时是阻塞的。

```java
int n;
n = input.read(); // 必须等待read()方法返回才能执行下一行代码
int m = n;
```

### InputStream实现类

> 用`FileInputStream`可以从文件获取输入流，这是`InputStream`常用的一个实现类。此外，`ByteArrayInputStream`可以在内存中模拟一个`InputStream`：

## OutputStream

> `OutputStream`是Java标准库提供的最基本的输出流。
> 
> 和`InputStream`类似，`OutputStream`也是抽象类，它是所有输出流的超类。这个抽象类定义的一个最重要的方法就是`void write(int b)`，签名如下：

```java
public abstract void write(int b) throws IOException;
```

> 这个方法会写入一个字节到输出流。<mark>要注意的是，虽然传入的是int参数，但只会写入一个字节</mark>，即只写入`int`最低8位表示字节的部分（相当于`b & 0xff`）。

> 和`InputStream`类似，`OutputStream`也提供了`close()`方法关闭输出流，以便释放系统资源。要特别注意：`OutputStream`还提供了一个`flush()`方法，它的目的是将缓冲区的内容真正输出到目的地。
> 
> 为什么要有`flush()`？
> 
> 因为向磁盘、网络写入数据的时候，出于效率的考虑，操作系统并不是输出一个字节就立刻写入到文件或者发送到网络，而是把输出的字节先放到内存的一个缓冲区里（本质上就是一个`byte[]`数组），等到缓冲区写满了，再一次性写入文件或者网络。`flush()`方法，能强制把缓冲区内容输出。

### FileOutputStream

> 以`FileOutputStream`为例，演示如何将若干个字节写入文件流：

```java
public void writeFile() throws IOException {
    // 向 out/readme.txt 写入数据
    OutputStream output = new FileOutputStream("out/readme.txt");
    output.write(72); // H
    output.write(101); // e
    output.write(108); // l
    output.write(108); // l
    output.write(111); // o
    output.close();
}
```

> 每次写入一个字节非常麻烦，更常见的方法是一次性写入若干字节。这时，可以用`OutputStream`提供的重载方法`void write(byte[])`来实现：

```java
public void writeFile() throws IOException {
    OutputStream output = new FileOutputStream("out/readme.txt");
    // File file = new File("out/readme.txt");
    // OutputStream output = new FileOutputStream(file.getAbsolutePath());
    output.write("Hello".getBytes("UTF-8")); // Hello
    output.close();
}
```

> 优雅的关闭输出流

```java
public void writeFile() throws IOException {
    try (OutputStream output = new FileOutputStream("out/readme.txt")) {
        output.write("Hello".getBytes("UTF-8")); // Hello
    } // 编译器在此自动为我们写入finally并调用close()
}
```

### 阻塞

> 和`InputStream`一样，`OutputStream`的`write()`方法也是阻塞的。

### OutputStream实现类

> 用`FileOutputStream`可以从文件获取输出流，这是`OutputStream`常用的一个实现类。此外，`ByteArrayOutputStream`可以在内存中模拟一个`OutputStream`

## 操作Zip

> `ZipInputStream`是一种`FilterInputStream`，它可以直接读取zip包的内容。
> 
> 另一个`JarInputStream`是从`ZipInputStream`派生，它增加的主要功能是直接读取jar文件里面的`MANIFEST.MF`文件。因为本质上jar包就是zip包，只是额外附加了一些固定的描述文件。

```textile
┌───────────────────┐
│    InputStream    │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│ FilterInputStream │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│InflaterInputStream│
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  ZipInputStream   │
└───────────────────┘
          ▲
          │
┌───────────────────┐
│  JarInputStream   │
└───────────────────┘
```

 **读取Zip包**

> 要创建一个`ZipInputStream`，通常是传入一个`FileInputStream`作为数据源，然后，循环调用`getNextEntry()`，直到返回`null`，表示zip流结束。
> 
> 一个`ZipEntry`表示一个压缩文件或目录，如果是压缩文件，我们就用`read()`方法不断读取，直到返回`-1`：

```java
try (ZipInputStream zip = new ZipInputStream(new FileInputStream(...))) {
    ZipEntry entry = null;
    while ((entry = zip.getNextEntry()) != null) {
        String name = entry.getName();
        if (!entry.isDirectory()) {
            int n;
            while ((n = zip.read()) != -1) {
                ...
            }
        }
    }
}
```

**写入zip包**

> `ZipOutputStream`是一种`FilterOutputStream`，它可以直接写入内容到zip包。我们要先创建一个`ZipOutputStream`，通常是包装一个`FileOutputStream`，然后，每写入一个文件前，先调用`putNextEntry()`，然后用`write()`写入`byte[]`数据，写入完毕后调用`closeEntry()`结束这个文件的打包。

```java
try (ZipOutputStream zip = new ZipOutputStream(new FileOutputStream(...))) {
    File[] files = ...
    for (File file : files) {
        zip.putNextEntry(new ZipEntry(file.getName()));
        zip.write(Files.readAllBytes(file.toPath()));
        zip.closeEntry();
    }
}
```

> 上面的代码没有考虑文件的目录结构。如果要实现目录层次结构，`new ZipEntry(name)`传入的`name`要用相对路径。

## 读取classpath资源

> 很多Java程序启动的时候，都需要读取配置文件。classpath下不止可以存放`.class`和`.jar`文件，还可以放其它任意类型的文件，把配置文件放到classpath中，就不用关心它的实际存放路径。

> 在classpath中的资源文件，路径总是以`/`开头，我们先获取当前的`Class`对象，然后调用`getResourceAsStream()`就可以直接从classpath读取任意的资源文件：

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    // TODO:
}
```

> 调用`getResourceAsStream()`需要特别注意的一点是，如果资源文件不存在，它将返回`null`。因此，我们需要检查返回的`InputStream`是否为`null`，如果为`null`，表示资源文件在classpath中没有找到：

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    if (input != null) {
        // TODO:
    }
}
```

> 如果我们把默认的配置放到jar包中，再从外部文件系统读取一个可选的配置文件，就可以做到既有默认的配置文件，又可以让用户自己修改配置：

```java
Properties props = new Properties();
props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
```

## 序列化与反序列化

> 序列化是指把一个Java对象变成二进制内容，本质上就是一个`byte[]`数组。
> 
> 序列化后可以把`byte[]`保存到文件中，或者把`byte[]`通过网络传输到远程，这样，就相当于把Java对象存储到文件或者通过网络传输出去了。

> 反序列化，即把一个二进制内容（也就是`byte[]`数组）变回Java对象。
> 
> 有了反序列化，保存到文件中的`byte[]`数组又可以“变回”Java对象，或者从网络上读取`byte[]`并把它“变回”Java对象。

> 一个Java对象要能序列化，必须实现一个特殊的`java.io.Serializable`接口，它的定义如下：

```java
public interface Serializable {
}
```

> `Serializable`接口没有定义任何方法，它是一个空接口。我们把这样的空接口称为“标记接口”（Marker Interface），实现了标记接口的类仅仅是给自身贴了个“标记”，并没有增加任何方法。

### 序列化

> 把一个Java对象变为`byte[]`数组，需要使用`ObjectOutputStream`。它负责把一个Java对象写入一个字节流：

```java
import java.io.*;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws IOException {
        // 要写入的file
        OutputStream file = new FileOutputStream("data.ser");
        try (ObjectOutputStream output = new ObjectOutputStream(file)) {
            // 将int写入file:
            output.writeInt(12345);
            // 将String写入file:
            output.writeUTF("Hello");
            // 将Object写入file:
            output.writeObject(Double.valueOf(123.456));
        }
        System.out.println(Arrays.toString(buffer.toByteArray()));
    }
}
```

> `ObjectOutputStream`既可以写入基本类型，如`int`，`boolean`，也可以写入`String`（以UTF-8编码），还可以写入实现了`Serializable`接口的`Object`。

### 反序列化

> 和`ObjectOutputStream`相反，`ObjectInputStream`负责从一个字节流读取Java对象：

```java
try (ObjectInputStream input = new ObjectInputStream(...)) {
    int n = input.readInt();
    String s = input.readUTF();
    Double d = (Double) input.readObject();
}
```

> 除了能读取基本类型和`String`类型外，调用`readObject()`可以直接返回一个`Object`对象。要把它变成一个特定类型，必须强制转型。
> 
> `readObject()`可能抛出的异常有：
> 
> * `ClassNotFoundException`：没有找到对应的Class；
> * `InvalidClassException`：Class不匹配。
> 
> 对于`ClassNotFoundException`，这种情况常见于一台电脑上的Java程序把一个Java对象，例如，`Person`对象序列化以后，通过网络传给另一台电脑上的另一个Java程序，但是这台电脑的Java程序并没有定义`Person`类，所以无法反序列化。
> 
> 对于`InvalidClassException`，这种情况常见于序列化的`Person`对象定义了一个`int`类型的`age`字段，但是反序列化时，`Person`类定义的`age`字段被改成了`long`类型，所以导致class不兼容。
> 
> 为了避免这种class定义变动导致的不兼容，Java的序列化允许class定义一个特殊的`serialVersionUID`静态变量，用于标识Java类的序列化“版本”，通常可以由IDE自动生成。如果增加或修改了字段，可以改变`serialVersionUID`的值，这样就能自动阻止不匹配的class版本：

```java
public class Person implements Serializable {
    private static final long serialVersionUID = 2709425275741743919L;
}
```

> <mark>反序列化时，由JVM直接构造出Java对象，不调用构造方法，构造方法内部的代码，在反序列化时根本不可能执行。</mark>

## Reader

> `Reader`是Java的IO库提供的另一个输入流接口。和`InputStream`的区别是，`InputStream`是一个字节流，即以`byte`为单位读取，而`Reader`是一个字符流，即以`char`为单位读取：

| InputStream                 | Reader                        |
| --------------------------- | ----------------------------- |
| 字节流，以`byte`为单位              | 字符流，以`char`为单位                |
| 读取字节（-1，0~255）：`int read()` | 读取字符（-1，0~65535）：`int read()` |
| 读到字节数组：`int read(byte[] b)` | 读到字符数组：`int read(char[] c)`   |

> `java.io.Reader`是所有字符输入流的超类，它最主要的方法是：

```java
public int read() throws IOException;
```

> 这个方法读取字符流的下一个字符，并返回字符表示的`int`，范围是`0`~`65535`。如果已读到末尾，返回`-1`。

### FileReader

> `FileReader`是`Reader`的一个子类，它可以打开文件并获取`Reader`。

```java
public void readFile() throws IOException {
    // 创建一个FileReader对象:
    Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8); 
    for (;;) {
        int n = reader.read(); // 反复调用read()方法，直到返回-1
        if (n == -1) {
            break;
        }
        System.out.println((char)n); // 打印char
    }
    reader.close(); // 关闭流
}
```

> 和`InputStream`类似，`Reader`也是一种资源，需要保证出错的时候也能正确关闭，所以我们需要用`try (resource)`来保证`Reader`在无论有没有IO错误的时候都能够正确地关闭：

```java
try (Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8)) {
    // TODO
}
```

### CharArrayReader -- 不重要

> `CharArrayReader`可以在内存中模拟一个`Reader`，它的作用实际上是把一个`char[]`数组变成一个`Reader`，这和`ByteArrayInputStream`非常类似：

```java
try (Reader reader = new CharArrayReader("Hello".toCharArray())) {
}
```

### StringReader -- 不重要

> `StringReader`可以直接把`String`作为数据源，它和`CharArrayReader`几乎一样：

```java
try (Reader reader = new StringReader("Hello")) {
}
```

### InputStreamReader

> 除了特殊的`CharArrayReader`和`StringReader`，普通的`Reader`实际上是基于`InputStream`构造的，因为`Reader`需要从`InputStream`中读入字节流（`byte`），然后，根据编码设置，再转换为`char`就可以实现字符流。

> 如果我们已经有一个`InputStream`，想把它转换为`Reader`，可以使用`InputStreamReader`这样的转换器。

```java
// 持有InputStream:
InputStream input = new FileInputStream("src/readme.txt");
// 变换为Reader:
Reader reader = new InputStreamReader(input, "UTF-8");
```

> 上述代码实际上就是`FileReader`的一种实现方式。

## Writer

> `Writer`是带编码转换器的`OutputStream`，它把`char`转换为`byte`并输出。
> 
> `Writer`和`OutputStream`的区别如下：

| OutputStream                    | Writer                            |
| ------------------------------- | --------------------------------- |
| 字节流，以`byte`为单位                  | 字符流，以`char`为单位                    |
| 写入字节（0~255）：`void write(int b)` | 写入字符（0~65535）：`void write(int c)` |
| 写入字节数组：`void write(byte[] b)`   | 写入字符数组：`void write(char[] c)`     |
| 无对应方法                           | 写入String：`void write(String s)`   |

> `Writer`是所有字符输出流的超类，它提供的方法主要有：
> 
> * 写入一个字符（0~65535）：`void write(int c)`；
> * 写入字符数组的所有字符：`void write(char[] c)`；
> * 写入String表示的所有字符：`void write(String s)`。

### FileWriter

> `FileWriter`就是向文件中写入字符流的`Writer`。它的使用方法和`FileReader`类似：

```java
try (Writer writer = new FileWriter("readme.txt", StandardCharsets.UTF_8)) {
    writer.write('H'); // 写入单个字符
    writer.write("Hello".toCharArray()); // 写入char[]
    writer.write("Hello"); // 写入String
}
```

### OutputStreamWriter

> 除了`CharArrayWriter`和`StringWriter`外，普通的Writer实际上是基于`OutputStream`构造的，它接收`char`，然后在内部自动转换成一个或多个`byte`，并写入`OutputStream`。因此，`OutputStreamWriter`就是一个将任意的`OutputStream`转换为`Writer`的转换器：

```java
try (Writer writer = new OutputStreamWriter(new FileOutputStream("readme.txt"), "UTF-8")) {
    // TODO:
}
```

> 上述代码实际上就是`FileWriter`的一种实现方式。

## Files

> 从Java 7开始，`java.nio`包里面提供了`Files`这个工具类，能极大地方便我们读写文件。

> 例如，要把一个文件的全部内容读取为一个`byte[]`：

```java
byte[] data = Files.readAllBytes(Path.of("/path/to/file.txt"));
```

> 如果是文本文件，可以把一个文件的全部内容读取为`String`：

```java
// 默认使用UTF-8编码读取:
String content1 = Files.readString(Path.of("/path/to/file.txt"));
// 可指定编码:
String content2 = Files.readString(Path.of("/path", "to", "file.txt"), StandardCharsets.ISO_8859_1);
// 按行读取并返回每行内容:
List<String> lines = Files.readAllLines(Path.of("/path/to/file.txt"));
```

> 写入文件也非常方便：

```java
// 写入二进制文件:
byte[] data = ...
Files.write(Path.of("/path/to/file.txt"), data);
// 写入文本并指定编码:
Files.writeString(Path.of("/path/to/file.txt"), "文本内容...", StandardCharsets.ISO_8859_1);
// 按行写入文本:
List<String> lines = ...
Files.write(Path.of("/path/to/file.txt"), lines);
```

> 此外，`Files`工具类还有`copy()`、`delete()`、`exists()`、`move()`等快捷方法操作文件和目录。

> <mark>注意：Files提供的读写方法，受内存限制，只能读写小文件，例如配置文件等，不可一次读入几个G的大文件。读写大型文件仍然要使用文件流，每次只读写一部分文件内容。</mark>

# 日期与时间







# 单元测试

> 什么是单元测试呢？单元测试就是针对最小的功能单元编写测试代码。Java程序最小的功能单元是方法，因此，对Java程序进行单元测试就是针对单个Java方法的测试。

> 为什么要有单元测试？一般要测试一个类中的方法，一个很自然的想法是编写一个`main()`方法，然后运行一些测试代码。
> 
> 不过，使用`main()`方法测试有很多缺点：
> 
> 1. 只能有一个`main()`方法，不能把测试代码分离
> 
> 2. 没有打印出测试结果和期望结果
> 
> 3. 很难编写一组通用的测试代码
> 
> 因此，我们需要一种测试框架，帮助我们编写测试。

## JUnit

> JUnit是一个开源的Java语言的单元测试框架，专门针对Java设计，使用最广泛。JUnit是事实上的单元测试的标准框架。
> 
> 使用JUnit编写单元测试的好处在于，我们可以非常简单地组织测试代码，并随时运行它们，JUnit就会给出成功的测试和失败的测试，还可以生成测试报告，不仅包含测试的成功率，还可以统计测试的代码覆盖率，即被测试的代码本身有多少经过了测试。对于高质量的代码来说，测试覆盖率应该在80%以上。

**maven模块目录结构**

```textile
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── ddw
│   │   │           ├── Factorial.java   -- 要测试的目标类
│   │   │           └── Main.java
│   │   └── resources
│   └── test
│       └── java
│           └── FactorialTest.java    -- 测试类
└── target
    ├── classes
    │   └── com
    │       └── ddw
    │           ├── Factorial.class
    │           └── Main.class
    └── test-classes
        └── FactorialTest.class
```

**添加依赖**

```xml
<dependency>
   <groupId>org.junit.jupiter</groupId>
   <artifactId>junit-jupiter</artifactId>
   <version>5.10.2</version> <!-- 建议使用最新稳定版本 -->
   <scope>test</scope>
</dependency>
```

**测试的目标类**

> 编写了一个`Factorial.java`文件后将其放入`src`

```java
package com.ddw;

public class Factorial {
    // 要测试的方法
    public static long fact(long n) {
        long r = 1;
        for (long i = 2; i < n; i++) {
            r *= i;
        }
        return r;
    }
}
```

**测试类**

> 编写一个对应的`FactorialTest.java`文件，以`Test`为后缀是一个惯例，放入`test`目录中

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

import com.ddw.Factorial;

public class FactorialTest {
    @Test
    void testFact() {
        assertEquals(1, Factorial.fact(1));
    }
}
```

> 核心测试方法`testFact()`加上了`@Test`注解，这是JUnit要求的，它会把带有`@Test`的方法识别为测试方法。
> 
> 在测试方法内部，我们用`assertEquals(1, Factorial.fact(1))`表示，期望`Factorial.fact(1)`返回`1`。`assertEquals(expected, actual)`是最常用的测试方法，它在`Assertion`类中定义。`Assertion`还定义了其他断言方法，例如：
> 
> * `assertTrue()`: 期待结果为`true`
> * `assertFalse()`: 期待结果为`false`
> * `assertNotNull()`: 期待结果为非`null`
> * `assertArrayEquals()`: 期待结果为数组并与期望数组每个元素的值均相等
> * ...

## 使用Fixture

> 在一个单元测试中，我们经常编写多个`@Test`方法，来分组、分类对目标代码进行测试。
> 
> 在测试的时候，我们经常遇到一个对象需要初始化，测试完可能还需要清理的情况。如果每个`@Test`方法都写一遍这样的重复代码，显然比较麻烦。
> 
> JUnit提供了编写测试前准备、测试后清理的固定代码，我们称之为Fixture。

**例如**

```java
public class Calculator {
    private long n = 0;

    public long add(long x) {
        n = n + x;
        return n;
    }

    public long sub(long x) {
        n = n - x;
        return n;
    }
}
```

> 上面这个类的功能很简单，但是测试的时候，我们要先初始化对象，我们不必在每个测试方法中都写上初始化代码，而是通过`@BeforeEach`来初始化，通过`@AfterEach`来清理资源：

```java
public class CalculatorTest {

    Calculator calculator;

    @BeforeEach
    public void setUp() {
        this.calculator = new Calculator();
    }

    @AfterEach
    public void tearDown() {
        this.calculator = null;
    }

    @Test
    void testAdd() {
        assertEquals(100, this.calculator.add(100));
        assertEquals(150, this.calculator.add(50));
        assertEquals(130, this.calculator.add(-20));
    }

    @Test
    void testSub() {
        assertEquals(-100, this.calculator.sub(100));
        assertEquals(-150, this.calculator.sub(50));
        assertEquals(-130, this.calculator.sub(-20));
    }
}
```

> 在上面`CalculatorTest`测试中，有两个标记为`@BeforeEach`和`@AfterEach`的方法，它们会在运行每个`@Test`方法前后自动运行。
> 
> 因此每个测试方法中的`calculator`都不一样。

> 还有一些资源初始化和清理可能更加繁琐，而且会耗费较长的时间，例如初始化数据库。JUnit还提供了`@BeforeAll`和`@AfterAll`，它们在运行所有`@Test`前后运行。
> 
> 因为`@BeforeAll`和`@AfterAll`在所有`@Test`方法运行前后仅运行一次，因此，它们<mark>只能初始化静态变量</mark>，例如：

```java
public class DatabaseTest {
    static Database db;

    @BeforeAll
    public static void initDatabase() {
        db = createDb(...);
    }

    @AfterAll
    public static void dropDatabase() {
        ...
    }
}
```

## 异常测试

> 在编写JUnit测试的时候，除了正常的输入输出，我们还要特别针对可能导致异常的情况进行测试。

**例如**

> 在方法的参数n小于0时会抛异常

```java
public class Factorial {
    public static long fact(long n) {
        if (n < 0) {
            throw new IllegalArgumentException();
        }
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}
```

> 下面的测试针对抛异常的情况。

```java
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, new Executable() {
        @Override
        public void execute() throws Throwable {
            Factorial.fact(-1);
        }
    });
}

```

> JUnit提供`assertThrows()`来期望捕获一个指定的异常。第二个参数`Executable`封装了我们要执行的会产生异常的代码。当我们执行`Factorial.fact(-1)`时，必定抛出`IllegalArgumentException`。`assertThrows()`在捕获到指定异常时表示通过测试，未捕获到异常，或者捕获到的异常类型不对，均表示测试失败。

> 因为`Executable`是单方法接口，可以简写如下：

```java
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, () -> {
        Factorial.fact(-1);
    });
}
```

## 条件测试

> 在运行测试的时候，有些时候，我们需要排除某些`@Test`方法，不要让它运行，这时，我们就可以给它标记一个`@Disabled`：

```java
@Disabled
@Test
void testBug101() {
    // 这个测试不会运行
}
```

> 为什么我们不直接注释掉`@Test`，而是要加一个`@Disabled`？这是因为注释掉`@Test`，JUnit就不知道这是个测试方法，而加上`@Disabled`，JUnit仍然识别出这是个测试方法，只是暂时不运行。它会在测试结果中显示：

```textile
Tests run: 68, Failures: 2, Errors: 0, Skipped: 5
```

> 类似`@Disabled`这种注解就称为条件测试（Conditional Test），JUnit根据不同的条件注解，决定是否运行当前的`@Test`方法。

**例子**

> 想要测试`getConfigFile()`这个方法，但是在Windows上跑，和在Linux上跑的代码路径不同，因此，针对两个系统的测试方法，其中一个只能在Windows上跑，另一个只能在Mac/Linux上跑：

```java
public class Config {
    public String getConfigFile(String filename) {
        String os = System.getProperty("os.name").toLowerCase();
        if (os.contains("win")) {
            return "C:\\" + filename;
        }
        if (os.contains("mac") || os.contains("linux") || os.contains("unix")) {
            return "/usr/local/" + filename;
        }
        throw new UnsupportedOperationException();
    }
}
```

> 因此使用条件测试注解`@EnableOnOs`，根据操作系统来进行不同的测试

```java
@Test
@EnabledOnOs(OS.WINDOWS)    // 在Windows上执行
void testWindows() {
    assertEquals("C:\\test.ini", config.getConfigFile("test.ini"));
}

@Test
@EnabledOnOs({ OS.LINUX, OS.MAC })    // 在Linux或mac上执行
void testLinuxAndMac() {
    assertEquals("/usr/local/test.cfg", config.getConfigFile("test.cfg"));
}
```

> 其它的一些条件测试注解：
> 
> - `@DisabledOnOs(OS.WINDOWS)`：不在指定的系统上执行测试，这里是Windows
> 
> - `@EnabledIfEnvironmentVariable`：需要传入环境变量`DEBUG=true`才能执行的测试

## 参数化测试

> 如果测试方法需要一组数据作为输入和输出，可以把测试数据组织起来，用不同的测试数据调用相同的测试方法，这就是参数化测试。
> 
> 参数化测试和普通测试稍微不同的地方在于，一个测试方法需要接收至少一个参数，然后，传入一组参数反复运行。
> 
> JUnit提供了一个`@ParameterizedTest`注解，用来进行参数化测试。

**例一：只需单个参数的测试**

> 如何为参数化测试提供数据，可以使用注解，这里`@ValueSource`注解来提供数据

```java
@ParameterizedTest
@ValueSource(ints = { 0, 1, 5, 100 })
void testAbs(int x) {
    assertEquals(x, Math.abs(x));
}
```

**例二：需要多个参数的测试**

> 当参数个数多于1个时，就不能使用`@ValueSource`来提供数据了

```java
public class StringUtils {
    public static String capitalize(String s) {
        if (s.length() == 0) {
            return s;
        }
        return Character.toUpperCase(s.charAt(0)) + s.substring(1).toLowerCase();
    }
}
```

> 针对上述方法的测试需要两个参数。
> 
> 最简单的方法是通过`@MethodSource`注解，它允许我们编写一个同名的静态方法来提供测试参数：
> 
> 这个方式可以提供任意个数的参数

```java
@ParameterizedTest
@MethodSource
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}

static List<Arguments> testCapitalize() {
    return List.of( // arguments:
            Arguments.of("abc", "Abc"),// 可以提供多于两个的参数，但是这个案例只需要两个
            Arguments.of("APPLE", "Apple"), //
            Arguments.of("gooD", "Good"));
}
```

> 另一种传入测试参数的方法是使用`@CsvSource`，它的每一个字符串表示一行，一行包含的若干参数用`,`分隔，因此，上述测试又可以改写如下：

```java
@ParameterizedTest
@CsvSource({ 
    "abc, Abc", // 第1组数据
    "APPLE, Apple", // 第2组数据
    "gooD, Good" // 第3组数据
})
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```

> 如果有成百上千的测试输入，那么，直接写`@CsvSource`就很不方便。这个时候，我们可以把测试数据提到一个独立的CSV文件中，然后标注上`@CsvFileSource`：

```java
@ParameterizedTest
@CsvFileSource(resources = { "/test-capitalize.csv" })
void testCapitalizeUsingCsvFile(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```

> JUnit只在classpath中查找指定的CSV文件，因此，`test-capitalize.csv`这个文件要放到`test`目录下，内容如下：

```csv
apple, Apple
HELLO, Hello
JUnit, Junit
reSource, Resource
```

# 多线程

## 多线程基础--概念相关

> 进程：进程是指运行中的程序，比如我们使用QQ，就启用了一个进程，操作系统就会为其分配资源。进程是程序的一次执行过程，或是正在运行的一个程序。是动态的过程：有它自身的产生、存在和消亡的过程。
> 线程：线程由进程创建的，是进程的一个实体，一个进程可以拥有多个线程。
> 单线程：同一时刻，只允许执行一个线程
> 多线程：同一时刻，可以执行多个线程，比如：一个qq进程，可以同时打开多个聊天窗口。
> 并发：同一个时刻，多个任务交替执行，单核cpu实现的多任务就是并发。
> 并行：同一个时刻，多个时刻，多个任务同时执行，多核cpu可以实现并行。

## 创建新线程

> Java语言内置了多线程支持。当Java程序启动的时候，实际上是启动了一个JVM进程，然后，JVM启动主线程来执行`main()`方法。在`main()`方法中，我们又可以启动其他线程。

创建一个新线程只需要实例化一个Thread类

1. 方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法
   
   ```java
   // 定义
   public class Thread1 extends Thread {
    @Override
    public void run() {
        System.out.println("线程1");
    }
   }
   // 使用
   Thread t = new Thread1();
   // 调用start方法才能启动一个新线程，调用run()只是执行普通函数
   t.start();
   ```

2. 方法二：创建Thread实例时，传入一个Runnable实例
   
   ```java
   // 实现Runnable实例
   class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("MyRunnable");
    }
   }
   // 使用
   Thread thread2 = new Thread(new MyRunnable());
   // 使用lambda语法可以进一步简化
   Thread thread3 = new Thread(() -> {
    System.out.println("MyRunnable");
   });
   ```
   
   > ==注：==只有调用`start()`方法才能创建新线程，直接调用`run()`方法只能当作普通函数调用

## 线程的状态

> 在Java程序中，一个线程对象只能调用一次`start()`方法启动新线程，并在新线程中执行`run()`方法。一旦`run()`方法执行完毕，线程就结束了

java线程的状态有以下几种：

- New：新创建的线程，尚未执行
- Runnable：运行中的线程，正在执行`run()`方法的Java代码
- Blocked：运行中的线程，因为某些操作被阻塞而挂起
- Waiting：运行中的线程，因为某些操作在等待中
- Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待
- Terminated：线程已终止

<img src="./pic/java查缺补漏/屏幕截图 2025-08-18 123407.png">

> 线程启动之后，可以在`Runnable`、`Blocked`、`Waiting`和`Timed Waiting`这几个状态之间切换

线程终止的原因有：

- 线程正常终止：`run()`方法执行到`return`语句返回
- 线程意外终止：`run()`方法因为未捕获的异常导致线程终止
- 对某个线程的Thread实例调用`stop()`方法强制终止（强烈不推荐使用）

**线程的join方法**

> 一个线程还可以等待另一个线程直到其运行结束，例如：线程的`join()`方法

例：

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            System.out.println("hello");
        });
        System.out.println("start");
        t.start(); // 启动t线程
        t.join(); // 此处main线程会等待t结束
        System.out.println("end");
    }
}
```

> 此外，`join(long)`的重载方法也可以指定一个等待时间，超过等待时间后就不再继续等待

## 线程中断

1. 使用`interrupt()`方法中断线程
   
   > 中断一个线程只需要在其他线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就立刻结束运行
   
   ```java
   // 中断线程
   public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1); // 暂停1毫秒
        t.interrupt(); // 中断t线程
        System.out.println("end");
    }
   }
   
   ```

class MyThread extends Thread {
    public void run() {
        int n = 0;
        while (! isInterrupted()) { // 检查是否被中断
            n ++;
            System.out.println(n + " hello!");
        }
    }
}

```
> 如果线程处于等待状态，例如，`t.join()`会让`main`线程进入等待状态，此时，如果在`main`线程调用`interrupt()`，`join()`方法会立刻抛出`InterruptedException`，因此，目标线程只要捕获到`join()`方法抛出的`InterruptedException`，就说明有其他线程对其调用了`interrupt()`方法，通常情况下该线程应该立刻结束运行。
> 简单来说，一个用了`join`方法的线程，它被父线程中断时，它内部会抛出异常
```java
// 中断线程
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1000);
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        Thread hello = new HelloThread();
        hello.start(); // 启动hello线程
        try {
            hello.join(); // 等待hello线程结束，收到t.interrupt()时会抛出异常
        } catch (InterruptedException e) {
            System.out.println("interrupted!");
        }
        hello.interrupt();
    }
}

class HelloThread extends Thread {
    public void run() {
        int n = 0;
        while (!isInterrupted()) {
            n++;
            System.out.println(n + " hello!");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}

```

2. 设置标志位来中断线程
   
   > 另一个常用的中断线程的方法是设置标志位。我们通常会用一个`running`标志位来标识线程是否应该继续运行，在外部线程中，通过把`HelloThread.running`置为`false`，就可以让线程结束

```java
// 中断线程
public class Main {
    public static void main(String[] args)  throws InterruptedException {
        HelloThread t = new HelloThread();
        t.start();
        Thread.sleep(1);    // 暂停当前线程，也就是main线程
        t.running = false; // 标志位置为false
    }
}

class HelloThread extends Thread {
    public volatile boolean running = true;
    public void run() {
        int n = 0;
        while (running) {   //检查标志位
            n ++;
            System.out.println(n + " hello!");
        }
        System.out.println("end!");
    }
}
```

> ==注意：== 线程间共享变量需要使用`volatile`关键字标记，确保每个线程都能读取到更新后的变量值。

## 守护线程

> 守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。
> 因此，JVM退出时，不必关心守护线程是否已结束。

**创建守护进程**

> 创建守护线程方法和普通线程一样，只是在调用`start()`方法前，调用`setDaemon(true)`把该线程标记为守护线程

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start();
```

> ==注：== 守护线程不能持有任何需要关闭的资源，例如打开文件等，因为虚拟机退出时，守护线程没有任何机会来关闭文件，这会导致数据丢失。

## 线程同步

> 线程的执行是随机不可控，所以当多个线程对同一个数据进行读写的时候，会出现数据不一致的问题，通过加锁的方法可以让线程同步执行。

> Java程序使用`synchronized`关键字对一个对象进行加锁

```java
// lock是一个对象，任意对象
synchronized(lock) { // 加锁
    // 这里的代码块称之为临界区
    // 临界区内的代码无论是正常结束还是抛出异常结束，都会释放锁
    n = n + 1;
}   // 解锁
```

多线程使用锁的例子：

```java
// 多线程
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock = new Object(); // 锁
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            // 只有选择同一个锁对象，才会有同步效果
            synchronized(Counter.lock) {    // 获取锁，得不到锁一直等待
                Counter.count += 1;
            }   // 释放锁
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            // 只有选择同一个锁对象，才会有同步效果
            synchronized(Counter.lock) {    // 获取锁
                Counter.count -= 1;
            }   // 释放锁
        }
    }
}
```

> ==注：== 在使用synchronized的时候，不必担心抛出异常。因为无论是否有异常，都会在synchronized结束处正确释放锁

### 不需要synchronized加锁的操作

> JVM规范定义了几种原子操作：

- 基本类型（long和double除外）赋值，例如：`int n = m`
- 引用类型赋值，例如：`List<String> list = anotherList`

> 单条原子操作不需要同步，多行原子操作需要保证同步操作

**不可变对象无需同步**

> 如果多线程读写的是一个不可变对象，那么无需同步，因为不会修改对象的状态

### 同步方法

```java
public class Counter {
    private int count = 0;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }

    public void dec(int n) {
        // 锁住的是当前Counter实例
        synchronized(this) {
            count -= n;
        }
    }

    public int get() {
        return count;
    }
}
```

> 多个线程调用同一个Counter实例分别使用add和dec方法是安全，因为add和dec都选择同一个Counter实例对象作为锁对象，是同步的。不同Counter实例之间调用add和dec方法是并发的，因为锁住的不是同一个对象。

> 当我们锁住的是`this`实例时，实际上可以用synchronized修饰这个方法。下面两个写法是等价的：

```java
public void add(int n) {
    synchronized(this) { // 锁住this
        count += n;
    } // 解锁
}
```

等价于

```java
public synchronized void add(int n) { // 锁住this
    count += n;
} // 解锁
```

> 用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁

### 死锁

> java线程是可重入的锁

> 可重入的锁：同一个线程在获取到锁以后继续获取同一个锁，JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做可重入锁。

> 所以，获取锁的时候，不但要判断是否是第一次获取，还要记录这是第几次获取。每获取一次锁，记录+1，每退出synchronized块，记录-1，减到0的时候，才会真正释放锁

例：

```java
public class Counter {
    private int count = 0;

    public synchronized void add(int n) {   // 获得锁this
        if (n < 0) {
            dec(-n);    // 需要锁对象this，已获得锁this，再次获取this，总共有两个锁this
            // 执行完毕，释放锁this，还剩一个锁this
        } else {
            count += n;
        }
    }

    public synchronized void dec(int n) {   // 锁对象为this
        count += n;
    }
}
```

> 死锁：两个线程各自持有不同的锁，然后各自试图获取对方手里的锁，造成了双方无限等待下去，这就是死锁

### 使用wait和notify

> 在Java程序中，`synchronized`解决了多线程竞争的问题，但是`synchronized`并没有解决多线程协调的问题

1. `wait()`方法
   
   > 调用`wait()`方法后，线程进入等待状态，`wait()`方法不会返回，直到将来某个时刻，线程从等待状态被其他线程唤醒后，`wait()`方法才会返回，然后，继续执行下一条语句。

> `wait()`方法必须由锁对象来调用

> 等待的线程会释放线程获得的锁，wait()方法返回时，线程又会重新试图获得锁。

```java
public synchronized String getTask() {
    while (queue.isEmpty()) {
        this.wait();    // 释放this锁
        // 重新获取this锁
    }
    return queue.remove();
}
```

2. `notify()`方法 和 `notifyAll()`方法
   
   > 在相同的锁对象上调用`notify()`方法会唤醒相同锁对象上`wait`的线程

> `notify()`方法必须由锁对象来调用

> 当有多个`wait`的·线程，`notify()`会随机唤醒一个，而`notifyAll()`会全部唤醒，然后被唤醒的线程开始竞争锁，得到锁的开始执行。未得到锁的线程继续阻塞

```java
public synchronized void addTask(String s) {
    this.queue.add(s);
    this.notify(); // 唤醒在this锁等待的线程
}
```

### 使用ReentrantLock

> `ReentrantLock`从Java5开始提供，一种用于替代`synchronized`加锁的工具

`synchronized`的缺点：

- 这种锁一是很重
- 获取时必须一直等待，没有额外的尝试机制

传统的`synchronized`代码:

```java
public class Counter {
    private int count;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
}
```

用`ReentrantLock`替代:

```java
public class Counter {
    // 如果两个线程使用的是同一个ReentrantLock对象，那么它们在lock.lock() 处会互斥，保证同一时刻只有一个线程能进入临界区
    private final Lock lock = new ReentrantLock();
    private int count;

    public void add(int n) {
        lock.lock();    // 加锁，进入临界区
        try {
            count += n;
        } finally {
            lock.unlock();  // 解锁，离开临界区
        }
    }
}
```

> ==注：== 使用`ReentrantLock`必须考虑异常，异常可能会导致无法正确释放锁，所以必须在`finally`中释放

> `ReentrantLock`是可重入锁，它和`synchronized`一样，一个线程可以多次获取同一个锁。

> 与`synchronized`不同的是，`ReentrantLock`可以尝试获取锁

例：

```java
// lock.tryLock(1, TimeUnit.SECONDS) 尝试获取锁，最多等待1s，1s后没有获得锁返回false
// synchronized得不到锁会一直等待
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        ...
    } finally {
        lock.unlock();
    }
}
```

### 使用Condition ---- 实现ReentrantLock的wait和notify功能

> 使用`ReentrantLock`时，用`Condition`对象来实现`wait`和`notify`的功能

例：

```java
class TaskQueue {
    // 获得锁对象
    private final Lock lock = new ReentrantLock();
    // 获得Condition对象
    private final Condition condition = lock.newCondition();
    private Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();    // 加锁
        try {
            queue.add(s);
            condition.signalAll();  // 等同于notifyAll，唤醒所有由同一个condition引起的阻塞线程
        } finally {
            lock.unlock();  // 解锁
        }
    }

    public String getTask() {
        lock.lock();    // 加锁
        try {
            while (queue.isEmpty()) {
                condition.await();  // 等同于wait
            }
            return queue.remove();
        } finally {
            lock.unlock();  // 解锁
        }
    }
}
```

> 和`tryLock()`类似，`await()`可以在等待指定时间后，如果还没有被其他线程通过`signal()`或`signalAll()`唤醒，可以自己醒来

例：

```java
if (condition.await(1, TimeUnit.SECOND)) {
    // 被其他线程唤醒，执行这里
} else {
    // 指定时间内没有被其他线程唤醒，执行这里
}
```

### 使用ReadWriteLock ---- 读写锁

> `ReentrantLock`保证了只有一个线程可以执行临界区代码，但有时我们想要：多个线程同时读，但只要有一个线程在写 其他线程就必须等待。

例：
|     | 读     | 写     |
| --- | ------ | ------ |
| 读  | 允许   | 不允许 |
| 写  | 不允许 | 不允许 |

**使用`ReadWriteLock`可以解决这个问题，它保证：**

- 只允许一个线程写入（其他线程既不能写入也不能读取）
- 没有写入时，多个线程允许同时读（提高性能）

使用例：

```java
public class Counter {
    // 获取读写锁
    private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
    // 注意: 一对读锁和写锁必须从同一个rwlock获取:
    private final Lock rlock = rwlock.readLock();   // 读锁
    private final Lock wlock = rwlock.writeLock();  // 写锁
    private int[] counts = new int[10];

    public void inc(int index) {
        wlock.lock(); // 加写锁，不允许读和写
        try {
            counts[index] += 1;
        } finally {
            wlock.unlock(); // 释放写锁
        }
    }

    public int[] get() {
        rlock.lock(); // 加读锁，允许读 不允许写
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            rlock.unlock(); // 释放读锁
        }
    }
}
```

### 使用StampedLock ---- 乐观读写锁

> StampedLock java8后引入，是一种乐观锁，也是一种==不可重入锁==

> 分析`ReadWriteLock`，会发现它有个潜在的问题：如果有线程正在读，写线程需要等待读线程释放锁后才能获取写锁，即读的过程中不允许写，这是一种悲观的读锁。

> `StampedLock`和`ReadWriteLock`相比，改进之处在于：==读的过程中也允许获取写锁后写入。==

> 乐观锁的意思就是乐观地估计读的过程中大概率不会有写入，因此被称为乐观锁。

> 悲观锁则是读的过程中拒绝有写入，也就是写入必须等待。

> ==乐观锁的并发效率更高== ,但一旦有小概率的写入导致读取的数据不一致，需要能检测出来，再读一遍就行

例：

```java
public class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock(); // 获取写锁
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp); // 释放写锁
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
        // 注意下面两行代码不是原子操作
        // 假设x,y = (100,200)
        double currentX = x;
        // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
        double currentY = y;
        // 此处已读取到y，如果没有写入，读取是正确的(100,200)
        // 如果有写入，读取是错误的(100,400)
        if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
        // 如果乐观读时由写锁发生，stampedLock.validate(stamp)返回false
            stamp = stampedLock.readLock(); // 获取一个悲观读锁
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp); // 释放悲观读锁
            }
        }
        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```

### 使用Semaphore ---- 信号量

> `Semaphore` 是信号量，用来控制同时访问某个资源的线程数。它需要保证同一时刻最多有N个线程能访问，比如同一时刻最多创建100个数据库连接，最多允许10个用户下载等。

例：

```java
public class AccessLimitControl {
    // 任意时刻仅允许最多3个线程获取许可:
    final Semaphore semaphore = new Semaphore(3);

    public String access() throws Exception {
        // 如果超过了许可数量,其他线程将在此等待:
        semaphore.acquire();    // 获得一个许可，还剩2个许可
        try {
            // 执行一些代码
            // return UUID.randomUUID().toString();
        } finally {
            semaphore.release();    // 释放一个许可
        }
    }
}
```

> 调用acquire()可能会进入等待，直到满足条件为止。也可以使用tryAcquire()指定等待时间：

```java
// 指定等待时间3秒内获取到许可，未获得返回false
if (semaphore.tryAcquire(3, TimeUnit.SECONDS)) {
    try {
        // 执行一些代码
    } finally {
        semaphore.release();
    }
}
```

### 使用Concurrent集合 ---- 并发集合类

> `java.util.concurrent`包提供了对应的并发集合类，使用这些并发集合与使用非线程安全的集合类完全相同。

| interface | non-thread-safe         | thread-safe                              |
| --------- | ----------------------- | ---------------------------------------- |
| List      | ArrayList               | CopyOnWriteArrayList                     |
| Map       | HashMap                 | ConcurrentHashMap                        |
| Set       | HashSet / TreeSet       | CopyOnWriteArraySet                      |
| Queue     | ArrayDeque / LinkedList | ArrayBlockingQueue / LinkedBlockingQueue |
| Deque     | ArrayDeque / LinkedList | LinkedBlockingDeque                      |

使用这些并发集合与使用非线程安全的集合类完全相同。我们以`ConcurrentHashMap`为例:

```java
Map<String, String> map = new ConcurrentHashMap<>();
// 在不同的线程读写:
map.put("A", "1");
map.put("B", "2");
map.get("A", "1");
```

## 线程池

> Java语言虽然内置了多线程支持，启动一个新线程非常方便，但是，创建线程需要操作系统资源（线程资源，栈空间等），频繁创建和销毁大量线程需要消耗大量时间。线程池可以复用一组线程减少损耗。

Java标准库提供了`ExecutorService`接口表示线程池，它的典型用法如下：

```java
// 创建固定大小为3的线程池:
ExecutorService executor = Executors.newFixedThreadPool(3);
// 提交任务:
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.submit(task4);
executor.submit(task5);
```

`ExecutorService`是接口，它有几个常用实现类：

- FixedThreadPool：线程数固定的线程池
- CachedThreadPool：线程数根据任务动态调整的线程池
- SingleThreadExecutor：仅单线程执行的线程池

**创建这些线程池的方法都被封装到`Executors`这个类中**
例：

```java
// thread-pool
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        // 创建一个固定大小的线程池:
        ExecutorService es = Executors.newFixedThreadPool(4);
        for (int i = 0; i < 6; i++) {
            es.submit(new Task("" + i));
        }
        // 关闭线程池:
        es.shutdown();
    }
}

class Task implements Runnable {
    private final String name;

    public Task(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println("start task " + name);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println("end task " + name);
    }
}
```

> 一次性放入6个任务，由于线程池只有固定的4个线程，因此，前4个任务会同时执行，等到有线程空闲后，才会执行后面的两个任务。

**创建指定动态范围的线程池**

```java
int min = 4;
int max = 10;
ExecutorService es = new ThreadPoolExecutor(
        min, max,
        60L, TimeUnit.SECONDS,
        new SynchronousQueue<Runnable>());
```

**ScheduledThreadPool**

> `ScheduledThreadPool`是一种定期执行线程池中任务的线程池

## 使用Future ---- 线程的返回值

> 使用Java标准库提供的线程池是非常方便的。我们提交的任务只需要实现`Runnable`接口，就可以让线程池去执行。

> 但是`Runnable`接口有个问题，它的方法没有返回值。

> Java标准库还提供了一个`Callable`接口，和`Runnable`接口比，它多了一个返回值：

```java
class Task implements Callable<String> {
    public String call() throws Exception {
        return "这是返回值"; 
    }
}
```

> `ExecutorService.submit()`方法，可以看到，它返回了一个`Future`类型，一个`Future`类型的实例代表一个未来能获取结果的对象：

```java
ExecutorService executor = Executors.newFixedThreadPool(4); 
// 定义任务:
Callable<String> task = new Task();
// 提交任务并获得Future:
Future<String> future = executor.submit(task);
// 从Future获取异步执行返回的结果:
String result = future.get(); // 可能阻塞，为获取结果前会一直阻塞
```

一个`Future<V>`接口表示一个未来可能会返回的结果，它定义的方法有：

- `get()`：获取结果（可能会等待）
- `get(long timeout, TimeUnit unit)`：获取结果，但只等待指定的时间
- `cancel(boolean mayInterruptIfRunning)`：取消当前任务
- `isDone()`：判断任务是否已完成

## 使用CompletableFuture ---- 非阻塞的Future

## 使用ThreadLocal ---- 在线程内传递状态

==注意：== spring中的线程是复用的，所有在使用`ThreadLocal`时记得及时清理

> 我们可以在代码中调用`Thread.currentThread()`获取当前线程

> 例如：使用`Thread.currentThread().getName()`打印出当前线程的名字

> 在一个线程中，横跨若干方法调用，需要传递的对象，我们通常称之为上下文（Context），它是一种状态，可以是用户身份、任务信息等。

例：

```java
// process是一个线程要执行的方法
public void process(User user) {
    checkPermission(user);
    doWork(user);
    saveStatus(user);
    sendResponse(user);
}
// doWork内也有很多方法
void doWork(User user) {
    queryStatus(user);
    checkStatus();
    setNewStatus(user);
    log();
}
```

> Java标准库提供了一个特殊的`ThreadLocal`，它可以在一个线程中传递同一个对象

> `ThreadLocal`实例通常总是以静态字段初始化如下:

```java
static ThreadLocal<User> threadLocalUser = new ThreadLocal<>();
```

它的典型使用方式如下：

```java
void processUser(user) {
    try {
        threadLocalUser.set(user);  // 在ThreadLocal上挂载值
        step1();
        step2();
        log();
    } finally {
        threadLocalUser.remove();   // 移除ThreadLocal
    }
}
void step1() {
    User u = threadLocalUser.get(); // 使用ThreadLocal值
    log();
    printUser();
}
```

> 各个线程的`ThreadLocal`关联的实例互不干扰

### 使用虚拟线程 ---- 类似于协程

> 线程是一种重量级资源。
> ==在服务器端，对用户请求，通常都实现为一个线程处理一个请求。== 由于用户的请求数往往远超操作系统能同时调度的线程数量，所以通常使用线程池来尽量减少频繁创建和销毁线程的成本。

# Maven基础

> Maven是一个Java项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建，是Java项目不可缺少的工具。

### Maven介绍 --- pom

> `groupId`类似于Java的包名，通常是公司或组织名称，`artifactId`类似于Java的类名，通常是项目名称，再加上`version`，一个Maven工程就是由`groupId`，`artifactId`和`version`作为唯一标识。

`<properties>`定义了一些属性，常用的属性有：

- `project.build.sourceEncoding`：表示项目源码的字符编码，通常应设定为`UTF-8`
- `maven.compiler.release`：表示使用的JDK版本，例如21
- `maven.compiler.source`：表示Java编译器读取的源码版本
- `maven.compiler.target`：表示Java编译器编译的Class版本

### 依赖管理

Maven定义了几种依赖关系，分别是`compile`、`test`、`runtime`和`provided`：
| scope    | 说明                                          | 示例            |
| -------- | --------------------------------------------- | --------------- |
| compile  | 编译时需要用到该jar包（默认）                 | commons-logging |
| test     | 编译Test时需要用到该jar包                     | junit           |
| runtime  | 编译时不需要，但运行时需要用到                | mysql           |
| provided | 编译时需要用到，但运行时由JDK或某个服务器提供 | servlet-api     |

### 构建流程

#### Lifecycle和Phase ---- 生命周期 和 阶段

> Maven的生命周期(lifecycle)由一系列阶段（phase）构成

**以内置的生命周期`default`为例，它包含以下`phase`：**

- validate
- initialize
- generate-sources
- process-sources
- generate-resources
- process-resources
- compile
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install
- deploy

如果我们运行`mvn package`，`Maven`就会执行`default`生命周期，它会从开始一直运行到`package`这个`phase`为止：

- validate
- initialize
- ...
- prepare-package
- package

如果我们运行`mvn compile`，Maven也会执行`default`生命周期，但这次它只会运行到`compile`，即以下几个phase：

- validate
- initialize
- ...
- process-resources
- compile

**Maven另一个常用的生命周期是`clean`，它只包含3个`phase`:**

- pre-clean
- clean （注意这个clean不是lifecycle而是phase）
- post-clean

更复杂的例子是指定多个`phase`，例如，运行`mvn clean package`，Maven先执行`clean`生命周期并运行到`clean`这个`phase`，然后执行`default`生命周期并运行到`package`这个`phase`，实际执行的phase如下：

- pre-clean
- clean （注意这个clean是phase）
- validate （开始执行default生命周期的第一个phase）
- initialize
- ...
- prepare-package
- package

在实际开发过程中，经常使用的命令有：

- `mvn clean`：清理所有生成的class和jar；
- `mvn clean compile`：先清理，再执行到compile；
- `mvn clean test`：先清理，再执行到test，因为执行test前必须执行compile，所以这里不必指定compile；
- `mvn clean package`：先清理，再执行到package。

#### Goal

> 执行一个phase又会触发一个或多个goal
> goal的命名总是abc:xyz这种形式

类比一下:

- lifecycle相当于Java的package，它包含一个或多个phase；
- phase相当于Java的class，它包含一个或多个goal；
- goal相当于class的method，它其实才是真正干活的。

#### 模块管理 ---- 重要

[学习地址](https://liaoxuefeng.com/books/java/maven/module/index.html)

#### 使用mvnw

> `mvnw`是Maven Wrapper的缩写。因为我们安装Maven时，默认情况下，系统所有项目都会使用全局安装的这个Maven版本。但是，对于某些项目来说，它可能必须使用某个特定的Maven版本，这个时候，就可以使用Maven Wrapper，它可以负责给这个特定的项目安装指定版本的Maven，而其他项目不受影响。

#### 发布Artifact

# 网络编程

### Http编程

> HTTP是HyperText Transfer Protocol的缩写，翻译为超文本传输协议，它是基于TCP协议之上的一种请求-响应协议。

早期的jdk是通过`HttpURLConnection`访问HTTP，比较麻烦

从<mark>Java 11</mark>开始，引入了新的`HttpClient`，它使用<mark>链式调用的API</mark>，能大大简化HTTP的处理。

使用`HttpClient`，首先需要创建一个全局`HttpClient`实例，因为`HttpClient`内部使用线程池优化多个HTTP连接，可以复用：

```java
static HttpClient httpClient = HttpClient.newBuilder().build();
```

使用GET请求获取文本内容代码如下：

```java
import java.net.URI;
import java.net.http.*;
import java.net.http.HttpClient.Version;
import java.time.Duration;
import java.util.*;

public class Main {
    // 全局HttpClient:
    static HttpClient httpClient = HttpClient.newBuilder().build();

    public static void main(String[] args) throws Exception {
        String url = "https://www.sina.com.cn/";
        HttpRequest request = HttpRequest.newBuilder(new URI(url))
            // 设置Header:
            .header("User-Agent", "Java HttpClient").header("Accept", "*/*")
            // 设置超时:
            .timeout(Duration.ofSeconds(5))
            // 设置版本:
            .version(Version.HTTP_2).build();
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        // HTTP允许重复的Header，因此一个Header可对应多个Value:
        Map<String, List<String>> headers = response.headers().map();
        // d读取响应头中的内容
        for (String header : headers.keySet()) {
            System.out.println(header + ": " + headers.get(header).get(0));
        }
        System.out.println(response.body().substring(0, 1024) + "...");
    }
}
```

使用POST请求，要准备好发送的Body数据并正确设置Content-Type：

```java
String url = "http://www.example.com/login";
// 这是要放在body中的数据
String body = "username=bob&password=123456";
HttpRequest request = HttpRequest.newBuilder(new URI(url))
    // 设置Header:
    .header("Accept", "*/*")
    // 设置Header中Content-Type字段
    .header("Content-Type", "application/x-www-form-urlencoded")
    // 设置超时:
    .timeout(Duration.ofSeconds(5))
    // 设置版本:
    .version(Version.HTTP_2)
    // 使用POST并设置Body:
    .POST(BodyPublishers.ofString(body, StandardCharsets.UTF_8)).build();
HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
String s = response.body();
```

### RMI远程调用 ---- java代码通过网络向另一个java代码发起调用



# 函数式编程

> 函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为没有副作用。而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。

> 函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

## Lambda基础

> Lambda表达式的形式为：(arg1, arg2) -> {方法体}

> Lambda表达式可以替代方法中单抽象方法接口(只有一个抽象方法的接口)

**例:**

```java
String[] array = ...
// Comparator接口是一个单方法的接口，这里通过匿名内部类，快速生成一个实现Comparator对象
Arrays.sort(array, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});
// 使用Lambda表达式替代，可以省略形参和返回值的类型自动推导，->{...}中写方法体
Arrays.sort(arr, (o1, o2) -> {return o2.compareTo(o1);});
// 如果方法体只有一行，还可以省略{} ; return
Arrays.sort(arr, (o1, o2) -> o2.compareTo(o1));
```

> 我们把只定义了单抽象方法的接口称之为`FunctionalInterfac`，

用注解`@FunctionalInterface`标记。

> <mark>函数式接口(FunctionalInterface)的定义是：</mark>
> 
> - 只包含一个抽象方法的接口
> - 其他方法（比如`default`方法、`static`方法、`Object`里的方法）不算在内

例：`Callable`接口:

```java
// 只有一个抽象方法
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```

例：`Comparator`接口：

```java
// Comparator虽然有不只一个抽象的方法，但是它的equals抽象方法继承自Object
@FunctionalInterface
public interface Comparator<T> {

    int compare(T o1, T o2);    // 抽象方法

    boolean equals(Object obj); // 来自 Object，不算接口抽象方法

    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }

    default Comparator<T> thenComparing(Comparator<? super T> other) {
        ...
    }
    ...
}
```

## 方法引用

> 对于单方法接口`FunctionalInterfac`除了可以传入Lambda表达式，还可以传入方法引用

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, Main::cmp);
        System.out.println(String.join(", ", array));
    }

    static int cmp(String s1, String s2) {
        return s1.compareTo(s2);
    }
}
```

> 上述代码在`Arrays.sort()`中直接传入了静态方法`cmp`的引用，用`Main::cmp`表示。

> 所谓方法引用，是指如果某个方法签名和接口恰好一致，就可以直接传入方法引用。

> `Comparator<String>`接口定义的方法是`int compare(String, String)`，和静态方法`int cmp(String, String)`相比，除了方法名外，方法参数一致，返回类型相同，因此，我们说两者的方法签名一致。

<mark>一个方法的形参个数、类型和返回值类型与单方法接口的抽象方法一致，就可以用这个方法直接引用</mark>

**上述代码也可以改造成这样**

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, String::compareTo);
        System.out.println(String.join(", ", array));
    }
}
```

> 虽然`String::compareTo`的方法签名只有一个参数

```java
// String.compareTo()的方法定义
public final class String {
    public int compareTo(String o) {
        ...
    }
}
```

> 这个方法的签名只有一个参数，为什么和`int Comparator<String>.compare(String, String)`能匹配呢？
> 因为实例方法有一个隐含的this参数，String类的compareTo()方法在实际调用的时候，第一个隐含参数总是传入this，相当于静态方法：

```java
public static int compareTo(String this, String o);
```

==有意思的是：对于方法引用，隐含参数this在需要的时候就传入，不需要时不传入。例如：String类的compareTo()只有一个形参，如果单方法接口中要匹配的抽象方法有两个形参，那么可以传入，理由如上。如果方法接口中要匹配的抽象方法有一个形参，刚好匹配，可以传入，隐含参数this忽略不计。==

#### 构造方法引用

```java
// 引用构造方法
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Bob", "Alice", "Tim");
        List<Person> persons = names.stream().map(Person::new).collect(Collectors.toList());
        System.out.println(persons);
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "Person:" + this.name;
    }
}
```

<mark>构造方法的引用写法是：</mark>`类名::new`

## Stream ---- 流





# Web开发







# JVM

<img src="./pic/java查缺补漏/屏幕截图 2025-07-28 233929.png">
<img src="./pic/java查缺补漏/屏幕截图 2025-08-24 150557.png">
<img src="./pic/java查缺补漏/屏幕截图 2026-02-02 184727.png">

jvm的生命周期：

- 虚拟机的启动：java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的
- 虚拟机的退出有如下几种情况：
  - 某个线程调用Runtime类或System类的exit方法
  - 程序正常执行结束
  - 程序在执行过程中遇到了异常或错误而异常终止
  - 由于操作系统出现错误而导致java虚拟机进程终止

### 字节码文件

> java字节码文件是跨平台的

字节码文件里是什么：

- 字节码是一种二进制的类文件，它的内容是JVM的指令

### 类加载器子系统 -- Class Loader

<img src="./pic/java查缺补漏/屏幕截图 2026-02-02 190133.png">

> 类加载器子系统负责从文件系统或者网络中加载Class文件，Class文件在文件开头有特定的文件标识

> `ClassLoader`只负责`Class文件`的加载，至于字节码文件是否可以运行，则由`Execution Engine`决定

> 被加载的类信息存放于一块称为方法区的内存空间。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字面量和数字常量（这部分常量信息是Class文件中常量池部分的内存映射）

#### 类的加载过程

<img src="./pic/java查缺补漏/屏幕截图 2026-02-02 190726.png">

==类加载完后会在内存中自动创建一个字节码对象（class对象）==

##### 第一个阶段 -- 加载（Loading）

加载：

- 1. 通过一个类的全限定名获取定义此类的二进制字节流
- 2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构
- 3. ==在内存中生成一个代表这个类的java.lang.Class对象==,作为方法区这个类的各种数据的访问入口

被加载的class文件来源：

- 从本地系统中直接加载
- 通过网络获取
- 从zip压缩包中读取，jar、war的基础
- 运行时计算生成，使用最多的是：动态代理技术
- 有其他文件生成
- 从加密文件中获取，典型的就是防Class文件被反编译的保护措施

##### 第二个阶段 -- 链接（Linking）

链接：

- 1. 验证（Verify）
  - 目前在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性
  - 主要包括四种验证：文件格式验证，元数据验证，字节码验证，符合引用验证
- 2. 准备（Preparation）
  - 为类`静态变量`分配内存并且设置该类`静态变量`的默认初始值，即零值
  - 这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显示初始化
  - 这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到java堆中
- 3. 解析（Resolve）
  - 将常量池内的符号引用（就是类中使用其它的类）转换为直接引用的过程，直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄

##### 第三个阶段 -- 初始化（）

初始化：执行静态代码和静态变量初始化

- 初始化阶段就是执行类的构造器方法`<clinit>()`的过程
- 此方法不需定义，是javac编译器自动收集类中的所有类`静态变量`的赋值动作和`静态代码块`中的语句合并而来
- 构造器方法中指令按语句在源文件中出现的顺序执行
- `<clinit>()`不同于类的构造器。（关联：构造器是虚拟机视角下的`<init>()`）
- 若该类具有父类，jvm会保证子类的`<clinit>()`执行前，父类的`<clinit>()`已经执行完毕
- 虚拟机必须保证一个类的`<clinit>()`方法在多线程下被加锁，该方法只会执行一次

#### 类加载器的分类

<!-- > jvm支持两种类型的类加载器，分别为引导类加载器（Bootstrap ClassLoader）和自定义类加载器（User-Defined ClassLoader） -->

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 093907.png">

> 引导类加载器（Bootstrap ClassLoader）：用于加载java的核心类库，用c++编写的嵌在jvm中，没有父加载器

> 虚拟机自带的加载器：
> 
> - 扩展类加载器（Extension ClassLoader）：
>   - java语言编写，派生于ClassLoader类
>   - 从java.ext.dirs系统属性所指向的目录中加载类库，==或从jdk的安装目录的jre/lib/ext子目录下加载类库。如果用户创建的jar放在此目录下，也会自动由扩展类加载器加载。==
> - 系统类加载器（AppClassLoader）：
>   - java语言编写，派生与ClassLoader类，父类加载器是扩展类加载器
>   - ==它负责加载环境变量classpath或系统属性java.class.path指定路径下的类库==
>   - 该类加载器是程序中默认的类加载器，一般来说，java应用的类都是由它来完成加载
>   - 通过`ClassLoader.getSystemClassLoader()`方法可以获取到该类加载器

> 自定义类加载器：日常开发中，类的加载几乎是由上述3种类加载器配合执行的
> 
> 为什么要自定义类加载器
> 
> - 隔离加载类
> - 修改类加载的方式
> - 扩展加载源
> - 防止源码泄露

#### CLassLoader的常用方法及获取方法

> CLassLoader类，是一个抽象类，其后所有的类加载器都继承自CLassLoader（不包括启动类加载器）

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 101513.png">

#### 双亲委派机制

> java虚拟机对class文件采用的是按需加载的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，java虚拟机采用的是双亲委派模式，即把请求交由父类处理，它是一种任何委派模式。

> 工作原理：
> 
> - 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行
> - 父类加载器如果还有父类加载器，则往上请求，一直到顶
> - 如果父类加载器可以完成类加载任务，就成功返回，不行则子类加载器尝试加载

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 103811.png">

##### 双亲委派机制的优势

- 避免类的重复加载
- 保护程序的安全，防止核心API被随意篡改
  - 例如：自定义类：java.lang.String 根据双亲委派机制，会由引导类加载器加载java核心类String而不是用户自定义String，防止了API被篡改

#### 补充内容

1. 在jvm种表示两个class对象是否为同一个类的两个必要条件：
   
   1. 类的完整类名必须一致，包括包名
   2. 加载这个类的ClassLoader必须相同

2. jvm必须知道一个类型是由启动加载器加载还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么==jvm会将这个类加载器的一个引用作为类型信息的一部分保持在方法区中。==当解析一个类型到另一个类型的引用的时候，jvm需要保证这两个类型的类加载器是相同的

3. java中对类的使用分为：主动使用和被动使用
   
   - 主动使用，又分为七种情况：
     - 创建类的实例
     - 访问某个类或接口的静态变量，或者对该静态变量赋值
     - 调用类的静态方法
     - 反射（如：Class.forName("全类名") ）
     - 初始化一个类的子类
     - java虚拟机启动时被标明为启动的类
   - 除了以上七种情况，其它使用java类的方式都被看作是对类的被动使用

==注：被动使用在类加载过程中没有初始化==

### 运行时数据区 -- Runtime Data Area

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 112622.png">
粗略的运行时数据区
<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 113529.png">
具体的运行时数据区
<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 113637.png">

> java虚拟机定义了若干程序运行期间会使用到的运行时数据区，其中有一些会随着虚拟机启动而创建，随着虚拟机退出而销毁。另一些则是与线程一一对应，这些与线程对应的数据区会随着线程开始和结束而创建和销毁

> 灰色的为线程私有，红色的为多个线程共享。即：
> 
> - 每个线程：独立包括程序计数器、栈、本地栈
> - 线程间共享：堆、堆外内存（代码缓存）

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 114237.png">

> 每个jvm只有一个Runtime实例。即为运行时环境，相当于上面具体的运行时数据区。

#### JVM中的线程说明

在Hostpot JVM里，每个线程都与操作系统本地线程直接映射

- 当一个java线程准备好执行以后，此时一个操作系统的本地线程也同时创建。java线程执行终止后，本地线程也会回收

在Hostpot JVM里主要的后台系统线程是：

- 虚拟机线程
- 周期任务线程
- GC线程
- 编译线程
- 信号调度线程

#### 程序计数寄存器 -- PC Register

<img src="./pic/java查缺补漏/屏幕截图 2026-02-03 120812.png">

> jvm中的程序计数寄存器（Program Counter Register），jvm中的寄存器是对物理寄存器的一种抽象模拟

> 作用：
> 
> - pc寄存器用来存储指向下一条指令的地址，也即将要执行的指令代码。由执行引擎读取下一条指令

> 程序计数器特点：
> 
> - 是一块很小的内存空间，运行速度很快
> - 线程私有，每个线程都有自己的计数器
> - 不会存在内存溢出
> - 任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法。程序计数器会存储当前线程正在执行的java方法的jvm指令地址；如果是在执行native方法，则是未指定值（undefined）

> java程序执行过程，java源代码编译成jvm指令，jvm指令由解释器生成机器码，机器码由cpu来执行。

##### 常见问题

1. 为什么使用pc寄存器记录当前线程的执行地址呢？

> 因为cpu需要不停的切换各个线程，这时候切换回来以后，就得知道接着从哪开始继续执行。
> jvm的字节码解释器就需要通过改变pc寄存器的值来明确下一条应该执行什么样的字节码指令

2. pc寄存器为什么是线程私有的

> 每个线程的下一条执行地址是不一样的，公有的话会导致线程执行错乱

#### 虚拟机栈

**1. 虚拟机栈简介**

> 出现的背景：由于跨平台的设计，java的指令都是根据栈来设计的。不同平台cpu架构不同，所以不能设计为基于寄存器的。
> ==优点：== 是跨平台，指令集小，编译器容易实现，缺点是性能下降，实现同样的功能需要更多的指令。

> ==栈是运行时的单位，而堆是存储的单位==
> 即：栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。堆解决的数据存储的问题，即数据怎么放、放在哪

> java虚拟机栈，也叫java栈。每个线程在创建时都会创建一个虚拟机栈，其内部保存一个个的栈帧（stack frame）对应着一次次的java方法调用。
> ==java虚拟机栈是线程私有的，生命周期与线程一致==

<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 094608.png">

> 作用：主管java程序的运行，它保存方法的局部变量(8种基本数据类型、对象的引用地址)、部分结果，并参与方法的调用和返回。

栈的特点：

- 栈是一种快速有效的分配存储方式，访问速度仅次于程序计数器
- jvm直接对java栈的操作只有两个：
  - 每个方法执行，伴随着进栈（入栈、压栈）
  - 执行结束后的出栈工作
- 对于栈来说不存在垃圾回收问题

**2. 虚拟机栈异常**

栈中可能出现的异常：

- java虚拟机规范`允许java栈的大小是动态的或者固定不变的。`
  - 如果采用固定大小的java虚拟机栈，那每一个线程的java虚拟机栈容量可以在线程创建的时候独立选定。如果线程请求分配的栈容量超过java虚拟机栈允许的最大容量，java虚拟机将会抛出一个`StackOverflowError`异常
  - 如果java虚拟机栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈，那java虚拟机将会抛出一个`OutOfMemoryError`异常

**3. 设置栈内存大小**

> 可以使用`-Xss`选项来设置线程的最大栈空间，栈的大小直接决定了函数调用的最大可达深度

**4. 栈运行原理**

- jvm直接对java栈的操作只有两个，就是对栈帧的压栈和出栈，遵循“先进后出”/“后进先出”原则
- 在一条活动线程中，一个时间点上，只会有一个活动的栈帧。即只有当前正在执行的方法的栈帧（栈顶栈帧）是有效的，这个栈帧被称为当`前栈帧（Current Frame）`，与当前栈帧相对应的方法就是`当前方法（Current Method）`，定义这个方法的类就是`当前类（Current Class）`
- 执行引擎运行的所有字节码指令只针对当前栈帧进行操作
- 如果在该方法中调用了其它方法，对应的新的栈帧会被创建出来，放在栈的顶端，成为新的当前帧。
- 不同线程中所包含的栈帧是不允许存在相互引用的
- 如果当前方法调用了其它方法，方法返回之时，当前栈帧会传回此方法的执行结果给前一个栈帧，接着，虚拟机会丢弃当前栈帧，使得前一个栈帧重新成为当前栈帧
- java方法有两种返回函数的方式，一种是正常的函数返回，使用return指令；另一个是抛出异常。不管使用哪种方式，都会导致栈帧被弹出。

**5. 栈帧的内部结构**
每个栈帧中存储着：

- 局部变量表（Local Variables）
- 操作数栈（Operand Stack）（或表达式栈）
- 动态链接（Dynamic Linking）（或指向运行时常量池的方法引用）
- 方法返回地址（Return Address）（或方法正常退出或者异常退出的定义）
- 一些附加信息

<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 105038.png">
<img src="./pic/java查缺补漏/屏幕截图 2026-02-04 110404.png">

**5.1 局部变量表**
局部变量表：

- 局部变量表也被称之为局部变量数组或本地变量表
- `定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量`，这些数据类型包括各类基本数据类型、对象引用，以及returnAddress类型
- 由于局部变量表是建立在线程的栈上，是线程的私有数据，因此`不存在数据安全问题`
- `局部变量表所需的容量大小是在编译期确定下来的`，并保存在方法的Code属性的`maximum local variables`数据项中。在方法运行期间是不会改变局部变量表的大小的
- 局部变量表中的变量只在当前方法调用中有效。当方法调用结束后，随着方法栈帧的销毁，局部变量表也会随之销毁。

Slot的理解 -- 局部变量表存储单元：

- 参数值的存放总是局部变量数组的index0开始，到数组长度-1的索引结束
- 局部变量表，最基本的存储单元是Slot（变量槽）
- 局部变量表里，32位以内的类型只占用一个slot（包括returnAddress类型）64位的类型（long和double）占用两个slot
- JVM会为局部变量表中的每一个slot都分配一个访问索引，通过这个索引即可成功访问到局部变量表中指定的局部变量值
- 当一个实例方法被调用的时候，它的方法参数和方法体内部定义的局部变量将会<mark>按照顺序被复制</mark>到局部变量表中的每一个slot中
- <mark>如果需要访问局部变量表中一个64bit局部变量值时，只需要使用前一个索引即可</mark>。（比如：访问long或double类型变量）
- 如果当前帧是由构造方法或者实例方法创建的，那么<mark>该对象引用this将会存放在index为0的slot处</mark>，其余的参数按照参数表顺序继续排列。

<img title="" src="file:///C:/Users/dwl/OneDrive/%E4%B8%B4%E6%97%B6%E6%96%87%E7%AB%A0/pic/java%E6%9F%A5%E7%BC%BA%E8%A1%A5%E6%BC%8F/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202026-02-20%20162037.png" alt="">

slot的重复利用：

> 栈帧中的局部变量表中的槽位是可以重用的，如果一个局部变量过了其作用域，那么在其作用域之后声明的局部变量就有可能会重复利用过期的局部变量的槽位，从而达到节省资源的目的。

补充说明：

> - 在栈帧中，与性能调优关系最为密切的部分就是前面提到的局部变量表。
> 
> - <mark>局部变量表中的变量也是重要的垃圾回收根节点，只要被局部变量表中直接或间接引用的对象都不会被回收。</mark>

**5.2 操作数栈（Operand Stack）**

- 每一个独立的栈帧中除了包含局部变量表以外，还包含一个后进先出的操作数栈，也可以称之为表达式栈。

- 操作数栈，在方法执行过程中，根据字节码指令，往栈中写入数据或提取数据，即入栈(push) / (pop)
  
  - 某些字节码指令将值压入操作数栈，其余字节码指令将操作数取出栈。使用它们后再把结果压入栈。
  
  - 比如：执行复制、交换、求和等操作。

- <mark>如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中，</mark>并更新pc寄存器中下一条需要执行的字节码指令。

- 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这由编译器在编译器期间进行验证，同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证。

- 另外，Java虚拟机的<mark>解释引擎是基于栈的执行引擎</mark>，其中的栈指的就是操作数栈。

- 操作数栈，<mark>主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间。</mark>

- 操作数栈就是JVM执行引擎的一个工作区，当一个方法刚开始执行的时候，一个新的栈帧也会随之被创建出来，<mark>这个方法的操作数栈是空的。</mark>

- 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，其所需的最大深度在编译期就定义好了，保存在方法的Code属性中，为max_stack的值。

- 栈中的任何一个元素都可以是任意的java数据类型。
  
  - 32bit的类型占用一个栈单位深度
  
  - 64bit的类型占用两个栈单位深度

- 操作数栈<mark>并非采用访问索引的方式来进行数据访问</mark>的，而是只能通过标准的入栈（push）和出栈（pop）操作来完成一次数据访问。

### JVM --- 内存与垃圾回收



# 设计模式

软件设计模式：又称为设计模式，是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。它描述了在软件设计过程中的一些不断重复发生的问题、以及该问题的解决方案。

设计模式分类：

1. 创建型模式：用于描述“怎样创建对象”，它的主要特点是“将对象的创建与使用分离”。
2. 结构型模式：用于描述如何将类或对象按某种布局组成更大的结构。
3. 行为型模式：用于描述类或对象之间怎样相互协作共同完成单个对象无法单独完成的任务。

### UML

UML（统一建模语言）是用来设计软件的可视化建模语言。它的特点是简单、统一、图形化、能表达软件设计中的动态与静态信息。UML从目标系统的不同角度出发，定义了用例图、类图、对象图、状态图、活动图、时序图、协作图、构建图、部署图等9种图。

#### 类图

> 概述：类图是显示了模型的静态结构，特别是模型中存在的类、类内部结构以及它们与其它类的关系等。
> 
> 作用：在软件工程中，类图是一种静态的结构图，描述了系统的类的集合，类的属性和类之间的关系，可以简化了人们对系统的理解。
>       类图是系统分析和设计阶段的重要产物，是系统编码和测试的重要模型。

##### 类图的表示方式

在类图中，类使用包含类名、属性

### 类与类之间关系的表示方式

1. 关联关系
   
   > 关联关系是对象之间的一种引用关系，用于表示一类对象与另一类对象之间的关系，如老师和学生等。
   
   关联又可以分为单向关联、双向关联、自关联
   
   1. 单向关联
      
      <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 090609.png">
   
   2. 双向关联
      
      <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 091149.png">
   
   3. 自关联
      
      <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 091243.png">

2. 聚合关系
   
   > 聚合关系是关联关系的一种，是强关联关系，是整体和部分之间的关系。
   
   <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 091854.png">

3. 组合关系
   
   > 组合关系表示类之间的整体与部分的关系，但它是一种更强烈的聚合关系。
   
   <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 092119.png">

4. 依赖关系
   
   > 依赖关系是一种使用关系，它是对象之间耦合度最弱的一种关联方式，是临时性的关联。在代码中，某个类的方法通过局部变量、方法的参数或者对静态方法的调用来访问另一个类(被依赖类)中的某些方法来完成一些职责。
   
   <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 093555.png">

5. 继承关系
   
   > 继承关系是对象之间耦合度最大的一种关系，表示一般与特殊的关系，是父类与之类之间的关系，是一种继承关系。
   
   <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 094125.png">

6. 实现关系
   
   > 实现关系是接口与实现类之间的关系。在这种关系中，类实现了接口，类中的操作实现了接口中所声明的所有的抽象操作。
   
   <img src="./pic/java查缺补漏/屏幕截图 2025-07-26 094421.png">

### 软件设计原则

目的是为了提高软件系统的可维护性和可复用性，增加软件的可扩展性和灵活性。

1. 开闭原则
   
   > 对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简而言之，是为了是程序的扩展性好，易于维护和升级。
   > 想要达到这样的效果，需要使用接口和抽象类。

### 创建者模式

> 创建型模式的主要关注点是“怎么创建对象”，它的主要特点是“将对象的创建与使用分离”。
> 可以降低系统的耦合度，使用者不需要关注对象的创建细节

创建者模式分为：

- 单例模式
- 工厂方法模式
- 抽象工程模式
- 原型模式
- 建造者模式

#### 单例设计模式

> 单例模式是java中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 
> 这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。==这个类提供了一种访问其唯一的对象的方式==，可以直接访问，不需要实例化该类的对象。

单例模式只能创建一个实例

##### 实现

1. 饿汉式 ：类加载就会导致该单实例对象被创建

> 缺点：==该方式存在内存浪费的问题==，无论是否使用该实例，实例都会被创建

```java
// 单例模式 -- 饿汉式
public class Singleton {
    // 1.私有构造方法
    private Singleton(){}

    // 2. 在本类中创建本类对象
    private static final Singleton instance = new Singleton();

    // 提供一个公共的访问方式，让外界获取实例
    public static Singleton getInstance() {
        return instance;
    }
}
// 饿汉式 -- 静态代码块
public class Singleton {
    private Singleton(){}
    private static Singleton instance;
    static {
        instance = new Singleton();
    }
    public static Singleton getInstance() {
        return instance;
    }
}

// 饿汉式 -- 枚举方式
public enum Singleton {
    INSTANCE;
}
```

2. 懒汉式：类加载不会导致该单实例对象被创建，而是首次使用该对象时才会创建

```java
// 懒汉式
public class Singleton {
    private Singleton(){}
    private static Singleton instance;

    public Singleton getInstance(){
        // 如果instance是null才创建，保证是同一个实例
        // 这个是线程不安全的，在多线程时可能创建多个实例
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// 推荐使用
// 懒汉式 -- 双重检查锁
public class Singleton {
    private Singleton() {}
    private static volatile Singleton instance;
    public static Singleton getInstance() {
        if(instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// 推荐使用
// 懒汉式 -- 静态内部类模式
// 一种优秀的单例模式，开源项目常用，没有任何性能影响且线程安全
public class Singleton {
    private Singleton() {}
    // 静态内部类只会被加载一次
    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

##### 问题 --- 单例模式破坏

1. 序列化破坏单例模式
   
   > 如果将单例模式创建的对象通过序列化写入文件，再从文件中读取出来会导致对象不再是同一个对象

2. 反射破坏单例模式
   
   > 反射

##### 解决问题

1. 序列化破坏解决
   
   > 在单例模式的类中添加`readResolve()`方法，在反序列化时被反射调用，如果定义了这个方法，就能返回这个方法的值，如果没有定义，则返回新new出来的对象

单例模式的类

```java
public class Singleton {
    private Singleton() {}
    // 静态内部类只会被加载一次
    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
    // 当进行反序列化时，会自动调用该方法，将该方法的返回值返回
    public Object readResolve() {
        return getInstance();
    }
}
```

2. 反射破坏解决

```java
public class Singleton {
    private static boolean flag = false;
    private Singleton() {
        synchronized (Singleton.class) {
            // 判断flag的值是否为true，如果是true说明不是第一次访问，抛异常，如果是false说明是第一次，正常创建对象
            if (flag) {
                throw new RuntimeException("不能创建多个对象");
            }
            flag = true;
        }
    }
    // 静态内部类只会被加载一次
    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
    // 当进行反序列化时，会自动调用该方法，将该方法的返回值返回
    public Object readResolve() {
        return getInstance();
    }
}
```

#### 工厂模式

==工厂模式：理解就是业务代码和使用代码分离==

先看一个咖啡店的例子，根据不同类型返回不同的类对象实例

```java
// Coffee基类
public abstract class Coffee {
    public abstract String getName();
    public void AddSugar() {
        System.out.println("加糖");
    }
    public void AddMilk() {
        System.out.println("加奶");
    }
}
// 美式咖啡类
public class AmericanCoffee extends Coffee{
    @Override
    public String getName() {
        return "美式咖啡";
    }
}
// 拿铁咖啡类
public class LatteCoffee extends Coffee{
    @Override
    public String getName() {
        return "拿铁咖啡";
    }
}
// 咖啡店类
public class CoffeeStore {
    // 声明coffee类型的变量，根据type的不同创建不同的对象
    public Coffee oderCoffee(String type) {
        Coffee coffee;
        if ("美式".equals(type)) {
            coffee = new AmericanCoffee();
        } else if ("latte".equals(type)) {
            coffee = new LatteCoffee();
        } else {
            throw new RuntimeException("您点的咖啡没有");
        }
        coffee.AddMilk();
        coffee.AddSugar();
        return coffee;
    }
}
```

上面这个例子有个问题，就是如果要添加新类型的咖啡就要修改咖啡店类，违背了对修改关闭的原则，耦合严重。

**解决：解耦，使用工厂模式**

##### 1. 简单工厂模式

[1] 简单工厂包含如下结构：

- 抽象产品：定义了产品的规范，描述了产品的主要特性和功能
- 具体产品：实现或者继承抽象产品的子类
- 具体工厂：提供了创建产品的方法，调用者通过该方法来创建产品

[2] 实现
上面例子中coffee类不变，加一个咖啡工厂类，咖啡店类修改一下

```java
// 业务代码和使用代码分离

// 咖啡工厂类
public class SimpleCoffeeFactory {
    public Coffee oderCoffee(String type) {
        Coffee coffee;
        if ("美式".equals(type)) {
            coffee = new AmericanCoffee();
        } else if ("latte".equals(type)) {
            coffee = new LatteCoffee();
        } else {
            throw new RuntimeException("您点的咖啡没有");
        }
        coffee.AddMilk();
        coffee.AddSugar();
        return coffee;
    }
}
// 咖啡店类 -- 这样添加新咖啡就不用修改咖啡店类了
public class CoffeeStore {
    SimpleCoffeeFactory factory = new SimpleCoffeeFactory();
    Coffee coffee = factory.oderCoffee("latte");
}
```

优缺点：

- 优点：封装了代码，把对象的创建和业务逻辑层分开，这样就避免了修改客户端代码，降低了客户端代码修改的可能性，更加容易扩展。
- 缺点：增加新产品时还是需要修改工厂类的代码，违背了“开闭原则”

静态工厂：不是23种设计模式之一，只是开发中一些人的习惯

```java
public class SimpleCoffeeFactory {
    // 相对于简单工厂，改成静态的就行
    public static Coffee oderCoffee(String type) {
        Coffee coffee;
        if ("美式".equals(type)) {
            coffee = new AmericanCoffee();
        } else if ("latte".equals(type)) {
            coffee = new LatteCoffee();
        } else {
            throw new RuntimeException("您点的咖啡没有");
        }
        coffee.AddMilk();
        coffee.AddSugar();
        return coffee;
    }
}
```

##### 2. 工厂方法模式

> 解决简单工厂违背开闭原则

> 概念：定义一个用于创建对象的接口，让子类决定实例化哪个产品类对象。工厂方法使一个产品类的实例化延迟到其工厂的子类。

工厂方法模式的主要角色：

- 抽象工厂：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法来创建产品
- 具体工厂：主要是实现抽象工厂中的抽象方法，完成具体产品的创建。
- 抽象产品：定义了产品的规范，描述了产品的主要特性和功能
- 具体产品：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间一一对应。

实现

```java
// 抽象的咖啡工厂类
public interface CoffeeFactory {
    Coffee createCoffee();
}
// 具体实现的工厂类
// 美式咖啡工厂实现
public class AmericanCoffeeFactory implements CoffeeFactory{
    @Override
    public Coffee createCoffee() {
        return new AmericanCoffee();
    }
}
// 拿铁咖啡工厂实现
public class LatteCoffeeFactory implements CoffeeFactory {

    @Override
    public Coffee createCoffee() {
        return new LatteCoffee();
    }
}
// 咖啡店类
public class CoffeeStore {
    private CoffeeFactory factory;

    public void setFactory(CoffeeFactory factory) {
        this.factory = factory;
    }
    // 点咖啡
    public Coffee orderCoffee() {
        Coffee coffee = factory.createCoffee();
        // 加配料
        coffee.AddSugar();
        coffee.AddMilk();
        return coffee;
    }
}
// 客户端使用
public class Client {
    public static void main(String[] args) {
        //创建咖啡店类
        CoffeeStore store = new CoffeeStore();
        // 创建要想的咖啡工厂类
        CoffeeFactory factory = new AmericanCoffeeFactory();
        store.setFactory(factory);
        // 点咖啡
        Coffee coffee = store.orderCoffee();
    }
}
```

优缺点：

- 优点：
  - 用户只需要知道具体工厂的名称就可得到所要的产品，无需知道产品的具体创建过程
  - 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则
- 缺点：
  - 每增加一个产品就要增加一个具体产品类和对应具体工厂类，这增加了系统的复杂度 

##### 3. 抽象工厂模式

> 前面介绍的工厂方法模式中考虑的是一类产品的生产，抽象工厂模式将考虑多等级产品的生产，将同一个具体工厂所生产的位于不同等级的一组产品称为产品族。

> ==概念：== 抽象工厂是一种为访问类提供一个创建一组相关或相互依赖对象的接口，且访问类无须指定所要产品的具体类就能得到同族的不同等级的产品的模式结构。抽象工厂模式是工厂方法模式的升级版本。

结构：

抽象工厂模式的主要角色如下：

- 抽象工厂：提供了创建产品的接口，它包含多个创建产品的方法，可以创建多个不同等级的产品
- 具体工厂：主要是实现抽象工厂中的多个抽象方法，完成具体产品的创建
- 抽象产品：定义了产品的规范，描述了产品的主要特性和功能，抽象工厂模式有多个抽象产品
- 具体产品：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间是多对一的关系。

实现：

```java
// 抽象工厂接口
public interface DessertFactory {
    // 生产咖啡功能
    Coffee createCoffee();
    // 生产甜品功能
    Dessert createDessert();
}
// 具体工厂
// 美式风味的咖啡工厂，可以生产美式咖啡和抹茶慕斯
public class AmericanDessertFactory implements DessertFactory{
    @Override
    public Coffee createCoffee() {
        return new AmericanCoffee();
    }
    // 生产甜品
    @Override
    public Dessert createDessert() {
        return new MatchaMousse();
    }
}
// 意大利风味甜品工厂，可以生产提拉米苏和拿铁咖啡
public class ItalyDessertFactory implements DessertFactory{
    @Override
    public Coffee createCoffee() {
        return new LatteCoffee();
    }
    // 生产甜品
    @Override
    public Dessert createDessert() {
        return new Trimisu();
    }
}
```

优缺点：

- 优点：
  - 当产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象
- 缺点：
  - 当产品族中需要增加一个新的产品时，所有工厂类都需要进行修改

##### 4. 模式扩展

**开发套路：简单工厂+配置文件解除耦合**

> 可以通过工厂模式+配置文件的方式解除工厂对象和产品对象的耦合。在工厂类中加载配置文件中的全类名，并创建对象进行存储，客户端如果需要对象，直接进行获取即可。

实现

```java
// 配置文件中内容，配置类的全类名
american=org.ddw.pattern.factory.config_factory.AmericanCoffee
latte=org.ddw.pattern.factory.config_factory.LatteCoffee

//咖啡工厂内容，简单工厂中的静态工厂
public class CoffeeFactory {
    // 加载配置文件，获取配置文件中的全类名，并创建该类的对象进行存储
    // 1.定义容器对象存储咖啡对象
    private static HashMap<String, Coffee> map = new HashMap<>();
    // 2.加载配置文件，只需要加载一次
    static {
        // 2.1 创建properties对象
        Properties properties = new Properties();
        // 2.2 调用properties对象中的load方法进行配置文件的加载
        InputStream resourceAsStream = CoffeeFactory.class.getClassLoader().getResourceAsStream("bean.properties");
        try {
            properties.load(resourceAsStream);
            // 从properties集合中获取全类名并创建对象
            Set<Object> keys = properties.keySet();
            for (Object key : keys) {
                String className = properties.getProperty((String) key);
                // 通过反射创建对象
                Class clazz = Class.forName(className);
                Coffee coffee = (Coffee) clazz.newInstance();
                // 将名称和对象存储到容器中
                map.put((String) key, coffee);
            }
        } catch (IOException | ClassNotFoundException | InstantiationException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
    public static Coffee createCoffee(String name) {
        return map.get(name);
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Coffee coffee = CoffeeFactory.createCoffee("american");
        System.out.println(coffee.getName());
    }
}
```

#### 原型模式

> 概述：用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型对象相同的新对象。

结构：

原型模式包含如下角色：

- 抽象原型类：规定了具体原型对象必须实现的`clone()`方法
- 具体原型类：实现抽象原型类的`clone()`方法，它是可被复制的对象
- 访问类：使用具体原型类中的`clone()`方法来复制新的对象

实现：

原型模式的克隆分为浅克隆和深克隆：

- 浅克隆：创建一个新对象，新对象的属性和原来对象完全相同，对于引用类型属性地址与旧对象属性一致，但是新对象本身的地址与旧对象地址不一样
- 深克隆：创建一个新对象，属性中引用的其它对象也会被克隆，不再指向原有对象地址 

java中的Object类中提供了`clone()`方法来实现`浅克隆`。`Cloneable`接口是上面的结构中的抽象原型类，而实现了`Cloneable`接口的子实现类就是具体的原型类

```java
// Cloneable是抽象原型类
// 具体原型类
public class Realizetype implements Cloneable{
    @Override
    protected Realizetype clone() throws CloneNotSupportedException {
        return (Realizetype) super.clone();
    }
}
```

使用案例

```java
// 具体原型类
public class Citation implements Cloneable{
    // 三好学生的姓名
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println(this.name);
    }
    @Override
    protected Citation clone() throws CloneNotSupportedException {
        return (Citation) super.clone();
    }
}
// 使用
public class CitationTest {
    public static void main(String[] args) throws CloneNotSupportedException {
        // 创建原型对象
        Citation citation = new Citation();
        // 创建克隆对象
        Citation citation1 = citation.clone();
        // 设置name
        citation.setName("张三");   //张三
        citation1.setName("李四");  //李四
        // 调用show方法
        citation.show();
        citation1.show();
    }
}
```

使用场景：

- 对象的创建非常复杂，可以使用原型模式快捷的创建对象
- 性能和安全要求比较高

##### 扩展 -- 深克隆

#### 建造者模式

> 概述：将一个复杂对象的构建与表示分离，使得同样的构建过程可以创建不同的表示。
> 
> - 分离了部件的构造（由Builder来负责）和装配（由Director负责）。从而可以构造出复杂的对象。这个模式适用于：某个对象的构建过程复杂的情况
> - 由于实现了构建和装配的解耦。不同的构建器，相同的装配，也可以做出不同的对象；相同的构建器，不同的装配顺序也可以做出不同的对象。也就是实现了构建算法、装配算法的解耦，实现了更好的复用。
> - 建造者模式可以将部件和其组装过程分开，一步一步创建一个复杂的对象。用户只需要指定复杂对象的类型就可以得到该对象，而无需知道内部的具体构造细节。

结构

建造者（Builder）模式包含如下角色：

- 抽象建造类（Builder）：这个接口规定要实现复杂对象的那些部分的创建，并不涉及具体的对象部件的创建
- 具体建造者类（ConcreteBuilder）：实现Builder接口，完成复杂产品的各个部件的具体创建方法。在构造过程完成后，提供产品的实例
- 产品类（Product）：要创建的复杂对象
- 指挥者类（Director）：调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建

实现：以组装自行车为案例

```java
// 具体产品对象类
public class Bike {
    // 车架
    private String frame;
    // 车座
    private String seat;

    public String getFrame() {
        return frame;
    }
    public void setFrame(String frame) {
        this.frame = frame;
    }

    public String getSeat() {
        return seat;
    }
    public void setSeat(String seat) {
        this.seat = seat;
    }
}

// 抽象建造类
public abstract class Builder {
    // 声明bike类型的变量，并进行赋值
    protected Bike bike = new Bike();

    public abstract void buildFrame();

    public abstract void buildSeat();

    // 构建自行车的方法
    public abstract Bike createBike();
}

// 具体的构建者，用于构建摩拜单车
public class MobileBuilder extends Builder{

    @Override
    public void buildFrame() {
        bike.setFrame("碳纤维车架");
    }

    @Override
    public void buildSeat() {
        bike.setSeat("真皮座椅");
    }

    @Override
    public Bike createBike() {
        return bike;
    }
}

// ofo单车构建者
public class OfoBuilder extends Builder{
    @Override
    public void buildFrame() {
        bike.setFrame("铝合金车架");
    }

    @Override
    public void buildSeat() {
        bike.setSeat("橡胶车座");
    }

    @Override
    public Bike createBike() {
        return bike;
    }
}

// 指挥者类
public class Director {
    // 声明builder类型的变量
    private final Builder builder;

    public Director(Builder builder){
        this.builder = builder;
    }
    // 组装自行车的功能
    public Bike construct(){
        builder.buildFrame();
        builder.buildSeat();
        return builder.createBike();
    }
}
```

优缺点：

- 优点：
  - 建造者模式的封装性很好。使用建造者模式可以有效的封装变化，在使用建造者模式的场景中，一般产品类和建造者类是比较稳定的，因此，将主要的业务逻辑封装在指挥者类中对整体而言可以取得比较好的稳定性
  - 在建造者模式中，客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象
  - 可以更加精细的控制产品的创建过程。将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更方便使用程序来控制创建过程
  - 建造者模式很容易进行扩展
- 缺点：
  - 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。

使用场景：

- 建造者模式创建的是复杂对象，其产品的各个部分经常面临着剧烈的变化，但将它们组合在一起的算法却相对稳定，所以它通常在以下场合使用
  - 创建的对象较复杂，由多个部件构成，各部件面临着复杂的变化，但构件间的建造顺序是稳定的。
  - 创建复杂对象的算法独立于该对象的组成部分以及它们的装配方式，即产品的构建过程和最终的表示是独立的

##### 模式扩展 -- 通过builder来构建对象

> 建造者模式除了上面的用途外，在开发中还有一个常用的使用方式，就是当一个类构造器需要传入参数是，创建这个类的实例的代码可读性会很差，而且很容易引入错误，此时就可以利用建造者模式进行重构。

以手机类为案例，这个类需要很多参数

```java
// 手机类
public class Phone {
    private String cpu;
    private String screen;
    private String memory;
    private String mainboard;
    // 私有内部构造函数
    private Phone(Builder builder){
        this.cpu = builder.cpu;
        this.screen = builder.screen;
        this.memory = builder.memory;
        this.mainboard = builder.mainboard;
    };
    // 静态内部构建者类
    public final static class Builder {
        private String cpu;
        private String screen;
        private String memory;
        private String mainboard;

        public Builder cpu(String cpu) {
            this.cpu = cpu;
            return this;
        }
        public Builder screen(String screen) {
            this.screen = screen;
            return this;
        }
        public Builder memory(String memory) {
            this.memory = memory;
            return this;
        }
        public Builder mainboard(String mainboard) {
            this.mainboard = mainboard;
            return this;
        }

        // 使用构建者创建对象
        public Phone build() {
            return new Phone(this);
        }
    }
}

// 使用案例
// 获取构建者对象
Phone.Builder builder = new Phone.Builder();
// 生成phone对象
Phone phone = builder.cpu("intel")
                     .screen("三星")
                     .memory("金士顿")
                     .mainboard("华硕")
                     .build();
```

### 结构型模式

> 结构型模式描述如何将类或对象按某种布局组成更大的结构。它分为类结构型模式和对象结构型模式，前者采用继承机制来组织接口和类，后者采用组合或聚会来组合对象。

#### 代理模式

> 由于某些原因需要给某个对象提供一个代理以控制该对象的访问。这时，访问对象不适合或者不能直接引用目标对象，代理对象作为访问对象和目标对象之间的中介。
