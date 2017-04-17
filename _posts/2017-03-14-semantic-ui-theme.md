---
layout: post
title: Semantic UI 自定义主题方案
author: juily
---
## Semantic UI 自定义主题方案
-----

### 一、写在正文前

最近公司的一个boss项目在用semantic-ui框架，因为项目需要实现不同经纪商的自定义主题配置化，于是深入研究了一下semantic中的主题方案。

目前只是配置了颜色主题。

### 二、如何实现的

经过研究，发现semantic本身已经提供了两个变量，去配置我们的主色、辅助色，但是可惜的是，semantic只支持了 button 元素跟随主色或者辅助色，其他的并不支持。

但是，我们可以修改semantic的源码吖！

#### 三、修改源码

找到我们用到的元素、组合、视图或者模块的less文件（在 src > definitions > element/collections/views/modules 下面）

在less文件中增加：加上了 .primary/.secondary 类名的样式，以实现用 .primary/.secondary 控制元素跟随主色/辅助色。

在html中写dom的时候，只需要把需要跟随主色/辅助色的元素加上 .primary 类名即可。

#### 四、最后， 主题如何实现更换

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

其中， src > 下的theme.config文件是用于配置主题的,默认是'default'。

我们可以在 src > themes > 下新建一个文件夹，代表我们自定义的主题文件，例如为 myTheme 。

在新生成的文件夹myTheme下，globals>site.variables文件中，修改以下变量（或者在site.overrides中，或者在reset.variables中）：
{% highlight bash %}
@primaryColor : @primary
@secondaryColor: @secondary
@lightPrimaryColor: 客户选定 / 我们根据primary生成一个浅色
@lightSecondaryColor: 客户选定 / 我们根据secondary生成一个浅色
{% endhighlight %}

然后将 src > themes > default 文件夹下的内容复制到到新建的 myTheme 文件夹中。

如果想要用我们自定义的 myTheme 主题，只需要将 theme.config 文件中的配置改为'myTheme'，在semantic项目文件下执行 gulp build 命令即可生成新的css。
