---
title: react propTypes
date: 2018-06-14 14:17:50
categories:
  - 技术
tags:
  - react
---

组件属性类型检查 —— react内置的类型检查功能，需要配置propTypes属性
`注意: React.PropTypes 自 React v15.5 起已弃用。请使用 prop-types 库代替。`
```
栗子：
import React from 'react';
import PropTypes from 'prop-types';
class Greeting extends React.Component {
  render() {
    return (
      <h1>hello, {this.props.name}</h1>
    )
  }
}
Greeting.propTypes = {
  name: PropTypes.string
}
```
<!-- more -->
PropTypes包含一整套验证器，可用于确保你接收的数据是有效的。在这个示例中，使用了PropTypes.string。当你给属性传递了无效值时，javascript控制台将会打印出警告。处于性能原因，propTypes只在开发模式下进行检查。
#### PropTypes
下面是使用不同的验证器的例子：
```
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为以下 JS 原生类型
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
  optionalNode: PropTypes.node,

  // 一个 React 元素
  optionalElement: PropTypes.element,

  // 你也可以声明属性为某个类的实例，这里使用 JS 的
  // instanceof 操作符实现。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你也可以限制你的属性值是某个特定值之一
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 限制它为列举类型之一的对象
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个指定元素类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 一个指定类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个指定属性及其类型的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 你也可以在任何 PropTypes 属性后面加上 `isRequired` 
  // 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你也可以指定一个自定义验证器。它应该在验证失败时返回
  // 一个 Error 对象而不是 `console.warn` 或抛出异常。
  // 不过在 `oneOfType` 中它不起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 不过你可以提供一个自定义的 `arrayOf` 或 `objectOf` 
  // 验证器，它应该在验证失败时返回一个 Error 对象。 它被用
  // 于验证数组或对象的每个值。验证器前两个参数的第一个是数组
  // 或对象本身，第二个是它们对应的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```
#### 限制单个子代
使用`PropTypes.element`你可以指定只传递一个子代
```
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // This must be exactly one element or it will warn.
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};
```
#### 属性默认值
你可使用配置`defaultProps`为`props`定义默认值：
```
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 为属性指定默认值:
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染 "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```
如果你在使用[transform-class-properties][1]的Babel转换器，你也可以在React组件类中声明`defaultProps`作为静态属性。这个语还没有最终通过，在浏览器中需要一步编译工作。更多信息，查看[类字段提议][2]。
```
class Greeting extends React.Component {
  static defaultProps = {
    name: 'stranger'
  }

  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}
```
`defaultProps`用来确保`this.props.name`在父组件没有特别指定的情况下，有一个初始值。类型检查发生在`defaultProps`赋值之后，所以类型检查也会应用到`defaultProps`上面。

[查看文档链接](https://doc.react-china.org/docs/typechecking-with-proptypes.html)

[1]: <https://babeljs.io/docs/en/babel-plugin-transform-class-properties/>
[2]: <https://github.com/tc39/proposal-class-fields>



