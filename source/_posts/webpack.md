---
title: "webpack5基础配置"
date:
  "[object Object]": null
tags:
  - webpack5
categories:
  - 前端工具
description: " "
---

## 前言

### 为什么需要打包工具？

日常开发中，我们会使用框架（React、Vue），ES6 模块化语法，Less/Sass 等 css 预处理器等语法进行开发。

这些代码要想在浏览器运行必须经过编译成浏览器能识别的 JS、Css 等语法，才能运行。

所以我们需要打包工具帮我们做完这些事。

除此之外，打包工具还能压缩代码、做兼容性处理、提升代码性能等。

### 有哪些打包工具？

- Grunt
- Gulp
- Parcel
- Webpack
- Rollup
- Vite
- ...

目前市面上最流行的打包工具是 Webpack

## 基本使用

**Webpack 是一个静态资源打包工具。**
它会以一个或多个文件作为打包的入口，将我们整个项目所有文件编译组合成一个或多个文件输出出去。

输出的文件就是编译好的文件，就可以在浏览器段运行了。

我们将 Webpack 输出的文件叫做 bundle。

### 功能介绍

Webpack 本身功能是有限的:

- 开发模式：仅能编译 JS 中的 ES Module 语法
- 生产模式：能编译 JS 中的 ES Module 语法，还能压缩 JS 代码

### 开始使用

#### 1. 资源目录

```
webpack_code # 项目根目录（所有指令必须在这个目录运行）
    └── src # 项目源码目录
        ├── js # js文件目录
        │   ├── count.js
        │   └── sum.js
        └── main.js # 项目主文件

```

#### 2.创建文件

- count.js

```
export default function count(x, y) {
  return x - y;
}
```

- sum.js

```
export default function sum(...args) {
  return args.reduce((p, c) => p + c, 0);
}
```

- main.js

```
import count from "./js/count";
import sum from "./js/sum";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

#### 3.下载依赖

打开终端，在项目根目录。运行以下指令：

- 初始化 package.json

```
npm init -y
```

此时会生成一个基础的 package.json 文件。
**需要注意的是 package.json 中 name 字段不能叫做 webpack, 否则下一步会报错**

- 下载依赖

```
npm i webpack webpack-cli -D
```

#### 4. 启用 Webpack

- 开发模式

```
npx webpack ./src/main.js --mode=development
```

- 生产模式

```
npx webpack ./src/main.js --mode=production
```

`npx webpack`: 是用来运行本地安装 Webpack 包的。
`./src/main.js`: 指定 Webpack 从 main.js 文件开始打包，不但会打包 main.js，还会将其依赖也一起打包进来。
`--mode=xxx`：指定模式（环境）。

#### 5. 观察输出文件

默认 `Webpack` 会将文件打包输出到 `dist` 目录下，我们查看 `dist` 目录下文件情况就好了

## 基本配置

### 5 大核心概念

- **entry（入口）** :指示 Webpack 从哪个文件开始打包
- **output（输出）**:指示 Webpack 打包完的文件输出到哪里去，如何命名等
- **loader（加载器）**:webpack 本身只能处理 js、json 等资源，其他资源需要借助 loader，Webpack 才能解析
- **plugins（插件）**:扩展 Webpack 的功能
- **mode（模式）**
  主要有两种模式：
  - 开发模式：development
  - 生产模式：production

### 创建 Webpack 配置文件

在项目根目录下新建文件：webpack.config.js

```
module.exports = {
  // 入口
  entry: "",
  // 输出
  output: {},
  // 加载器
  module: {
    rules: [],
  },
  // 插件
  plugins: [],
  // 模式
  mode: "",
};

```

Webpack 是基于 Node.js 运行的，所以采用 Common.js 模块化规范

### 修改配置文件

1. 配置文件

```javascript
// Node.js的核心模块，专门用来处理文件路径
const path = require("path");

module.exports = {
  // 入口
  // 相对路径和绝对路径都行
  entry: "./src/main.js",
  // 输出
  output: {
    // path: 文件输出目录，必须是绝对路径
    // path.resolve()方法返回一个绝对路径
    // __dirname 当前文件的文件夹绝对路径
    path: path.resolve(__dirname, "dist"),
    // filename: 输出文件名
    filename: "main.js",
  },
  // 加载器
  module: {
    rules: [],
  },
  // 插件
  plugins: [],
  // 模式
  mode: "development", // 开发模式
};
```

2. 运行指令

```
npx webpack
```

此时功能和之前一样，也不能处理样式资源

## 开发模式介绍

开发模式顾名思义就是我们开发代码时使用的模式。
这个模式下我们主要做两件事：

1. 编译代码，使浏览器能识别运行
   开发时我们有样式资源、字体图标、图片资源、html 资源等，webpack 默认都不能处理这些资源，所以我们要加载配置来编译这些资源
2. 代码质量检查，树立代码规范
   提前检查代码的一些隐患，让代码运行时能更加健壮。
   提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。

## 处理样式资源

### 介绍

Webpack 本身是不能识别样式资源的，所以我们需要借助 Loader 来帮助 Webpack 解析样式资源

我们找 Loader 都应该去官方文档中找到对应的 Loader，然后使用

官方文档找不到的话，可以从社区 Github 中搜索查询

[Webpack 官方 Loader 文档](https://webpack.docschina.org/loaders/)

### 处理 Css 资源

1. 下载包

```
npm i css-loader style-loader -D
```

注意：需要下载两个 loader 2. 功能介绍

- `css-loader`: 负责将 Css 文件编译成 Webpack 能识别的模块
- `style-loader`: 会动态创建一个 Style 标签，里面放置 Webpack 中 Css 模块内容
  此时样式就会以 Style 标签的形式在页面上生效

3. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

4. 添加 Css 资源

```css src/css/index.css
.box1 {
  width: 100px;
  height: 100px;
  background-color: pink;
}
```

```js src/main.js
import count from "./js/count";
import sum from "./js/sum";
// 引入 Css 资源，Webpack才会对其打包
import "./css/index.css";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

```html public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <!-- 准备一个使用样式的 DOM 容器 -->
    <div class="box1"></div>
    <!-- 引入打包后的js文件，才能看到效果 -->
    <script src="../dist/main.js"></script>
  </body>
</html>
```

5. 运行指令

```
npx webpack
```

打开 index.html 页面查看效果

### 处理 Less 资源

1. 下载包

```
npm i less-loader -D
```

2. 功能介绍

- `less-loader`: 负责将 Less 文件编译成 Css 文件

3. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

4. 添加 Less 资源

```less src/less/index.less
.box2 {
  width: 100px;
  height: 100px;
  background-color: deeppink;
}
```

```js src/main.js
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/index.css";
import "./less/index.less";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

```html public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <div class="box1"></div>
    <div class="box2"></div>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

5. 运行指令

```
npx webpack
```

打开 index.html 页面查看效果

### 处理 Sass 和 Scss 资源

1. 下载包

```
npm i sass-loader sass -D
```

注意：需要下载两个

2. 功能介绍

- **sass-loader**: 负责将 **Sass** 文件编译成 **css** 文件
- **sass**: **sass-loader** 依赖 **sass** 进行编译

3. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

4. 添加 Sass 资源

```css src/sass/index.sass
/* 可以省略大括号和分号 */
.box3
  width: 100px
  height: 100px
  background-color: hotpink
```

```css src/sass/index.scss
.box4 {
  width: 100px;
  height: 100px;
  background-color: lightpink;
}
```

```html public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

5. 运行指令

```
npx webpack
```

打开 index.html 页面查看效果

### 处理 Styl 资源

1. 下载包

```
npm i stylus-loader -D
```

2. 功能介绍

- **stylus-loader**: 负责将 **Styl** 文件编译成 **Css** 文件

3. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

4. 添加 Styl 资源

```style src/styl/index.styl
/* 可以省略大括号、分号、冒号 */
.box
  width 100px
  height 100px
  background-color pink
```

```js src/main.js
import { add } from "./math";
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/index.css";
import "./less/index.less";
import "./sass/index.sass";
import "./sass/index.scss";
import "./styl/index.styl";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

```html public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <!-- 准备一个使用样式的 DOM 容器 -->
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
    <div class="box5"></div>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

5. 运行指令

```
npx webpack
```

打开 index.html 页面查看效果

## 处理图片资源

过去在 Webpack4 时，我们处理图片资源通过 file-loader 和 url-loader 进行处理
现在 Webpack5 已经将两个 Loader 功能内置到 Webpack 里了，我们只需要简单配置即可处理图片资源

1. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

2. 添加图片资源

- src/images/1.jpeg
- src/images/2.png
- src/images/3.gif

3. 使用图片资源

```less src/less/index.less
.box2 {
  width: 100px;
  height: 100px;
  background-image: url("../images/1.jpeg");
  background-size: cover;
}
```

```sass src/sass/index.sass
.box3
  width: 100px
  height: 100px
  background-image: url("../images/2.png")
  background-size: cover
```

```styl src/styl/index.styl
.box5
  width 100px
  height 100px
  background-image url("../images/3.gif")
  background-size cover

```

4. 运行指令

```
npx webpack
```

打开 index.html 页面查看效果

5. 输出资源情况
   此时如果查看 dist 目录的话，会发现多了三张图片资源
   因为 Webpack 会将所有打包好的资源输出到 dist 目录下

- 为什么样式资源没有呢？
  因为经过 style-loader 的处理，样式资源打包到 main.js 里面去了，所以没有额外输出出来

6. 对图片资源进行优化
   将小于某个大小的图片转化成 data URI 形式（Base64 格式）

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

- 优点：减少请求数量
- 缺点：体积变得更大

## 修改输出资源的名称和路径

1. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          // 将图片文件输出到 static/imgs 目录中
          // 将图片文件命名 [hash:8][ext][query]
          // [hash:8]: hash值取8位
          // [ext]: 使用之前的文件扩展名
          // [query]: 添加之前的query参数
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

2. 修改 index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
    <div class="box5"></div>
    <!-- 修改 js 资源路径 -->
    <script src="../dist/static/js/main.js"></script>
  </body>
</html>
```

3. 运行指令

```
npx webpack
```

此时输出文件目录：（注意：需要将上次打包生成的文件清空，再重新打包才有效果）

```
├── dist
    └── static
         ├── imgs
         │    └── 7003350e.png
         └── js
              └── main.js

```

## 自动清空上次打包资源

1. 配置

```js
module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js",
    clean: true, // 自动将上次打包目录资源清空
  },
  .
  .
  .
```

2. 运行指令

```
npx webpack
```

观察 dist 目录资源情况

## 处理字体图标资源

### 1. 下载字体图标文件

1. 打开[阿里巴巴矢量图标库](https://www.iconfont.cn/)
2. 选择想要的图标添加到购物车，统一下载到本地

### 2. 添加字体图标资源

- src/fonts/iconfont.ttf

- src/fonts/iconfont.woff

- src/fonts/iconfont.woff2

- src/css/iconfont.css
  注意字体文件路径需要修改

```js src/main.js
import { add } from "./math";
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/iconfont.css";
import "./css/index.css";
import "./less/index.less";
import "./sass/index.sass";
import "./sass/index.scss";
import "./styl/index.styl";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

```html public/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>webpack5</title>
  </head>
  <body>
    <h1>Hello Webpack5</h1>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
    <div class="box5"></div>
    <!-- 使用字体图标 -->
    <i class="iconfont icon-arrow-down"></i>
    <i class="iconfont icon-ashbin"></i>
    <i class="iconfont icon-browse"></i>
    <script src="../dist/static/js/main.js"></script>
  </body>
</html>
```

### 3. 配置

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
    clean: true, // 自动将上次打包目录资源清空
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          // 将图片文件输出到 static/imgs 目录中
          // 将图片文件命名 [hash:8][ext][query]
          // [hash:8]: hash值取8位
          // [ext]: 使用之前的文件扩展名
          // [query]: 添加之前的query参数
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
      {
        test: /\.(ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "static/media/[hash:8][ext][query]",
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

`type: "asset/resource"`和`type: "asset"`的区别： 1.` type: "asset/resource"` 相当于 file-loader, 将文件转化成 Webpack 能识别的资源，其他不做处理 2.`type: "asset"` 相当于 url-loader, 将文件转化成 Webpack 能识别的资源，同时小于某个大小的资源会处理成 data URI 形式

### 4.运行指令

```
npx webpack
```

打开 index.html 页面查看效果


## 处理其他资源
开发中可能还存在一些其他资源，如音视频等，我们也一起处理了
### 1.配置
```
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
    clean: true, // 自动将上次打包目录资源清空
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          // 将图片文件输出到 static/imgs 目录中
          // 将图片文件命名 [hash:8][ext][query]
          // [hash:8]: hash值取8位
          // [ext]: 使用之前的文件扩展名
          // [query]: 添加之前的query参数
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
      {
        test: /\.(ttf|woff2?|map4|map3|avi)$/,
        type: "asset/resource",
        generator: {
          filename: "static/media/[hash:8][ext][query]",
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```
就是在处理字体图标资源基础上增加其他文件类型，统一处理即可
### 2.运行指令
```
npx webpack
```