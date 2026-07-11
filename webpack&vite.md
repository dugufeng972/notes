# webpack
> 使用webpack让前端工程可以像在node中那样使用模块化，以及解决模块化时模块过多浏览器网络加载问题（因为每个模块就是一个文件，模块过多时浏览器要向后端请求很多文件）
>
> webpack（构建工具）可以对代码进行打包，将多个文件打包成一个文件，这样一来即解决了兼容性问题，又解决了模块过多导致请求多次的问题。
> webpack（构建工具）同时也将ESM规范编写的代码转换成旧的js语法

## 使用步骤
1. 初始化项目：`npm init -y` 或者 `yarn init -y`
2. 安装依赖：`webpack` `webpack-cli` 都是开发时依赖
3. 在项目中创建src目录，然后编写代码
4. 终端执行`webpack`或`yarn webpack`打包代码

## 配置文件
> 项目文件夹下的 `webpack.config.js`

### entry

> 指定打包时的入口文件，默认./src/index.js

```js
export default {
  mode: "development", // 设置打包的模式，production表示生产模式，development开发模式
  // 这里设置src目录下的hello.js为入口文件
  entry: "./src/hello.js", // 指定打包时的入口文件，默认./src/index.js
  // 可以设置多个入口文件，多个文件会打包成一个文件
  entry: ["./src/a.js", "./src/b.js"],
  // 可以设置对象，会将"./src/hello.js"打包成hello.js，"./src/b.js"打包成b.js
  entry: {
    hello: "./src/hello.js",
    b: "./src/b.js",
  },
};
```

### output

> 配置代码打包后的地址

```js
import path from "path";

export default {
  // 配置代码打包后的地址
  output: {
    filename: "bundle.js", // 打包后的文件名
    clen: true, // 是否清空打包输出的目录
    path: path.resolve(__dirname, "hello"), // 指定打包的目录，必须是绝对目录
  },
};
```

### loader
> webpack默认情况下只能处理js文件，如果希望处理其它类型的文件，则要为其引入loader

1. 以css为例
   - 使用css-loader可以处理js中的样式
   - 使用步骤：
     - 1. 安装css-loader：`npm i -D css-loader` 或者 `yarn add css-loader -D`
     - 2. 安装style-loader：`npm i -D style-loader`
     - 3. 配置：
       ```js
       export default {
            // loader
             module: {
               // npm i -D style-loader
               rules: [
                 {
                   test: /\.css$/i,
                   // 这里css-loader将css转成js代码，style-loader绑定元素
                   // 注意：数组里的执行顺序是从后往前执行，顺序不能乱
                   use: ["style-loader", "css-loader"],
                 },
               ],
            }
        }
       ```
2. loader还可以用于加载图片等非js文件

### babel
> 在写js时经常会用一些js中的新特性，为了在旧浏览器中保证兼容性，需要将新特性js代码转成旧js代码，babel就是这样一个工具。

> 如果希望webpack支持babel，则需要向webpack中引入babel的loader

使用步骤：
  - 1. 安装 `npm i -D babel-loader @babel/core @babel/preset-env`
  - 2. 配置
    ```js
    export default {
      // loader
      module: {
        rules: [
          // 配置babel
          {
            test: /\.m?js$/,
            exclude: /(node_modules | bower_components)/,
            use: {
              loader: "babel-loader",
              options: {
                presets: ["@babel/preset-env"],
              },
            },
          },
        ],
      },
    };
    ```
  - 3. 在 package.json中设置浏览器兼容列表
    ```js
    {
        "browserslist": [
            "defaults"
        ]
    }
    ```

### plugin 插件
> plugin用来为webpack扩展功能

> html-webpack-plugin：这个插件可以在打包代码后，自动在打包目录生成html页面

以html-webpack-plugin使用为例
> 作用：可以自动创建html，并将js挂载上去

使用步骤：
  - 1. 安装：`npm i -D html-webpack-plugin` 
  - 2. 配置：
    ```js
    import HtmlWebpackPlugin from "html-webpack-plugin";
    export default {
        // plugins
        plugins: [new HtmlWebpackPlugin()],
    }
    ```

### 开发服务器
> 作用：在本地创建一个服务器，将前端代码运行在本地服务器上，可以热更新

安装：`npm i -D webpack-dev-server`

### sourceMap
> 打包后，代码被压缩了不利于调试阅读，为了方便调试可以使用如下方法

```js
export default {
    devtool: "inline-source-map",
}
```

# vite
> vite也是一种前端构建工具

> 相较于webpack，vite采用了不同的运行方式：
>   - 开发时，而是直接采用ESM（import方式，模块化）的方式来运行项目
>   - 在项目部署时，再对项目进行打包
> 
> 除了速度快，vite还开箱即用

安装：`npm i -D vite`

在package.json中配置
```js
{
    "scripts": {
      "dev": "vite",    // 启动开发服务器
      "build": "vite build",    // 打包代码
      "preview": "vite preview" // 预览打包后的代码显示的页面
    },
}
```

==注意：vite打包后的是按esm方式（type: "module"）模块化的，esm有一个特点就是只能用url方式加载，本地打开打包后的代码是file://协议，无法显示内容，只能部署到服务器上或者使用`vite preview`才能看到内容==

## vite的配置
> vite是开箱即用的，像css这些loader在vite中默认是集成的

> vite配置文件是项目目录下的`vite.config.js`

```js
import { defineConfig } from "vite";
// 使用 defineConfig 包裹一下的目的是在写配置时有提示，如果不包裹也是可以的
// 等价于 export default {};
export default defineConfig({
    // 配置插件
    plugins: [
        // 启用安装的插件
        legacy()
    ]
});

```