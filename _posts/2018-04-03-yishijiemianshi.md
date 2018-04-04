---
layout:     post
title:		面试汇总
date:       2018-03-26
author:     "handsameliu"
header-img: "img/post-bg-js-version.jpg"
tags:
  - javascript
  - 面试
  - 总结
---

### 医视界 20180403

----------------------------------

```javascript
var f = '123' + 4 - 1;
console.log(f);			// 1233
console.log(typeof f);	// number
console.log(f+=1);		// 1234

```

----------------------------------

```javascript
var i=0, j=100, k=100;
function f(n){
	var j = n;
	console.log(j);		// 0,1,2,3,4,5,6,7,8,9
}

for(i=0;i<10;i++){
	var k = i;
	f(k);
}

console.log(j);			// 100
console.log(k);			// 9
```

----------------------------------

> * 写出熟悉的新增的css样式，伪类。

vertical-align
border-radius
box-shadow
text-shadow
transform
transition
animation
@keyframes

:nth类选择器	
:first-child	
:lase-child		
:nth-child()	

------

> * call和apply的区别，==和===的区别，null和undefined的区别。

call和apply的区别: 功能一样，都可以改变方法的执行上下文，将一个对象的方法交给另一个方法来执行，并且立即执行。不同点是call方法的第二个参数开始可以接收任意个参数，每个参数会映射到被call运行了的方法参数上。而apply方法最多允许两个参数,第二个参数是一个数组。

==和===的区别："==="叫做严格运算符，"=="叫做相等运算符。严格运算符会严格比较两个值是否一致，包括类型，如果类型不相同也会返回false。引用类型会比较是否指向同一对象。相等运算符在比较相同类型时与严格模式运算符一样，类型不同时并不会比较类型，会进行隐式转换，转换后执行严格运算比较。

null和undefined的区别：两个都自身严格相等。null表示没有对象，即这里不应该有值。undefined表示缺少值，即变量已被声明，但还未赋值。
	
------

> * javascript事件流是什么？

事件流描述的是从页面中接收事件的顺序。事件发生时会在元素阶段与根节点之间按照特定的顺序传播，路径所经过的所有节点都会收到该事件，这个传播过程就是DOM事件流。

事件传播的顺序对应的浏览器的两种事件流模型：`捕获型事件流` 和 `冒泡型事件流`。

`冒泡型事件流`：事件的传播是从最特定的事件目标到最不特定的事件目标。即从DOM树的叶子到根。`推荐`

`捕获型事件流`：事件的传播是从最不特定的事件目标到最特定的事件目标。即从DOM树的根到叶子。

`DOM0级`事件方法指定的事件程序被认为是元素的方法，程序是在元素作用域中进行的，程序中this是引用的当前元素。

`DOM2级`事件规定的事件流分为三个阶段：`捕获`，`目标`，`冒泡`。事件在冒泡阶段被触发，与DOM0级方法一样，这里添加的事件处理程序也是在其依赋的元素作用域中运行，使用DOM2级添加处理程序的好处是可以添加多个事件处理程序。

非IE浏览器中使用`addEventListener`方法，在IE中使用的是`attachEvent`方法，且具体事件名称前要加`on`，例如`onclick`。在IE中使用attachEvent()和使用DOM0级方法的主要区别在于事件处理程序的作用域，在使用DOM0级情况下，事件处理程序在其所属元素的作用域内运行，在使用attachEvent()方法的情况下，事件处理程序在全局作用域下运行，其中的this指向window。

可以做兼容处理 `var e = e || window.event`.

阻止冒泡：非IE: `e.stopPropagation()`; IE: `window.event.cancelBubble = true`;

阻止默认事件：非IE: `e.preventDefault()`; IE: `window.event.returnValue = false`;

获取事件目标：非IE: `e.target`; IE: `window.event.srcElement`;	

------

> * js事件委托是什么？使用事件委托对一个列表中的链接进行操作，点击每一个链接单独打开新窗口。

红包书上是这么说的：事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。

```html
<ul id="ulid">
	<li>
		<a href="http://www.baidu.com">baidu</a>
	</li>
	<li>
		<a href="http://www.jd.com">jd</a>
	</li>
	<li>
		<a href="http://www.taobao.com">taobao</a>
	</li>
	<li>
		<a href="http://www.bilibili.com">bilibili</a>
	</li>
	<li>
		<a href="http://www.youku.com">youku</a>
	</li>
</ul>
```

```javascript
var UL = document.getElementById("ulid");
UL.onclick = function (ev) {
	var ev = ev || window.event;
	ev.preventDefault();
	var target = ev.target || ev.srcElement;
	if (target.nodeName.toLowerCase() == 'a') {
		window.open(target.href);
	}　　
}
```	

------

> * 根据不同后台接口的返回值（json格式或纯数组，但具体返回值不固定，返回格式不固定，无特殊处理），创建动态渲染表格，使其可复用多个项目。

略，参考dataTable插件	

------


> * 还有三道题，没记住。都是原生js基础。

----------------------------------

### 360企业安全 20180313

----------------------------------

> * 1.经典的作用域题，但长时间没有看又忘了。
##### [参考](http://blog.csdn.net/mongo__/article/details/72810338)

```javascript
function Foo() {  
    getName = function () {   
        console.log (1);   
    };
    return this;  
}  
Foo.getName = function () {   
    console.log (2);   
};  
Foo.prototype.getName = function () {   
    console.log('3');   
};  
var getName = function () {   
    console.log (4);  
};  
function getName() {   
    console.log (5);  
} 
// 请写出一下的输出结果  
Foo.getName();   
getName();   
Foo().getName();    
getName();    
new Foo.getName();    
new Foo().getName();    
new new Foo().getName();
```

答案是 `2 4 1 1 2 3 3`

> 执行语句第一行： Foo.getName() 
	>> 调用方式2的函数代码块，结果为`2`！
	
> 执行语句第二行：getName() 
    >> 如果单独看这条执行语句，一般会有个全局声明的函数方式5"function getName(){} ",但要是放在这么个复杂的上下文环境中，那肯定是不一样的，
	>> 因为还有个方式4 “var getName = function(){}” 来捣乱，`当定义的变量和声明的函数重名了怎么办？`
	>> 答：它们都会进行预解析，函数声明提前于变量声明，但是`最终会被变量覆盖`！so方式4获胜，输出结果为`4`！
	
> 执行语句第三行：Foo().getName()
	>> 本句，有个执行顺序的，先执行方式1的“Foo()”,结果是"this" 并指向window，并产生了一个全局getName(window.getName)指针指向一个匿名函数，然后再执行"this.getName()" , 其实就是执行刚刚造出来的那个全局getName指向的匿名函数，所以输出`1`.
	
> 执行语句第四行：getName()
    >> 此句执行的是方式1执行出来的那个全局变量getName 指针指向的匿名函数，此时方式4已经被覆盖了！所以结果为 `1`.
    
> 执行语句第五行：new Foo.getName()
    >> 首先还是先看运算符优先级吧，结果是【new Foo() >  Foo() > new Foo】，先运算方式2的Foo.getName() 结果为`2`，再new一个Foo实例对象。
    
> 执行语句第六行：new Foo().getName()
    >> 先执行 new Foo(), 结果产生一个新的实例对象，并且继承了Foo()这个构造函数中的getName方法，所以再执行方式3函数块，因此结果为 `3`。
    
> 执行语句第七行：new new Foo().getName()
    >> ~~先实例化new Foo,执行后就是变成了 new Foo的实例对象.getName(), 然后再实例化 new Foo的实例对象.getName(), 也就是回到了方式3的函数块，最后执行结果为`3`~~
    先实例化Foo，再实例化new Foo().getName，也就是 new 和 () 是成对出现的 -- @Lamonfly
    
------

> * 2，逐行输入下列，输出是什么
```javascript
let a = a;
a
let a = a;
```

注意是`逐行输入`，都报错

------

> * 3，以下输出为

```javascript
console.log(1)
setTimeout(function(){
	console.log(2);
},0);
new Promise(function(resolve,reject){
	resolve(3)
	console.log(4)
	return 
	console.log(5)
}).then(function(num){
	console.log(num)
})
console.log(6)
```

答案是 1,4,6,3,2,

> 从上往下看定时器异步最后这个肯定没有问题，但值得我注意的是只有在resolve的then中才会是异步的，所以1先输出，而4会优先于3输出，然后输出6。return 后不再执行。promise执行return后才会执行resolve的3。
promise执行完成后才会输出定时器中的2

------

> * 4，css 水平垂直居中的方法

> 很多种，比较简单，就不系说了，可以自行搜索

------

> * 5，git如何对分支进行跟踪
##### [参考](http://gitbook.liuhui998.com/4_7.html)

在Git中‘追踪分支’是用与联系本地分支和远程分支的. 如果你在’追踪分支'(Tracking Branches)上执行推送(push)或拉取(pull)时,　它会自动推送(push)或拉取(pull)到关联的远程分支上.
如果你经常要从远程仓库里拉取(pull)分支到本地,并且不想很麻烦的使用"git pull "这种格式; 那么就应当使用‘追踪分支'(Tracking Branches).
‘git clone‘命令会自动在本地建立一个'master'分支，它是'origin/master'的‘追踪分支’. 而'origin/master'就是被克隆(clone)仓库的'master'分支.
译者注: origin一般是指原始仓库地址的别名.
你可以在使用'git branch'命令时加上'--track'参数, 来手动创建一个'追踪分支'.

```
git branch --track experimental origin/experimental
git pull experimental
```

它会自动从‘origin'抓取(fetch)内容，再把远程的'origin/experimental'分支合并进(merge)本地的'experimental'分支.
当要把修改推送(push)到origin时, 它会将你本地的'experimental'分支中的修改推送到origin的‘experimental'分支里,　而无需指定它(origin).

------

> * 6，vue2 组件之间的通信

> 只要答出父子间，同级兄弟间，vuex状态管理（项目没有用到过所以就答了一个名词）即可

简单，具体不写了。

------

> * 7，做一个简易的vue状态管理，a组件中有time属性，b组件中有area属性，当a组件中的time或b组件中的area发生改变时，同时在控制台打印time和area

> 我理解的是实现一个类似vuex的状态管理控件。想到的是父子间通信和同级间通信，说出来后，面试官并不是很满意。回公司后问了一下其他人，也没有人能答出具体的实现，索性就学一下vuex的实现原理吧。


------
> * 8，平时用到那些css，js，svg，canvas

> 只说了一下平时用到的animate.css，echarts，bootstrap，vux，elementui，iview这些

