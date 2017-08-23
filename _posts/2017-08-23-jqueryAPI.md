---
layout:     post
title: jQuery常用API
subtitle:   "jQuery"
date:       2017-08-23
author:     "handsameliu"
header-img: "img/post-bg-jquery.jpg"
tags:
  - 总结
  - jQuery
---

> 项目要重构了，来复习一下老版的jquery(项目兼容ie8，或者更低)...

## 基本选择器

<!-- more -->

`$(document);`获取文档对象     

`$('#id');`获取某个id元素

`$('.className');`获取某个class元素

`$('input[name=username]');`获取name为username的input元素

`$('a:first');`获取a标签的第一个

`$('tr:odd');`获取表格行的奇数行

`$('#id:input');`获取某个id元素中的input元素

`$('div:visible');`获取div的状态是visible的元素

`$('div:gt(2)');`获取除了前三个的所有div元素

`$('div:animated');`获取div中处于animated状态的元素

## 过滤器

`$('div').has('p');`div元素中所有的p元素

`$('div').not('className');`div元素集合中不是此className的元素

`$('div').filter('className');`div元素集合中class为className的元素

`$('div').first();`div元素集合中的第一个

`$('div').eq(5);`div元素集合中的第六个（下标0开始）

`$('div').next('p');`div元素的后面的第一个p元素

`$('div').parent();`div元素的父级

`$('div').closest('#id');`离div元素最近的名为id的元素

`$('div').childen();`div元素的子集，子元素

`$('div').siblings();`div元素的兄弟元素，同级元素

## 链式操作

> 在选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来。

    $('div').find('h3').eq(2).html('Hello');
    分解开来，就是下面这样：
　　$('div') //找到div元素
　　　.find('h3') //选择其中的h3元素
　　　.eq(2) //选择第3个h3元素
　　　.html('Hello'); //将它的内容改为Hello
    这是jQuery最令人称道、最方便的特点。它的原理在于每一步的jQuery操作，返回的都是一个jQuery对象，所以不同操作可以连在一起。

    jQuery还提供了.end()方法，使得结果集可以后退一步：
    $('div')
　　　.find('h3')
　　　.eq(2)
　　　.html('Hello')
　　　.end() //退回到选中所有的h3元素的那一步
　　　.eq(0) //选中第一个h3元素
　　　.html('World'); //将它的内容改为World

## 元素的取值和赋值

> 使用同一个函数，来完成取值（getter）和赋值（setter），即"取值器"与"赋值器"合一。到底是取值还是赋值，由函数的参数决定。

`$('p').html();`取出p标签内的代码内容

`$('p').html('holle world');`给指定的p标签赋值

`$('p').text();`取出p标签中的内容或赋值

`$('p').val();`取出p标签中的值或赋值

`$('p').attr();`取出p标签的属性或赋值

`$('p').width();`取出p标签的宽或赋值

`$('p').height();`取出p标签的高或赋值

> 需要注意的是，如果结果集包含多个元素，那么赋值的时候，将对其中所有的元素赋值；取值的时候，则是只取出第一个元素的值（.text()例外，它取出所有元素的text内容）。

## 移动

> 提供两组方法，来操作元素在网页中的位置移动。一组方法是直接移动该元素，另一组方法是移动其他元素，使得目标元素达到我们想要的位置。

    $('div1').insertAfter('div2');
    第一种方法是使用.insertAfter()，把div1元素移动div2元素后面

    $('div2').after('div1');
    第二种方法是使用.after()，把div2元素加到div1元素前面

表面上看，这两种方法的效果是一样的，唯一的不同似乎只是操作视角的不同。但是实际上，它们有一个重大差别，那就是返回的元素不一样。第一种方法返回div1元素，第二种方法返回div2元素。你可以根据需要，选择到底使用哪一种方法。

使用这种模式的操作方法，一共有四对：

    .insertAfter()和.after()：在现存元素的外部，从后面插入元素
　　.insertBefore()和.before()：在现存元素的外部，从前面插入元素
　　.appendTo()和.append()：在现存元素的内部，从后面插入元素
　　.prependTo()和.prepend()：在现存元素的内部，从前面插入元素

## 元素的复制、删除和创建

`.clone();`复制元素

`.remove();`删除元素，但不会保留元素的事件，删的一干二净

`.detach();`同为删除元素，但会保留被删除元素的事件，有利于重新插入文档时使用

`.empty();`清空元素内的内容

$('div1')`.append`('div2');追加元素到指定元素之后

## 工具方法

> 除了对选中的元素进行操作以外，还提供一些与元素无关的工具方法（utility）。不必选中元素，就可以直接使用这些方法。

如果你懂得Javascript语言的继承原理，那么就能理解工具方法的实质。它是定义在jQuery构造函数上的方法，即`jQuery.method()`，所以可以直接使用。而那些操作元素的方法，是定义在构造函数的`prototype对象`上的方法，即`jQuery.prototype.method()`，所以必须生成实例（即选中元素）后使用。如果不理解这种区别，问题也不大，只要把工具方法理解成，是像javascript`原生函数`那样，可以直接使用的方法就行了。

常用的工具方法：

`$.trim();`清空字符串两端的空格

`$.each();`循环数组或对象

`$.inArray();`返回目标所在数组中的位置，如果不存在返回-1

`$.grep();`返回数组中符合某种标准的元素

`$.extend();`将多个对象合并为一个对象

`$.makeArray();`将对象转化为数组

`$.type();`判断对象的类型（函数对象，日期对象，正则对象，数组对象等等）

`$.isArray();`判断是否是标准的数组

`$.isEmptyObject();`判断目标对象是否是空的

`$.isFunction();`判断是否是函数对象

`$.isPlainObject();`判断目标对象是否使用'{}'或'new Object()' 创建出的

`$.support();`判断浏览器是否支持某种特性

## 事件操作

> jquery的设计思想就是把`事件绑定到元素上`。

例如：

```javascript
    $('a').click(function(){
　　　　alert('Hello');
　　});
```

事件列表：

`.click();`单击事件

`.change();`当指定元素内容改变后触发

`.blur();`光标离开失去焦点

`.focus();`获取焦点

`.hover();`同时为mouseenter和mouseleave事件处理函数

`.keyup();`键盘抬起，松开键盘时触发

`.keydown();`按下键盘时触发

`.keypress();`按下键盘，长时间按键后将返回多个事件

`.mousedown();`按下鼠标

`.mouseup();`松开鼠标

`.mouseenter();`鼠标进入到指定元素内，进入子元素不会触发

`.mouseleave();`鼠标离开指定元素，离开子元素不触发

`.mousemove();`鼠标在指定元素内移动

`.mouseout();`鼠标离开，离开子元素也会触发

`.mouseover();`鼠标进入，进入子元素也会触发

`.focusin();`子元素获得焦点

`.focusout();`子元素失去焦点

`.dblclick();`双击事件

`.load();`元素加载完毕

`.ready();`dom加载完成

`.scroll();`滚动条位置的变化

`.select();`用户选中文本框的内容

`.submit();`提交表单

`.toggle();`根据鼠标点击的次数，依次运行多个函数

`.unload();`用户离开页面时触发

以上事件在jquery中都是用`bind`方法绑定的，它可以更灵活的控制事件。

比如你可以为多个事件绑定同一个函数：

```javascript
    $('input').bind(
　　　　'click change', //同时绑定click和change事件
　　　　function() {
　　　　　　alert('Hello');
　　　　}
　　);
```

有时你指向让他运行一次，可以使用`one()`方法

``javascript`
    $("p").one("click", function() {
　　　　alert("Hello"); //只运行一次，以后的点击不会运行
　　});
```

可使用 `.unbind()` 解除事件绑定

所有的事件处理函数，都可以接受一个事件对象(event object)作为参数，比如下面例子中的'e':

```javascript
    $("p").click(function(e) {
        //event 事件对象
　　　　alert(e.type); // "click"
　　});
```

这个event事件对象有许多的有用的方法和属性可以使用：

`event.pageX`  事件发生时，鼠标距离网页左上角的水平定位

`event.pageY`  事件发生时，鼠标距离网页左上角的垂直定位

`event.type`   事件的类型

`event.target` 事件针对的网页元素

`event.which`  按下了哪一个键

`event.data`   在事件上绑定数据，然后传入事件处理函数

`event.stopPropagation()`  阻止冒泡

`event.preventDefault()`   阻止默认行为

在事件的处理过程中，可以使用this关键字，返回事件针对的dom元素：

```javascript
    $('a').click(function(e) {
　　　　if ($(this).attr('href').match('evil')) { //如果确认为有害链接
　　　　　　e.preventDefault(); //阻止打开
　　　　　　$(this).addClass('evil'); //加上表示有害的class
　　　　}
　　});
```

有两种方法可以出发一个事件，一种是直接触发，另一种是使用trigger()或者trigger Handler()：

    $('id').click();
    $('id').trigger('click');

## 特殊效果

`.show();` 显示元素 相当于display:block

`.hide();` 隐藏元素

`.fadeIn();` 淡入

`.fadeOut();` 淡出

`.fadeTo();`调整透明度

`.slideDown();` 向下打开

`.slideUp();` 向上卷起

`.slideToggle();` 根据当前元素的情况打开或卷起  (slideDown,slideUp)

`.toggle();`根据当前元素的情况显示或隐藏元素  （show,hide）

除了`.show()`和`.hide()`外，其他的特效默认都会有400ms(毫秒),但是你可以改变这个设置。

    $('id').fadeIn(300);  300毫秒内淡入
    $('id').fadeIn('slow');  换面的淡出

特效结束后还能指定接下来运行的函数:

```javascript
$('p').fadeOut(300, function() { 
    $(this).remove(); 
});  //更复杂的特效，可以用.animate()自定义。
$('div').animate(
    {
        left : "+=50", //不断右移
        opacity : 0.25 //指定透明度
    },
    300, // 持续时间
    function() { alert('done!'); } //回调函数
);
```

`.stop()`和`.delay()`用来停止或延缓特效的执行

$.fx.off如果设置为true，则关闭页面特效












