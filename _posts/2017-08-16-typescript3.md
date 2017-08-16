---
layout: post
title: TypeScript 学习（三）-- 接口
author: juily
---
## TypeScript 学习（三）-- 接口
-----

### <font color="blue">一、接口</font>

#### 1、接口介绍

TypeScript 的核心原则之一，是对值所有的结构进行类型检查。它有时被称为‘鸭式辨型法’或‘结构性子类型化’。

TypeScript 里，接口的作用是为这些类型命名和为你的代码（或第三方代码）定义契约。

{% highlight bash %}
function printLabel(labelledObj: { label: string }) {
    console.log(labelledObj.label);
}
let myObj = {
    size: 10,
    label: 'Size 10 Object'
};
printLabel(myObj);
{% endhighlight %}

上面的代码：函数 printLabel 有一个参数，并要求这个参数有一个名为 label、类型为 string 的属性。编译器会检查参数的 label 是否存在，且类型是否匹配。

下面重写上面的例子，用接口来描述：必须包含一个 label 属性、且类型为 string：

{% highlight bash %}
interface LabelledValue {
    label: string;
}
function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}
let myObj = {
    size: 10,
    lable: 'Size 10 Object'
};
printLabel(myObj);
{% endhighlight %}

上面的代码：我们定义了 LabelledValue 接口，LabelledValue 接口就好比一个名字，用来描述我们定义的要求。这里 LabelledValue 接口代表了有一个 label 属性且类型为 string 的对象。

注意：只要函数传入的对象满足接口定义的要求，它就是被允许的。

#### 2、可选属性

接口里的属性不全是必需的。有些是只在某些条件下存在，或者根本不存在。可选属性在应用 ‘option bags’ 模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。

下面是应用了 ‘option bags’ 的例子：

{% highlight bash %}
interface SquareConfig {
    color?: string; // 可选属性，有 ? 符号
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = {
        color: 'white',
        area: 100
    };
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.width = config.width;
    }
    return newSquare;
}
let mySquare = createSquare({color: 'black'});
{% endhighlight %}

可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

比如，我们故意将 createSquare 里的 color 属性名拼错，就会得到一个错误提示：

{% highlight bash %}
interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = {
        color: 'white',
        area: 100
    };
    if (config.color) {
        newSquare.color = config.clor; // 报错；Error: Property 'clor' does not exist on type 'SquareConfig'
    }
    if (config.width) {
        newSquare.area = config.area;
    }
    return newSquare;
}
let mySquare = createSquare({color: 'black'});
{% endhighlight %}

#### 3、只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。可以在属性名前用 readonly 来指定只读属性：

{% highlight bash %}
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = {
    x: 10,
    y: 20
};
p1.x = 5; // 报错；error
{% endhighlight %}

TypeScript 具有 ReadonlyArrar<T> 类型，与 Array<T> 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能修改：

{% highlight bahs %}
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArrar<number> = a;
ro[0] = 12; // error
ro.push(5); // error
ro.length = 100; // error
a = ro; // error；就算把整个 ReadonlyArrar 赋值到一个普通数组也是不可以的
a = ro as number[]; // 正确；可以用类型断言重写。
{% endhighlight %}

readonly vs const（作为变量使用的用 const，若作为属性则使用 readonly ）

#### 4、额外的属性检查

{% highlight bash %}
interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}
let mySquare = createSquare({ colour: 'red', width: 100 });
{% endhighlight %}

上面的代码：传入的是 colour 不是 color，会报错。但有时候，我们的确想要传入一个接口中没有定义的属性，那如何绕开这些检查？

（1）最简便的方法是使用类型断言：

{% highlight bash %}
let mySquare = createSquare({ colour: 'red', width: 100 } as SquareConfig);
{% endhighlight %}

（2）最佳的方式是添加一个字符串索引签名（前提是你能够确定这个对象可能具有某些作为特殊用途使用的额外属性）。如果 SquareConfig 带有上面定义的类型的 color 和 width 属性，并且还会带有任意数量的其他属性，那么可以这样定义：

{% highlight bash %}
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
{% endhighlight %}

（3）将这个对象赋值给另一个对象，因为 squareOptions 不会经过额外书检查，所以编译器不会报错（不推荐绕开检查的做法）：

{% highlight bash %}
let squareOptions = { colour: 'red', width: 100 };
let mySquare = createSquare(squareOptions);
{% endhighlight %}

#### 5、函数类型

接口能够描述 JavaScript 中对象拥有的各种各样的外形。除了描述带有属性的普通对象外，接口也可以描述函数类型。

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

{% highlight bash %}
interface SearchFunc {
    (source: string, subString: string): boolean;
}
{% endhighlight %}
