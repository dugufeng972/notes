# 构建工具
在代码中可能会用到ts、vue、react、less、sass等工具，这些工具浏览器并不认识，所以在没有构建工具之前要自己手动将这些东西构建成浏览器能认识的js和html代码。有构建工具之后会自动帮我们构建。

构建工具做了什么：
1. 模块化开发支持：浏览器不能直接从node_modules中引入代码，构建工具能支持直接从node_modules中引入代码 + 多种模块化支持(es6 module和commonjs)
2. 处理代码兼容性：比如babel语法降级、less、ts语法转换(**不是构建工具做的，构建工具将这些语法对应的处理工具集成进来自动化处理**)
3. 提高项目性能：压缩文件，**代码分隔**
4. 优化开发体验：
   - 构建工具会自动监听文件的变化，当文件变化之后自动调用对应的集成工具进行重新打包，然后浏览器重新运营
   - 开发服务器：解决跨域问题

**构建工具让我们不用关心生产代码怎么在浏览器运行，只需要关心怎么写代码**

市面上主流构建工具：
- webpack
- vite
- esbuild
- 。。。

# vite相比于webpack的优势
起因：项目越大 ----> 构建工具(webpack)所要处理的js代码越多
后果：构建工具需要很长时间才能启动开发服务器(启动开发服务器 ---> 把项目跑起来)
这个问题目前无法解决
因为webpack支持多种模块化方式(es6 module和commonjs)
所以webpack必须要读取所有依赖，将它们转成直接可以兼容性代码

兼容rollup

vite只支持es6 module，不需要读取所有依赖，速度要快一点

# vite脚手架和vite的区别
`yarn create vite`解释
1. 帮我们全局安装：create-vite（vite脚手架）
2. 直接运行这个create-vite bin目录下的一个执行配置
   
create-vite和vite的关系 ------> create-vite 内置了vite，并且提供了一些vite配置，能够开箱即用(out of box)
vite是空白的、从0开始的

|对比项|直接使用 Vite|	使用 Vite 脚手架|
|---|---|---|
|初始化方式|手动安装vite，自行配置项目结构|	通过官方命令（如 npm create vite）一键生成预配置项目|
|配置复杂度|	需手动配置所有功能（如插件、路由等）|	提供预设模板（React/Vue/Svelte等），开箱即用|
|适用场景|	深度定制化需求或学习 Vite 底层机制|	快速启动标准项目，避免重复配置|

# vite

## 依赖预构建
有些包是以commonjs规范的格式导出的，例如：axios。vite会先用依赖预构建处理。
**依赖预构建**：首先vite会找到对应的依赖，然后调用esbuild(对js语法进行处理的一个库)，将其它规范(如：commonjs)的代码转成esmodule规范，然后放到当前目录下的`node_modules/.vite/deps`，同时对esmodule规范的各个模板进行统一集成

依赖预构建解决了：
1. 不同的第三发包会有不同的导出格式，导致的依赖导入问题。
2. 对路径的处理上可以直接使用`./vite/deps`,方便路径重写
3. 网络多包传输的性能问题，把多个依赖集中到一起，这样网络传输时只发一个包减少性能损耗。

## vite配置文件语法以及环境区分
1. vite配置文件的语法提示
   - vscode配置，导入defineConfig，在defineConfig内写配置就会有代码提示
```js
import { defineConfig } from 'vite';

export default defineConfig ({
      //配置
})
```
2. 环境区分

## vite环境变量配置
> 环境变量：会根据当前的代码环境的变化导致值的变化的变量叫做环境变量
>
**在vite中的环境变量处理：**
使用dotenv这个第三方库
dotenv会自动读取.env文件，并解析这个文件中的对应环境变量，并将其注入到process对象下(这是node的对象)


================ ===分隔线=== ===================

## 使用vite脚手架创建项目
### 创建vue3项目
```shell
npm init @vitejs/app
```
1. 让vue支持jsx
安装@vitejs/plugin-vue-jsx
```bash
npm i @vitejs/plugin-vue-jsx -D
```
在vite config中导入并配置
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
// 导入
import vueJsx from '@vitejs/plugin-vue-jsx'
export default defineConfig({
   // 配置
  plugins: [vue(), vueJsx()],
})
```

### 创建vue2项目
不推荐用vite创建vue2

### 创建react项目
```shell
npm init @vitejs/app
```

### vite中使用css的各种功能
1. vite支持postcss

2. 让vite支持路径映射，
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx' 

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue(), vueJsx()],
  resolve: {
    alias: {
      // 进行路径映射，@style代表/src/style
      "@style": "/src/style",
    }
  }
})
```

3. vite天然支持css预处理器(less sass等)
以less为例：
只需下载less，编写less文件然后导入即可

### vite中使用ts
vite对ts只编译不校验
使用tsc --noEmit 可以只对ts校验而不把ts编译成js
1. vue-tsc for SFC
用于检查vue文件中ts，需要下载vue-tsc并在package.json中进行配置

2. isolatedModules
vite编译ts是对单文件进行的，而ts可以使用其它文件中的东西，vite在编译时无法读取其它文件的内容，所以ts有些功能无法使用