---
layout:     post
title:		js的三座大山
date:       2018-03-26
author:     "handsameliu"
header-img: "img/post-bg-js-version.jpg"
tags:
  - 教程
  - javascript
  - 原型
  - 原型链
  - 作用域
  - 闭包
  - 异步
  - 单线程
---

> js的三座大山：
> 1.原型  原型链
> 2.作用域  闭包
> 3.异步  单线程

### 一.原型  原型链

#### （1）构造函数

```javascript
function Foo(name, age){
	this.name = name
	this.age = age
	// return this
}
var f = new Foo('张三', 18)
var f1 = new Foo('李四', 19)
```

1. 构造函数需要以`大写字母开头`，提高了代码的可读性；

2. `return this` 这句可有可无，因为他是默认存在的;

构造函数实例化过程：首先创建一个空对象，再让this指向这个对象，然后执行代码，即给this.name赋值，最后返回this，并将值赋给f和f1.

#### （2）构造函数扩展

1. 所有的应用类型都有构造函数

var a = {} 其实是var a = new Object() 的语法糖

var b = [] 其实是var b = new Array() 的语法糖

function Foo(){...} 其实是 var Foo = new Function() 的语法糖

2. 可以使用instanceof判断一个函数知否是一个变量的函数构造，例如

```javascript
var a = new Array()
console.log(a instanceof Array)		// true
``` 

#### （3）原型规则和示例

在javascript中所有的对象都是Object的实例，并继承自Object.prototype的属性和方法，有些属性是隐藏的。换句话说，在对象创建时会存在预定义的属性，其中有一个属性就是原型对象。

原型规则：

1. 所有引用类型（数组，对象，函数）都具有对象性，即可自由扩展属性（除了null）

```javascript
var obj = {}
obj.a = 100
var arr = []
arr.a = 100
function fn1 () {}
fn1.a = 100
```

2. 所有的引用类型（数组，对象，函数）都有一个__proto__（隐式原型）属性，属性值也是一个普通的对象

```javascript
console.log(obj.__proto__);
console.log(arr.__proto__);
console.log(fn1.__proto__);
```

3. 所有的函数，都有一个prototype（显示原型）属性，属性值也是一个普通的对象

```javascript
console.log(fn1.prototype);
```

4. 所有引用类型（数组，对象，函数）的__proto__属性值都指向他的构造函数的protorype属性值

```javascript
console.log(obj.__proto__ === Object.prototype);        
console.log(arr.__proto__ === Array.prototype);   
console.log(fn1.__proto__ === Function.prototype);
```

5. 当视图得到一个对象的属性时，如果这个对象本身没有这个属性，那么就会去他的__proto__（即构造函数的prototype）中寻找

```javascript
//构造函数
function Foo(name) {
    this.name = name;
}
Foo.prototype.alertName = function () {
    alert(this.name);
}
//创建实例
var f = new Foo("xiaohui");
f.printName = function () {
    console.log(this.name);
}
//测试
f.printName();
f.alertName();
```

有些时候我们需要循环对象当中的属性，那么久需要用到hasOwnProperty方法了

```javascript
for (var item in f) {

    //高级浏览器已经在for in中屏蔽了来自原型的属性
	
    //但是还是加上判断，保证程序的健壮性
    if (f.hasOwnProperty(item)) {
        console.log (item);
    }
}
```

如果现在我想要得到f.toString()，我们会发现f和Foo都没有这个方法，于是就去f._proto._proto（Foo.prototype._proto）中寻找，此时找到并调用。

### 二.作用域  闭包

#### （1）执行上下文

每个函数都有自己的执行上下文EC(执行环境execution context)， 并且每个执行上下文中都有它自己的变量对象VO(Variable object)，用于存储执行上下文中的变量、函数申明、函数参数、这解释了JS如何找到我们定义的函数和变量。并且函数是JS中唯一一个能创建出作用域的，（注意：for if else不能创建作用域）

```javascript
console.log(a)				// undefined
var a = 10
```

这里会打印出`undefined`，这是因为在代码还没有执行之前，a已经被提前申明，此时的代码可以理解为：

```javascript
var a
console.log(a)		// undefined
a = 10;
```

代码执行到console.log(a)时a只是声明了，并没有被赋值，所以结果为undefined。

```javascript
fn('张三',20)
function fn(name){
	age = 20
	console.log(name, age)		// 张三 20
	var age
}
```

此时打印的结果为 “张三” 20。可以将代码理解为

```
function fn(name){
	var age
	age = 20
	console.log(name, age)		// 张三 20
}
fn('张三', 20)
```

无论是变量还是函数，在各自的作用域内都进行了变量提升。

一段<script></script>或者一个函数都有自己的作用域所以都会生成执行上下文。
针对一段<script></script>会生成全局执行上下文，在执行之前先将变量定义和函数定义声明提前。
针对一个函数会生成一个函数执行上下文，在函数执行之前，将函数中的变量定义，函数声明，this，arguments取出。

但要注意的是函数声明和函数表达式的区别

```javascript
function fn(){}				// 函数声明
var fn = function (){}		// 函数表达式
```

#### （2）this

从本质来说，js中的this是一个纸箱函数执行环境的指针。this在哪个函数里面，就指向这个函数的context，this永远指向最后调用他的对象，并且this要在执行时才能确定其值，定义时无法确认。

```javascript
var a = {
	name: 'A',
	fn: function () {
		console.log(this.name)
	}
}
a.fn()						// this === a
a.fn.call({name: 'B'})		// this === {name: 'B'}
var fn1 = a.fn;	
fn1()						// this === window
```

第一个执行函数，是a调用了自身的fn方法，所以此时this就是a。第二个执行函数，使用了call，将this改为了传入的{name: 'B'}。第三个执行函数，将a.fn方法赋值给fn1，执行时确实在window下执行，所以此时this为window。

this其实有好几种使用场景，在这里我们主要介绍其中四种

1. 作为构造函数使用

```javascript
function Foo (name, age) {
	this.name = name		// this === f
	this.age = age			// this === f
	// return this
}
var f = new Foo('张三', 10)
```

new操作符会改变函数this的指向，将this指向创建的实例f。那么为什么this会指向f？首先new关键字会创建一个空的对象，然后会自动调用函数apply方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象替代。

值得注意的是，如果手动给当前构造函数返回一个对象{}或一个函数，那么new出来的实例将会被返回的对象或函数替代。这么做的好处是可以暴露我们想要的东西，其他东西可以在构造函数中保存（非this绑定，直接var的变量），避免污染（函数作用域特性，向父级查找变量。某些后台语言中也会有这样的情况）。

```javascript
// 若构造函数中返回this或返回值是基本类型（number、string、boolean、null、undefined）之一，则返回新实例对象；虽然null也是对象，但返回null还是会指向构造函数的新实例。
// 若返回值是引用类型的值，则实际返回值为这个引用类型。

function Fn () {
	this.user = 'jj'
	return {}
}
var a = new Fn()
console.log(a.user)		// undefined

function Fn () {
	this.user = 'jj'
	return function (){}
}
var a = new Fn()
console.log(a.user)		// undefined

function Fn () {
	this.user = 'jj'
	return 1
}
var a = new Fn()
console.log(a.user)		// jj

function Fn () {
	this.user = 'jj'
	return null
}
var a = new Fn()
console.log(a.user)		// jj
```

在严格模式下'use strict'中默认的this不再是window，而是`undefined`

2. 作为普通函数执行

```javascript
function fn(){
	console.log(this)		
}
fn()					// this === window
```

3. 作为对象属性执行

```javascript
var obj = {
	name: 'A',
	printName: function () {
		console.log(this.name)
	}
}
obj.printName()			// this === obj
```

4. call,apply,bind

这三种方法都可以改变函数体内部this的指向，但是又存在一些差异。

```javascript
var name = '小明', age = '18'
var obj = {
	name: '安妮',
	objAge: this.age,
	fn: function () {
		console.log(this.name + '今年' + this.age)
	}
}
console.log(obj.objAge)			// 18
obj.fn()						// 安妮今年undefined
```

出现这样的结果是因为，在obj.objAge中，this的指向本该是obj，但是这句话并没有被函数包裹，所以这里的执行环境是全局的；在obj.fn()中，this的指向也是obj，但是obj中没有定义age，所以是age输出undefined。

```javascript
var name = 'jj'
function show () {
	console.log(this.name)
}
show()
```

函数作用域指向window，所以打印jj。

```javascript
var name = '小明', age = '18'
var obj = {
	name: '安妮',
	objAge: this.age,
	fn: function () {
		console.log(this.name + '今年' + this.age)
	}
}

var a = {
	name: 'jj',
	age: 20
}

obj.fn.call(a)			// jj今年20
obj.fn.apply(a)			// jj今年20
obj.fn.bind(a)			// jj今年20	 这里不会直接运行输出，必须调用才能执行
```

再看一下传参的情况：

```javascript
var name = '小明', age = '18'
var obj = {
	name: '安妮',
	objAge: this.age,
	fn: function (like, dislike) {
		console.log(this.name + '今年' + this.age, '喜欢吃' + like + '不喜欢' + dislike)
	}
}

var a = {
	name: 'jj',
	age: 20
}

obj.fn.call(a, '苹果', '香蕉')			// jj今年20 喜欢吃苹果不喜欢香蕉
obj.fn.apply(a, ['苹果', '香蕉'])		// jj今年20 喜欢吃苹果不喜欢香蕉
obj.fn.bind(a, '苹果', '香蕉')			// jj今年20 喜欢吃苹果不喜欢香蕉
```

call,apply,bind 这三个方法的第一个参数都是要改变的this指向对象，主意如果call和apply的第一个参数写的是null，那么this指向的是window对象；第二个参数就有区别了

①call的参数是直接放入，用逗号分隔（call算是apply的语法糖）；

②apply的所有参数都是放在一个数组中传递；

③bind参数和call一样，但是返回的是函数且不能立即执行。

call和apply都是改变上下文中的this并立即执行这个函数，bind方法可以让对应的函数在想要调用的时候执行，并且参数可以在执行时添加。

在非严格模式下，call和apply的第一个参数如果指定为null或undefined，会自动指向全局对象。

#### （3）作用域

什么是作用域？ 作用域是针对变量的，一个变量在某个区域内起作用，这份区域就是他的作用域。

在js中不存在块级作用域，只有函数作用域和全局作用域。下面是很经典的例子

```javascript
if(true){
	var name = 'jj'
}
console.log(name)		// jj
```

在高级语言中这样写会报错，但在js中，这么写不会报错，因为js没有块级作用域，但是也不推荐这样写。

```javascript
var a = 1
function fn(){
	var a = 2
	console.log('fn', a)
}
console.log('global', a)		// 1	全局作用域
fn()							// 2	函数作用域
```

#### （4）作用域链

内部函数可以访问外部函数变量的机制，用链式查找决定哪些数据能被内部函数访问。

在高程三种是这么描述的：`当代码在一个环境中执行时，会创建变量对象的一个作用域链。作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问。`作用域链的前端始终是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象。活动对象在最开始时只包含一个变量，即arguments对象。作用域链的下一个变量对象来自包含环境，而在下一个变量对象则来着下一个包含环境。这样一直延续到全局执行环境；全局执行的变量对象始终都是作用域链中的最后一个对象。

这就又回到了执行环境和变量对象的概念。继而又有一个新概念`自由变量`，即当前作用域没有定义的变量。

```javascript
var a = 1
function fn () {
	var b = 2
	console.log(a)
	console.log(b)
}
fn()
```

在console.log(a)中的a就是自由变量，在当前作用域中并没有定义a，所以要去函数定义时的父级作用域中找，直到顶级window作用域。

```javascript
var a = 1
function fn () {
	var b = 2
	function fn1 () {
		var c = 3
		console.log(a)
		console.log(b)
		console.log(c)
	}
	fn1()
}
fn()
```

在执行console.log(a)时，由于当前作用域没有定义a，所以要去fn在定义时的父级作用域找，此时他的父级作用域是fn，fn中也没有定义a，所以再去fn在定义时的父级作用域（即全局作用域）中找，此时就能找到a。b、c也是同理。上述过程就形成了一条作用域链，简单来说作用域链就是自由变量的查找，子对象会一级一级的向上寻找父对象的变量。

而当内部函数的作用域链任然保持对父级函数活动对象的引用，就是`闭包(closure)`

#### （5）闭包

函数在执行时产生一个封闭的作用域，这个封闭的作用域就是闭包，闭包是指有权访问另一个函数作用域中的变量的函数。

```javascript
// 这就是一个闭包
function fn(){}
fn()

// 这只是闭包的一种形式, 这样使用的好处是避免变量冲突和污染，能够自动执行；闭包前后最好都要写分号，防止多文件继承或高压缩出现的语法错误。
;(function fn(){})()
```

闭包有两个作用：

1. 可以读取自身函数外部的变量（沿着作用域链寻找）

```javascript
function f1(){
	var n = 1
	function f2(){
		console.log(n)
	}
}
```

f2被包在函数f1内部，f1内的所有局部变量，对f2都是可见的。反过来则不行，f2内的局部变量对f1是不可见的。

2. 让这些外部变量始终保存在内存中

无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。但是闭包的情况有所不同。

使用闭包的注意点：

1. 由于闭包会使得函数中的变量被保存在内存中，内存小号很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决的方法是：在退出函数之前，将不是用的局部变量全部删除。

2. 闭包会在父函数外部，改变父函数内部变量的值。所以如果你把父函数当做对象（object）使用，吧闭包当做他的功用方法（public method），把内部变量当做他的私有属性（private value），这是一定要小心，不要随便改变父函数内部的值。

对于闭包经典的面试题都是循环绑定事件或输出了

```javascript
// 原题
for(var i=0;i<10;i++){
	var a = document.createElement('a')
	a.innerHTML = i
	a.addEventListener('click', function(e){
		e.preventDefault()
		console.log(i)
	})
	document.bodu.appendChild(a);
}

// 解决方法1 闭包
for(var i=0;i<10;i++){
	var a = document.createElement('a')
	a.innerHTML = i
	(function (i) {
		a.addEventListener('click', function(e){
			e.preventDefault()
			console.log(i)
		})
	})(i)
	document.bodu.appendChild(a);
}


// 解决方法2 索引
for(var i=0;i<10;i++){
	var a = document.createElement('a')
	a.innerHTML = i
	a.index = i
	a.addEventListener('click', function(e){
		e.preventDefault()
		console.log(this.index)
	})
	document.bodu.appendChild(a);
}

```

作用域链与闭包总结

当代码在一个环境中执行时，都会创建一个作用域链。作用域链的用途是保证对执行环境有权访问的所有变量和函数有序访问。整个作用域链的本质是一个指向变量对象的指针列表。作用域链的最前端，始终是当前正在执行的代码所在的环境变量对象。

如果整个环境是函数，则将其活动对象（activation object）作为变量对象。活动对象在最开始时只包含一个变量，就是函数内部的arguments对象。作用域链中的下一个变量对象来自该函数的包含环境，而下一个变量对象来自再下一个包含对象。这样一直延续到全局执行环境，全局执行环境的变量对象始终是作用域链中的最后一个对象。

### 三.异步和单线程

#### （1）什么是异步

先说一下浏览器，在浏览器的一个页面中，该页面的js程序只有一个线程，这就是单线程。因为是单线程，所以程序的执行书序就是从上到下依次执行，同一时间内只能有一段代码被执行。虽然javascript是单线程的，但是浏览器内部不是单线程的。它扩展了一些IO操作、定时器、事件监听、http请求，都是由浏览器提供的多线程来完成。

由于javascript代码是自上而下执行的，当执行到IO操作、定时器、事件监听、http请求时，会自动触发对应事件，将回调函数存储到队列中。javascript执行完主程代码后，回去执行任务队列，队列内这个时间点是否有代码要执行；如果有则执行队列中的代码。

那么当异步任务完成后，主线程怎么知道呢？答案就是回调函数，整个程序是事件驱动的，每个事件都会绑定相应的回调函数。当代码执行到事件时，会将回调函数传给队列，该回调的事件就是事件触发的事件。当代码执行到定时器的时候，传入队列的回调函数的事件时定时器触发的事件加上延迟，而http请求传递给队列的回调函数的事件则是其取得数据的时间（onreadystatechange方法）这样就可以根据时间对队列进行进一步排序，形成一个自上而下的队列。

```javascript
console.log('start')
setTimeout(function(){
	console.log('setTimeout')
}, 1000)
consoole.log('end')
```

由于异步的原因，输出的结果是 start,end,setTimeout.

```javascript
console.log(1)
alert(2)
console.log(3)
```

由于同步的原因，阻塞线程了，所以必须要等alert点击确定后才能进行下一步，程序的执行顺序与任务的排列顺序是一致的、同步的；异步是异步代码会被放入一个事件队列，等到鄋其他代码执行后才进行，而不会阻塞线程。

#### （2）何时需要异步

1. 定时任务：setTimeout,setInterval

2. 网络请求：http的Ajax请求，动态img加载

3. 事件绑定

```javascript
// ajax如下

console.log('start')
$.ajax({
	type:'GET',
	url:'data.json',
	dataType:'json',
	success:function(data){
		console.log(data)
	}
})
console.log('end')

// img如下
console.log('start')
var img = document.createElement('img')
img.onload = function (){
	console.log('load')
}
img.src = '1.jpg'
console.log('end')

// 事件绑定如下
console.log('start')
document.getElementById('id').addEventListener('click',function(){
	console.log('clicked')
})
console.log('end')

```

#### （3）前端使用异步的场景

当我们的程序需要大量I/O操作和用户请求时，js时多么的应景。相比于其他多线程语言，他不必耗费多多的系统开销，同事也不必把精力用于处理多线程管理，相比同步执行的语言，宿主环境的异步和事件驱动机制又让它实现了非阻塞I/O。

