# node环境搭建及webpack简单配置

1.掌握node环境搭建，熟悉使用npm指令；

2.掌握webpack的简单配置；

3.独立搭建以上项目环境，使用webpack进行一些常规配置。

**安装环境**：macOX

## node环境搭建

Node.js是一个开源的跨平台JavaScript运行环境，可在浏览器之外执行JavaScript代码。

##### **1.通过官网下载node安装包**

官网地址：https://nodejs.org/zh-cn/download/

下载后双击根据指示操作即可完成安装。

##### 2.通过nvm安装node

通过nvm，可以方便的对多版本node进行管理和切换。

**（1）安装nvm**

```curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash```

或

```wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash```

**（2）通过nvm安装指定版本node**

```nvm install <version>```可安装指定版本的node，安装之前可以使用`nvm ls-remote`查看可安装的node版本。

```nvm install —lts```安装最新的官方长期支持的node版本，一般推荐新安装node的用户安装。同样`nvm ls-remote --lts`查看远程可安装的长期支持版本。

可以使用```nvm list```来查看已安装到本地的node版本，并使用```nvm use <version>```进行版本切换。

在终端输入```node -v```若显示node版本号，即为安装成功。

![node1](/Users/ldt1997/Desktop/zhuanzheng/2/node1.png) 

##### 3.npm换源

npm是基于Node.js的包管理工具，通常情况可以随同Node.js一起安装。通过npm，可以安装、共享、分发代码包。

由于npm下载源在国外，导致下载速度被拖慢，因此可以把npm换成淘宝镜像源。*它是一个完整 npmjs.org 镜像，可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。*

终端输入```$ npm install -g cnpm --registry=https://registry.npm.taobao.org```

安装完毕后即可用cnpm代替npm完成npm所有指令。

**设置私有源**

之前跑公司项目，用cnpm装依赖的时候，提示全部包都装好了，可是跑的时候报错信息提示少了很多依赖，原因是cnpm没有设置成私有源。

```cnpm set registry <address>```

终端输入```cnpm config list```，可以看到已经换成私有源了。

![屏幕快照 2019-07-30 下午2.42.33](/Users/ldt1997/Desktop/zhuanzheng/2/npm1.png)

##### 4.npm常用指令

**安装模块**

```npm install <Module Name>```

**安装指令区别**

```python
npm install moduleName # 安装模块到项目目录下
npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
```

**本地安装：**将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录，可以通过 require() 来引入本地安装的包。

**全局安装**：将安装包放在 /usr/local 下或者你 node 的安装目录，可以直接在命令行里使用。

**(macOS全局安装需在cnpm前加上sudo指令，输入密码后即可全局安装。)**

**查看所有已安装模块**

```npm ls```

**卸载模块**

``` npm uninstall <Module Name>```

**更新模块到最新版本**

```npm update <Module Name>```

**搜索模块**

```npm search <Module Name>```

**创建模块**

```npm init```

## webpack的简单配置

> 本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。[（webpack中文文档）](https://www.webpackjs.com/)

总体来说，webpack对项目各模块的依赖关系进行分析，并将js、css、less等静态资源按照制定规则转化为静态文件，从而减少页面请求。

##### 1.安装webpack

```cnpm install webpack -g```

##### 2.初始化项目

创建新的项目文件夹，在文件夹下执行```npm init```初始化项目。

仿照create-react-app初始项目结构如下：

```python
├── package.json
├── public                  # 这个是webpack的配置的静态目录
│   ├── index.html          # 默认是单页面应用，这个是最终的html的基础模板
├── src
│   ├── App.js              # App组件代码
│   ├── index.js            # 启动的文件（开始执行的入口）
└── node_modules
```

Index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="bundle.js"></script>
  </body>
</html>
```

Index.js

```javascript
import app from "./App";

document.querySelector("#root").appendChild(app());
```

App.js

```javascript
module.exports = function() {
  var app = document.createElement("div");
  app.textContent = "hi:D";
  return app;
};
```

##### 3.通过命令行使用webpack

在项目目录终端下运行```webpack src/index.js -o public/bundle.js```，打包src下的index.js，并将打包生成的bundle.js文件放到public文件夹下。可以看到入口文件index引用的app模块也被编译了。（新版本的webpack要加上-o）

![打包1](/Users/ldt1997/Desktop/zhuanzheng/2/打包1.png)

浏览器打开index.html，可以看到刚刚写的页面。

![webpack运行结果1](/Users/ldt1997/Desktop/zhuanzheng/2/webpack运行结果1.png)

##### 4.通过配置文件使用Webpack(推荐)

在当前文件夹的根目录下新建一个名为`webpack.config.js`的文件，在其中写入如下所示的简单配置代码，目前的配置主要涉及到的内容是入口文件路径和打包后文件的存放路径。

```javascript
module.exports = {
  entry: __dirname + "/src/index.js", //入口文件
  output: {
    path: __dirname + "/public", //打包后的文件存放的地方
    filename: "bundle.js" //打包后输出文件的文件名
  }
};
```

> “__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。

现在在终端输入`webpack`，就会自动运行webpack配置文件，生成bundle.js文件。

除此之外，也可以在package.json使用start命令来打包。

```javascript
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack" 
  },
```

##### 5.loaders

> *loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

在配置文件中使用loader示例如下：

```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
    ]
  }
};

// 指定多个loader
module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }

```

以上配置意思是，告诉webpack在import模块中遇到后缀为.css的文件时，先用css-loader转换后再执行打包。

Loaders的配置包括以下几方面：

- `test`：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
- `loader`：loader的名称（必须）
- `include/exclude`:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- `query/options`：为loaders提供额外的设置选项（可选）【webpack2.5之前为`query`，之后为`options`】

**babel**

> Babel 是一个 JavaScript 编译器，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

babel主要用于将es5/es6语法转换成浏览器兼容的js语法，以及编译react所使用的`.jsx`文件。

##### 6.使用webpack搭建本地服务器

安装webpack-dev-server组件：

```javascript
cnpm install --save-dev webpack-dev-server

```

在webpack.config中进行配置：

```
devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
    port:8080 //不配置时默认8080端口
  } 

```

在package.json里添加命令开启服务器：

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open" //开启本地服务器
  },

```

##### 7.使用loader配置项目

**Todo：将`App.js`改为react组件，并用less为其添加样式；进行Babel和less的配置；并开启本地服务器监听页面。**

**安装babel-loader、style-loader、css-loader等**

```
npm install --save react react-dom

```

```
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev babel-core babel-loader babel-preset-env babel-preset-react

```

```
npm install --save-dev style-loader css-loader

```

**报错：**这里我安装babel时webpack一直报错：Plugin/Preset files are not allowed to export objects，原因是babel7.0（@babel/？-？）和6.0（babel-？-？）的版本冲突。

**解决方法：**全部更新或回退成一个版本。

（还有就是我全部更新成了7.0版本还是一直报错说找不到babel-preset-env，我就下了个7.0beta版的babel-preset-env，终于不报错了。。。）

**改写App.js**

App.js

```javascript
import React, { Component } from "react";
import styles from "./App.less";

class App extends Component {
  render() {
    return (
      <div className={styles.root}>
        <div className={styles.content}>hi:D</div>
      </div>
    );
  }
}
export default App;

```

App.less

```less
.root {
  .content {
    margin: 50px;
    color: skyblue;
    font-size: 50px;
  }
}

```

Index.js

```js
import React from "react";
import { render } from "react-dom";
import App from "./App";

render(<App />, document.getElementById("root"));// 使用ES6的模块定义和渲染app模块

```

**配置Babel**

由于项目中babel配置较为复杂，一般会把babel的配置选项放在单独的配置文件.babelrc中，webpack会自动调用`.babelrc`里的babel配置选项。

Webpack.config.js

```js
module: {
    rules: [
      {
        test: /(\.jsx|\.js)$/,
        use: {
          loader: "babel-loader"
        },
        exclude: /node_modules/
      },
      ]
}

```

.babelrc

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react", "react"]
}

```

**配置less**

> Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

```js
module: {
    rules: [
      {
        test: /(\.less)$/,
        use: [
          {
            loader: "style-loader"
          },
          {
            loader: "css-loader",
            options: {
              modules: true
            }
          },
          {
            loader: "less-loader",
            options: { javascriptEnabled: true }
          }
        ],
        exclude: /node_modules/
      }
    ]
  }

```

以上使用的loader用处：

- style-loader 将css添加到DOM的内联样式标签style里；
- css-loader 允许将css文件通过require的方式引入，并返回css代码；
- less-loader 处理less；

**loader的特性：**loader支持链式调用，从右至左执行，支持同步或异步函数。例如，以上的代码，当webpack遇到.less文件时，会先执行less-loader，再继续执行css-loader，最后执行style-loader。

如果只调用less-loader，webpack会报错：You may need an additional loader to handle the result of these loaders.

打包后启动本地服务器，可以看到页面已经更新。

![webpack2](/Users/ldt1997/Desktop/zhuanzheng/2/webpack2.png)

##### 8.插件

> 插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
>    Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。
>
> Webpack有很多内置插件，同时也有很多第三方插件，可以让我们完成更加丰富的功能。

```js
const webpack = require('webpack');

module.exports = {
...
    module: {
        rules: [
            // 这里配置loader
        ]
    },
    plugins: [
        // 这里配置Plugins
      new webpack.BannerPlugin('版权所有，翻版必究')// 例子
    ],
};

```

配置方法：在webpack配置中的plugins关键字部分添加该插件的一个实例。

```js
plugins: [
      new webpack.BannerPlugin('版权所有，翻版必究')
    ],

```


> 参考：[入门Webpack，看这篇就够了](https://www.jianshu.com/p/42e11515c10f)
