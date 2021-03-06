---
layout: post
title: "Callbacks模块解析"
keywords: jQuery CallBacks
description: jQuery CallBacks 源码解读
date:   2015-10-27 18:11:36
categories: [jQuery]
tags: [Callbacks]
---

某个时候调用某个函数，这个函数被称作回调函数。

某个时候，要干某些事情，某些事情分别由不同的函数来完成，并且应该有先后顺序。

回调列表就是用来解决这个问题的。

* add的函数，被放在LIST列表，调用的时候遍历，按照添加的先后顺序依次调用各函数，注意某个时候，参数是相同的，执行所有的函数一遍
* 如果add的时候，调用已经结束，且是 `memory` 的情况下，那么直接执行。 否则不执行。
* 如果在调用中，又add， 那么应该把该函数，添加到执行队列中去。执行过去的时候会执行到。具体是直接设置循环的长度，就会读到新增的条目。
* 如果在某个状态的调用中，又有某个状态要反复调用，那么会放到 `stack` 中，这是一个队列，存放了这个状态的参数，然后在本次调用完成之后，按照队列的顺序依次递归调用
* `once` 的情况下不存在队列，正常情况下，执行完成之后，就 `diable` 了； 如果还是 `memory` 情况，那么，清空了列表表示只执行一次，新添加的，会按照 `memory`
的方式调用
* `lock` 会把 `stack` 设置为 `undefined`，并且 `disable` 如果 `lock` 的时候还是 `memory` 那么表示还可以调用一次，然后会清空 `list`, 以后只支持 `add` 的调用方式 
跟 `once memory`的情况是类似的。  
* `once` 表示只调用一次，所以不存在 `stack`, 所以天生就死锁定状态。
* `stopOnFalse` 表示执行过程中返回 false的时候，停止本次执行， `stack` 中的还是可以执行的。
* `unique` 表示队列中函数应该是唯一的。不能重复 `add`

```js
add: function() {
	if (list) {
		var start = list.length;
		(function add(args) { 
			jQuery.each(args, function(_, arg) {
				var type = jQuery.type(arg);
				if (type === "function") {
					if (!options.unique || !self.has(arg)) {
						list.push(arg);
					}	
				} else if (arg && arg.length && type !== "string") {	
					add(arg);
				}
			});
		})(arguments);
		if (firing) {
			firingLength = list.length;
		} else if (memory) {
			firingStart = start;
			fire(memory);
		}
	}
	return this;
}
```

```js
fireWith: function(context, args) { 
	if (list && (!fired || stack)) {
		args = args || [];
		args = [context, args.slice ? args.slice() : args];
		if (firing) {
			stack.push(args);
		} else { 
			fire(args);
		}
	}
	return this;
}
```

```js
fire = function(data) {
	memory = options.memory && data;
	fired = true;
	firingIndex = firingStart || 0;
	firingStart = 0;
	firingLength = list.length;
	firing = true;
	for (; list && firingIndex < firingLength; firingIndex++) {
		if (list[firingIndex].apply(data[0], data[1]) === false && options.stopOnFalse) {
			memory = false; 
			break; 
		}
	}
	firing = false;
	if (list) {
		if (stack) {
			if (stack.length) {
				fire(stack.shift());
			}
		} else if (memory) {			
			list = [];
		} else { 
			self.disable();
		}
	}
}
```