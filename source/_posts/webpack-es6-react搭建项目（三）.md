---
title: webpack+es6+react搭建项目（3：相关配置）
date: 2018-07-05 18:16:40
tags:
  - react
---
1. [配置devtool](#devtool)
2. [配置babel stage-1](#stage-1)
3. [ESLint语法校验](#eslint)
4. [其他常用加载器](#elseConfig)


**1. <span id="devtool">配置devtool</span>**
为便于开发调试，在chorme中调试源代码，需要在`webpack.config.js`中配置devtool
```js
  ...
  module.exports = {
    ...
    devtool: 'eval-source-map', // source 查看源代码，有助于开发调试
    ...
  }
```
<!-- more -->
**2. <span id="stage-1">配置babel stage-1</span>**
`npm install babel-preset-stage-1 --save-dev`
修改`.babelrc`配置
```json
  {
    "presets": [
      ...
      "stage-1"
    ],
    ...
  }
```
**3. <span id="eslint">ESLint语法校验</span>**
一个`javascript`语法校验工具，可以像IDE一样静态监测代码错误并提示，需要安装`eslint`和`eslint-loader`
`npm install eslint eslint-loader --save-dev`
修改`webpack.config.js`，新增一条规则配置
```js
  ...
  module.exports = {
    ...
    module: {
      ...
      rules: [
        ...
        {
          enforce: 'pre',
          test: /\.(js|jsx)$/,
          loader: 'eslint-loader',
          exclude: /node_modules/
        }
      ]
    },
  }
```
在项目根目录下新建`.eslintrc.json`文件
```json
  {
    "rules": {

    }
  }
```
此时，运行项目`npm run dev`，会报错`error  Parsing error: The keyword 'import' is reserved`，因为项目中使用的es6新语法还不能被识别，需要安装babel-eslint进行转义。
`npm install babel-eslint --save-dev`
修改`.eslintrc.json`文件
```json
  {
    "parser": "babel-eslint",
    "rules": {

    }
  }
```
重新运行项目`npm run dev`，项目正常运行，不会报错。
**4. <span id="elseConfig">其他常用加载器</span>**
  其他常用加载器还有很多，这里简单将css配置一下，其余自行按需配置。最后`webpack.config.js`配置如下：
  `css-loader`: 解析css代码
  `style-loader`: 将编译后的css样式导入到html中
  `less-loader`: 加载和编译less文件
```js
  const path = require('path');
  const webpack = require('webpack');
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: [
      'babel-polyfill',
      'react-hot-loader/patch',
      'webpack-dev-server/client?http://localhost:9000',
      'webpack/hot/only-dev-server',
      path.resolve(__dirname, './src/index.js') // __dirname表示当前目录
    ], 
    output: {
      path: path.resolve(__dirname, './dist'), // 输出路径
      filename: '[name]-[hash].js' // 打包后的文件
    },
    mode: 'development',
    devtool: 'eval-source-map', // source 查看源代码，有助于开发调试
    module: {
      // babel-loader: babel加载器
      // babel-preset-2015：支持es2015
      // bable-preset-react：jsx转换成js
      rules: [
        {
          test: /\.(js|jsx)$/,
          loader: 'babel-loader',
          exclude: /node_modules/
        },
        {
          enforce: 'pre',
          test: /\.(js|jsx)$/,
          loader: 'eslint-loader',
          exclude: /node_modules/
        },
        {
          test: /\.css$/,
          use: [
            {
              loader: 'style-loader'
            },
            {
              loader: 'css-loader'
            }
          ]
        },
        {
          test: /\.less$/,
          use: [
            {
              loader: 'style-loader'
            },
            {
              loader: 'css-loader'
            },
            {
              loader: 'less-loader',
              options: {
                sourceMap: true
              }
            }
          ]
        }
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: path.resolve(__dirname, './src/index.html'),
        inject: true
      }),
      new webpack.HotModuleReplacementPlugin()
    ]
  }
```





