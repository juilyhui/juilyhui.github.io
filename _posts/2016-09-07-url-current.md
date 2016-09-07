---
layout: post
title: 判断当前页改变菜单栏的样式
author: juily
---
## 判断当前页改变菜单栏的样式
-----

这个办法是通过获取当前页面的完整url，在根据html中写的href地址来判断的。

把当前页的url和href参数进行匹配，如果匹配成功，加上不同的类名改变样式即可。

js代码示例（依赖jQuery）：
{% highlight bash %}

var links = $(".nav-list"); // .nav-list 是带有href链接地址的a标签
var currentUrl = document.location.href;
for(var i= 0; i< links.length; i++){
	var linkUrl = links[i].getAttribute("href");
	if(currentUrl.indexOf(linkUrl) != -1){
		$(links[i]).addClass("current-nav")
	}
}

{% endhighlight %}
