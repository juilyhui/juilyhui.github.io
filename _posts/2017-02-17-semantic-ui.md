---
layout: post
title: Semantic UI -- 一个比Bootstrap更美观的UI框架
author: juily
---
## Semantic UI -- 一个比Bootstrap更美观的UI框架
-----

### Semantic UI 简介

[Semantic UI 官网](http://semantic-ui.com/)

### Semantic UI 安装方法

参考[http://www.qdfuns.com/notes/18123/af344b6aac29f2da66fcb746f82c4070.html](http://www.qdfuns.com/notes/18123/af344b6aac29f2da66fcb746f82c4070.html)

### Semantic UI 源码阅读

semantic-ui 第一级目录结构如下：
{% highlight bash %}
    Semantic-UI
        -- dist
        -- example
        -- node_modules
        -- src
        -- tasks
        -- test
       .csscomb.json
       .csslintrc
       .DS_Store
       .gitignore
       .jshintrc
       brower.json
       composer.json
       CONTRIBUTING.md
       gulpfile.js
       karma.conf.js
       LICENSE.md
       logo.png
       package.json
       README.md
       RELEASE-NOTES.md
       semantic.json
{% endhighlight %}

其中，-- src 文件夹源文件所在位置，那么 -- src 文件夹下面的目录结构如下：
{% highlight bash %}
    -- src
        -- definitions
        -- site
        -- themes
        .DS_Store
        README.md
        semantic.less
        theme.config
        theme.less
{% endhighlight %}

definitions/ 真正意义上的src，这里才是真正的源文件
site/ 你自己网站的样式
themes/ 主题文件夹

在这些文件夹下面的文件，有 XXX.overrides 和 XXX.variables 这两种文件，.overrides是空的，.variables里面是有的。

override，对面向对象熟悉的人对这个不陌生，重载，又或者说是覆盖，其实在readme里面就有说明，你可以把你先要覆盖的写在override中，因为我们无论用什么框架最优的方法就是不要修改框架里面的东西，这样不利于升级。

具体参考文章：[http://www.jianshu.com/p/cf7a134be03a](http://www.jianshu.com/p/cf7a134be03a)

### 主要总结使用过程中遇到的一些问题。

1、比如说，在写复选框checkbox时，

html代码如下：
{% highlight bash %}
<div class="ui checkbox">
  <input type="checkbox">
  <label>Poodle</label>
</div>
{% endhighlight %}

光写html代码是不会有选中效果的，还需要在js中写入触发动作代码：
{% highlight bash %}
$('.ui.checkbox')
  .checkbox()
;
{% endhighlight %}

这个问题不大，按照官方文档给出的API写即可，但是，为什么在js中写入了触发代码依旧不起效果呢？？

此时，在确保已经引入了semantic-ui相关的css、js文件后，我们需要修改js的触发代码，如下：
{% highlight bash %}
$(documrnt).ready(function(){
    $('.ui.checkbox')
      .checkbox()
    ;
})
{% endhighlight %}

这样就OK了！

2、个人觉得semantic-ui的js部分不够强大，比如上面说到的复选框checkbox，在元素上并不支持click事件，而semantic给出的API又获取不到触发的DOM；以及有人说表单验证也支持的不太到位，获取不到错误的对象之类的。

3、在初始化模块时遇到的问题

最近项目用的Vue2.0，因此正在写很多组件，然后自己写的组件和同事写的组件中都用到了下拉菜单这个模块。

在semantic中，下拉菜单的初始化是这样的：
{% highlight bash %}
    $('.ui.dropdown').dropdown()
{% endhighlight %}

现在遇到的问题是，我和同事写的两个组件中都用了上面的代码去初始化我们各自的下拉菜单，我的组件引用在同事的后面，我在上面的初始化事件中加入了一个onChange的回调函数，用来获取到选择到的值，如下：
{% highlight bash %}
    $('.ui.dropdown').dropdown({
        onChange: function(value){
            console.log('seleced value:', value)
        }
    })
{% endhighlight %}

but，就这么一个简单的函数，里面的console却一直没有被执行。后来，发现同事的组件也用这个初始化代码，把同事的组件暂时拿走，函数就正常执行了。

这就证明：一个模块只能被初始化一次，只有第一次的初始化函数里的相关代码会起作用，后续再有初始化的代码都是不起作用的。

所以嘞，应该在获取DOM元素的时候都用自己定义的类名，而不是用semantic里定义的类名，否则就会出现这种问题。
