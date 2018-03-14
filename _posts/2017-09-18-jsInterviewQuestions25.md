---
layout:     post
title:		25个js面试题
subtitle:   "面试题"
date:       2017-09-18
author:     "handsameliu"
header-img: "img/post-bg-js-version.jpg"
tags:
  - 总结
  - 面试题
---

http://blog.csdn.net/fengyinchao/article/details/50456497   可参考这篇文章，比我写的好

https://mp.weixin.qq.com/s/qdn_13vADwl56wNRXz4m-w  我参考的是这篇，有些题都不全


> 1、使用 typeof bar === "object" 来确定 bar 是否是对象的潜在陷阱是什么？如何避免这个陷阱？

如果bar是null的话 结果也是正确的，因为null也是object，而object.prototype也是null

> 2、以下的输出是什么？

```javascript
(
	function(){
		var a = b = 3;   
	}
)();   
console.log("a defined? " + (typeof a !== 'undefined'));   
console.log("b defined? " + (typeof b !== 'undefined'));
```

结果是：

```
	a defined? false
	b defined? true
```

因为只有变量a被直接声明，变量b时没有被声明，那么在非严格模式下b就是一个全局变量了(严格模式下这种赋值是错误的，直接报错)。而a所在的函数又被自运行函数包裹，所以由于作用域原因a是无法获取到的，即为undefined。

而且此处的连续赋值`var a = b = 3;`并非是

```
var b = 3;
var a = b;
```

而是

```
b = 3;   var a = b;
(严格模式下这种赋值是错误的，直接报错)
```

> 3、下面的代码将输出什么到控制台，为什么？

```javascript
var myObject = {       
	foo: "bar",       
	func: function() {           
		var self = this;           
		console.log("outer func:  this.foo = " + this.foo);           
		console.log("outer func:  self.foo = " + self.foo);           
		(console.log("inner func:  this.foo = " + this.foo);
			function() {               
			console.log("inner func:  this.foo = " + this.foo);               
			console.log("inner func:  self.foo = " + self.foo);           
		}());
	}
};   
myObject.func();
```

输出结果为：

```
outer func:  this.foo = bar
outer func:  self.foo = bar
inner func:  this.foo = undefined
inner func:  self.foo = bar
```

这道题也是作用域问题。首先，在调用myObject.func()时，其方法的this便指向了myObject自身的this。再有方法func中一开始就声明了一个self的变量，将this赋值给了self。自然前两个输出的结果肯定是一样的。代码继续向下运行是一个代码块，这个代码块本身是一个自运行函数，这就可想而知了，又是代码块又是自运行函数，他的this肯定被隔离了，此时自运行函数的this是指向自己的，所以第三个输出就是undefined，第四个输出由于使用了self变量所以还是可以正常输出。

> 4、封装JavaScript源文件的全部内容到一个函数块有什么意义及理由？

看到这个问题，第一个想到的就是防止变量污染，设立一个独立的作用域（独立的命名空间）后，不管是this还是变量名就都是当前作用域下的，不会受到其他文件内变量名冲突的影响。

> 5、在JavaScript源文件的开头包含 use strict 有什么意义和好处？

use strict代表当前文件采用的是严格模式加载和运行，防止未经声明的全局变量，this指向当前作用域，如果this中的属性没有就会报错，这样有利于提高编写代码人员的技术水品，杜绝代码错误及瑕疵。

严格模式的一些主要优点包括：

- 使调试更加容易。那些被忽略或默默失败了的代码错误，会产生错误或抛出异常，因此尽早提醒你代码中的问题，你才能更快地指引到它们的源代码。
- 防止意外的全局变量。如果没有严格模式，将值分配给一个未声明的变量会自动创建该名称的全局变量。这是JavaScript中最常见的错误之一。在严格模式下，这样做的话会抛出错误。
- 消除 this 强制。如果没有严格模式，引用null或未定义的值到 this 值会自动强制到全局变量。这可能会导致许多令人头痛的问题和让人恨不得拔自己头发的bug。在严格模式下，引用 null或未定义的 this 值会抛出错误。
- 不允许重复的属性名称或参数值。当检测到对象（例如，var object = {foo: "bar", foo: "baz"};）中重复命名的属性，或检测到函数中（例如，function foo(val1, val2, val1){}）重复命名的参数时，严格模式会抛出错误，因此捕捉几乎可以肯定是代码中的bug可以避免浪费大量的跟踪时间。
- 使eval() 更安全。在严格模式和非严格模式下，eval() 的行为方式有所不同。最显而易见的是，在严格模式下，变量和声明在 eval() 语句内部的函数不会在包含范围内创建（它们会在非严格模式下的包含范围中被创建，这也是一个常见的问题源）。
- 在 delete使用无效时抛出错误。delete操作符（用于从对象中删除属性）不能用在对象不可配置的属性上。当试图删除一个不可配置的属性时，非严格代码将默默地失败，而严格模式将在这样的情况下抛出异常。

> 6、考虑以下两个函数。它们会返回相同的东西吗？ 为什么相同或为什么不相同？

```javascript
function foo1(){
	return{         
		bar: "hello"     
	};   
}   
function foo2(){     
	return
	{         
		bar: "hello"     
	};   
}
console.log("foo1 returns:");   
console.log(foo1());   
console.log("foo2 returns:");   
console.log(foo2());
```

结果是第一个输出一个object，第二个输出undefind。因为ECMAScript设定的ASI，return后会自动追加;分号。导致相当于没有返回。return后不要换行写其他东西，都不会执行。

> 7、NaN 是什么？它的类型是什么？你如何可靠地测试一个值是否等于 NaN ？

NaN代表这不是一个数字，但他是一个数字类型。一般是非数字类型使用数字运算才会返回此类型。NaN===NaN的结果是false，这是肯定的因为从字面意思看非数字怎么可能等于非数字呢？天知道这个非数字到底是个啥。。。我们一般使用isNaN()来检测是否为NaN。另外，ES6提供了一个新的 Number.isNaN() 函数，这是一个不同的函数，并且比老的全局 isNaN() 函数更可靠。


> 8、下列代码将输出什么？并解释原因。

```javascript
console.log(0.1 + 0.2);   console.log(0.1 + 0.2 == 0.3);
```

第一眼看到这题想到的就是js浮点型运算的问题了，因为js浮点型运行会出现浮点精度问题

	结果是：0.30000000000000004   false

> 9、讨论写函数 isInteger(x) 的可能方法，用于确定x是否是整数。

想到的解决方法如同源网页作者给出的结果，但是代码还是写多了一点

```javascript
// 作者给出的最稳健的方法 位运算
function isInteger(x) { return (x^0) === x; }
// 并不优雅但是同样能算出的方法 四舍五入
function isInteger(x) { return Math.round(x) === x; }
// Math.ceil() 和 Math.floor() 在上面的实现中等同于 Math.round()。
// 第三种方式 取余
function isInteger(x) { return (typeof x === 'number') && (x % 1 === 0);
```

```javascript
// 不正确的方式
function isInteger(x) { return parseInt(x, 10) === x; }
// 虽然这个以 parseInt函数为基础的方法在 x 取许多值时都能工作良好，但一旦 x 取值相当大的时候，就会无法正常工作。问题在于 parseInt() 在解析数字之前强制其第一个参数到字符串。因此，一旦数目变得足够大，它的字符串就会表达为指数形式（例如， 1e+21）。因此，parseInt() 函数就会去解析 1e+21，但当到达 e字符串的时候，就会停止解析，因此只会返回值 1。注意：
String(1000000000000000000000)   '1e+21'   
parseInt(1000000000000000000000, 10)   1   
parseInt(1000000000000000000000, 10) === 1000000000000000000000   
false
```

```javascript
// 我写出的方法，并不优雅
function isInt(val){
	if(typeof val == 'number' || !isNaN(val)){
		return Math.floor(val) === val;
	}else{
		return false;
	}
}
```

	http://www.w3school.com.cn/js/pro_js_operators_bitwise.asp 顺便补了补运算符

> 10、下列代码行1-4如何排序，使之能够在执行代码时输出到控制台？ 为什么？

```
(function() {       
	console.log(1);
	setTimeout(function(){
		console.log(2);
	},1000);
	setTimeout(function(){
		console.log(3);
	}, 0);
	console.log(4);
})();
```

一眼看到后反应是1432，因为使用setTimeout后就进入了异步队列，在函数中的1和4就被输出了，而2和3就会延迟输出了，3所在的定时器延迟是0秒，所以3先输出。

```
要回答这个问题，你需要正确理解JavaScript的事件和时间设置。

浏览器有一个事件循环，会检查事件队列和处理未完成的事件。例如，如果时间发生在后台（例如，脚本的 onload 事件）时，浏览器正忙（例如，处理一个 onclick），那么事件会添加到队列中。当onclick处理程序完成后，检查队列，然后处理该事件（例如，执行 onload 脚本）。

同样的， setTimeout() 也会把其引用的函数的执行放到事件队列中，如果浏览器正忙的话。

当setTimeout()的第二个参数为0的时候，它的意思是“尽快”执行指定的函数。具体而言，函数的执行会放置在事件队列的下一个计时器开始。但是请注意，这不是立即执行：函数不会被执行除非下一个计时器开始。这就是为什么在上述的例子中，调用 console.log(4) 发生在调用 console.log(3) 之前（因为调用 console.log(3) 是通过setTimeout被调用的，因此会稍微延迟）。
```

> 11、写一个简单的函数（少于80个字符），要求返回一个布尔值指明字符串是否为回文结构。

```javascript
// 回文结构是顺读和反读都一样的序列

function isPalindrome(str) {
	str = str.replace(/W/g, '').toLowerCase();
	return (str == str.split('').reverse().join(''));   
}
```

```javascript
console.log(isPalindrome("level"));               // logs 'true'   
console.log(isPalindrome("levels"));              // logs 'false'   
console.log(isPalindrome("A car, a man, a maraca"));  // logs 'true'
```

> 12、写一个 sum方法，在使用下面任一语法调用时，都可以正常工作。

幸好之前看过一篇关于柯理化函数的文章，所以看到这题还是有思路的，但是第一次写失败了。这里就不上代码了，因为和方法1是一样的。。

```javascript
// 方法1
function sum(x) {     
	if (arguments.length == 2) {       
		return arguments[0] + arguments[1];     
	} else {       
		return function(y) { 
			return x + y; 
		};     
	}   
}
```

	在JavaScript中，函数可以提供到 arguments 对象的访问，arguments 对象提供传递到函数的实际参数的访问。这使我们能够使用 length 属性来确定在运行时传递给函数的参数数量。

	如果传递两个参数，那么只需加在一起，并返回。

	否则，我们假设它被以 sum(2)(3)这样的形式调用，所以我们返回一个匿名函数，这个匿名函数合并了传递到 sum()的参数和传递给匿名函数的参数。

```javascript
// 方法2
function sum(x, y) {     
	if (y !== undefined) {       
		return x + y;     
	} else {       
		return function(y) { 
			return x + y; 
		};     
	}   
}
```

	当调用一个函数的时候，JavaScript不要求参数的数目匹配函数定义中的参数数量。如果传递的参数数量大于函数定义中参数数量，那么多余参数将简单地被忽略。另一方面，如果传递的参数数量小于函数定义中的参数数量，那么缺少的参数在函数中被引用时将会给一个 undefined值。所以，在上面的例子中，简单地检查第2个参数是否未定义，就可以相应地确定函数被调用以及进行的方式。

> 13.请看下面的代码片段：

```javascript
for (var i = 0; i < 5; i++) {        
	var btn = document.createElement('button');        
	btn.appendChild(document.createTextNode('Button ' + i));        
	btn.addEventListener('click', function(){ console.log(i); });        
	document.body.appendChild(btn);      
}
```

	（a）当用户点击“Button 4”的时候会输出什么到控制台，为什么？（b）提供一个或多个备用的可按预期工作的实现方案。

```
我的答案：

第一个问题根据for循环这个i在循环完成后结果为5，所以无论点击哪个button打印的结果都是5，因为点击的时候循环已经结束了，而click函数中又没有i这个变量，所以会向上级作用域找变量i。

增加闭包，或者使用let申明变量。
```
	
官方答案

（a）无论用户点击什么按钮，数字5将总会输出到控制台。这是因为，当 onclick 方法被调用（对于任何按钮）的时候， for 循环已经结束，变量 i 已经获得了5的值。（面试者如果能够谈一谈有关如何执行上下文，可变对象，激活对象和内部“范围”属性贡有助于闭包行为，则可以加分）。

（b）要让代码工作的关键是，通过传递到一个新创建的函数对象，在每次传递通过 for 循环时，捕捉到 i 值。下面是三种可能实现的方法：

```
for (var i = 0; i < 5; i++) {     
	var btn = document.createElement('button');     
	btn.appendChild(document.createTextNode('Button ' + i));     
	btn.addEventListener('click', (function(i) {       
		return function() { console.log(i); }; }
	)(i));     
	document.body.appendChild(btn);   
}
```


```
for (var i = 0; i < 5; i++) {     
	var btn = document.createElement('button');     
	btn.appendChild(document.createTextNode('Button ' + i));     
	(function (i) {       
		btn.addEventListener('click', function() { console.log(i); });
	})(i);     
	document.body.appendChild(btn);   
}

```

```
['a', 'b', 'c', 'd', 'e'].forEach(function (value, i) {     
	var btn = document.createElement('button');     
	btn.appendChild(document.createTextNode('Button ' + i));     
	btn.addEventListener('click', function() { console.log(i); });     
	document.body.appendChild(btn);   
});

```

> 14.下面的代码将输出什么到控制台，为什么？

```
var arr1 = "john".split('');   
var arr2 = arr1.reverse();   
var arr3 = "jones".split('');   
arr2.push(arr3);   
console.log("array 1: length=" + arr1.length + " last=" + arr1.slice(-1));   
console.log("array 2: length=" + arr2.length + " last=" + arr2.slice(-1));
```

看到这道题想到的是数组之间的赋值。由于是引用类型，所以赋值其实是指向地址。操作arr2其实就是操作arr1。这两个的输出其实是一样的。

官方答案

"array 1: length=5 last=j,o,n,e,s"   
"array 2: length=5 last=j,o,n,e,s"

arr1 和 arr2 在上述代码执行之后，两者相同了，原因是：

调用数组对象的 reverse() 方法并不只返回反顺序的阵列，它也反转了数组本身的顺序（即，在这种情况下，指的是 arr1）。
 reverse() 方法返回一个到数组本身的引用（在这种情况下即，arr1）。其结果为，arr2 仅仅是一个到 arr1的引用（而不是副本）。因此，当对 arr2做了任何事情（即当我们调用 arr2.push(arr3);）时，arr1 也会受到影响，因为 arr1 和 arr2 引用的是同一个对象。

这里有几个侧面点有时候会让你在回答这个问题时，阴沟里翻船：

传递数组到另一个数组的 push() 方法会让整个数组作为单个元素映射到数组的末端。其结果是，语句 arr2.push(arr3); 在其整体中添加 arr3 作为一个单一的元素到 arr2 的末端（也就是说，它并没有连接两个数组，连接数组是 concat() 方法的目的）。

和Python一样，JavaScript标榜数组方法调用中的负数下标，例如 slice() 可作为引用数组末尾元素的方法：例如，-1下标表示数组中的最后一个元素，等等。

> 15、下面的代码将输出什么到控制台，为什么？

```javascript
console.log(1 +  "2" + "2");   
console.log(1 +  +"2" + "2");   
console.log(1 +  -"1" + "2");   
console.log(+"1" +  "1" + "2");   
console.log( "A" - "B" + "2");   
console.log( "A" - "B" + 2);
```

我的结果是 122,32,02,112,NaN2,NaN，和标答是一样的。

首先数字加字符串得到的会是一个字符串，而第一个正好符合，所以结果是122。当字符前面有+-号的时候会做运算，如果字符串是数字那么就会变为数字类型，所以第二个是32 第三个是02。同理第四个符合第一个和第二个的标准，所以第四个结果是112。当两个不是数字类型的字符进行运算的话得到的结果会是NaN，而第五个又是NaN+'2'所以数字类型与字符类型相加得到的还是一个字符串类型NaN2。第六个是两个字符类型相减所以结果是NaN，最后一个数是数字类型，而NaN也是一个数字类型，所以两个是可以相加的，但是结果肯定是NaN。

官方答案：

```
原因是…

这里的根本问题是，JavaScript（ECMAScript）是一种弱类型语言，它可对值进行自动类型转换，以适应正在执行的操作。让我们通过上面的例子来说明这是如何做到的。

例1：1 + "2" + "2" 输出："122" 说明： 1 + "2" 是执行的第一个操作。由于其中一个运算对象（"2"）是字符串，JavaScript会假设它需要执行字符串连接，因此，会将 1 的类型转换为 "1"， 1 + "2"结果就是 "12"。然后， "12" + "2" 就是 "122"。

例2： 1 + +"2" + "2" 输出： "32" 说明：根据运算的顺序，要执行的第一个运算是 +"2"（第一个 "2" 前面的额外 + 被视为一元运算符）。因此，JavaScript将 "2" 的类型转换为数字，然后应用一元 + 号（即，将其视为一个正数）。其结果是，接下来的运算就是 1 + 2 ，这当然是 3。然后我们需要在一个数字和一个字符串之间进行运算（即， 3 和 "2"），同样的，JavaScript会将数值类型转换为字符串，并执行字符串的连接，产生 "32"。

例3： 1 + -"1" + "2" 输出： "02"  说明：这里的解释和前一个例子相同，除了此处的一元运算符是 - 而不是 +。先是 "1" 变为 1，然后当应用 - 时又变为了 -1 ，然后将其与 1相加，结果为 0，再将其转换为字符串，连接最后的 "2" 运算对象，得到 "02"。
例4： +"1" + "1" + "2" 输出： "112" 说明：虽然第一个运算对象 "1"因为前缀的一元 + 运算符类型转换为数值，但又立即转换回字符串，当连接到第二个运算对象 "1" 的时候，然后又和最后的运算对象"2" 连接，产生了字符串 "112"。

例5： "A" - "B" + "2" 输出： "NaN2" 说明：由于运算符 -  不能被应用于字符串，并且 "A" 和 "B" 都不能转换成数值，因此，"A" - "B"的结果是 NaN，然后再和字符串 "2" 连接，得到 "NaN2" 。

例6： "A" - "B" + 2 输出： NaN 说明：参见前一个例子， "A" - "B" 结果为 NaN。但是，应用任何运算符到NaN与其他任何的数字运算对象，结果仍然是 NaN。

```

> 16、下面的递归代码在数组列表偏大的情况下会导致堆栈溢出。在保留递归模式的基础上，你怎么解决这个问题？

```javascript
var list = readHugeList();   
var nextListItem = function() {
	var item = list.pop();       
	if (item) {           
		// process the list item...           
		nextListItem();       
	}   
};
```

官方答案：

潜在的堆栈溢出可以通过修改nextListItem 函数避免：

```javascript
var list = readHugeList();   
var nextListItem = function() {       
var item = list.pop();       
	if (item) {           
		// process the list item...           
		setTimeout( nextListItem, 0);       
	}   
};

```

堆栈溢出之所以会被消除，是因为事件循环操纵了递归，而不是调用堆栈。

当 nextListItem 运行时，如果 item不为空，timeout函数（nextListItem）就会被推到事件队列，该函数退出，因此就清空调用堆栈。当事件队列运行其timeout事件，且进行到下一个 item 时，定时器被设置为再次调用 nextListItem。因此，该方法从头到尾都没有直接的递归调用，所以无论迭代次数的多少，调用堆栈保持清空的状态。


> 17、JavaScript中的“闭包”是什么？请举一个例子。

官方答案：

闭包是一个可以访问外部（封闭）函数作用域链中的变量的内部函数。闭包可以访问三种范围中的变量：这三个范围具体为：（1）自己范围内的变量，（2）封闭函数范围内的变量，以及（3）全局变量。

下面是一个简单的例子：

```javascript
var globalVar = "xyz";   
(function outerFunc(outerArg) {     
	var outerVar = 'a';     
	(function innerFunc(innerArg) {       
		var innerVar = 'b';       
		console.log("outerArg = " + outerArg + "" +"innerArg = " + innerArg + "" +"outerVar = " + outerVar + "" +"innerVar = " + innerVar + "" +"globalVar = " + globalVar);
	})(456);   
})(123);

```

在上面的例子中，来自于 innerFunc， outerFunc和全局命名空间的变量都在 innerFunc的范围内。因此，上面的代码将输出如下：

```
outerArg = 123   innerArg = 456   outerVar = a   innerVar = b   globalVar = xyz
```

> 18、下面的代码将输出什么：

官方答案如下，因为网页上并没有题。。。

解释你的答案。闭包在这里能起什么作用？

上面的代码不会按预期显示值0，1，2，3，和4，而是会显示5，5，5，5，和5。
原因是，在循环中执行的每个函数将整个循环完成之后被执行，因此，将会引用存储在 i中的最后一个值，那就是5。

闭包可以通过为每次迭代创建一个唯一的范围，存储范围内变量的每个唯一的值，来防止这个问题，如下：

```
for (var i = 0; i < 5; i++) {       
	(function(x) {
		setTimeout(function(){
		console.log(x); }, x * 1000 );
	})(i);
}
```

这就会按预期输出0，1，2，3，和4到控制台。

> 19、以下代码行将输出什么到控制台？

```javascript
console.log("0 || 1 = "+(0 || 1));   
console.log("1 || 2 = "+(1 || 2));   
console.log("0 && 1 = "+(0 && 1));   
console.log("1 && 2 = "+(1 && 2));
```

输出结果是：0 || 1 = 1，1 || 2 = 1，0 && 1 = 0，1 && 2 = 2

在JavaScript中， || 和 &&都是逻辑运算符，用于在从左至右计算时，返回第一个可完全确定的“逻辑值”。

或（ || ）运算符。在形如 X||Y的表达式中，首先计算X 并将其解释执行为一个布尔值。如果这个布尔值true，那么返回true（1），不再计算 Y，因为“或”的条件已经满足。如果这个布尔值为false，那么我们仍然不能知道 X||Y是真是假，直到我们计算 Y，并且也把它解释执行为一个布尔值。

因此， 0 || 1 的计算结果为true（1），同理计算1 || 2。

与（ &&）运算符。在形如 X&&Y的表达式中，首先计算 X并将其解释执行为一个布尔值。如果这个布尔值为 false，那么返回 false（0），不再计算 Y，因为“与”的条件已经失败。如果这个布尔值为true，但是，我们仍然不知道 X&&Y 是真是假，直到我们去计算 Y，并且也把它解释执行为一个布尔值。

不过，关于 &&运算符有趣的地方在于，当一个表达式计算为“true”的时候，那么就返回表达式本身。这很好，虽然它在逻辑表达式方面计算为“真”，但如果你希望的话也可用于返回该值。这就解释了为什么，有些令人奇怪的是， 1 && 2返回 2（而不是你以为的可能返回 true 或 1）。

> 20、执行下面的代码时将输出什么？请解释。

```javascript
console.log(false == '0')
console.log(false === '0')
```

输出结果是：true，false

很简单，双等于不会比较类型，字符串0会被转成数字，两个false结果就是true了。三等于会判断两边变量的类型，所以为false。

官方答案：

在JavaScript中，有两种等式运算符。三个等于运算符 === 的作用类似传统的等于运算符：如果两侧的表达式有着相同的类型和相同的值，那么计算结果为true。而双等于运算符，会只强制比较它们的值。因此，总体上而言，使用 ===而不是 ==的做法更好。 !==vs !=亦是同理。

> 21、以下代码将输出什么？并解释你的答案。

```javascript
var a={},b={key:'b'},c={key:'c'};
a[b]=123;
a[c]=456;
console.log(a[b]);
```

之前遇到过，key不能是对象，所以会被转成字符串，对象转成字符串后为[object object]，所以b和c转换后得到的值时一样的，而重复给a[b],a[c]赋值的话 相当于给同一个key赋值了，所以结果为456.

官方答案：

这段代码将输出 456（而不是 123）。

原因为：当设置对象属性时，JavaScript会暗中字符串化参数值。在这种情况下，由于 b 和 c都是对象，因此它们都将被转换为"[object Object]"。结果就是， a[b]和a[c]均相当于a["[object Object]"] ，并可以互换使用。因此，设置或引用 a[c]和设置或引用 a[b]完全相同。

> 22、以下代码行将输出什么到控制台？

```javascript
console.log((function f(n){return ((n > 1) ? n * f(n-1) : n)})(10));
```

这道题之前见到过，`阶乘` 10*9*8*7*6*5*4*3*2*1  结果就是 `3628800`

官方答案：

```javascript
f(1): returns n, which is 1   
f(2): returns 2 * f(1), which is 2   
f(3): returns 3 * f(2), which is 6   
f(4): returns 4 * f(3), which is 24   
f(5): returns 5 * f(4), which is 120   
f(6): returns 6 * f(5), which is 720   
f(7): returns 7 * f(6), which is 5040   
f(8): returns 8 * f(7), which is 40320   
f(9): returns 9 * f(8), which is 362880   
f(10): returns 10 * f(9), which is 3628800
```

命名函数 f()递归地调用本身，当调用 f(1)的时候，只简单地返回1。

其实就是一个立即执行函数+递归，求个阶乘而已（10!）。给立即执行函数加了个名字 f，方便在递归里调用，其实完全可以用arguments.callee 代替：

```
var ans = (function(n){
  return ((n > 1) ? n * arguments.callee(n-1) : n)
})(10);

console.log(ans);
```

> 23.请看下面的代码段。控制台将输出什么，为什么？

```javascript
(function(x) {       
	return (function(y) {           
		console.log(x);       
	})(2)   
})(1);
```

这道题懂一点闭包的就可以打出来，根据作用域输出为1。虽然return出了自执行函数，他的this是外层this，但是return后的函数当前作用域如果没有此变量他就会向上找。

> 24.下面的代码将输出什么到控制台，为什么：

```javascript
var hero = {       
	_name: 'John Doe',       
	getSecretIdentity: function (){           
		return this._name;       
	}   
};   
var stoleSecretIdentity = hero.getSecretIdentity;   
console.log(stoleSecretIdentity());   
console.log(hero.getSecretIdentity());
```

这道题考的是this作用域，第一个输出的this是window所以输出是undefined，第二个的this是hero，所以他会正常输出John Doe。

第一行之所以this是window，是因为他将hero.getSecretIdentity的值赋值给了window下的stoleSecretIdentity字面量，这个方法运行时window中并没有_name字段，所以报undefined。

第一个 console.log之所以输出 undefined，是因为我们正在从 hero对象提取方法，所以调用了全局上下文中（即窗口对象）的 stoleSecretIdentity()，而在此全局上下文中， _name属性不存在。

其中一种修复stoleSecretIdentity() 函数的方法如下：

```javascript
var stoleSecretIdentity = hero.getSecretIdentity.bind(hero);
```

> 25.创建一个给定页面上的一个DOM元素，就会去访问元素本身及其所有子元素（不只是它的直接子元素）的函数。对于每个被访问的元素，函数应该传递元素到提供的回调函数。

```
function Traverse(p_element,p_callback) {
   p_callback(p_element);
   var list = p_element.children;
   for (var i = 0; i < list.length; i++) {
       Traverse(list[i],p_callback);  // recursive call
   }
}
```















