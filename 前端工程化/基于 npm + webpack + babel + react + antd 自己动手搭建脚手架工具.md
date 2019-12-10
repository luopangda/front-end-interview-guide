> 此试题由 kurryluo 整理，用于自学前端群成员
> 
> 更多知识点：请访问 Github 项目：[https://github.com/kurryluo/front-end-interview-guide](https://github.com/kurryluo/front-end-interview-guide) 
> 
> 听说给了 star 的人都找到工作，升职加薪了~

## 01 初始化

npm init 一步步地生成 package.json 文件，在 package.json 文件的 scripts 属性中添加 build 命令。

包含以下信息：

```json

{
  "name": "tensor-playground",
  "version": "1.0.0",
  "description": "By \r [Luo liangkui](http://www.kurryluo.com),\r [Lu yuhuan](),\r [Han yaxiong](), and \r [He Zhaocheng]()",
  "main": "index.js",
  "scripts": {
    "build": "webpack --config build/webpack.prod.conf.js",
    "test": "echo \"Error: no test specified\"&& exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://gitee.com/simulation_group_of_sysu/tensor-playground.git"
  },
  "keywords": [
    "tensor"
  ],
  "author": "kurryluo",
  "license": "MIT"
}
```

## 02 安装并配置 webpack

> webpack 是一个 JavaScript 应用程序的静态模块打包器 (module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图 (dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

### 安装
npm install --save-dev webpack  webpack-cli

--save-dev 是将依赖安装到开发环境中，即在 package.json 中的 devDependencies 目录下。--save 是安装到生产环境中也就是在 package.json 中的 dependencies 目录下。

此时会看到一个文件夹，叫做 node_modules，装的是各种依赖包。各种包的作用，本人打算做一个 github 项目：前端有包(TODO)，简单说明各种前端包的作用，现在还没有开始。

本人安装的 webpage 版本：

```text
+ webpack@4.41.2
+ webpack-cli@3.3.10
```

### 配置 webpack
根目录下建立 build 文件夹， 新建三个 js 文件，分别命名为：webpack.base.conf（公共配置）、webpack.dev.conf（开发配置）、webpack.prod.conf（生产配置）。


webpack.base.conf（公共配置）:
```js
// webpack.base.conf.js 文件
const path = require('path'); //node.js 自带的路径参数
const DIST_PATH = path.resolve(__dirname, '../dist'); // 生产目录
const APP_PATH = path.resolve(__dirname, '../src'); // 源文件目录

module.exports = {
    entry: {
        app: './src/index.js',
    },
    output: {
        filename: 'js/[name].[hash].js', // 使用 hash 进行标记
        path: DIST_PATH
    },
};
```
webpack.dev.conf（开发配置）:
```js
// webpack.prod.conf.js 文件
const merge = require('webpack-merge'); // 合并配置
const baseWebpackConfig = require('./webpack.base.conf');
module.exports = merge(baseWebpackConfig, {
    mode: 'production',  //mode 是 webpack4 新增的模式
});
```

webpack.prod.conf（生产配置）:

```js
// webpack.prod.conf.js 文件
const merge = require('webpack-merge'); // 合并配置
const baseWebpackConfig = require('./webpack.base.conf');
module.exports = merge(baseWebpackConfig, {
    mode: 'production',  //mode 是 webpack4 新增的模式
});
```


三个文件通过 3 个 webpack-merge 来进行合并

安装 webpack-merge：

npm install --save-dev webpack-merge

本人使用版本：

```text
+ webpack-merge@4.2.2
```

### 创建入口文件

创建 src 文件，存放 index.js；创建 public 文件，存放 index.html

index.js 文件：

```js
const element =document.getElementById('root');
element.innerHTML = 'hello, world!';
```

index.html 文件：

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title > 菜鸟想拿前端 offer - produced by kurryluo</title>
</head>
<body>
<body>
    <div id="root"></div>
    <script src="../dist/js/bundle.js"></script>
</body>
</body>
</html>
```

在 package.json 文件 scripts 属性添加一个 build 命令

```json
// package.json
"scripts": {
    "build": "webpack --config build/webpack.prod.conf.js",
    "test": "echo \"Error: no test specified\"&& exit 1"
},
```

## 03 安装 Babel

将 ES6 语法转换为 ES5，以及起到 js 和 css 按需加载的作用（后面会解释）

> javascript在不断的发展，各种新的标准和提案层出不穷，但是由于浏览器的多样性，导致可能几年之内都无法广泛普及，babel可以让你提前使用这些语言特性，他是一种用途很多的javascript编译器，他把最新版的javascript编译成当下可以执行的版本，简言之，利用Babel就可以让我们在当前的项目中，随意地使用最新的ES6，甚至ES7的语法。

为了避免版本问题，建议复制以下代码到 package.json 中，直接进行 npm install
```json
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-core": "^6.26.3",
    "babel-loader": "^7.0.2",
    "babel-plugin-import": "^1.9.1",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1",
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.10",
    "webpack-merge": "^4.2.2"
  },
```

创建 .babelrc 文件，配置 presets

```json
{
  "presets": [
	 "env",
    "react"
  ]
}
```

修改 webpack.base.conf.js 文件，添加 module 属性

```js
// webpack.base.conf.js
const path = require('path');
const APP_PATH = path.resolve(__dirname, '../src');
const DIST_PATH = path.resolve(__dirname, '../dist');
module.exports = {
    entry: {
        app: './app/index.js'
    },    
    output: {
        filename: 'js/bundle.js',
        path: DIST_PATH
    },
    module: {
        rules: [
            {
                test: /\.js?$/,
                use: "babel-loader",
                include: APP_PATH
            }
        ]
    }
};
```

运行 npm run build，观察报错信息。如果按照上面的步骤进行安装，理应不会报错。

注意：感受错误也是编程必不可少的一环，因为谁也不可能一下子写出完全正确的代码。

如果构建成功，会出现一个 dist 文件，里面是打包好的 js 文件。这也就是 webpack 的作用，将一些 js 文件、react 文件、图片、样式打包，这些包之间按照某种依赖联系。

控制台会提示：

```text
webpack --config build/webpack.prod.conf.js

Hash: 312e49a5d9b2de29b485
Version: webpack 4.41.2
Time: 2741ms
Built at: 2019-11-25 15:36:59
                         Asset     Size  Chunks                         Chunk Names
js/app.312e49a5d9b2de29b485.js  128 KiB       0  [emitted] [immutable]  app
Entrypoint app = js/app.312e49a5d9b2de29b485.js
[2] ./src/index.js 416 bytes {0} [built]
    + 7 hidden modules

```

## 04 安装 React

npm install react react-dom -S

对 index.js 文件进行编辑

```js
import React from "react";
import ReactDom from "react-dom";

ReactDom.render(
    <h1>hello, world!</h1>,
    document.getElementById("root")
);

## 05 安装 webpack 插件 HtmlWebpackPlugin。

作用：自动生成 HTML 文件
命令：
npm install --save-dev html-webpack-plugin

安装版本：

```text
+ html-webpack-plugin@3.2.0
```

修改 public 中的 index.html 文件如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>
    <%= htmlWebpackPlugin.options.title %>
  </title>
</head>
<body>
<!-- 空的 -->
</body>
</html>
```

紧接着添加 webpack.prod.conf.js 中配置 plugins 属性

```json
// webpack.prod.conf.js 文件
const merge = require('webpack-merge'); // 合并配置
const baseWebpackConfig = require('./webpack.base.conf');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = merge(baseWebpackConfig, {
    mode: 'production',  //mode 是 webpack4 新增的模式
    plugins: [ // 新增 plugins
        new HtmlWebpackPlugin({
            template: 'public/index.html',
            title: '菜鸟想拿前端 offer - produced by kurryluo', // 更改 HTML 的 title 的内容
            minify: {
                removeComments: true,
                collapseWhitespace: true,
                removeAttributeQuotes: true
            },
        }),
    ],
});

```

再修改一下 src 文件夹中的 index.js 文件，在增加 id 为 root 的标签。

```
import React from "react";
import ReactDom from "react-dom";

// 原生代码开始
const Div = document.createElement("div");
Div.setAttribute("id", "root");
document.body.appendChild(Div);
// 原生代码结束

ReactDom.render(
    <h1>hello, world!</h1>,
    document.getElementById("root")
);

```

现在用 npm run build 重新构建一下

可以看到 dist 文件内自动生成了 index.html 文件，这个文件在部署的时候非常关键，一般配置 nginx 文件就是指向这个文件。

控制台出现：

```
webpack --config build/webpack.prod.conf.js

Hash: 89bb306c13fa4dee0d64
Version: webpack 4.41.2
Time: 2950ms
Built at: 2019-11-25 15:59:29
                         Asset       Size  Chunks                         Chunk Names
                    index.html  334 bytes          [emitted]
js/app.89bb306c13fa4dee0d64.js    128 KiB       0  [emitted] [immutable]  app
Entrypoint app = js/app.89bb306c13fa4dee0d64.js
[2] ./src/index.js 566 bytes {0} [built]
    + 7 hidden modules
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = index.html
    [0] ./node_modules/html-webpack-plugin/lib/loader.js!./public/index.html 611 bytes {0} [built]
    [2] (webpack)/buildin/global.js 472 bytes {0} [built]
    [3] (webpack)/buildin/module.js 497 bytes {0} [built]
        + 1 hidden module
```

可以直接在浏览器中打开 index.html 文件，页面显示 hello, world!，即为成功。


## 06 优化 webpack

- 生成的文件名添加 Hash 值

在 webpack.base.conf.js 文件中，修改 filename 属性，如下：

```
output: {
    filename: "js/[name].[chunkhash].js",
},
```
- 插件 clean-webpack-plugin：清理 dist 文件夹

命令：npm install --save-dev clean-webpack-plugin

版本：
```
+ clean-webpack-plugin@3.0.0
```

安装完毕以后，修改 webpack.prod.conf.js，添加插件

- 添加热加载模块：每次修改完代码都要 control  + s 再点预览？不用不用，热加载帮你。

命令：npm install --save-dev webpack-dev-server

在 build 文件夹的 webpack.dev.conf.js 文件中添加：

```
const path = require('path');
const merge = require('webpack-merge');
const baseWebpackConfig = require('./webpack.base.conf.js');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

module.exports = merge(baseWebpackConfig, {
    mode: 'development',
    output: {
        filename: "js/[name].[hash:16].js",
    },
    // 源错误检查
    devtool: 'inline-source-map',
    plugins: [
        // 处理 html
        new HtmlWebpackPlugin({
            template: 'public/index.html',
            inject: 'body',
            title: '菜鸟想拿前端 offer - produced by kurryluo', // 更改 HTML 的 title 的内容
            minify: {
                html5: true
            },
            hash: false
        }),
        // 热更新
        new webpack.HotModuleReplacementPlugin(),
    ],
    // 热更新
    devServer: {
        port: '3000',
        contentBase: path.join(__dirname, '../public'),
        compress: true,
        historyApiFallback: true,
        hot: true, // 开启
        https: false,
        noInfo: true,
        open: true,
        proxy: {}
    }
});
```


在 package.json scripts 属性添加以下 dev 命令，然后长这样了：

```
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "build": "webpack --config build/webpack.prod.conf.js",
    "test": "echo \"Error: no test specified\"&& exit 1"
  },
```

接着可以启动开发环境，看一下效果：

用 chrome 浏览器打开网址——http:localhost:3000

修改一下 src 的 index.js 内容，比如在 hello world 后面加上 “哈哈”，你会看到浏览器自动更新。


## 07 添加 loader 处理 css、less、sass 文件

安装依赖：

npm install --save extract-text-webpack-plugin

版本：
```
+ extract-text-webpack-plugin@3.0.2
```

npm install --save-dev style-loader css-loader postcss-loader autoprefixer 

npm install --save-dev less sass less-loader sass-loader stylus-loader node-sass 

版本：
```
+ autoprefixer@9.7.2
+ postcss-loader@3.0.0
+ style-loader@1.0.0
+ css-loader@3.2.0
+ sass-loader@8.0.0
+ less@3.10.3
+ stylus-loader@3.0.2
+ less-loader@5.0.0
+ sass@1.23.7
+ node-sass@4.13.0
```

修改 build 文件夹中的 webpack.base.conf.js 文件

在 rules 中，加入规则：

```
            {
                test: /\.css$/,
                use: ['style-loader',
                    'css-loader',],
            },
            {
                test:/\.less$/,
                use: [
                    {  loader: "style-loader"  },
                    {  loader: "css-loader" },
                    {
                        loader: "postcss-loader",// 自动加前缀
                        options: {
                            plugins:[
                                require('autoprefixer')({
                                    browsers:['last 5 version']
                                })
                            ]
                        }
                    },
                    {  loader: "less-loader" }
                ]
            },
            {
                test: /\.scss$/,
                use: [
                    { loader: "style-loader" },
                    {
                        loader: "css-loader",
                    },
                    { loader: "sass-loader" },
                    {
                        loader: "postcss-loader",
                        options: {
                            plugins: [
                                require('autoprefixer')({
                                    browsers: ['last 5 version']
                                })
                            ]
                        }
                    }
                ]
            },
```

## 08 添加 loader，对图片和字体进行编译

安装依赖：
npm install --save-dev file-loader url-loader 

版本：

```text
+ file-loader@4.3.0
+ url-loader@2.3.0
```

在 rules 中，加入规则：

```json
            {
                test: /\.(png|jpg|gif)$/,
                use: [{
                    loader: 'url-loader',
                    options: {
                        // outputPath:'../',// 输出 ** 文件夹
                        publicPath: '/',
                        name: "images/[name].[ext]",
                        limit: 1000  // 是把小于 1000B 的文件打成 Base64 的格式，写入 JS
                    }
                }]
            },
            {
                test: /\.(woff|svg|eot|woff2|tff)$/,
                use: 'url-loader',
                exclude: /node_modules/
                // exclude 忽略 / node_modules / 的文件夹
            }
```


然后可以在 index.js 引入一个图片, 还有 css 文件进行编译试试看。

修改 index.js 文件，如下：

```

import React from "react";
import ReactDom from "react-dom";
import "./index.less";
// 加载图片
const image = require('./image.jpg');
// 原生代码开始
const Div = document.createElement("div");
Div.setAttribute("id", "root");
document.body.appendChild(Div);
// 原生代码结束

ReactDom.render(
    <div>
        <h1>hello, world ! 哈哈 </h1>
        <img src={image} className="image"/>
    </div>,
    document.getElementById("root")
);

```

## 09 添加 webpack-bundle-analyzer 插件，代码分析工具：对生成的代码进行分析

安装依赖：

npm install --save-dev webpack-bundle-analyzer

版本：

```text
+ webpack-bundle-analyzer@3.6.0
```

修改 webpack.prod.conf.js 文件，变成如下：

```js
// webpack.prod.conf.js 文件
const merge = require('webpack-merge'); // 合并配置
const baseWebpackConfig = require('./webpack.base.conf');
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 插件
const CleanWebpackPlugin = require('clean-webpack-plugin'); // 插件
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = merge(baseWebpackConfig, {
    mode: 'production',  //mode 是 webpack4 新增的模式
    plugins: [ // 新增 plugins
        // 自动生成 HTML 文件
        new HtmlWebpackPlugin({
            template: 'public/index.html',
            title: '菜鸟想拿前端 offer - produced by kurryluo', // 更改 HTML 的 title 的内容
            minify: {
                removeComments: true,
                collapseWhitespace: true,
                removeAttributeQuotes: true
            },
        }),
        // 清除 dist 打包的旧文件
        new CleanWebpackPlugin(['../dist'], { allowExternal: true}),
        // 分析哪些文件体积过大
        new BundleAnalyzerPlugin(),
    ],
});

```

## 10 引入 antd 设计语言

安装依赖：

npm install --save antd

版本：

```text
+ antd@3.25.3
```

修改 index.js，引入 antd 的按钮组件。你会发现样式并没有被加载

官方给出两种方案：

- 一种是在 index.css, 在文件顶部引入 antd/dist/antd.css。
这种方式将所有的 antd 样式都囊括进来，压缩以后（gzipped 后）一共大约 60kb。不优雅，没腔调。

  修改 index.less 文件如下：
  ```
  @import '~antd/dist/antd.css';
  .image{
    height: 200px;
    width: 200px;
  }
  ```
- 一种是使用 babel-plugin-import，官网下面还有一行小字：
antd 默认支持基于 ES module 的 tree shaking，js 代码部分不使用这个插件也会有按需加载的效果。
正是因为这行小字，让我明白前面学习的基础知识、背的那些知识点是连贯的。那行小字是性能优化的内容，按需加载的最终目的还是减少包的大小，能够起到节约带宽资源的作用。

babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件。注意：这不是 webpack 的插件，而是 babel 的插件。

安装插件（前面已经采用配置 package.json 的方式安装过了，就不用再安装）：

npm install --save-dev babel-plugin-import

修改一下. babelrc 文件

```
{
  "presets": [
    "env",
    "react"
  ],
  "plugins": [
    [
      "import",
      {
        "libraryName": "antd",
        "libraryDirectory": "lib",
        "style": true
      }
    ]
  ]
}
```

此时不用第一种引入方式，将 index.less 文件修改为：

```
.image{
  height: 200px;
  width: 200px;
}
```

运行以后，你会发现，莫名其妙的错误又来了，不要慌，一切都在掌握中。

修改一下 .babelrc 中的 style 属性为 "css"，如下：

```
{
  "presets": [
    "env",
    "react"
  ],
  "plugins": [
    [
      "import",
      {
        "libraryName": "antd",
        "libraryDirectory": "lib",
        "style": "css"
      }
    ]
  ]
}
```

现在重新运行项目，就会看到 antd 的样式已经被加载出来了。目前为止，脚手架工具已经完成得差不多了。

写一些简单的单页面应用足够了，但是想要构建更复杂的应用，需要用到 react 全家桶，构建可视化图形，还需要引入各类可视化组件。
