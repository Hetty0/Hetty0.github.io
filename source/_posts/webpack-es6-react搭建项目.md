---
title: webpack+es6+react搭建项目（1：基础）
date: 2018-07-04 11:18:34
tags:
  - react
---

1.项目初始化：`npm init`
2.安装全局webpack环境：`npm install webpack -g`
3.安装react、react-dom、webpack
```js
  npm install react react-dom --save
  npm install webpack --save-dev
```
4.安装babel编译插件
  因为我们要用的是es6，浏览器没办法直接识别，所以需要安装babel插件,babel插件是webpack需要的加载器，如果没有这几个加载器我们的jsx语法，和es6语法就会报错。如果某些代码需要调用Babel的API进行转码，就要使用babel-core模块
  ```js
  npm install babel-loader babel-core --save
  npm install babel-preset-es2015 babel-preset-react --save-dev
  ```
  <!-- more -->
5.webpack配置：[webpack.config.js](#webpackConfig)
6.运行项目：`webpack --config webpack.config.js`
7.项目目录预览
```
-- your project
  |-- dist
    |-- bundle.js(webpack打包自动生成)
    |-- index.html
  |-- src
    |-- index.js
  |-- package.json
  |-- wabpack.config.js
```
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id='root'></div>
  <script src='./bundle.js'></script>
</body>
</html>
```
index.js
```js
import React from 'react';
import ReactDom from 'react-dom';

class App extends React.Component {

  render() {
    return (
      <div>
        Hello World!
      </div>
    )
  }
}

ReactDom.render(<App />, document.getElementById('root'));
```
<span id="webpackConfig">webpack.config.js</span>
```js
const path = require('path');
module.exports = {
  entry: path.resolve(__dirname, './src/index.js'), // __dirname表示当前目录
  output: {
    path: path.resolve(__dirname, './dist'), // 输出路径
    filename: 'bundle.js' // 打包后的文件
  },
  module: {
    // babel-loader: babel加载器
    // babel-preset-2015：支持es2015
    // bable-preset-react：jsx转换成js
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: {
          loader: 'babel-loader',
          options: {
              presets: ['es2015', 'react'],
          }
        },
        exclude: /node_modules/
      }
    ]
  }
}
```
还可以将webpack.config.js中的babel配置单独抽离出来：新建文件.babelrc
```json
  {
    "presets": [
      "es2015",
      "react"
    ]
  }
```
编辑`webpack.config.js`
```
  ...
  module.exports = {
    ...
    module: {
      rules: [
        {
          test: /\.(js|jsx)$/,
          loader: 'babel-loader',
          exclude: /node_modules/
        }
      ]
    }
  }
```
通过基础搭建可以发现，每次都要重新构建然后刷新index.html，才能得到最新的效果，开发效率极低。下节开始使用`webpack-dev-server`+`react-hot-loader`搭建前端服务器并实现热更新，动态刷新组件。