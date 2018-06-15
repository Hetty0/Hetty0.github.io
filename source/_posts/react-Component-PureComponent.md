---
title: react Component && PureComponent
date: 2018-06-15 15:31:37
categories:
  - 技术
tags:
  - react
---

`React.Component`和`React.PureComponent`都可以创建组件，但是`React.PureComponent`对组件的`props`和`state`只进行浅比较（比较内存地址），不需要自己写shouldComponentUpdate。一般使用`React.PureComponent`来进行性能优化。

对于更复杂的数据结构这可能成为一个问题。例如，假设你想要一个ListOfWords组件来渲染一个逗号分隔的单词列表，并使用一个带了点击按钮名字叫WordAdder的父组件来给子列表添加一个单词。以下代码并不正确：<!-- more -->
```
class ListOfWords extends React.PureComponent {
  render() {
    return <div>{this.props.words.join(',')}</div>;
  }
}

class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['marklar']
    };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // This section is bad style and causes a bug
    const words = this.state.words;
    words.push('marklar');
    this.setState({words: words});
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick} />
        <ListOfWords words={this.state.words} />
      </div>
    );
  }
}
```
问题是PureComponent将会在this.props.words的新旧值之间做一个简单的比较。由于代码中words数组在WordAdder的handleClick方法中被改变了，尽管数组中的实际单词已经改变，this.props.words的新旧值还是相等的，因此即便ListOfWords具有应该被渲染的新单词，它还是不会更新。

