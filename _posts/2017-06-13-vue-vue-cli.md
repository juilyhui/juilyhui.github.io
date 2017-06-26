---
layout: post
title: 【vue】用vue-cli快速构建一个vue项目
author: juily
---
## 【vue】用vue-cli快速构建一个vue项目
-----

### 一、简介

Vue.js 提供一个官方命令行工具，可用于快速搭建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目。

关于vue-cli生成的项目，[官方文档](http://vuejs-templates.github.io/webpack/)

类似插件：[vue-mulipage-cli](https://www.npmjs.com/package/vue-multipage-cli)，与vue-cli基本一样，但是是多页面应用程序。

### 二、用vue-cli构建项目

{% highlight bash %}
// 全局安装 vue-cli
npm install --global vue-cli

// 创建一个基于 webpack 模板的新项目
vue init webpack my-project

cd my-project

// 安装依赖
npm install

npm run dev

{% endhighlight %}

其中，在执行 vue init webpack my-project 命令时，会让你填写如下信息：

1、Project Name

2、Project description

3、Author

4、Vue build

5、Use ESLint to lint your code？
    是否使用ESLint校验代码

6、Pick an ESLint preset
    选择一种ESLint校验设置

7、Setup unit tests with Karma + Mocha
    使用Karma + Mocha进行设置单元测试

8、Setup e2e tests with Nightwatch
    用Nightwatch设置e2e测试

### 三、构建出的项目

如下图，是我们用vue-cli构建出的项目文件结构：

![](https://juilyhui.github.io/images/posts/vue-cli-project.jpeg)

#### 文件说明

![](https://juilyhui.github.io/images/posts/vue-cli-project-3.jpeg)

#### README.md 文件内容如下：

![](https://juilyhui.github.io/images/posts/vue-cli-project-2.jpeg)

npm install  安装依赖模块。

npm run dev  开启本地环境。

npm run build  打包项目，发布正式版本；会在目录下生成 dist 文件夹（生成的是压缩混淆后的文件）。

npm run build --report  打包项目，发布正式版本；同 npm run build，但是会同时输出构建情况。

（以下的测试命令，需要在创建项目，填写信息时，选择需要测试模块，才会有以下命令）

npm run unit  单元测试。

npm run e2e  e2e测试。

npm test  进行所有测试。

#### build/dev-serve.js 文件

npm run dev 命令其实就是是通过node执行项目下的该文件，启动一个express服务器。这个服务器加载了几个中间件，如：

http-proxy-middleware（代理转发中间价）

webpack-dev-middleware（webpack开发中间件）

webpack-hot-middleware（webpack热替换中间件）

#### build/dev-client.js 文件

该文件注入到浏览器，监听webpack-hot-middleware传过来的事件，用于代码热更新等。

#### build/utils.js 文件

该文件主要生成webpack，css-loader，sass-loader（与styles相关的Loader）等配置。
