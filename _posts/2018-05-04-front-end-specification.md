---
layout: post
title: 前端开发规范总结
author: juily
---
## 前端开发规范总结
-----

### HTML 规范：

#### 1、使用 HTML5 规范：<!DOCTYPE html>

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

#### 3、变量使用 Camel 命名法

#### 4、默认使用双引号

#### 5、总是使用分号结尾

#### 6、常量的形式如：NAMES_LIKE_THIS，使用大写字符，并用下划线分割；永远不要使用 const 关键词

#### 7、可以随意使用嵌套函数，可以减少重复代码、隐藏帮助函数等等

#### 8、不要在块内声明一个函数

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

#### 9、注释规范

· 模块功能描述说明：

{% highlight bash %}
/**
 * ------------------------------------------------------------------
 * 模块描述说明
 * ------------------------------------------------------------------
 */
{% endhighlight %}

· 模块内的小函数方法归类：

{% highlight bash %}
/**
 * 小函数方法归类说明，这些零散的小函数方法放在一起，对应一个业务方法逻辑
 * ------------------------------------------------------------------
 */
{% endhighlight %}

· 单个函数方法：

{% highlight bash %}
/**
 * 函数功能简述
 *
 * 具体描述一些细节
 *
 * @param    {string}    address    地址
 * @param    {array}     com        商品数组
 * @param    {string}    pay_status 支付方式
 * @returns  void
 *
 * @date     2014-04-12
 * @author   jinjinghui<juily.jjh@qq.com>
 */
{% endhighlight %}

· 单行注释：

{% highlight bash %}
// 这是一条单行注释
{% endhighlight %}

· 单个函数方法中变量注释：

{% highlight bash %}
// 商品属性变量（一组变量描述）
    // 商品名字（单个变量注释）
var name = $(item).find("js-name").val();
    // 商品数量
    count = $(item).find(".js-count").text();
    // 商品单价
    price = $(item).find(".js-price").val();
{% endhighlight %}

· 单个函数方法中代码片段注释：

{% highlight bash %}
/*
 | 代码片段的描述说明
 */
{% endhighlight %}

#### 10、文件命名统一使用小写 .js，同时推荐 “-” 而不是 “_”
