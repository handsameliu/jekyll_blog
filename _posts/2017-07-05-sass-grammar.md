---
layout:     post
title: sass
date:       2017-07-05
author:     "handsameliu"
header-img: "img/post-bg-js-version.jpg"
tags:
  - 总结
  - sass
---

## sass语法

sass有两种后缀方式：
- 1.后缀为`sass` 不使用大括号和分号，语法要求严格
- 2.后缀为`scss` 可以使用大括号和分号，语法于css相同

<!-- more -->

``` css
//文件后缀名为sass的语法
body
  background: #eee
  font-size:12px
p
  background: #0982c1

//文件后缀名为scss的语法
body {
  background: #eee;
  font-size:12px;
}
p{
  background: #0982c1;
}
```

## sass的导入

sass的导入(@import)规则和css的有所不同，编译时会将@import的scss文件合并进来只生成一个css文件。但是如果在sass文件中导入css文件如[@import 'reset.css']，那效果跟普通css导入样式文件一样，导入的css文件不会合并到编译后的文件中，而是以[@import]方式存在。
所有的sass导入文件都可以忽略后缀名.scss。一般来说基础的文件命名方法以_开头，如_mixin.scss。这种文件在导入的时候可以不写下划线，可写成"[@import 'mixin']"。
当前转译后的css文件会包含其他sass文件的内容。而通过css方式引入的还会保持引入的样子。

``` css
@import "reset.css";
@import "mixin";
```

## 注释

sass注释有两种方式：
- 1.标准的css注释方式/**/
- 2.以//双斜杠形式的单行注释，但是单行注释不会被转译

``` css
/*标准的css注释*/
//双斜杠注释
//在/* 后面加一个感叹号，标识为重要注释。即使是亚索模式编译，也会保留这行注释，通常可以用于申明版权信息
/*!
	重要注释！
*/

```

## 变量

sass的变量必须是$开头，后面紧跟变量名。变量与变量名之间需要使用冒号(:)隔开。
如果值后面加上~default则标识默认值,但也可以被修改。需要使用覆盖的方式，只需在默认变量之前重新声明变量即可。

``` css
$fontSize:12px;
$lineHeight:60px;
$lineHeight:50px !default;
div{
	font-size:$fontSize;  //12px
    line-height:$lineHeight;  //60px
}
```

## 特殊变量

指在特殊情况下使用的变量，以#{$stylename}的形式使用。

``` css
$baseFontSize:14px !default;
$baseTop:top !default;
.border-#{$baseTop}{
	border-#{$baseTop} : $baseFontSize solid #999;
}
```
## 多值变量

多值变量分为list类型（类似js数组）和map类型（类似js对象）

## list

list数据可以通过空格，逗号，或者小括号风格多个值，可以用nth($var,$index)取值。其还有很多函数可以调用使用，如：[length($list)],[join($list1,$list2,[$separator])],[append($list,$value,[$separator])]等等...

``` css
//一维数组
$borderList:2px 3px 4px 5px;
//二位数组
$borderList:(1px 2px) (3px 4px);
```

``` css
 //使用
 div{
 	border:nth($borderList,1) nth($borderList,2) nth($borderList,3) nth($borderList,4)
 }
```

## map

map数据以key，value键值对的形式承兑出现，其中value也可以是list。
格式为$map:(key:value,key:value,.....).可通过map-get($map,$key)取值。还带有函数可以调用，如[ map.gerge($map1.$map2),map-keys($map),map-valyes($map) ]等

``` css
$headings:(h1:18px,h2:14px,h3:12px);
@each $header,$size in $headings{
	#{$header} {
    	font-size:$size;
    }
}
```

## 全局变量
	
在变量值后面加上!golobal即可（3.4版本后正式应用）

## 目前变量机制

在选择器中声明的变量会覆盖外面全局声明的变量。(这也就是人们常说的sass没有局部变量)

``` css
$fontSize:12px;
body{
	$fontSize:14px;
    font-size:$fontSize;  //12px
}
```

## 计算功能

sass允许我们在代码中使用算数表达式

``` css
body{
	margin:(14px/2);
    top:50px+100px;
    right:$var * 10%;
}
```
## 嵌套

sass允许选择器与属性嵌套展示

``` css
div{
	h1{
    	color:red;
    }
    p{
    	border{
        	color:red;
        }
    }
}
// 注意border后面必须加上冒号。
//在嵌套的代码块内，可以使用&引用父元素。比如a:hover伪类，可以写成：
a{
	&:hover{color:yellow;}
}
```

## 继承

sass允许一个选择器，继承另一个选择器，

``` css
.class1{border:1px solid red;}
.class2{
	@extend .class1;
    font-size:120px;
}
```

## Mixin

Mixin有点像C语言的宏（macro）,是可以重用的代码块。

使用@mixin命令，定义一个代码块。

``` css
@mixin left($value:10px){
	float:left;
    margin-left:10px;
}
```

使用@include命令，调用这个mixin。

``` css
div{
	@include left(20px);
}
```

mixin实例如下

``` css
@mixin rounded($vert,$horz,$radius:10px){
	border-#{$vert}-#{$horz}-radius:$radius;
    -moz-border-radius-#{$vert}#{$horz}:$radius;
    -webkit-border-radius-#{$vert}#{$horz}:$radius;
}

```
使用的时候，下面这样调用：

``` css
#navbar li {@include rounded(top,left);}
#footer {@include rounded(top,left,5px);}
```

## 颜色函数

sass提供了一些内置的颜色函数，以便生成系列颜色。

``` css
lighten(#cc3,10%)  //#d6d65c
darken(#cc3,10%)   //#a3a329
grayscale(#cc3)    //#808080
complement(#cc3)   //#33c
```

## 插入文件

@import命令，用来插入外部文件。

``` css
@import "path/filename.scss";
```

如果插入的是.css文件，则等同于css的import命令

``` css
@import "foo.css";
```

## 条件语句

@if可以用来判断：

``` css
p{
    @if 1+1==2{border:1px solid;}
    @if 5<3 {border:2px dotted;}
}
```

配套的还有@else命令

``` css
@if lightness($color) > 30%{
	background-color:#000;
}@else {
	background-color:#fff;
}

```
## 循环语句

sass支持for循环

``` css
    @for $i from 1 to 10{
    	.border:#{$i}px solid blue;
    }
```

也支持while循环：

``` css
$i:6;
@while $i>0{
	.item-#{$i}{width:2em*$i;}
    $i:$i-2;
}
```

each 命令，作用域for类似：

``` css
@each $menber in a,b,c,d{
	.#{$menber}{
    	background-image:url("/image/#{$menber}.jpg");
    }
}
```

## 自定义函数
	
sass允许用户编写自己的函数

``` css
@function double($n){
	@return $n*2;
}
#sidebar{
	width:double(5px);
}
```




















































