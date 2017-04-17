---
layout: post
title: 【Javascript】Javascript面向对象精要记录
author: juily
---
## 【Javascript】Javascript面向对象精要记录
-----

### 一、数据类型

在JavaScript中，数据类型分为两类：

#### 1、原始数据：保存一些简单数据，如true，5等。有以下5种数据类型：

* boolean 布尔

* number 数字

* string 字符串

* null 空类型，其仅有一个值：null

* undefined 未定义，其仅有一个值：undefined

<font color="red">原始数据类型的值是直接保存在变量中，并可以用typeof进行检测。</font>

{% highlight bash %}
    var name = 'Pony';
    var blog = 'http://www.baidu.com';
    var age = 22;
    alert(typeof blog); // 'string'
    alert(typeof age); // 'number'
{% endhighlight %}

<font color="red">但是typeof对null的检测是返回object，而不是返回null。</font>

{% highlight bash %}
    if(typeof null){
        alert('Not Null'); // 执行了这一步
    }else{
        alert('Null');
    }
    // 弹出‘Not Null’
{% endhighlight %}

<font color="red">所以检测null时，最好用全等于(===),其还能避免强制类型转换：</font>

{% highlight bash %}
    console.log('21' === 21); // false
    console.log('21' == 21); // true
    console.log(undefined == null); // true
    console.log(undefined === null); // false
{% endhighlight %}

#### 2、引用类型：保存为对象，实质是指向内存位置的引用，所以不在变量中保存对象。除了自定义的对象，JavaScript提供了6种内建类型：

* Array 数组类型

* Date 日期和时间类型

* Error 运行期错误类型

* Function 函数类型

* Object 通用对象类型

* RegExp 正则表达式类型

<font color="red">可以用new来实例化每一个对象，或者用字面量形式来创建对象：</font>

{% highlight bash %}
    var obj = new Object;
    var own = {
        name: 'Pony',
        blog: 'http://www.baidu.com',
        'my age': 22
    }
    console.log(own.blog); // 访问属性
    console.log(own['my age']);
    obj = null; // 解除引用
{% endhighlight %}

以上例子中：obj并不包含对象实例，而是一个指向内存中实际对象所在位置的指针（或者说引用）。<font color="red">因为typeof对所有非函数的引用类型均返回object</font>，所以需要用instanceof来检测引用类型。

### 二、函数

<font color="red">在JavaScript中，函数就是对象。</font>

<font color="red">使函数不同于其他对象的决定性特性是--函数存在一个被称为[[Call]]的内部属性。内部属性无法通过代码访问而是定义了代码执行时的行为。</font>

#### 1、创建形式

（1）函数声明

用function关键字，会被提升至上下文。

{% highlight bash %}
    function sum(n1, n2){
        return n1 + n2;
    }
{% endhighlight %}

（2）函数表达式，又叫函数字面量

不能被提升

{% highlight bash %}
    var sum = function(n1, n2){
        return n1 + n2;
    }
{% endhighlight %}

<font color="red">自执行函数严格来说也叫函数表达式。主要用于创建一个新的作用域，在此作用域内声明的变量，不会和其他作用域内的变量冲突或混淆，大多是以匿名函数方式存在，且立即自动执行。</font>

{% highlight bash %}
    // 自执行函数
    (function(n1, n2){
        console.log(n1+n2)
    })(1, 3); // 4
{% endhighlight %}

（3）实例化Function内建类型，又称函数构造法

参数必须加引号

{% highlight bash %}
    var sum = new Function('n1', 'n2', 'return n1+n2');
    console.log(sum(2, 3)); // 5
{% endhighlight %}

从技术角度讲，这是一个函数表达式。一般不推荐用这种方法定义函数，因为这种语法会导致解析两次代码（第一次是解析常规ECMAScript代码，第二次是解析传入构造函数中的字符串），从而影响性能。

{% highlight bash %}
    var name = 'haox1';
    function fun(){
        var name = 'lili';
        return new Function('return name'); // 不能获取全局变量
    }
    console.log(fun()()); // haox1
{% endhighlight %}

以上代码：

    Function()构造函数每次执行时都会解析函数主体，并创建一个新的函数对象。所以当在一个循环或频繁执行的函数中调用Function()构造函数效率是非常低的。而函数字面量却不是每次遇到都会重新编译的，用Function()构造函数创建一个函数时并不遵循典型的作用域，它一直把它当做是顶级函数来执行。

3种形式：

{% highlight bash %}
    sayHi1(); // 函数提升

    function sayHi1(){ // 函数声明，形式（1）
        console.log('Hello');
    }

    var sayHi2 = function(){ // 函数字面量，形式（2）
        cosnole.log('Hello');
    }

    var sayHi3 = new Function('console.log(\'Hello\');'); // 函数构造法，形式（3）
{% endhighlight %}


<font color="red">形式（1）特点：解析器会先读取函数声明，并使其在执行任何代码之前可以访问。</font>

<font color="red">形式（2）特点：函数表达式则必须等到解析器执行到它所在的代码才会真正的被解释执行。</font>

#### 2、参数

JavaScript函数的另一个独特之处在于可以给函数传递任意数量的参数。函数参数被保存在arguments类数组对象中，其自动存在函数中，能通过数字索引来引用参数，但它不是数组实例：

{% highlight bash %}
    alert(Array.isArray(arguments)); // false
{% endhighlight %}

类数组对象arguments保存的是函数的实参，但并不会忽略形参。因而，arguments.length返回实参列表的长度，arguments.callee.length返回形参列表的长度。

{% highlight bash %}
    function ref(value){
        return value;
    }
    console.log(ref('HI'));
    console.log(ref('HI', 22));
    console.log(ref.length); // 1
{% endhighlight %}

#### 3、函数中的this

函数中的this可以是全局对象、当前对象或者任意对象，这完全取决于函数的调用方式。

有以下几种调用方式：

* 作为对象方法调用

* 作为函数调用

* 作为构造函数调用

* 使用apply或call调用

（1）作为对象方法调用

在JavaScript中，函数也是对象。因此函数可以作为一个对象的属性，此时该属性被称为该对象的方法，在使用这种方法调用时，this被自然绑定到该对象。

{% highlight bash %}
    var point = {
        x: 0,
        y: 0,
        moveTo: function(x, y){
            // this绑定到当前对象，即point对象
            this.x = this.x + x;
            this.y = this.y + y;
        }
    }
    point.moveTo(1, 1);
{% endhighlight %}

（2）作为函数调用

函数也可以直接被调用，此时this绑定到全局对象。在浏览器中，window就是该全局对象。如下例子：

函数被调用时，this被绑定到全局对象，接下来执行赋值语句，相当于隐式的声明了一个全局变量，这显然不是调用者希望的。

{% highlight bash %}
    function makeNoSense(x){
        this.x = x; // 相当于隐式的声明了一个全局变量x
    }
    makeNoSense(5);
    x; // 此时，x已经成为一个值为5的全局变量
{% endhighlight %}

对于内部函数，即声明在另外一个函数体内的函数，这种绑定到全局对象的方式会产生另外一个问题。如下例子：

    {% highlight bash %}
        var point = {
            x: 0,
            y: 0,
            moveTo: function(x){
                // 内部函数
                var moveY = function(x){
                    this.x = x; // 此时的this指向？ 隐式的声明一个全局变量x
                }
                // 内部函数
                var moveY = function(y){
                    this.y = y; // 此时的this指向？ 隐式的声明一个全局变量y
                }
                moveX(x);
                moveY(y);
            }
        }
        point.moveTo(1, 1);
        point.x; // 0
        point.y; // 0
        x; // 1
        y; // 1
    {% endhighlight %}

以上这个问题，属于JavaScript的设计缺陷，我们希望得到的是内部函数的this应该绑定到其外层函数对应的对象上，为了规避这一设计缺陷，我们可以使用<font color="red">变量替代</font>的放大，约定俗称，该变量一般被命名为that。如下：
    {% highlight bash %}
        var point = {
            x: 0,
            y: 0,
            moveTo: function(){
                var that = this;
                var moveX = function(x){
                    that.x = x;
                }
                var moveY = function(y){
                    that.y = y;
                }
                moveX(x);
                moveY(y);
            }
        }
        point.moveTo(1, 1);
        point.x; // 1
        point.y; // 1
    {% endhighlight %}

（3）作为构造函数使用

JavaScript支持面向对象式编程，与主流的面向对象式编程语言不同，JavaScript并没有类（class）的概念，而是使用基于原型（prototype）的继承方式。相应的，JavaScript中的构造函数也很特殊，如果不使用new调用，则和普通函数一样。作为又一项约定俗成的准则，构造函数以大写字母开头，提醒调用者使用正确的方式调用。如果调用正确，this绑定到新创建的对象上。

{% highlight bash %}
    function Point(x, y){
        this.x = x;
        this.y = y;
    }
{% endhighlight %}

（4）使用apply或call调用

<font color="red">在JavaScript中函数也是对象，对象则有方法，apply和call就是函数对象的方法。这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即this绑定的对象。</font>

如下例子：

{% highlight bash %}
    function Point(x, y){
        this.x = x;
        this.y = y;
        this.moveTo = function(x, y){
            this.x = x;
            this.y = y;
        }
    }

    var p1 = new Point(0, 0);
    var p2 = {
        x: 0,
        y: 0
    }

    p1.moveTo(1, 1);
    p1.moveTo.apply(p2, [10, 10]);
{% endhighlight %}

上面的例子：我们用构造函数生成一个对象p1，该对象上同事具有moveTo方法；使用对象字面量创建了另一个对象p2，使用apply可以将p1的方法应用到p2上，这时候this也被绑定到对象p2上。

另一个方法call也具备同样功能，不同的是最后的参数不是作为一个数组统一传入，而是分开传入。

### 三、对象

对象是一种引用类型，创建对象常见的三种方式：

* Object构造函数

* 对象字面量形式

* 工厂模式创建对象

{% highlight bash %}
    var per1 = {
        name: 'pony',
        blog: 'http://www.baidu.com'
    }

    var per2 = new Object; // 1、Object构建函数

    per2.name = '种植代码的码农'；// ==> per2['name']  2、对象字面量形式

    function createPerson(name, age, job){ // 3、工厂模式创建对象
        var o = new Object();
        o.name = name;
        o.age = 31;
        o.sayName = function(){
            alert(this.name);
        }
        return o;
    }
    createPerson('keven', 31, 'se').sayName();
{% endhighlight %}

#### 1、属性操作

（1）添加对象属性

在JavaScript中，可以随时为对象添加属性：

{% highlight bash %}
    var per1 = {
        name:"Pony",
        blog:"http://www.baidu.com"
    }
    per1.age = 0; // 为对象添加了属性age
    per1.sayName = function(){
        alert(this.name); // 'pony'
    }
{% endhighlight %}

（2）检测对象属性是否存在

因此，在检测对象属性是否存在时，如下是<font color="red">常犯的错误</font>：

{% highlight bash %}
    // 我们希望返回的是true，但其实返回的是false
    if(per1.age){
        alert(true);
    }else{
        alert(false);
    }
{% endhighlight %}

<font color="red">其实per1.age是存在的，但是值为0，所以不满足if条件</font>

<font color="blue">if判断中的值是一个对象、非空字符串、非零数字或true时，判断结果会为真；当值是一个null、undefined、0、false、NaN或空字符串时评估为假。</font>

<font color="blue">因而，检测属性是否存在是，有另外两种方式：in和hasOwnProperty()，前者会检测原型属性和自有（实例）属性，后者只检测自有（实例）属性。如下：</font>

{% highlight bash %}
    console.log('age' in per1); // true
    console.log(per1.hasOwnProperty('age')); // true
    console.log('toString' in per1); // true
    console.log(per1.hasOwnProperty('toString')); // false
{% endhighlight %}

对象per1并没有定义toString，该属性继承于Object.prototype，所以in和hasOwnProperty()检测该属性时出现差异。

如果只想判断一个对象属性是不是原型，可利用如下方法：

{% highlight bash %}
    function isPrototypeProperty(obj, name){
        return name in obj && !obj.hasOwnProperty(name);
    }
{% endhighlight %}

（3）删除属性

<font color="red">删除一个属性，用delete操作符，用于删除自有属性，不能删除原型属性。</font>

{% highlight bash %}
    per1.toString = function(){
        console.log('per1对象')；
    }
    console.log(per1.hasOwnProperty('toString')); // true
    per1.toString(); // 'per1对象'
    delete per1.toString; // 删除了自有属性toString
    console.log(per1.hasOwnProperty('toString')); // false
    console.log(per1.toString()); // [Object Object]
{% endhighlight %}

（4）枚举对象的可枚举属性

两种方式：

* for-in循环  遍历出原型属性

* Object.keys()  只返回自有属性

所有可枚举属性的内部属性[[Enumerable]]的值均为true。

{% highlight bash %}
    var per3 = {
        name: 'Pony',
        blog: 'http://www.baidu.com',
        age: 22,
        getAge: function(){
            return this.age;
        }
    }
{% endhighlight %}

实际上，大部分原生属性的[[Enumerable]]的值均为false，即该属性不能枚举。可以通过propertyIsEnumerable()检测属性是否可以枚举：

{% highlight bash %}
    console.log(per3.propertyIsEnumerable('name')); // true
    var pros = Object.keys(per3); // 返回可枚举属性的名字数组
    console.log('length' in pros); // true
    console.log(pros.propertyIsEnumerable('length')); // false
{% endhighlight %}

属性name是自定义的，可枚举；属性length是Array.prototype的内建属性，不可枚举。

#### 2、属性类型

属性的两种类型：

* 数据属性

* 访问器属性

二者分别具有四个属性特征：

* 数据属性：[[Enumerable]]、[[Configurable]]、[[Value]]、[[Writable]]

* 访问器属性：[[Enumerable]]、[[Configurable]]、[[Get]]、[[Set]]

[[Enumerable]] 布尔，属性是否可枚举，自定义属性默认是true。

[[Configurable]] 布尔，属性是否可配置（可修改或可删除），自定义属性默认是true。不可逆，及设置成false后，再设置为true会报错。

[[Value]] 保存属性的值。

[[Writable]] 布尔，属性是否可写，所有属性默认可写。

[[Get]] 获取属性值。

[[Set]] 设置属性值。

（1）设置这些内部属性

ES5提供了两个方法设置这些内部属性：

* Object.defineProperty(obj, pro, desc_map)

* Object.defineProperties(pbj. pro_map)

注意：通过这两种方式来定义新属性时，如果不指定特征值，则默认是false，也不能创建同时具有数据特征和访问器特征的属性。可以通过Object.getOwnPropertyDescriptor()方法来获取属性特征的描述，接受两个参数：对象和属性名。若属性存在，则返回属性描述对象。

{% highlight bash %}
    var desc = Object.getOwnPropertyDescriptor(per4, 'name');
    console.log(desc.enumerable); // false
    console.log(desc.configurable); // false
    console.log(desc.writable); // true
{% endhighlight %}

根据属性的属性类型，返回的属性描述对象包含其对应的四个属性特征。

#### 3、禁止修改对象

对象和属性一样具有指导其行为的内部特征。其中，[[Extensible]]是一个布尔值，指明改对象本身是否可以被修改（[[Extensible]]值为true）。创建的对象默认都是可以扩展的，可以随时添加新的属性。

ES5提供了三种方式：

* Object.preventExtensions(obj) 创建不可扩展的obj对象，可以利用Object.isExtensible(obj)来检测obj是否可以扩展。严格模式下给不扩展对象添加属性会报错，非严格模式下则添加失败。

* Object.seal(obj) 封印对象，此时obj的属性变成只读，不能添加、改变或删除属性（所有属性都不可配置），其[[Extensible]]值为false，[[Configurable]]值为false。可以利用Object.isSealed(obj)来检测obj是否被封印。

* Object.freeza(obj) 冻结对象，不能再冻结对象上添加或删除属性，不能改变属性类型，也不能写入任何数据类型。可以利用Object.isFrozen(obj)来检测obj是否被冻结。

注意：

* 冻结对象和封印对象均要在严格模式下使用。

* 禁止修改对象的三个方法只对对象的自有属性有效，对原型对象的属性无效，仍然可以在原型上添加或修改属性。

### 四、构造函数和原型对象

构造函数也是函数，用new创建对象时调用的函数，与普通函数的一个区别是，其首字母应该大写。但如果将构造函数当做普通函数调用（缺少new关键字）时，则应该注意this指向的问题。

{% highlight bash %}
    var name = 'Pony';
    function Per(){
        console.log('Hello' + this.name);
    }
    var per1 = new Per(); // 'Hello undefined'  此时this指向对象实例
    var per2 = Per(); // 'Hello Pony' 此时this指向window
{% endhighlight %}

使用new时，会自动创建this对象，其类型为构造函数类型，指向对象实例；缺少new关键字，this指向全局对象。

可以用instanceof来检测对象类型，同时每个对象在创建时都自动拥有一个constructor属性，指向其构造函数（字面量形式或Object构造函数创建的对象，指向Object，自定义构造函数创建的对象则指向它的构造函数）。

{% highlight bash %}
    console.log(per1 instanceof Per); // true
    console.log(per1.constructor === Per); // true
{% endhighlight %}

每个对象实例都有一个内部属性：[[Prototype]]，其指向该对象的原型对象。构造函数本身也具有prototype属性指向原型对象。所有创建的对象都共享该原型对象的属性和方法。

### 五、继承

实现继承的两种方式：

* 利用[[Prototype]]特性，可以实现原型继承。

* 对于字面量形式的对象，会隐式指定Object.prototype为其[[Prototype]]。

* 通过Object.create()显示指定，其接受两个参数：第一个是[[Prototype]]指向的对象(原型对象)，第二个是可选的属性描述符对象。

* 利用构造函数。

（1）利用[[Prototype]]特性

（2）字面量形式的对象继承、（3）Object.create()显示指定

以下两种方式一样：

{% highlight bash %}
    // 字面量形式
    var book1 = {
        title: '这是书名'
    }
    // 通过Object.create()显示指定
    var book2 = Object.create(Object.prototype, {
        title: {
            configurable: true,
            enumerable: true,
            value: '这是书名',
            writable: true
        }
    })
{% endhighlight %}

字面量对象会默认继承自Object，更有趣的用法是，在自定义对象之间实现继承。

{% highlight bash %}
    var book1 = {
        title: 'JS高级程序设计',
        getTitle: function(){
            console.log(this.title);
        }
    }

    var book2 = Object.create(book1, {
        title: {
            configurable: true,
            enumerable: true,
            value: 'JS权威指南',
            writable: true
        }
    })

    book1.getTitle(); // 'JS高级程序设计'
    book2.getTitle(); // 'JS权威指南'

    console.log(book1.hasOwnProperty('getTitle')); // true
    console.log(book1.isPrototypeOf('book2')); // false
    console.log(book2.hasOwnProperty('getTitle')); // false

{% endhighlight %}

当访问book2的getTitle属性时，JavaScript引擎会执行一个搜索过程：现在book2的自有属性中寻找，找到则使用，若没有找到，则搜索[[Prototype]]，若没有找到，则继续搜索原型对象的[[Prototype]]，直到继承链末端。末端通常是Object.prototype，其[[Prototype]]被设置为null。

（4）利用构造函数

实现继承的另外一种方式是利用构造函数。每个函数都具有可写的prototype属性，默认被自动设置为继承自Object.prototype,可以通过改写它来改变原型链。

{% highlight bash %}
    function Rect(length, width){
        this.length = length;
        this.width = width;
    }

    Rect.prototype.getArea = function(){
        return this.width * this.length;
    }

    Rect.prototype.toString = function(){
        return '[Rect'+this.length+'\*'+this.width+']';
    }

    function Square(size){
        this.length = size;
        this.width = size;
    }

    // 修改prototype属性
    Square.prototype = new Rect();
    Square.prototype.constructor = Square;
    Square.prototype.toString = function(){
        return '[Square'+this.length+'\*'+this.width+']'';
    }

    var rect = new Rect(5, 10);
    var square = new Square(6);

    console.log(rect.getArea()); // 50
    console.log(square.getArea()); 36

{% endhighlight %}

如果要访问父类的toString()，可以这样做：

{% highlight bash %}
    Square.prototype.toString = function(){
        var text = Rect.prototype.toString.call(this);
        return text.replace('Rect', 'Square');
    }
{% endhighlight %}
