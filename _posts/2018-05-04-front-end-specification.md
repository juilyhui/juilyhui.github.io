---
layout: post
title: 前端开发规范总结
author: juily
---
## 前端开发规范总结
-----

### HTML 规范：

#### 1、使用 HTML5 规范：<!DOCTYPE html>

· HTML4 规定了三种不同的 <!DOCTYPE> 声明，分别是：Strict（严格型）、Transitional（过渡型）、Frameset(框架型)。
· HTML5 中仅规定了一种：<!DOCTYPE html>

#### 2、注意 html 标签行为与语义

#### 3、多媒体标签向后兼容，要加上 alt 属性

#### 4、block 标签必须另起一行（li 是例外，可以放在一行），行标签可放在一行

#### 5、保持 4 个空格缩进

### CSS 规范

#### 1、保持 4 个空格缩进

#### 2、属性尽量使用简写形式，如 font 或 background

#### 3、属性值为 0 的后面不要加上单位，如 width: 0;

#### 4、url 中不要加入引号，例如 @import url(//www.google.com/css/go.css);

#### 5、颜色色值尽量使用 rgb 值

#### 6、属性以及前缀采用字典序声明

#### 9、属性: 与属性值之间用一个空格分开

#### 10、为每个选择符及每个属性声明单独使用一行，如：

{% highlight bash %}
h1,
h2,
h3 {
    color: black;
    font-weight: normal;
    font-size: 28px;
    line-height: 20px;
}
{% endhighlight %}

### JavaScript 规范

#### 1、保持 4 个空格缩进

#### 2、变量声明必须使用 var

#### 3、默认使用双引号

#### 4、总是使用分号结尾

#### 5、常量的形式如：NAMES_LIKE_THIS，使用大写字符，并用下划线分割；永远不要使用 const 关键词

#### 6、可以随意使用嵌套函数，可以减少重复代码、隐藏帮助函数等等

#### 7、不要在块内声明一个函数

{% highlight bash %}
// 不要写成：
if (x) {
    function foo() {}
}
// 建议使用函数表达式来初始化变量：
if (x) {
    var foo = function() {}
}
{% endhighlight %}

#### 8、注释规范

· 模块功能描述说明：

{% highlight bash %}
/**
 * ------------------------------------------------------------------
 * 模块描述说明
 * ------------------------------------------------------------------
 */
{% endhighlight %}

· 单个函数方法
