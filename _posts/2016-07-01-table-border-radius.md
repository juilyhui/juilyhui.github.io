---
layout: post
title: Table元素设置圆角border-radius
author: juily
---
## Table元素设置圆角border-radius
-----


首先，我们先对border-collapse属性进行简单的了解。

有以下三个属性值：
separate  默认值。边框会被分开。不会忽略 border-spacing 和 empty-cells 属性。 
collapse  如果可能，边框会合并为一个单一的边框。会忽略 border-spacing 和 empty-cells 属性。
inherit   规定应该从父元素继承 border-collapse 属性的值。



注意：
	1、所有主流浏览器都支持该属性。
	2、任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 "inherit"。
	3、如果没有规定 !DOCTYPE，则 border-collapse 可能产生意想不到的结果。


我们在个Table元素设置圆角的时候，table的border-collapse不同的属性值会影响圆角的产生，

如下所示:

设置border-collapse: collapse;
<table style="background-color: red; border: 1px solid blue;border-radius: 20px;border-collapse: collapse;">
	<thead>
		<tr>
			<th>1</th>
			<th>2</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>

代码如下：
{% highlight bash %}
<table style="background-color: red; border: 1px solid blue;border-radius: 20px; border-collapse: collapse;">
	<thead>
		<tr>
			<th>1</th>
			<th>2</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>
{% endhighlight %}


设置border-collapse: separate;
<table style="background-color: red; border: 1px solid blue;border-radius: 5px;border-collapse: separate;">
	<thead>
		<tr>
			<th>1</th>
			<th>2</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>

代码如下：
{% highlight bash %}
<table style="background-color: red; border: 1px solid blue;border-radius: 5px;border-collapse: separate;">
	<thead>
		<tr>
			<th>1</th>
			<th>2</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>
{% endhighlight %}

