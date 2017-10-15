---
title: 关于markdown的语法简要规则
date: 2017-08-14 16:42:17
tags: markdown
---

# 标题
<pre>
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题 
###### 六级标题 
</pre>

# 列表
## 无序列表
* 1 
* 2
* 3

## 有序列表
1. 1
2. 2
3. 3

## 定义型列表
`名词和解释组成。一行写上定义，紧跟一行写上解释。解释的写法:紧跟一个缩进(Tab)`
Markdown
:   轻量级文本标记语言。

# 引用
> 这里是引用

## 插入链接
[百度](http://baidu.com "百度")

### 这里是多个链接
我经常去的几个网站[Google][1]、[Leanote][2]以及[自己的博客][3]
[Leanote 笔记][2]是一个不错的[网站]。
[1]:http://www.google.com "Google"
[2]:http://www.leanote.com "Leanote"
[3]:http://blog.leanote.com/freewalk "梵居闹市"
[网站]:http://blog.leanote.com/freewalk

### 自动链接：可识别邮箱和网址
<http://example.com/>



## 插入图片
![Mou icon](http://mouapp.com/Mou_128.png)

# 锚点
`锚点：markdown只支持在标题后插入锚点，其他地方无效`
`Leanote编辑器右侧显示效果区域暂时不支持锚点跳转，但发布成博文后是支持跳转的。`

# 字体
这里是 **粗体**，__粗体__
这里是 *斜体*，_斜体_
这里是 ~~删除线~~
# 表格
| tables | Are | cool|
| ------ |:------:| ------:|
|col 3 is | right-aligned | $1600 |
| col2 2 is | centered | $12 |
| zebra stripes | are neat | $1 |

# 代码框
`使用反引号包裹代码，按tab键可缩进`

# 分割线
***










