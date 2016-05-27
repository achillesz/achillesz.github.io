---
layout: post
title: "js 垃圾回收机制"
keywords: js 垃圾回收机制 
description: js 垃圾回收机制
date: 2016-05-24 08:52:36
categories: [js]
tags: [js]
---

Js 有2种垃圾回收机制。一个是标记的，这个不会又问题。另一种是引用计数的。
引用计数原理是有一个引用，那么加1，如果变量重新赋值，引用减一。如果没有引用了，在垃圾回收的时候会被清除，以释放内存空间。出现的问题是，如果出现循环引用，那么内存永远不会被释放。

如上图的情况：

说明：如果循环引用中包含DOM对象或者ActiveX对象，那么就会发生内存泄露。内存泄露的后果是在浏览器关闭前，即使是刷新页面，这部分内存不会被浏览器释放。


![循环引用的一个场景](/assets/img/mem.jpg)


简单的循环引用：

但是通常不会出现这种情况。

```js
var el = document.getElementById('MyElement');
var func = function ()
{…}

el.func = func;
func.element = el; 
	 
```

通常循环引用发生在为dom元素添加在闭包，为他的事件添加一个匿名函数

```js
function init() {
var el = document.getElementById('MyElement');
el.onclick = function ()
{……}

}
init(); 
```

init在执行的时候，在init函数里面这个局部环境下，我们称作 “当前上下文环境”，我们叫做context。这个时候，context引用了el，el引用了function，function引用了context。这时候形成了一个循环引用。
下面2种方法可以解决循环引用：
1) 置空dom对象
服用后：

```js
function init() {
var el = document.getElementById('MyElement');
el.onclick = function ()
{……}

el = null;
}
init();

```

需要返回DOM

```js

function init() {
var el = document.getElementById('MyElement');
el.onclick = function ()
{……}

try{
return el;
} finally {
el = null;
}
}
init();

````

2) 构造新的context

```js
function elClickHandler()
{……}


function init() {
var el = document.getElementById('MyElement');
el.onclick = elClickHandler;
}
init()

```

把function抽到新的context中，这样，function的context就不包含对el的引用，从而打断循环引用。





