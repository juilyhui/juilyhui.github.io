---
layout: post
title: TypeScript 入门随笔
author: juily
---
## TypeScript 入门随笔
-----

### 一、简介

TypeScript 是 JavaScript 类型的超集，可以编译成纯 JavaScript。TypeScript 可以在任何浏览器、任何计算机和任何操作系统中运行。

TypeScript 是由微软开发的一个开源项目，使用 .ts 扩展名，但我们依旧可以在 .ts 文件中使用原生 JavaScript 语法。

TypeScript 自带了编译工具，安装 TypeScript 之后，只需要执行 tsc XXX.ts 命令，即可对 XXX.ts 文件进行编译。

### 二、主要概念

#### <font color="red">1、类型注解</font>

TypeScript 里的类型注解是一种轻量级的为函数或为变量添加约束的方式。

{% highlight bash %}
function greeter(person: string) { // 定义了 person 变量是 string 类型
    return 'Hello ' + person;
}

var user = ['0', '1', '2'];

document.body.innerHTML = greeter(user);
{% endhighlight %}

以上代码，我们定义了函数 greeter 的变量 person 是 string 类型，在调用函数 greeter 时，我们传入了数组 user，不符合我们之前的约束，在编译过充中会报错，错误信息如下：

{% highlight bash %}
greeter.ts(7,26): error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
{% endhighlight %}

需要注意的是，尽管在编译时报错，但是 .js 文件依旧会生成。

#### <font color="red">2、接口</font>

{% highlight bash %}
interface Person {
    firstName: string; // 注意这里是 ‘;’
    lastName: string;
}

function greeter(person: Person) {
    return 'Hello ' + person.firstName + ' ' + person.lastName;
}

var user = {
    firstName: 'Jin',
    lastName: 'Jinghui'
};

document.body.innerHTML = greeter(user);
{% endhighlight %}

以上代码，我们使用接口（interface语法）来描述一个拥有的 firstName 和 lastName 字段的对象。这段代码编译后会输出：

{% highlight bash %}
Hello Jin Jinghui
{% endhighlight %}

#### <font color="red">3、类</font>

TypeScript 支持 JavaScript 的新特性，比如支持基于类的面向对象编程。

{% highlight bash %}
// 创建一个 Student 类，带有一个构造函数和一个公共字段
class Student {
    fullName: string;
    constructor(public firstName, public middleName, public lastName) {
        this.fullName = firstName + ' ' + middleName + ' ' + lastName;
    }
}

// 接口
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return 'Hello ' + person.firstName + ' ' + person.lastName;
}

var user = new Student('Jin', 'Jing', 'hui');

document.body.innerHTML = greeter(user);
{% endhighlight %}

以上代码，编译之后会输出：

{% highlight bash %}
Hello Jin hui
{% endhighlight %}
