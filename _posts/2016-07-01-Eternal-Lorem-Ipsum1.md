---
layout: post
title: Tbale元素设置圆角border-radius
author: juily
---

## Tbale元素设置圆角border-radius
-----
我们在个Table元素设置圆角的时候，table背景有圆角，但是table的边框没有圆角出现，如下所示:

<table style="background-color: red; border: 1px solid blue;border-radius: 5px;">
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
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>

代码如下：
{% highlight bash %}
<table style="background-color: red; border: 1px solid blue;border-radius: 5px;">
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
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>
{% endhighlight %}

为什么边框没有圆角呢？ 其实，Table元素还有个属性border-collapse，如果想要Table边框也出现圆角，那么我们需要给该属性设置separate值。 效果如下

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
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>
{% endhighlight %}