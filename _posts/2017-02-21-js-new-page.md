---
layout: post
title: 用js创建一个新页面并在新标签内打开
author: juily
---
## 用js创建一个新页面并在新标签内打开
-----
今天在工作中遇到一个问题，是关于银联卡入金的，我在普通的接口中给后台传入需要的字段，后台去跟第三方进行的回调，结果是后台只能返回给我html源码的字符串形式，需要我把这些字符串放到一个新页面，并且在新窗口打开这个页面。

那么，我就需要用js去创建一个新的页面并且在新标签内打开。

js代码如下：
{% highlight bash %}
    window.open('about:blank;').document.write(string);
{% endhighlight %}
