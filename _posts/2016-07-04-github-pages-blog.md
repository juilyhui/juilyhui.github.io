---
layout: post
title: 如何在github上搭建个人主页（本博客的搭建过程）
author: juily
---
## 如何在github上搭建个人主页（本博客的搭建过程）
-----

文章开头先介绍一个本博客的基本性质。

1、操作简单，有git使用经验的只需要几分钟便可搭建完成。
2、没有数据库，不支持评论模块。
3、文章使用markdown格式编写

### 一、搭建须知
如果你没有github账号，那么麻烦去注册一个，我们的个人博客是依赖于github搭建的。
每个账号只有一个仓库来存放个人主页，也就是说，一个github账号只能搭建一个个人主页。
仓库名字必须是username/username.github.io，这是特殊的命名约定，遵守即可。搭建完成之后访问http://username.github.io就可以看到你的个人主页啦~
个人主页的网站内容是在master分支下的。貌似不能有别的分支，具体也没有尝试，不过对于个人主页来说，一个master分支应该也够用了。

### 二、关于jekyll
jekyll也是gihutb上面的一个开源项目，实际上就是一个模板转化引擎。博客基于Jekyll，有很多[现成的模板](http://jekyllthemes.org/)，可以直接使用。
以White Paper这个模板为例，可以直接下载压缩包，也可以使用如下命令clone到本地：
{% highlight bash %}
$ git clone https://github.com/vinitkumar/white-paper.git
{% endhighlight %}

把克隆下来的文件拷贝到你自己的目录就行了，这样你就有一个现成的网站结构了。

### 三、关于GitHub-Pages
GitHub-Pages仅仅为我们提供了静态页面的托管，只是静态的。有以下特点：
1.免空间费，免流量费
2.具有项目主页和个人主页两种选择
3.支持页面生成，可以使用jekyll来布局页面，使用markdown来书写正文
4.可以自定义域名


### 更详细的内容建议参考
[一步步在GitHub上创建博客主页 相关文章](http://www.pchou.info/ssgithubPage/2013-01-03-build-github-blog-page-01.html)
