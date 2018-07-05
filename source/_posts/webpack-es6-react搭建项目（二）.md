---
title: webpack+es6+react搭建项目（2：前端服务器+热加载）
date: 2018-07-04 13:17:22
tags:
  - react
---

webpack-dev-server是一个小型的静态文件服务器，为webpack打包的资源文件提供Web服务。并且提供自动刷新和Hot Module Replacement（模块热替换：前端代码变动后无需刷新整个页面，只把变化的部分替换掉）
1. [搭建前端服务器](#server)
2. [热更新](#hotReplace)
### <span id="server">搭建前端服务器</span>
1.搭建前端服务器 `npm install webpack-dev-server --save-dev`
2.在项目根目录创建 `webpack.server.config.js`
<!-- more -->
```js
'use strict';

const WebpackDevServer = require('webpack-dev-server');
const webpack = require('webpack');
const path = require('path');
const config = require('./webpack.config');
const compiler = webpack(config);

const server = new WebpackDevServer(compiler, {
  contentBase: path.resolve(__dirname, './dist'), // 默认会以根文件提供本地服务器，这里指定文件夹
  historyApiFallback: true, // 在开发单页面应用时非常有用，依赖于HTML5 History API，如果设置为true，所有的跳转将指向index.html
  port: 9000, // 省略则默认为：8080
  publicPath: "/"
});

server.listen(9000, 'localhost', function(err) {
  if(err) {
    throw err;
  }
});

```
3.配置启动脚本
```json
"scripts": {
    "dev": "node webpack.server.config.js",
  }
```
4.输入命令 `npm run dev` 启动服务器，可以看到改动文件后，项目会自动编译。此时刷新浏览器，可以看到最新的更改

### <span id="hotReplace">热更新</span>
`HMR`应该是`webpack`令人非常兴奋的一个特点，它在代码修改后重新打包并发送到浏览器，浏览器将获取的新模块替换老模块，在不刷新浏览器的情况下实现对应用的更新。由于我们使用的是`webpack-dev-server`，它提供了两种自动刷新方式供我们选择，`iframe`和`inline`模式。这里我们选择`inline`模式，更改`webpack.server.config.js`。

**1. 配置入口文件** 使用`html-webpapck-plugin`插件自动引入入口文件，之前我们手动将打包好的js文件引入到index.html中，现在通过插件实现自动引入。`npm install html-webpack-plugin --save-dev`，在src下创建index.html
```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <div id='app'></div>
  </body>
  </html>
```
编辑`webpack.config.js`
```js
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  ...
  output: {
    path: path.resolve(__dirname, './dist'), // 输出路径
    filename: '[name]-[hash].js' // 打包后的文件
  },
  ...
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, './src/index.html'),
      inject: true
    })
  ]
```
**2. 添加webpack热更新** 
编辑`webpack.server.config.js`
```js
  ...
  const server = new WebpackDevServer(compiler, {
    ...
    inline: true, // 自动刷新
    hot: true // 开启模块热替换
  });
  ...
```
编辑`webpack.config.js`
```js
  const webpack = require('webpack');
  module.exports = {
    ...
    entry: [
      'webpack-dev-server/client?http://localhost:9000',
      'webpack/hot/only-dev-server',
      path.resolve(__dirname, './src/index.js') // __dirname表示当前目录
    ]
    ...
    plugins: [
      new webpack.HotModuleReplacementPlugin()
    ]
  }
```
编辑`index.js`
```js
//将APP组件独立出去
import React from 'react';
import {render} from 'react-dom';
import App from './App';

const renderDom = Component => {
  render(<Component />, document.getElementById('app'));
}
renderDom(App);
if (module.hot) {
  module.hot.accept('./App', () => {
    const App = require('./App').default;
    renderDom(App);
  })
}
```
此时，项目可以实现热更新，但此时的热更新无法保存react的state，需要进行以下配置
**3. 安装react-hot-loader** `npm install react-hot-loader --save-dev`
编辑`index.js`
```js
  import {AppContainer} from 'react-hot-loader';
  ...
  const renderDom = Component => {
    render(
      <AppContainer>
        <Component />
      </AppContainer>,
      document.getElementById('app')
    );
  }
  ...
```
编辑`webpack.config.js`
```js
  ...
  module.exports = {
    ...
    entry: [
      'babel-polyfill',
      'react-hot-loader/patch',
      ...
    ]
  }
```
编辑`.babelrc`
```json
  {
    "presets": [
      [
        "es2015",
        {
          "module": false
        }
      ],
      "react"
    ],
    "plugins": ["react-hot-loader/babel"]
  }
```
完成之后，重启项目`npm run dev`，可以看到页面在编辑保存之后会实时更新。