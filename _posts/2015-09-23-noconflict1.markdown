---
layout: post
title: "noConflict函数干的事"
date:   2015-09-23 15:37:36
categories: [jQuery]
tags: [proxy,Core]
---

在拉入我们的库运行的时候，我们会暴露两个变量来使用我们的库，但也许我们之前，别人已经叫过这个名字了。那怎么办？

我们先保存别人的这个变量，我们通过一个方法，来还原别人的变量，而自己的，保存到用户自定义的变量里。

```js

	noConflict: function(deep) {
		if (window.$ === jQuery) {
			window.$ = _$;
		}

		if (deep && window.jQuery === jQuery) {
			window.jQuery = _jQuery;
		}

		return jQuery;
	}

```

如果不传参数，只会释放 `$` ，而要更彻底一点把 `jQuery` 也释放掉。

```js

var bbb = {a: function() {console.log(arguments)}}; var temp = $.proxy(bbb, 'a', '2'); temp('1')

```