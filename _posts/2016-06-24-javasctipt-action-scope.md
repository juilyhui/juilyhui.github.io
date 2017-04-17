---
layout: post
title: 【Javascript】Javascript变量声明与作用域
author: juily
---
## 【Javascript】Javascript变量声明与作用域
-----

### 初识变量声明和作用域

代码1：

{% highlight bash %}
    var a = 'hello';
    (function(){
        console.log(a); // 'hello'
    })();
{% endhighlight %}

代码2：

{% highlight bash %}
    var a = 'hello';
    (function(){
        console.log(a); // undefined
        var a = 'world';
    })();
{% endhighlight %}

说明：

* <font color="red">function作用域里的变量a会覆盖上层作用域变量a。</font>

* <font color="red">在function作用域里，变量a的声明被提升了。</font>代码2相当于：

代码3：

{% highlight bash %}
    var a = 'hello';
    (function(){
        var a; // 变量提升
        console.log(a); // undefined
        var a = 'world';
    })();
{% endhighlight %}

对上面的代码做些改动：

代码4：

{% highlight bash %}
    var a = 'hello';
    if(true){
        console.log(a); // 'hello'
        var a = 'world';
    }
{% endhighlight %}

说明：

* <font color="red">JavaScript没有块级作用域。函数式JavaScript中唯一拥有自身作用域的结构。</font>

## 一、变量声明提升（hoisting）

### 声明、定义与初始化

* 声明 宣称一个名字的存在

* 定义 为这个名字分配存储空间

* 初始化 为名字分配的存储空间赋初始值

{% highlight bash %}
    var a; // 声明变量a
    a = 'hello world'; // （定义并）初始化变量a
{% endhighlight %}

因为JavaScript为动态语言，其变量并没有固定的类型，其存储空间大小会被初始化与赋值而变化，所以其变量的‘定义’就不像传统的静态语言一样了，其定义显得无关紧要。

### 声明提升

<font color="red">当前作用域内的声明都会提升到作用域的最前面，包括变量和函数声明</font>

{% highlight bash %}
    (function(){
        var a = '1';
        var f = function(){};
        var b = '2';
        var c = '3';
    })();
{% endhighlight %}

上面代码的变量a，f，b，c的声明都会被提升到函数作用域的最前面，类似如下：

{% highlight bash %}
    (function(){
        var a, f, b, c;
        a = '1';
        f = function(){};
        b = '2';
        c = '3';
    })();
{% endhighlight %}

<font color="red">请注意函数表达式并没有被提升，这也是函数表达式与函数声明的区别。</font>进一步看两者的区别：

{% highlight bash %}
    (function(){
        // var f1, function f2(){} ==> hoisting，被隐式提升的声明

        f1(); // ReferenceError: f1 is not defind
        f2();

        var f1 = function();
        function f2(){}

    })();
{% endhighlight %}

上面代码中，函数声明f2被提升，所以在前面调用f2是没有问题的。虽然变量f1也被提升，但f1提升后的值为undefined，其真正的初始值是在执行到函数表达式处被赋值的。<font color="red">所以只有声明是被提升的</font>

### 名字解析顺序

JavaScript中一个名字（name）以四种方式进入作用域（scope），其优先级顺序如下：

1、语言内置 所有的作用域中都有this和arguments关键字

2、形式参数 函数的参数在函数作用域中都是有效的

3、函数声明 形如 function foo() {}

4、变量声明 形如 var bar;

名字声明的优先级如上所示，也就是说，<font color="red">如果一个变量的名字与函数的名字相同，那么函数的名字会覆盖变量的名字，无论其在代码中的顺序如何。但是，名字的初始化却是按其在代码中的顺序进行的，不受以上优先级的影响。</font>如下面代码：

{% highlight bash %}
    (function(){
        var foo;
        console.log(typeof foo); // function

        function foo(){};

        foo = 'foo';
        console.log(typeof foo); // string
    })();
{% endhighlight %}

<font color="red">如果形式参数中有多个同名变量，那么最后一个同名参数会覆盖其他同名参数，即使最后一个同名参数并没有定义。</font>

这里我们列出来的名字解析优先级存在例外，比如可以覆盖语言内置的名字arguments。

### 命名函数表达式

<font color="red">可以像函数声明一样为函数表达式指定一个名字，但这并不会使函数表单式成为函数声明。命名函数表达式的名字不会进入名字空间，也不会被提升。</font>

{% highlight bash %}
    f(); // TypeError: f is not a function
    foo(); // ReferenceError: foo is not defined
    var f = function foo(){
        console.log(typeof foo); // 命名函数表达式的名字只在该函数的作用域内部有效。这里有效。
    }
    f(); // function
    foo(); // ReferenceError: foo is not defined 命名函数表达式的名字只在该函数的作用域内部有效。这里无效
{% endhighlight %}

命名函数表达式的名字只在该函数的作用域内部有效。

## 二、Js作用域与作用域链详解

### 函数作用域

{% highlight bash %}
    var scope = 'global';
    function t(){
        console.log(scope); // undefined
        var scope = 'local';
        console.log(scope); // 'local'
    }
    t();
{% endhighlight %}

以上代码可以重述为：

{% highlight bash %}
    var scope = 'global';
    function t(){
        var scope; // function 里的变量提升
        console.log(scope);
        scope = 'local';
        console.log(scope);
    }
    t();
{% endhighlight %}

<font color="red">JavaScript没有块级作用域。</font>如下代码：

{% highlight bash %}
    var name = 'global';
    if(true){
        var name = 'local';
        console.log(name); // 'local'
    }
    console.log(name); // 'local'
{% endhighlight %}

所以，再来理解一下下面的代码：

{% highlight bash %}
    function t(flag){
        if(flag){
            var s = 'ifscope';
            for(var i = 0; i < 2; i ++);
        }
        console.log(i);
        console.log(s);
    }
    t(true); // 2 'ifscope'
{% endhighlight %}

### 变量作用域

{% highlight bash %}
    function t(flag){
        if(flag){
            s = 'ifscope';
            for(var i = 0; i < 2; i++);
        }
        console.log(i);
    }
    t(true); // 2
    console.log(s); // 'ifscope'
{% endhighlight %}

为什么这时候的s依旧是‘ifscope’呢？这是由于<font color="red">Js中没有用var声明的变量都是全局变量，而且是顶层对象的属性。</font>

<font color="red">当使用var声明一个变量时，创建的这个属性是不可配置的，也就是说无法通过delete运算符删除</font>如下：

var name = 1  ==> 不可删除

sex = 'girl'  ==> 可删除

this.age = 22 ==> 可删除

### 作用域链

{% highlight bash %}

    name = 'Lily';

    function t(){
        var name = 'Any';

        function s(){
            var name = 'Job';
            console.log(name);
        }

        function ss(){
            console.log(name);
        }

        s(); // s()->t()->windo
        ss(); // ss()->t()->windo
    }

    t(); // 'Job' 'Any'

{% endhighlight %}

当执行s时，将创建函数s的执行环境（调用对象），并将该对象置于链表开头，然后将函数t的调用对象链接在之后，最后是全局对象。然后从链表开头找变量name。

下面看一个很容易犯错的例子：

{% highlight bash %}
    // js
    function buttonInit(){
        for(var i=1; i<4; i++){
            var b = document.getElementById('button' + i);
            b.addEventListener('click', function(){
                alert('Button' +i);
            }, false);
        }
    }
    window.onload = buttonInit;
{% endhighlight %}
{% highlight bash %}
    // html
    <button id="button1">Button1</button>
    <button id="button2">Button2</button>
    <button id="button3">Button3</button>
{% endhighlight %}

当文档加载完毕，给几个按钮注册点击事件，当我们点击按钮时，三个按钮都是弹出："Button4"。

当注册事件结束后，i的值为4，当点击按钮时，事件函数即function(){ alert("Button"+i);}这个匿名函数中没有i,根据作用域链，所以到buttonInit函数中找，此时i的值为4，所以弹出”button4“。

### with语句

<font color="red">with语句主要用来临时扩展作用域链，将语句中的对象添加到作用域的头部。</font>如下代码：

{% highlight bash %}
    person = {
        name: 'Lily',
        age: 22,
        height: 160,
        friend: {
            name: 'Job',
            age: 21
        }
    }
    with(person.friend){
        console.log(name); // 'Job'
    }
{% endhighlight %}

with语句将person.wife添加到当前作用域链的头部，所以输出的就是：“lwy"。

with语句结束后，作用域链恢复正常。
