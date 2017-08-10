---
layout: post
title: TypeScript 学习（二）
author: juily
---
## TypeScript 学习（二）
-----

### 一、深入学习

#### <font color="blue">1、基础类型</font>

TypeScript 支持‘数字、字符串、结构体、布尔值、枚举类型’等数据类型。

{% highlight bash %}
// 布尔值
let isDone: boolean = false;

// 数字
// TypeScript 里所有数字都是浮点数 number；支持十进制、十六进制、二进制、八进制。
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;

// 字符串
let name: string = 'Jin';
let sentence: string = 'Hello, my name is ${ name }'; // 模板字符串

// 数组
let list: number[] = [1, 2, 3]; // 元素类型[]
let list: Array<number> = [1, 2, 3]; // 数组泛型，即 Array<元素类型>

// 元祖 Tuple
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

// 枚举 enmu
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

// any
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

// void
// 表示没有任何类型（某种程度上和 any 类型相反）
function warnUser(): void { // 函数没有返回值，通常是 void 类型
    alert('This is my warning message');
}
// 声明一个 void 的变量，只能为它赋予 undefined 和 null
let unusable: void = undefined;

// null & undefined 和 void 相似，作用不大
let u: undefined = undefined;
let n: null = null;

// never
// 表示永不存在的值的类型。如：总是抛出异常、根本不会有返回值的函数等等。
function error(message: string): never {
    throw new Error(message);
}
function infiniteLoop(): never {
    while (true) {};
}

// 类型断言
// 好比其他语言里的类型转化，但是不会进行特殊的数据检查和解构。不影响运行，只在编译阶段起作用。TypeScript 会假设程序员已经进行了必须的检查。
// 尖括号语法 '<类型>'
let someValue: any = 'this is a string';
let strLength: number = (<string>someValue).length;
// as 语法 'as 类型' （在 TypeScript 里使用 JSX 时，只有 as 语法断言被允许）
let someValue: any = 'this is a string';
let strLength: number = (someValue as string).length;

{% endhighlight %}
