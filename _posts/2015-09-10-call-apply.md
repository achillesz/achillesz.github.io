---
layout: post
title:  "Function.call Function.apply"
category: javascript
tags: [javascript-core]
---

js内部API

#### apply

```js
/**
@param {Object} [thisArg]
@param {Array} [argArray]
@return {*}
*/
Function.prototype.apply = function(thisArg,argArray) {};
```
#### call

```js
/**
@param {Object} [thisArg]
@param {...*} [args]
@return {*}
*/
Function.prototype.call = function(thisArg,args) {};
Function = {};
```
```js
function a(x) { console.log(x); return this;}; a.call(window, [1,2])
VM569:2 [1, 2]
Window {top: Window, location: Location, document: document, window: Window, external: Object…}
```



