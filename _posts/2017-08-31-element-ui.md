---
layout: post
title: Element-UI 源码分析
author: juily
---
## Element-UI 源码分析
-----

### 一、目录结构

我们可以看到 clone 下来的代码目录结构如下：

![](https://juilyhui.github.io/images/posts/elementui1.jpeg)

{% highlight bash %}
|-- build       // 构建配置
|-- examples    // 页面
|-- packages    // components 源码
|-- src
|-- test
{% endhighlight %}

#### 1、packages 文件夹

packages 里面放的是所有的 components ，每个组件是一个文件夹，文件夹名字以组件名命名。下面以 alert 组件为例：

![](https://juilyhui.github.io/images/posts/elementui2.jpeg)

{% highlight bash %}
|-- packages
    |-- alert
        |-- src
            |-- main.vue    // .vue 文件，真正的组件代码
        |-- index.js        // 组件的全局注册代码
{% endhighlight %}

#### 2、examples 文件夹

examples 里面是页面代码。

![](https://juilyhui.github.io/images/posts/elementui3.jpeg)

其中 docs 文件夹下面的 .md 文件是所有的组件示例，即我们在页面上看到的示例（分中英文两种）：

![](https://juilyhui.github.io/images/posts/elementui4.jpeg)
