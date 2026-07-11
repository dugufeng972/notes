### 动态组件

vue 中用`component`来实现动态切换组件的功能，例：

```vue
<!-- is属性后面是当前展示的组件 -->
<component is="currentComponent"></component>
<!-- is属性可以使用绑定 -->
<component :is="currentComponent"></component>
```

# react

## 安装使用

`npx create-react-app projectName`：这只是一个最基础的创建，没有路由和状态管理，底层由webpack构建


## 目录结构
`src`:项目的代码部分
`index.html`:实际展示的页面
`index.js`:是项目入口，用来在index.html中id为root节点中渲染App组件
`App.js`:是项目的根组件
``

## JSX
> JSX：JSX是js和xml(html)的缩写，表示在js代码中编写html模板结构，它是react中编写ui模板的方式

优势：
- html的声明式模板写法
- js的可编程能力

jsx不是标准的js语法，它是js的语法扩展，浏览器本身不能识别，需要通过解析工具(babel)做解析之后才能在浏览器中运行

### jsx中使用js表达式
在jsx中可以在`标签内`通过大括号语法{}识别js中的表达式，比如常见的变量、函数调用、方法调用等
布尔值不能渲染

### jsx中实现列表渲染
```jsx
<ul>
    {list.map(item => <li key={item.id}>{item.name}</li>)}
</ul>
```

### jsx实现条件渲染
在react中，可以通过逻辑与运算符&&、三元表达式实现基础的条件渲染
```js
// flag为true显示<span>this is span</span>
//flag为false不显示
{flag && <span>this is span</span>}
//loading
{loading ? <span>loading...</span> : <span>this is span</span>}
//复杂条件渲染
function getA() {
  if(count === 1) {
    return <span>1</span>
  } else if (count === 2) {
    return <span>2</span>
  } else {
    return <span>3</span>
  }
}
```

## react中事件绑定
语法：on + 事件名称 = {事件处理程序}，整体上遵循驼峰命名法
```js
const clickHandler = () => {
  console.log("sd");
}
<button onClick={clickHandler}></button>
```
### 传递自定义参数
语法：事件中绑定的位置改造成箭头函数的写法，在执行clickHandler实际处理业务函数的时候传递实参
```js
function clh(uname) {
  console.log(uname);
}
<button onClick={() => clh(count)}>btn</button>
```

## 组件
在react中，一个组件就是首字母大写的函数，内部存放了组建的逻辑和视图ui，渲染组件只需要把组件当成标签书写即可

### useState基础使用---用于创建响应式变量
> useState是一个react hook，它允许我们向组件添加一个状态变量，从而控制影响组件的渲染结果
<img src="./pic/vue 与 react/屏幕截图 2025-08-09 130709.png">

```js
// cc是变量，setCC是用来改变cc的，0是cc的初始值
const [cc, setCC] = useState(0);

<button onClick={() => setCC(cc+1)}>cc++</button>
```

### useState修改状态规则
> 在react中，状态被认为是只读的，应该始终替换它而不是修改它，直接修改不能引发视图更新

<img src="./pic/vue 与 react/屏幕截图 2025-08-09 131452.png">

> 对于对象类型的状态变量，应该始终传递给set方法一个全新的对象来进行修改，直接修改不能引发视图更新

```jsx
function App() {
  const [person, setPerson] = useState({ name: "ddw", age: 22 });
  return (
    <>
      <div className="counter">{person.name}</div>
      <div className="counter">{person.age}</div>
      //传入一个新的对象来修改
      <button onClick={() => setPerson({ ...person, age: person.age + 1 })}>
        +1
      </button>
    </>
  );
}
```

### 组件基础样式方案
react组件基础的样式控制有两种方式
1. 行内样式(不推荐)
2. class类名控制
<img src="./pic/vue 与 react/屏幕截图 2025-08-09 132717.png">

### classnames优化类名控制
classnames是一个简单的js库，可以非常方便的通过条件动态控制class类名的显示
<img src="./pic/vue 与 react/屏幕截图 2025-08-09 141715.png">

## 受控表单绑定 ---- v-model
> 概念：使用react组件的状态(useState)控制表单的状态

使用：
1. 准备一个React状态值
```js
const [value, setValue] = useState('')
```
2. 通过value属性绑定状态，通过onChange属性绑定状态同步的函数
```js
<input type="text" value={value} onChange={(e) => setValue(e.target.value)} />
```

## react中获取dom
在react组件中获取/操作dom，需要使用useRef钩子函数，分为两步：
1. 使用useRef创建ref对象，并与jsx绑定
```js
const inputRef = useRef(null);
<input type="text" ref={inputRef} />
```
2. 在dom可用时，通过inputRef.current拿到dom对象
```js
console.log(inputRef.current);
```

## 组件通信

### 父传子
所有绑定在子组件上的属性，都会成为props对象的属性传递给子组件，可以用结构的方式，获取对应的属性
```js
// 结构出props中msg对象
function Son({msg}) {
    console.log(props);
    return (
        <div>this is son + {props.name}</div>
    );
}
```
实现步骤：
1. 父组件传递数据 - 在子组件标签上绑定属性
2. 子组件接收数据 - 子组件通过props参数接收数据

```js
// 1. 父组件传递数据 - 在子组件标签上绑定属性
function Fa() {
    const name = 'this is app name';
    return (
        <div>
            <Son name={name}></Son>
        </div>
    );
}
// 2. 子组件接收数据 - 子组件通过props参数接收数据
function Son(props) {
    console.log(props);
    return (
        <div>this is son + {props.name}</div>
    );
}
```

#### 父传子 - props说明
1. props可传递任意的类型数据
   数字、字符串、布尔值、数组、对象、函数、jsx
   <img src="./pic/vue 与 react/屏幕截图 2025-08-09 151822.png">

2. props只是只读对象
   子组件不能修改props数据，只能由父组件自行修改

#### 父传子 - 特殊的prop children （类似slot）
当把内容嵌套在子组件标签中时，父组件会自动在名为children的prop属性中接收该内容
<img src="./pic/vue 与 react/屏幕截图 2025-08-09 152734.png">

### 子传父
核心思路：在子组件中调用父组件中的函数并传递参数
```js
// 父组件
function Fa() {
    // 定义状态来接收子组件返回的数据
    const [msg, setMsg] = useState('');
    function getMsg(msg) {
        setMsg(msg);
    }
    return (
        <div>
            <Son getMsg={getMsg}>
                <span>children</span>
                <span>pppp</span>
            </Son>
            <div>{msg}</div>
        </div>
    );
}
// 子组件
function Son(props) {
    let [count, setCount] = useState(1);
    return (
        <div>
            <button onClick={() => {
                // 通过调用父组件传递的函数来通信
                props.getMsg(count);
                setCount(count+1);
            }}>msg</button>
        </div>
    );
}
```

### 使用状态提升实现兄弟组件通信
实现思路：
1. A组件先通过子传父的方式把数据传给父组件App
2. App拿到数据后通过父传子的方式再传递给B组件

### 使用context机制跨层级组件通信
实现步骤
1. 使用`createContext`方法创建一个上下文对象ctx
2. 在顶层组件中通过Ctx.Provider组件提供数据
3. 在底层组件中通过useContext钩子获取消费数据
```js
// 顶级组件
// 使用createContext方法创建一个上下文对象ctx，组件不在同一个文件内的需要导出
export const Ctx = createContext();
export function Fa() {
    const name = 'this is app name';
    return (
        <div>
            <Ctx.Provider value={name}>
                this is Fa!
                <S1></S1>
            </Ctx.Provider>
        </div>
    );
}
// 中间组件
import S2 from "./S2";
function S1() {
    return (
        <S2></S2>
    );
}
export default S1;
// 底层组件
import { useContext } from "react";
// 导入上下文
import { Ctx } from "./Fa";

function S2() {
    // 获取消息
    const msg = useContext(Ctx);
    return (
        <div>
            {/* 使用消息 */}
            <span>{msg}</span>
        </div>
    );
}
export default S2;
```

## 钩子函数 -- useEffect 
> useEffect是一个react hook函数，用于react组件中创建不是由事件引起而是由渲染本身引起的操作，比如发送ajax请求，更改dom等等
语法：
```js
useEffect(() => {}, []);
```
参数1是一个函数，称为副作用函数，函数内为要执行的操作
参数2是一个数组(可选)，在数组内放置依赖项，不同依赖项会影响第一个参数函数的执行，当是一个空数组的时候，副作用函数只会在组件渲染完毕之后执行一次

### useEffect依赖项参数说明
| 依赖项         | 副作用函数执行时机                  |
| -------------- | ----------------------------------- |
| 没有依赖项     | 组件初始渲染 + 组件更新时执行       |
| 空数组依赖     | 只在初始渲染时执行一次              |
| 添加特定依赖项 | 组件初始渲染 + 特性依赖项变化时执行 |

### useEffect -- 清除副作用
> 在useEffect中编写的由渲染本身引起的对接组件外部的操作，通常称为副作用操作，比如在useEffect中开启了一个定时器，在组件卸载时把这个定时器清理掉，这个过程就是清理副作用
```js
useEffect(() => {
    // 实现副作用操作逻辑
    return () => {  // 组件卸载时自动执行
        // 清除副作用逻辑
    }
}, []);
```
> 清除副作用的函数最常见的执行时机是在组件卸载时自动执行

## 钩子函数 -- useReducer
> useReducer 是 useState 的一种更高级的写法，有时不同组件里的状态修改方法一致，在每个组件中都写一个方法太过麻烦，可以使用useReducer这样只用写一次方法

```jsx
// 创建公有方法
const reducer = (state, action) => {
  switch (action.type) {
    case "add":
      return { ...state, age: state.age + 1 };
    case "i":
      return { ...state, age: state.age - 1 };
    default:
      return state;
  }
};

export function Home() {
  const [person, dispatch] = useReducer(reducer, { age: 20 });
  return (
    <div>
      <p>{person.age}</p>
      <button
        onClick={() => {
          dispatch({ type: "add" });
        }}
      >
        +1
      </button>
      <button
        onClick={() => {
          dispatch({ type: "i" });
        }}
      >
        -1
      </button>
    </div>
  );
}
```

## 钩子函数 -- useCallback
> useCallback有两个参数：
>   - 第一个参数：要记忆的函数
>   - 第二个参数：是一个数组，包含了函数中使用的所有外部变量。当这些变量变化时，函数会重新创建。
### memo方法优化
> - 用于优化代码
> - 当组件更新时，组件一定会被重新渲染，但是当该组件存在父组件和兄弟组件时，这个组件的重新渲染会导致父组件重新渲染，进而导致没有更新的兄弟组件重新渲染，造成性能浪费
> - React.memo 是一个高阶组件，用于记忆化整个组件，当 props 没变化时跳过重渲染。
> memo原理，memo会会浅比较props，如果没有变化就跳过渲染，浅比较是指：值比较，如果是引用就比较地址。

> 使用vite创建项目默认自带react compiler,会自动优化上面的问题

```jsx
function A() {
  console.log("A 被渲染");
  return <div>ppp:</div>;
}
const Amemo = memo(A);
function B({ num }: { num: number }) {
  return <div>num:{num}</div>;
}
function Tt() {
  const [num, setNum] = useState(20);
  return (
    <div>
      <A></A>   {/*会被重新渲染*/}
      <Amemo /> {/*不会被重新渲染*/}
      <B num={num}></B>
      <button onClick={() => setNum(num + 1)}>+1</button>
    </div>
  );
}
```

### useCallback 配合 memo使用

> 上面存在一个问题==因为memo是浅比较，所以但传入函数时比较的是地址，而不是值，会造成值相同但还是渲染==

```jsx
const Child = React.memo(function Child({ onClick }) {
  console.log('Child 渲染')  // 每次父组件渲染都会打印！
  return <button onClick={onClick}>子组件按钮</button>
})

function Parent() {
  const [count, setCount] = useState(0)
  
  // ❌ 每次 Parent 渲染，handleClick 都是新函数，地址会变
  const handleClick = () => {
    console.log('clicked')
  }
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        点击次数: {count}
      </button>
      {/* Child 用了 memo，但 handleClick 每次都是新函数 */}
      <Child onClick={handleClick} />
    </div>
  )
}
```
> 使用useCallback

```jsx
const Child = React.memo(function Child({ onClick }) {
  console.log('Child 渲染')  // 每次父组件渲染都会打印！
  return <button onClick={onClick}>子组件按钮</button>
})

function Parent() {
  const [count, setCount] = useState(0)
  
  // 使用useCallback记忆
  const handleClick = useCallback(() => {
    // 使用的外部变量
    console.log('clicked', count)
  }, [count]);  // 当外部变量变化时，函数会重新创建
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        点击次数: {count}
      </button>
      {/* handleClick不会变 */}
      <Child onClick={handleClick} />
    </div>
  )
}
```

## 钩子函数 -- useMemo
> useMemo 是 React 的一个 Hook，用于记忆化计算结果，避免在每次渲染时都执行昂贵的计算。

### useMemo作用
**1. 缓存函数运算结果**
> 例如：组件内部有一个对数组进行过滤的运算，如果不使用useMemo每次组件重新渲染都会重新进行数组过滤，造成性能浪费。使用useMemo会缓存过滤结果。
```jsx
// 没有 useMemo：每次渲染都重新计算
const result = expensiveCalculation(data)

// 使用 useMemo：只在依赖变化时重新计算
const result = useMemo(() => expensiveCalculation(data), [data])
```

**2. 保持对象/数组引用稳定**
> 当子组件接收的props为对象或数组时，为了防止不必要的重新渲染(上面useCallback提到的问题)，使用useMemo保持引用稳定。
```jsx
// 没有 useMemo：每次渲染都创建新对象
const config = { theme: 'dark', lang: 'zh' }

// 使用 useMemo：对象引用稳定
const config = useMemo(() => ({ theme: 'dark', lang: 'zh' }), [])
```

## 钩子函数 -- useRef
> 当组件重新渲染时，不会更新ref的值

```jsx

```

## 自定义Hook函数
> 自定义hook是以use开头的函数，通过自定义hook函数可以用来实现逻辑的封装和复用
> 自定义Hook函数内部有官方提供的hook函数(以use开头的函数)


### ReactHooks(官方提供的hook函数)使用规则
1. 只能在组件中或其他自定义hook函数中调用
2. 只能在组件的顶层调用，不能嵌套在if、for、其它函数中

## Redux -- 类似pinia
> redux是react最常用的状态管理工具

使用步骤：
1. 定义一个reducer函数(根据当前想要做的修改返回一个新的状态)
2. 使用createStore方法传入reducer函数生成一个store实例对象
3. 使用store实例的subscribe方法订阅数据的变化(数据一旦变化，可以得到通知)
4. 使用store实例的dispatch方法提交action对象触发数据变化(告诉reducer你想怎么改数据)
5. 使用store实例的getState方法获取最新的状态数据更新到视图中
```js
// 1. 定义一个reducer函数(根据当前想要做的修改返回一个新的状态)
// 作用根据不同的action对象，返回不同的新的state
// state：管理数据初始状态
function reducer(state = {count: 0}, action) {
    if (action.type === 'I') {
        // 基于数据不可变，返回一个新值
        return {count: state.count + 1};
    }
    if (action.type === 'D') {
        return {count: state.count - 1};
    }
    return state;
}
// 2. 使用createStore方法传入reducer函数生成一个store实例对象
const store = Redux.createStore(reducer);
// 3. 使用store实例的subscribe方法订阅数据的变化
store.subscribe(() => {
    console.log("变化了");
})
// 4. 使用store实例的dispatch方法提交action对象触发数据变化
// 这个对象与reducer中的action对应
store.dispatch({type:"I"});
// 5. 使用store实例的getState方法获取最新的状态
store.getState();
```

### redux与react -- 环境准备
> 在react中使用redux，官方要求安装两个插件 - Redux Toolkit 和 react-redux 

1. Redux Toolkit - 官方推荐编写redux逻辑的方式，是一套工具的集合，简化书写方法
2. react-redux - 用来链接redux和react组件的中间件

在没有状态管理的项目进行如下操作
```bash
npm i @reduxjs/toolkit react-redux
```

目录结构设计；
- 通常集中状态管理的部分都会单独创建一个单独的store目录(在src中)
- 应用通常会有很多个子store模块，所以创建一个modules目录
- store中的入口文件index.js的作用是组合modules中所有的子模块，并导出store


### redux 使用
1. 使用redux toolkit工具来创建对应的模块store
```js
import { createSlice } from "@reduxjs/toolkit"

const counterStore = createSlice({
    name: 'counter',
    // 初始化state
    initialState: {
        count: 200000,
    },
    // 修改状态的方法 同步方法 支持直接修改
    reducers: {
        inscrement(state) {
            state.count++
        },
        decrement(state) {
            state.count++
        }
    }
})

// 结构出来actionCreater函数
const {inscrement, decrement} = counterStore.actions
// 获取reducer
const reducer = counterStore.reducer

// 以按需导入方式导出actionCreater函数
export {inscrement, decrement}
export default reducer
```
2. 将模块store注入store中的index.js
```js
import { configureStore } from "@reduxjs/toolkit"

// 导入子模块reducer
import counterReducer from './modules/counterStore'

const store = configureStore({
    reducer: {
        // 模块store写到这里
        counter: counterReducer
    }
});

export default store
```

3. 为react注入store
react-redux负责把redux和react链接起来，内置provider组件 通过store参数把创建好的store参数实例注入到应用中，链接正式建立。
以为App为注入对象为例
```js
//导入store
import store from './store/index.js'
//导入provider
import { Provider } from 'react-redux'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    {/* 用provider包裹组件 */}
    <Provider store={store}>
      <App />
    </Provider>
  </StrictMode>,
)
```

4. 在react组件中使用store中的数据
在react组件中使用store中的数据，需要用到一个钩子函数 useSelector, 它的作用是把store中的数据映射到组件中，使用如下：
```js
// 导入useSelector
import { useSelector, useDispatch } from "react-redux"

function App() {
  // 解构出count参数，counter是指的store中的index.js中的 counter: counterReducer
  const {count} = useSelector(state => state.counter)
  return (
    <>
      <div>
        {count}
      </div>
    </>
  )
}
```

5. 在react组件中修改store中的数据
react组件中修改store中的数据需要借助另外一个hook函数 useDisPatch，它的作用是生成提交action对象的dispatch函数，使用如下：
```js
// 导入useDispatch
import { useSelector, useDispatch } from "react-redux"
// 导入action函数
import { inscrement } from "./store/modules/counterStore"

function App() {
  const {count} = useSelector(state => state.counter)
  // 生成dispatch
  const dispatch = useDispatch()
  return (
    <>
      <div>
        {count}
        {/* 使用dispatch */}
        <button onClick={() => dispatch(inscrement())}>修改count</button>
      </div>
    </>
  )
}
```

6. action函数传参
为action函数添加一个action形参
```js
// 在模块store中
const counterStore = createSlice({
    name: 'counter',
    // 初始化state
    initialState: {
        count: 200000,
    },
    // 修改状态的方法 同步方法 支持直接修改
    reducers: {
        // 添加action形参 action结构为 {payload:any, type: string}
        addToNum(state, action) {
            state.count = action.payload
        }
    }
})
// 调用
// 传入的参数自动挂载到action.payload上
<button onClick={() => dispatch(addToNum(10))}>10</button>
```

### redux 异步操作
1. 创建store的写法不变
2. 单独封装一个函数(这个函数封装到模块store文件中)，在函数内部return一个新函数，在新函数中封装异步操作
3. 组件中dispatch写法不变
```js
// counterStore写法与上述同步一样
// 在counterStore文件中添加异步操作，写在顶级作用域中
const fetchList = () => {
    return async (dispatch) => {
        const res = await fetch("xx")
        dispatch(addToNum(res))
    }
}
// 调用，使用useEffect进行调用
useEffect()

```

## 路由
安装路由，在没有路由的项目中使用如下命令:
```bash
npm i react-router-dom
```
