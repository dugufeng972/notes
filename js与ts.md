# Typescript

## 类型注解 与 类型推导

```ts
// 类型注解
let data: number = 234;
// 类型推导，可以推导出money是number类型，后面不能改变
let money = 32;
// 下面也是类型推导
interface Student {
  name: string;
  age: number;
}
let stu2 = {
  name: "yang",
  age: 18,
};
```

## tsconfig.json

1. 命令`tsc ts文件路径`
   - 将 ts 文件编译成 js 文件
2. 命令`tsc`不带文件路径
   - 将所有指定目录下的 ts 文件编译成 js 文件
3. `compilerOptions > outDir` 设置`tsc`命令要编译的 ts 文件位置
   - `tsconfig.json`文件中`compilerOptions`键中的`outDir`
4. 设置编译成 js 的文件输出位置
   - `compilerOptions > outDir`
5. 命令`ts-node`
   - 直接运行 ts 文件

## TS 数据类型

### 基本数据类型

> number、 string、 boolean、symbol、 null、 undefined

<mark>严格模式下，null 和 undefined 只能赋值给 null 和 undefined</mark>

### 对象类型

> Array、object、function

#### 1. function 类型

```ts
// ts中函数定义
function sum(a: number, b: number): number {
  return a + b;
}

// 函数类型
type ISum = (a: number, b: number) => number;
// 给函数类型变量赋值
let sum1: ISum = (a: number, b: number): number => {
  return a + b;
};
// 可以省略类型，ts可以自动推导
let sum2: ISum = (a, b) => {
  return a + b;
};
// 函数中的可选参数 （使用?表示可选，只能放到最后）
let sum3 = (a: string, b?: string): string => {
  return a + b;
};
// 函数的默认参数 (可以对可选参数给一个默认参数表示不传参就用默认参数：b?: string = "b")
let sum4 = (a: string, b: string = "b"): string => {
  return a + b;
};
//函数的剩余参数  (剩余运算符，类型是数组)
let sum5 = (...rest: number[]): number => {
  return 0;
};
```

##### 重载 --- 与其它语言不同

```ts
// 1. 首先声明所有重载签名 (没有函数体)
function tot(a: string): void;
function tot(a: string | number): string;
// 这里不能插入其它代码否则会报错
// 2. 最后提供实现签名 (包含具体的函数体)
function tot(a: string | number): string | void {
  if (typeof a === "string") {
    // 对于字符串参数，根据重载签名，这里可以不返回内容（void）
    console.log(a);
  } else {
    // 对于数字参数，返回字符串
    return "a";
  }
}
```

<mark>ts 中可以用 typeof 获取变量类型</mark>

```ts
let person = {
  name: "ts",
  age: 20,
};

// typeof person 得到的是person的类型{name: string; age: number}
// ts中this类型需要手动指定，且默认是函数的第一个参数
// 可以用keyof获取对象类型中的key类型，这里的key类型是："name" | "age"
function getVal(this: typeof person, key: keyof typeof person): void {
  console.log(this.age + key);
}

getVal.call(person, "name");
```

#### 2. 类

> 类本身就可以充当类型

1. 类的定义
   
   > ts 中所有没有默认值的属性，必须要在构造函数中赋值(可选属性和静态属性除外)
   
   > 可以不给属性添加类型，ts 可以在构造函数中或从默认值中推导出属性类型

```ts
// ts中所有没有默认值的属性，必须要在构造函数中赋值(可选属性除外)
// 可以不给属性添加类型，ts可以在构造函数中或从默认值中推导出属性类型
class Circle {
  public r: number;
  public x = 0;
  public y = 0;
  public z?;
  constructor(r: number, z?: number) {
    this.r = r;
    this.z = z;
  }
}
let c = new Circle(20);
```

2. 类的简化定义
   
   > ts 类可以将属性简写到构造函数中

```ts
class Animal {
  constructor(public name: string, public age?: number) {}
}
let tom = new Animal("tom");
```

3. 类的继承
   
   > 必须包含父类中所有必选属性，必须使用 super 调用父类中的构造函数

```ts
class Cat extends Animal {
  // 必须包含父类中所有必选属性
  constructor(public home: string, public name: string, public age?: number) {
    super(name, age);
  }
}
```

4. 类的访问类型
- `public`：公开属性，不加修饰符默认是 `public`

- `protected`：受保护属性（只有本类和子类能访问）

- `private`：只有本类中能访问

- `readonly`：只读属性，初始化不能再次修改

- `static`：静态属性
5. 类的功能 -- 实例属性、原型属性、静态属性、属性访问器

> 1. 属性访问器：用于访问父类中私有属性

```ts
class A {
  private _sound!: string;
  constructor(name: string, age: number) {}
  // 属性访问器
  get sound() {
    return this._sound;
  }
  set sound(value: string) {
    this._sound = value;
  }
}
class B extends A {
  constructor(name: string, age: number) {
    super(name, age);
  }
}
// 使用
let b = new B("cat", 10);
b.sound = "喵喵叫";
console.log(b.sound);
```

> 2. 访问器属性：允许像访问普通属性一样访问和设置类成员（通常是私有属性），但背后实际上是调用函数。

```ts
class A {
  private _sound!: string;
  constructor(name: string, age: number) {}
  get sound() {
    return this._sound;
  }
  set sound(value: string) {
    this._sound = value;
  }
  eat(foot: string): void {
    console.log(foot);
  }
}

const a = new A("aa", 23);
// 像属性一样使用sound方法
// 通过sound方法设置私有属性sound
a.sound = "sss";
// 通过sound方法获取私有属性sound
console.log(a.sound);

class B extends A {
  constructor(name: string, age: number) {
    super(name, age);
  }
  // 覆写父类中发方法
  eat(foot: string): void {}
}
```

> 3. 静态属性：静态的属性和方法，静态属性是挂载在类上的，不需要构造器初始化，该类的所有之类都可以访问静态属性

```ts
class A {
  static habitat: string;
  private _sound!: string;
  constructor(name: string, age: number) {}
  get sound() {
    return this._sound;
  }
  set sound(value: string) {
    this._sound = value;
  }
  eat(foot: string): void {
    console.log(foot);
  }
}
class B extends A {
  constructor(name: string, age: number) {
    super(name, age);
  }
  eat(foot: string): void {}
}
```

6. 私有化的构造器 --- 单例模式

```ts
class Singleton {
  private constructor() {}
  private static instance = new Singleton();
  static getIns() {
    return this.instance;
  }
}

let ins1 = Singleton.getIns();
let ins2 = Singleton.getIns();
console.log(ins1 === ins2); // true
```

7. 抽象类
   
   > 包含抽象方法的类，不能被实例化只能被继承实现，不能包含静态属性和方法
   
   > 抽象类可以包含具体的属性和方法

```ts
abstract class Person {
  abstract eat(a: string): void;
  drink(a: string): void {
    console.log(a);
  }
}
```

### 根类型

> Object、{}
> 根类型除了 null 和 undefined 不能赋值给它，其余变量都可以赋值给它

注意：`object`只能接受对象类型的，如`{age: 18}`。而`Object`则可以接受很多类型

### 枚举

> enum

```ts
// 默认情况下枚举的value值是number类型的且是从0自增的，可以设定第一个value的值，来控制后面的值
enum Week {
  Monday = 1,
  Tuesday,
  Wensday,
  Thirsday,
}
// 调用
console.log(Week.Monday);

console.log(Week["Wensday"]);
// 通过value来找key
console.log(Week[1]);

// 字符串枚举，自定义字符串value后面所有value必须是自定义的
// 除了不能从value找到key，其余与数字枚举一样
enum Weekend {
  Monday = "MyMonday",
  Tuesday = "T",
  Wensday = "W",
  Thirsday = "t",
}

enum Weekend1 {
  Monday,
  Tuesday,
  Wensday = "w",
  Thirsday = "t",
}
// 常量枚举
const enum User {
  USER,
  TH,
}
```

### 其它特殊类型

> any、unknown、never、void、tuple(元组)

1. any 和 unknown
- any：含义是任何类型，被 any 修饰的变量将会被放弃类型检查，any 也可以被赋值给任何类型的变量
- unknown：含义是不确定的类型，可以接受任何类型的变量，unknown 只能赋值给 unknown，unknown 类型的变量不能获取属性和方法

```ts
let a: any = 90;
a = "ap"; // 这是对的
let n: number = a; // 这是可以的
```

2. never

3. void
   
   > void 常用于声明函数的返回值类型，表示没有任何返回值。
   > 但在 TypeScript 中，返回值类型为 void 的函数类型可以接受任何返回值的函数。例如下面不会报错

```ts
type VoidFunc = (a: string) => void;
function fn(callback: VoidFunc) {}
fn(("a") => {
  // 这个返回值不会报错，这个是为了兼容js
  return "a";
});
```

### 合成类型

1. 联合类型
   
   > 表示一个可以是几种类型之一，在被赋值时确定下到底是什么类型

```ts
// i只要是string或number都可以赋值给它
let i: string | number;
i = "string";
i = 20;
// 可以将i赋值给string或number，但类型要匹配
let ni: number;
ni = i; // 此时i是number类型可以赋值
let j: string | number | boolean;
// 定义一个类型
type A = string | number;
// 可以直接用值来定义类型，B只接受0或者1
type B = 0 | 1;
let b: B;
b = 0; // b只能是0或1
// 联合类型比较灵活，有很多组合
let a: (string | number)[];
a = [2, 3, 4, 5];
```

### 断言与 typeof

<mark>断言收缩类型</mark>

> 断言只能在变量赋值后使用

```ts
// 非空断言，这个值一定不为空
let ele: HTMLElement | null = document.getElementById("app");
// ele!，非空断言，认定ele不为空，这时可以使用HTMLElement的方法，如果为空则报错
ele!.style.background = "red";
// as 断言，将ele认定为某种类型，注意这种类型必须是ele可能的类型
(ele as HTMLElement).style.background = "red";

//双重断言
let sss: string | boolean = false;

sss as boolean as true;
```

<mark>注：typeof 收缩类型</mark>

```ts
// 1. 使用typeof收缩类型
function getR(strOrNumberOrBool: string | number | boolean): void {
  // 这里strOrNumberOrBool不能用字符串的方法，因为不确定strOrNumberOrBool的类型
  if (typeof strOrNumberOrBool == "string") {
    // 这里可以用字符串的方法，strOrNumberOrBool的类型被收缩成string了
    strOrNumberOrBool.includes("a");
  }
}
```

<mark>ts 中可以用 typeof 获取变量类型</mark>

```ts
let person = {
  name: "ts",
  age: 20,
};

// typeof person 得到的是person的类型{name: string; age: number}
// ts中this类型需要手动指定，且默认是函数的第一个参数
// 可以用keyof获取对象类型中的key类型，这里的key类型是："name" | "age"
function getVal(this: typeof person, key: keyof typeof person): void {
  console.log(this.age + key);
}

getVal.call(person, "name");
```

1. 交叉类型
   
   > 多个不同类型合成一个类型

```ts
// 定义一个类型
type Obj1 = {
  usn: string;
};
// 定义一个类型
type Obj2 = {
  age: number;
};

let obj: Obj1;
// 联合类型，必须同时满足Obj1和Obj2
let obj1: Obj1 & Obj2 = { usn: "usn", age: 18 };
```

## 类与接口

### 类

1. 类可以实现多个接口

见上面类型中的类

### 接口

```ts

```

1. 接口定义

```ts
interface IFullname {
  firstname: string;
  lastname: string;
}
```

2. interface 与 type 的区别
   
   > 如果只是用来描述类一样的结构采用 interface
   > 如果涉及联合类型，则只能使用 type 来进行声明，且 type 不能被扩展
   > type 不能重名，interface 重名可以合并
   > type 在后续的学习中可以使用循环和条件，interface 不行

3. interface 和 type 中匿名函数赋值方法
   
   > 匿名函数可以定义多个，但是前面的匿名函数会覆盖后面的匿名函数无法调用
   > 给有匿名函数的 interface 和 type 类型赋值用`Object.assign`

```ts
interface Ifn {
  (): number;
  count: number;
  (a: string): string; // 被覆盖无法调用
}

let click: Ifn = Object.assign(
  () => {
    return 0;
  },
  { count: 0 },
  (a: string) => {
    return "r";
  }
);

console.log(click("a")); // 输出0
```

4. 给 interface 添加修饰符
   
   ```ts
   interface IVeg {
     readonly color: string;
     size: number;
     taste?: "sweet" | "sour"; // 可选的
   }
   
   const tomato: IVeg = {
     color: "red",
     size: 20,
   };
   ```

5. 如果对象的属性或方法多于接口，可以用下面的办法解决
- 1. 采用断言的方式

```ts
interface IVeg {
  readonly color: string;
  size: number;
  taste?: "sweet" | "sour";
}

const tt: IVeg = {
  color: "r",
  size: 20,
  taste: "sour",
  a: 1, // 多一个a
} as IVeg;
```

- 2. 基于接口的特性写一个同名的接口
     
     > 注意这种方法会影响实现同一个接口的对象，最好在新加的属性上加上可选

```ts
interface IVeg {
  readonly color: string;
  size: number;
  taste?: "sweet" | "sour";
}

interface IVeg {
  a: number;
}
const tt: IVeg = {
  color: "r",
  size: 20,
  taste: "sour",
  a: 1, // 多一个a
};
```

- 3. 产生新类型，通过继承原有属性的方式

```ts
class II implements IVeg {
  constructor(readonly color = "c", public size = 20) {}
  taste?: "sweet" | "sour";
  a?: number;
}
```

- 4. 通过任意类型来扩展 --- 最常用

```ts
interface IVeg {
  readonly color: string;
  size: number;
  taste?: "sweet" | "sour";
  [key: string]: any; // 任意类型，可以接受任意数量的键值对
}
```

6. 通过索引访问符，可以获得值的类型

```ts
type IVegColor = IVeg["color"];
```

## 泛型

### 1. 泛型在函数上的使用

```ts
// 具名函数
function Ct<T>(a: T) {
  let arr: T[];
  return a;
}
// 调用
let s = Ct("ss");
// 和上面的效果是一样的
let s1 = Ct<string>("sss");
// 匿名函数
type func = <T>(a: T) => T;
// 下面与上面不一样，上面T的类型取决于调用函数传入的类型，下面T的类型取决于使用func1类型时传入的类型
type func1<T> = (a: T) => T;
```

### 2. 泛型在接口上的使用

```ts
interface IClass<T> {
  func(params: T): T;
  func2<U>(params: U): U;
  readonly a: string;
}
// 实现该接口时必须有具体的类型代替泛型
class IIcl implements IClass<string> {
  func(params: string): string {
    throw new Error("Method not implemented.");
  }
  a: string;
  constructor(a: string) {
    this.a = a;
  }
  func2<U>(params: U): U {
    throw new Error("Method not implemented.");
  }
}
```

### 3. 泛型在类上的使用

```ts
class Cll<T> {
  constructor(public a: T, public b: T[]) {}
}
// new对象
let cll = new Cll<number>(1, [1, 1]);
```

### 4. 泛型是有默认值的

```ts
type Union<T = boolean> = T | number | string;

let u: Union = true;
```

### 5. 泛型约束

- `extends` : `A extends B`要求 A 必须是 B 的子类或同类

```ts
// T extends string | number
// T必须是string或者number

// T 必须是 string的子类，也就是T必须是string类型
function handle<T extends string>(val: T): T {
  return val;
}
handle("string");

// 对于对象而言
function handle2<T extends IWithLen>(val: T) {}
//{ a: 1, number: 2, length: 3 } 可以看作是 IWithLen 的子类
handle2({ a: 1, number: 2, length: 3 });

// 下面的K只能是name或者age
function handle3<T, K extends keyof T>(obj: T, key: K) {}
handle3({ name: "ts", age: "20" }, "age");
```

## 条件类型及类型层级

### 1. 条件类型

> 条件类型通常和泛型约束一起使用，类似三元运算符

```ts
// 根据传入的类型影响泛型T的类型，看泛型T是否符合条件，如果符合则R<T>类型为"success"，反之为"fail"
type R<T> = T extends 200 | 20 ? "success" : "fail";
// 下面的a是fail类型
let a: R<"a"> = "fail";
```

### 2. 类型层级

1. 兼容性
   
   > 兼容性指的是什么值可以赋值给什么类型

2. 层级
   
   > 窄类型可以赋值给宽类型
   > 例如："a" 可以赋值给 "a" | "b" | "c"
   > `string`类型可以赋值给 `String`类型
   
   > 从结构上考虑，交叉类型 可以赋予 交叉前的类型
- never 是最窄的类型
- 字面量类型可以赋值给字面量联合类型
- 字面量类型可以赋予基础类型
- 基础类型是包装类型的子类型
- any unknown 是最大的类型
- never < 字面量 < 字面量联合类型 | 字面量类型 < 原始数据类型 < 包装类型 < Object < any | unknown

```ts
// "a" 可以赋值给 "a" | "b" | "c"
```

## 装饰器

> 装饰器就是一个函数，只能在类中使用（类本身，类成员使用）
> 装饰器的分类：类的装饰器，方法装饰器，属性装饰器，访问装饰器，参数装饰器

## 模块与命名空间

### 1. 常用模块规范 --- es6 和 commonjs

> 这两个规范不能在 ts 中混用

### 命名空间

> 在命名空间中声明的变量，如果不导出则在命名空间外无法访问，也可将命名空间导出，其实就是一个变量作用域

```ts
namespace Zoo {
  // 在命名空间外可以正常访问
  export let dog = "dog";
  export let cat = "cat";
  // 未导出，命名空间外无法访问
  let dock = "dock";
  console.log(dock);
}
// 访问命名空间内变量
console.log(Zoo.dog);

// 导出命名空间
export namespace Home {
  export let dog = "dog";
  export let cat = "cat";
}
```

## 类型声明文件

> 有些代码使用 cdn 引入，或者有些包使用 js 来写的没有提示，这些代码直接在 ts 中使用会报错其实没有错

> ts 默认会检测当前项目下所有的.d.ts 文件
> **d.ts 文件示例**

```ts
declare let age: number;
// 这里面不能有实现，只能是类型声明帮助提示
declare function sum(a: number, b: number): number;

declare class Person {}

declare enum Seasons {
  Spring,
  Summer,
  Autumn,
  Winte,
}

declare interface IVeg {
  size: number;
  color: string;
}

declare namespace Zoo {
  export let dog: string;
}
```

# Javascript

## Map 和 Set

> JavaScript 的默认对象表示方式{}可以视为其他语言中的 Map 或 Dictionary 的数据结构，即一组键值对。但是 JavaScript 的对象有个小问题，就是键必须是字符串。如果不是字符串就没办法通过`.属性名`的方法来访问了。Map 可以解决这个问题。

```javascript
//Map的使用
let m = new Map(); // 空Map
m.set("Adam", 67); // 添加新的key-value
m.set("Bob", 59);
m.has("Adam"); // 是否存在key 'Adam': true
m.get("Adam"); // 67
m.delete("Adam"); // 删除key 'Adam'
m.get("Adam"); // undefined
```

Set 是一组不重复的值的集合，输入的重复的值会自动被过滤。

```javascript
let s1 = new Set(); // 空Set
let s2 = new Set([1, 2, 3]); // 含1, 2, 3
let s = new Set([1, 2, 3, 3, "3"]);
s; // Set {1, 2, 3, "3"}
//通过add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
//通过delete(key)方法可以删除元素
let s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```

### iterable

遍历 Array 可以采用下标循环，遍历 Map 和 Set 就无法使用下标。为了统一集合类型，ES6 标准引入了新的 iterable 类型，Array、Map 和 Set 都属于 iterable 类型。
具有 iterable 类型的集合可以通过新的 for ... of 循环来遍历。

```javascript
let a = ["A", "B", "C"];
let s = new Set(["A", "B", "C"]);
let m = new Map([
  [1, "x"],
  [2, "y"],
  [3, "z"],
]);
for (let x of a) {
  // 遍历Array
  console.log(x);
}
for (let x of s) {
  // 遍历Set
  console.log(x);
}
for (let x of m) {
  // 遍历Map
  console.log(x[0] + "=" + x[1]);
}
```

## 函数

### 解构赋值

从 ES6 开始，JavaScript 引入了解构赋值，可以同时对一组变量进行赋值。

```javascript
//在ES6中，可以使用解构赋值，直接对多个变量同时赋值
let [x, y, z] = ["hello", "JavaScript", "ES6"];
//如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致
let [x, [y, z]] = ["hello", ["JavaScript", "ES6"]];
//解构赋值还可以忽略某些元素
let [, , z] = ["hello", "JavaScript", "ES6"]; // 忽略前两个元素，只对z赋值第三个元素
//如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性
let person = {
  name: "小明",
  age: 20,
  gender: "male",
  passport: "G-12345678",
  school: "No.4 middle school",
};
let { name, age, passport } = person;
//对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的
let person = {
  name: "小明",
  age: 20,
  gender: "male",
  passport: "G-12345678",
  school: "No.4 middle school",
  address: {
    city: "Beijing",
    street: "No.1 Road",
    zipcode: "100001",
  },
};
let {
  name,
  address: { city, zip },
} = person;
/**
 * 使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为undefined
 * 可以使用默认赋值的办法来防止对应的属性不存在
 */
let person = {
  name: "小明",
  age: 20,
  gender: "male",
  passport: "G-12345678",
};
// 如果person对象没有single属性，默认赋值为true:
let { name, single = true } = person;
```

### 方法

在一个对象中绑定函数，称为这个对象的方法。

```javascript
let xiaoming = {
  name: "小明",
  birth: 1990,
  age: function () {
    let y = new Date().getFullYear();
    return y - this.birth;
  },
};

xiaoming.age; // function xiaoming.age()
xiaoming.age();
//也可以分开写
function getAge() {
  let y = new Date().getFullYear();
  return y - this.birth;
}

let xiaoming = {
  name: "小明",
  birth: 1990,
  age: getAge,
};

xiaoming.age(); // 25, 正常结果
getAge(); // NaN
```

**在一个方法内部,`this`是一个特殊变量，它始终指向当前对象，也就是上面这个案例中的`xiaoming`这个变量**
**==注：==JavaScript 的函数内部如果调用`this`,那么`this`总是指向调用者**

```javascript
let fn = xiaoming.age; // 先拿到xiaoming的age函数
//在全局作用域中所有函数和方法都挂载在window上，所以此时调用fn()内的this指向window
fn(); // NaN
```

为了解决 this 指向不明确导致错误的问题，可以使用 apply 来控制 this 的指向。

要指定函数的 this 指向哪个对象，可以用函数本身的 apply 方法，它接收两个参数，**第一个参数就是需要绑定的 this 变量**，**第二个参数是 Array**，表示函数本身的参数。

```javascript
function getAge() {
  let y = new Date().getFullYear();
  return y - this.birth;
}

let xiaoming = {
  name: "小明",
  birth: 1990,
  age: getAge,
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

另一个与 apply()类似的方法是 call()，唯一区别是:

- apply()把参数打包成 Array 再传入
- call()把参数按顺序传入

比如调用 Math.max(3, 5, 4)，分别用 apply()和 call()实现如下:

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

==对普通函数调用，我们通常把 this 绑定为 null==

### 高级函数

一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数

```javascript
//f是定义在他处的一个函数
function add(x, y, f) {
  return f(x) + f(y);
}
```

### map/reduce

map()方法定义在 JavaScript 的`Array`中，我们调用`Array`的 map()方法，传入我们自己的函数，map 会将调用它的数组中每一个元素都传入接收到的函数中处理，如果接受到的函数有返回值，就将每个返回值放到一个数组中返回。

```javascript
function pow(x) {
  return x * x;
}
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
//将arr中每个数取平方，并将每个结果放到一个数组中返回
let results = arr.map(pow);
console.log(results); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Array 的 reduce()函数必须接收两个参数，reduce()把数组的前两个元素按传入的函数处理的结果与数组的下一个元素再次放入参数函数中处理。。。依次类推。

```javascript
let arr = [1, 3, 5, 7, 9];
//1+3=4
//4+5=9
//9+7=16
//16+9=25
arr.reduce(function (x, y) {
  return x + y;
}); // 25
//如果数组元素只有1个，那么还需要提供一个额外的初始参数以便至少凑够两
let arr = [123];
arr.reduce(function (x, y) {
  return x + y;
}, 0); // 123
```

### filter

filter 也是一个常用的操作，它用于把 Array 的某些元素过滤掉，然后返回剩下的元素。
filter()把传入的函数依次作用于每个元素，然后根据返回值是 true 还是 false 决定保留还是丢弃该元素。

```javascript
//例如，在一个Array中，删掉偶数，只保留奇数，可以这么写
let arr = [1, 2, 4, 5, 6, 9, 10, 15];
let r = arr.filter(function (x) {
  return x % 2 !== 0; //这里返回布尔值,返回true保留
});
r; // [1, 5, 9, 15]
```

filter()接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示 Array 的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身

### sort

JavaScript 的 Array 的 sort()方法就是用于排序的,Array 的 sort()方法默认把所有元素先转换为 String 再排序,sort()可以接收一个比较函数来实现自定义的排序

```javascript
let arr = [10, 20, 1, 2];

arr.sort(function (x, y) {
  if (x < y) {
    return -1; //返回负值排在前面
  }
  if (x > y) {
    return 1; //返回正值排在前面
  }
  return 0; //返回0拍中间
});

console.log(arr); // [1, 2, 10, 20]
```

### Array 其它方法

> 对于数组，除了 map()、reduce、filter()、sort()这些方法可以传入一个函数外，Array 对象还提供了很多非常实用的高阶函数。

#### every

> `every()`方法可以判断数组的所有元素是否满足测试条件

```javascript
let arr = ["Apple", "pear", "orange"];
console.log(
  arr.every(function (s) {
    return s.length > 0;
  })
); // true, 因为每个元素都满足s.length>0
```

#### find

> `find()`方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回 undefined

```javascript
let arr = ["Apple", "pear", "orange"];
console.log(
  arr.find(function (s) {
    return s.toLowerCase() === s;
  })
); // 'pear', 因为pear全部是小写

console.log(
  arr.find(function (s) {
    return s.toUpperCase() === s;
  })
); // undefined, 因为没有全部是大写的元素
```

#### findIndex

> `findIndex()`和`find()`类似，也是查找符合条件的第一个元素，不同之处在于`findIndex()`会返回这个元素的索引，如果没有找到，返回-1

```javascript
let arr = ["Apple", "pear", "orange"];
console.log(
  arr.findIndex(function (s) {
    return s.toLowerCase() === s;
  })
); // 1, 因为'pear'的索引是1

console.log(
  arr.findIndex(function (s) {
    return s.toUpperCase() === s;
  })
); // -1
```

#### forEach

> `forEach()`和`map()`类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。`forEach()`常用于遍历数组

```javascript
let arr = ["Apple", "pear", "orange"];
arr.forEach((x) => console.log(x)); // 依次打印每个元素
```

### 闭包

### 箭头函数

箭头函数修复了 this 指向不明确的问题，箭头函数内部的 this 是词法作用域，由上下文确定。

```javascript
let obj = {
  birth: 1990,
  getAge: function () {
    let b = this.birth; // 1990
    let fn = function () {
      return new Date().getFullYear() - this.birth; // this指向window或undefined,这里的this总是指向调用者
    };
    return fn();
  },
};

//-------------------
let obj = {
  birth: 1990,
  getAge: function () {
    let b = this.birth; // 1990
    let fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
    return fn();
  },
};
obj.getAge(); // 25
```

> 由于 this 在箭头函数中已经按照词法作用域绑定了，所以，用 call()或者 apply()调用箭头函数时，无法对 this 进行绑定，即传入的第一个参数被忽略

### 标签函数

```javascript
const email = "test@example.com";
const password = "hello123";

function sql(strings, ...exps) {
  console.log(`SQL: ${strings.join("?")}`);
  console.log(`SQL parameters: ${JSON.stringify(exps)}`);
  return {
    name: "小明",
    age: 20,
  };
}
//这是一个奇怪的用法，模板字符串前面以sql开头，实际上这是一个标签函数，上述语法会自动转换为对sql()函数的调用
/**
 * sql()函数实际上接收两个参数：
    第一个参数strings是一个字符串数组，它是["SELECT * FROM users WHERE email=", " AND password=", ""]，即除去${xxx}剩下的字符组成的数组；
    第二个参数...exps是一个可变参数，它接收的也是一个数组，但数组的内容是由模板字符串里所有的${xxx}的实际值组成。
*/
const result = sql`SELECT * FROM users WHERE email=${email} AND password=${password}`;

console.log(JSON.stringify(result));
```

### 生成器

> 函数在执行过程中，如果没有遇到 return 语句（函数末尾如果没有 return，就是隐含的 return undefined;），控制权无法交回被调用的代码。

> generator 和函数不同的是，generator 由 function*定义（注意多出的*号），并且，除了 return 语句，还==可以用 yield 返回多次==

> generator 定义如下，例：

```javascript
function* foo(x) {
  yield x + 1;
  yield x + 2;
  return x + 3;
}
```

## Web worker --- JS 多线程工作

> js 语言采用单线程模型，也就是所有任务只能在一个线程上完成，web worker 的出现为 js 创造多线程环境，允许主线程创建 worker 线程，将一些任务分配给后者允许，在主线程运行的同时，worker 线程在后台运行，两者互不干扰。

### 

## 标准对象

==typeof 操作符可以判断出 number、boolean、string、function 和 undefined==
==typeof 无法区分 Object、Array==

### Date

在 JavaScript 中，Date 对象用来表示日期和时间

要获取系统当前时间，用：

```javascript
let now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

创建一个指定日期和时间的 Date 对象，可以用：

```javascript
let d = new Date(2015, 5, 19, 20, 15, 30, 123);
console.log(d); // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
```

将时间戳转换为一个 Date：

```javascript
let d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```

### JSON

如果我们收到一个 JSON 格式的字符串，只需要把它反序列化成一个 JavaScript 对象，就可以在 JavaScript 中直接使用这个对象了。
将 js 对象序列化成 json 格式：

```javascript
//xiaoming为js对象，s为序列化成json格式的字符串
let s = JSON.stringify(xiaoming);
```

反序列化：
拿到一个 JSON 格式的字符串，我们直接用 JSON.parse()把它变成一个 JavaScript 对象

```javascript
JSON.parse("[1,2,3,true]"); // [1, 2, 3, true]
JSON.parse('{"name":"小明","age":14}'); // Object {name: '小明', age: 14}
JSON.parse("true"); // true
JSON.parse("123.45"); // 123.45
```

## 面向对象

### 浏览器对象

#### window

window 对象不但充当全局作用域，而且表示浏览器窗口。
window 对象有 innerWidth 和 innerHeight 属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。还有一个 outerWidth 和 outerHeight 属性，可以获取浏览器窗口的整个宽高。

#### navigator

navigator 对象表示浏览器的信息，最常用的属性包括：

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的 User-Agent 字符串
  **注意**，navigator 的信息可以很容易地被用户修改，所以 JavaScript 读取的值不一定是正确的。

#### screen

screen 对象表示屏幕的信息，常用的属性有：

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如 8、16、24。

#### location

location 对象表示当前页面的 URL 信息。可以用`location.href`获取
要加载一个新页面，可以调用`location.assign()`。如果要重新加载当前页面，调用`location.reload()`方法

#### document(这个最重要)

document 对象表示当前页面。由于 HTML 在浏览器中以 DOM 形式表示为树形结构，document 对象就是整个 DOM 树的根节点。

#### history

history 对象保存了浏览器的历史记录，JavaScript 可以调用 history 对象的 back()或 forward ()，相当于用户点击了浏览器的“后退”或“前进”按钮。

#### ==操作 DOM(重要)==

HTML 文档被浏览器解析后就是一棵 DOM 树，要改变 HTML 的结构，就需要通过 JavaScript 来操作 DOM。
操作一个 DOM 节点实际上就是这么几个操作：

- 获取：获取要操作的 dom 节点，以方便后续操作
- 更新：更新该 DOM 节点的内容，相当于更新了该 DOM 节点表示的 HTML 的内容；
- 遍历：遍历该 DOM 节点下的子节点，以便进行进一步操作；
- 添加：在该 DOM 节点下新增一个子节点，相当于动态增加了一个 HTML 节点；
- 删除：将该节点从 HTML 中删除，相当于删掉了该 DOM 节点的内容以及它包含的所有子节点。

#### 获取 dom

1. 最常用的方法是`document.getElementById()`和`document.getElementsByTagName()`，以及 CSS 选择器`document.getElementsByClassName()`
2. 第二种方法是使用`querySelector()`和`querySelectorAll()`

```javascript
// 通过querySelector获取ID为q1的节点：
let q1 = document.querySelector("#q1");

// 通过querySelectorAll获取q1节点内的符合条件的所有节点：
let ps = q1.querySelectorAll("div.highlighted > p");
```

#### 更新 DOM

修改节点的文本，方法有两种：

- 一种是修改`innerHTML`属性
- 第二种是修改`innerText`或`textContent`属性

#### 插入 DOM 和 创建节点

有两个办法可以插入新的节点。

- 一个是使用`appendChild`
- 第二种是使用`insertBefore`，使用方法`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。

```javascript
//使用 appendChild
let list = document.getElementById("list"),
  //创建新节点
  haskell = document.createElement("p");
haskell.id = "haskell";
haskell.innerText = "Haskell";
list.appendChild(haskell);
//使用 insertBefore
let list = document.getElementById("list"),
  ref = document.getElementById("python"),
  haskell = document.createElement("p");
haskell.id = "haskell";
haskell.innerText = "Haskell";
list.insertBefore(haskell, ref);
```

#### 删除 DOM
