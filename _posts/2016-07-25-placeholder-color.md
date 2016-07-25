---
layout: post
title: 修改输入框placeholder文字默认颜色
author: juily
---
##  修改输入框placeholder文字默认颜色
-----


html5为input添加了原生的占位符属性placeholder，高级浏览器都支持这个属性，默认为淡灰色，例如：
<input type="text" placeholder="改变placeholder默认颜色" value="">

{% highlight bash %}
<input type="text" placeholder="改变placeholder默认颜色" value="">
{% endhighlight %}

如果想改变这个默认颜色，可以用css改变响应的属性值，解决方案如下：
（因为每个浏览器的CSS选择器都有所差异，所以需要针对每个浏览器做单独的设定(可以在冒号前面写input和textarea)。）

{% highlight bash %}
::-webkit-input-placeholder { /* WebKit browsers */
　　color:#999;
}
:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
　　color:#999;
}
::-moz-placeholder { /* Mozilla Firefox 19+ */
　　color:#999;
}
:-ms-input-placeholder { /* Internet Explorer 10+ */
　　color:#999;
}
{% endhighlight %}

或者写成这样：

{% highlight bash %}
input::-webkit-input-placeholder, textarea::-webkit-input-placeholder {
　　color: #666;
}
input:-moz-placeholder, textarea:-moz-placeholder {
　　color:#666;
}
input::-moz-placeholder, textarea::-moz-placeholder {
　　color:#666;
}
input:-ms-input-placeholder, textarea:-ms-input-placeholder {
　　color:#666;
}
{% endhighlight %}

我们注意到，css中出现了但冒号: 和双冒号: ，
知识点：单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。
　　	css伪类：CSS 伪类用于向某些选择器添加特殊的效果。
　　	css伪元素：CSS 伪元素用于向某些选择器设置特殊效果。
　　	伪元素由双冒号和伪元素名称组成。双冒号是在当前规范中引入的，用于区分伪类和伪元素。但是伪类兼容现存样式，浏览器需要同时支持旧的伪类， 如:first-line,:first-letter,:before,:after等等。因此对于css2之前已经有的伪元素两种写法的作用是一样的，但是为了兼容IE浏览器还是使用单冒号的写法。
