---
layout: post
title: "isPalindrome"
keywords: isPalindrome
description: isPalindrome
date: 2017-07-18 09:00:00
categories: [js]
tags: [功能函数]
---


是否回文

```js
    function isPalindrome(str) {
        str = str.replace(/\W/g, '').toLowerCase();
        return (str == str.split('').reverse().join(''));
    }
```


[是否回文](http://blog.csdn.net/esir82/article/details/52179453)

  