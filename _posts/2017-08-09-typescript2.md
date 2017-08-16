---
layout: post
title: TypeScript 学习（二）-- 基础类型 & 变量声明
author: juily
---
## TypeScript 学习（二）-- 基础类型 & 变量声明
-----

### <font color="blue">一、基础类型</font>

TypeScript 支持‘数字、字符串、结构体、布尔值、枚举类型’等数据类型。

{% highlight bash %}
// 1、布尔值
let isDone: boolean = false;


// 2、数字
// TypeScript 里所有数字都是浮点数 number；支持十进制、十六进制、二进制、八进制。
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;


// 3、字符串
let name: string = 'Jin';
let sentence: string = 'Hello, my name is ${ name }'; // 模板字符串


// 4、数组
let list: number[] = [1, 2, 3]; // 元素类型[]
let list: Array<number> = [1, 2, 3]; // 数组泛型，即 Array<元素类型>


// 5、元祖 Tuple
// 表示一个已知元素数量和类型的数组，个元素的类型不必相同。
let x: [string, number];
x = ['hello', 10]; // 正确
x = [10, 'hello']; // 错误
// 当访问一个已知索引的元素，会得到正确的类型
console.log(x[0].substr(1)); // 正确
console.log(x[1].substr(1)); // 错误，number 类型 没有 substr 属性
// 当访问一个越界的元素（定义时候不存在的元素），会使用联合类型（定义时候的所有类型）替代
x[3] = 'world'; // 正确，字符串可以赋值给（string | number）类型
x[4] = true; // 错误，布尔不是（string | number）类型


// 6、枚举 enmu
// 是对 JavaScript 标准数据类型的一个补充，使用枚举类型可以为一组数值赋予友好的名字。
enum Color {Red, Green, Blue};
let c: Color = Color.Green;
// 默认情况下，从0开始为元素编号；也可以手动指定成员的编号
// 从1开始编号
enum Color {Red = 1, Green, Blue};
let c: Color = Color.Green;
// 全部手动赋值
enum Color {Red = 1, Green = 2, Blue = 4};
let c: Color = Color.Green;
// 枚举提供的一个便利是可以由枚举的元素编号得到它的名字
enum Color {Red = 1, Green, Blue};
let colorName: string = Color[2];
console.log(colorName); // 打印出的是 Color.Green 的值


// 7、any
// 不清楚类型的变量，不进行检查直接通过编译阶段
let notSure: any = 4;
notSure = 'maybe a string instead'; // 正确
notSure = false; // 正确
// 还可以在它上面调用任意方法（与 Object 类型不同的地方）
let notSure: any = 4;
notSure.ifItExists(); // 正确；即使在定义时候没有这个方法
notSure.toFixed(); // 正确；toFixed 方法的确存在，但是不会进行检查
// 对比 Object 类型
let prettySure: Object = 4;
prettySure.toFixed(); // 错误；Object 类型没有 toFixed 这个方法
// any 类型还可以定义‘只知道一部分数据的类型’
let list: any[] = [1, true, 'free'];
list[1] = 100;


// 8、void
// 表示没有任何类型（某种程度上和 any 类型相反）
function warnUser(): void { // 函数没有返回值，通常是 void 类型
    alert('This is my warning message');
}
// 声明一个 void 的变量，只能为它赋予 undefined 和 null
let unusable: void = undefined;


// 9、null & undefined 和 void 相似，作用不大
let u: undefined = undefined;
let n: null = null;


// 10、never
// 表示永不存在的值的类型。如：总是抛出异常、根本不会有返回值的函数等等。
function error(message: string): never {
    throw new Error(message);
}
function infiniteLoop(): never {
    while (true) {};
}


// 11、类型断言
// 好比其他语言里的类型转化，但是不会进行特殊的数据检查和解构。不影响运行，只在编译阶段起作用。TypeScript 会假设程序员已经进行了必须的检查。
// 尖括号语法 '<类型>'
let someValue: any = 'this is a string';
let strLength: number = (<string>someValue).length;
// as 语法 'as 类型' （在 TypeScript 里使用 JSX 时，只有 as 语法断言被允许）
let someValue: any = 'this is a string';
let strLength: number = (someValue as string).length;


{% endhighlight %}

### <font color="blue">二、变量声明</font>

TypeScript 本身就支持 let 和 const。

1、let 声明

（1）块作用域

var 声明的变量可以在包含它们的函数外访问，但是块作用域变量在包含它们的块或 for 循环之外是不能访问的。

{% highlight bash %}
function f(input: boolean) {
    let a = 100;
    if (input) {
        let b = a +1;
        return b;
    }
    return b; // 在这里访问不到 b，b 的作用域是 if 语句块里
}
{% endhighlight %}

块作用域变量的另一个特点：不能再被声明前读写。虽然这些变量始终‘存在’于它们的作用域里，但是直到声明它的代码之前都属于‘暂时性死区’。

{% highlight bash %}
a++; // 报错
let a;

function foo() {
    return a; // 可以在声明前获取
}
foo(); // 执行该函数时会报错
let a;
{% endhighlight %}

（2）重定义及屏蔽

{% highlight bash %}
let x = 10;
let x = 20; // 错误；不能在1个作用域里多次声明 x

// 块级作用域变量需要在明显不同的块里声明
function f() {
    let x = 100;
}
function g() {
    let x = 200;
}
f(); // 100
g(); // 200
{% endhighlight %}

在一个嵌套作用域里引入一个新名字的行为称做屏蔽。它是一把双刃剑，它可能会不小心地引入新问题，同时也可能会解决一些错误。

{% highlight bash %}
function sumMatrix(matrix: number[][]) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }
    return sum;
}
{% endhighlight %}

这个版本的循环能得到正确的结果，因为内层循环的i可以屏蔽掉外层循环的i。

（3）块作用域变量的获取

当 let 声明出现在循环体里时拥有完全不同的行为。不仅在循环里引入了一个新的变量环境，而且针对‘每次迭代’都会创建一个新作用域。

{% highlight bash %}
for (let i < 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}
{% endhighlight %}

上面代码会输出如下结果：

{% highlight bash %}
0
1
2
3
4
5
6
7
8
9
{% endhighlight %}

2、const 声明

const 声明变量，被赋值后不能再改变。但是实际上，const 变量的内部状态是可修改的，幸运的是 TypeScript 允许将对象的成员设置成只读。

{% highlight bash %}
const numLiveForCat = 9;
const kitty = {
    name: 'Jin',
    numLives: numLiveForCat
};
kitty = {
    name : 'Lucy', // 错误
    numLives: numLiveForCat
};
kitty.name = 'Lucy'; // 正确；const 变量的内部状态是可修改的
{% endhighlight %}

3、let & const 使用原则

最小特权原则，所有变量除了计划要去修改的，都应该用 const。

4、解构

简单说，解构赋值允许你使用数组或对象字面量的语法，将数组和对象的属性付给各种变量。

（1）解构数组

{% highlight bash %}
let input = [1, 2];
let [first, second] = input;
console.log(first); // 1
console.log(second); // 2
{% endhighlight %}

上面代码创建了2个变量 first 和 second，相当于使用了索引：

{% highlight bash %}
first = input[0];
second = input[1];
{% endhighlight %}

解构作用于已声明的变量会更好：

{% highlight bash %}
let a: number = 1;
let b: number = 1;
let [first, second] = [a, b];
{% endhighlight %}

解构作用于函数参数：

{% highlight bash %}
let input = [1, 2];
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input);
{% endhighlight %}

在数组里使用 '...' 语法创建剩余变量：

{% highlight bash %}
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest); // [2, 3, 4];
{% endhighlight %}

也可以忽略尾随元素

{% highlight bash %}
let [first] = [1, 2, 3, 4];
console.log(first); 1
{% endhighlight %}

或者忽略其他元素：

{% highlight bash %}
let [, second, , fourth] = [1, 2, 3, 4];
{% endhighlight %}

（2）对象解构

{% highlight bash %}
let o = {
    a: 'foo',
    b: 12,
    c: 'bar'
};
let { a, b } = o;
{% endhighlight %}

上面代码，把 o.a 的值付给了 a，把 o.b 的值赋给了 b。（不需要 c，可以忽略）

可以用没有声明的赋值：

{% highlight bash %}
({ a, b } = { a: 'baz', b: 101 }); // 需要用括号，因为 JavaScript 通常会将以  '{' 起始的语句解析成一个块
{% endhighlight %}

使用 '...' 语法创建剩余变量：

{% highlight bash %}
let o = {
    a: 'foo',
    b: 12,
    c: 'bar'
};
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
{% endhighlight %}

属性重命名：

{% highlight bash %}
let { a: newName1, b: newName2 } = o; // 这里的冒号不是指定类型的

let { a, b }: {a: string, b: number} = o; // 指定类型仍要在后面写上完整的模式
{% endhighlight %}

上面的代码可以理解为如下：

{% highlight bash %}
let newName1 = o.a;
let newName2 = o.b;
{% endhighlight %}

默认值，默认值可以在属性为 undefined 时使用缺省值：

{% highlight bash %}
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject; // 如果 wholeObject 的 b 属性是 undefined，则给 b 复制为 1001，实现初始化
}
{% endhighlight %}

（3）函数声明

解构也能用于函数声明：

{% highlight bash %}
type C = { a: string, b?: number };
function f({ a, b }: C: void) {
    // ...
}
{% endhighlight %}

但是通常更多的是去指定默认值：

{% highlight bash %}
// 首先在默认值之前设置格式
fucntion f({ a, b } = { a: '', b: 0 }) {
    // ...
}
f(); // 函数 f 的参数值为 { a: '', b: 0 }
{% endhighlight %}

{% highlight bash %}
// 其次，在解构属性上给予一个默认或可选的属性用来替换主初始化列表
function f({ a, b = 0 } = { a: '' }) {
    // ...
}
f({ a: 'yes' }); // 函数 f 的参数值为 { a: 'yes', b: 0}
f(); // 函数 f 的参数值为 { a: '', b: 0 }
f({}); // 错误；函数 f 的参数值的 a 属性是必须的
{% endhighlight %}

5、展开

与解构相反。展开允许将一个数组展开为另一个数组，或一个对象展开为另一个对象。

展开数组：

{% highlight bash %}
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
console.log(bothPlus); // [0, 1, 2, 3, 4, 5];
{% endhighlight %}

展开操作创建的是 first 和 second 的一份浅拷贝，first 和 second 不会被展开操作所改变。

展开对象：

{% highlight bash %}
let defaults = {
    food: 'spicy',
    price: '$$',
    ambiance: 'noisy'
};
let search = { food: 'rich', ...defaults }; // defaults 里的 food 属性会重写 food: 'rich'
{% endhighlight %}

对象展开的限制：只包含只身的可枚举属性（展开一个对象实例时，会丢失该对象的方法）；TypeScript 编辑器不允许展开泛型函数上的类型参数。
