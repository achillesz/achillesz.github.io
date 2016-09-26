---
layout: post
title: "router 正则"
keywords: router reg backbone
description: backbone router
date: 2016-09-25 10:31:36
categories: [js]
tags: [router]
---

参考地址: 
[mozilla pop](https://developer.mozilla.org/zh-CN/docs/Web/Events/popstate)


```


```js

    var Route = Backbone.Router.extend({
        routes: {
            "about": "showAbout",
            "todo/:id": "getTodo",
            "search/:query": "searchTodos",
            "search/:query/p:page": "searchTodos",
            "todos/:id/download/*documentPath": 'downloadDocument',
            "*others": "defaultRoute",
            "optional(/:item)": "optionalItem",
            "named/optional/(y:z)": "namedOptionalItem"
        },
        showAbout: function () {
            // 无参数
        },
        getTodo: function (id) {
            // #todo/121fsdf/d
            // #todo/2'
            console.log('You are trying to reach todo ' + id);
        },
        searchTodos: function (query, page) {
            var page_number = page || 1;
            console.log("Page number: " + page_number + " of the results for todos containing the word: " + query);
        },
        downloadDocument: function (id, path) {
        },
        defaultRoute: function (other) {
            console.log('Invalid. You attempted to reach: ' + other);
        }
    });

    var route = new Route();

    Backbone.history.start();


```

####   "named/optional/(y:z)"     "namedOptionalItem"
生成的匹配路由: 
^named\/optional\/(?:y([^\/?]+))?(?:\?([\s\S]*))?$

这个正则的匹配情况有:
1. named/optional/ 以这部分开头或者结尾的可以匹配因为后面2个括号的问好部分都可表示存在一个或不存在
2. 如果后面括号的存在:
    1. 非捕获 y 跟上非 `/` 非 `?` 的任意字符
    2. `?` 加上任意字符

所以上述可匹配的情况有: 
1. named/optional/
2. named/optional/yddff (y后面必须加一个是因为可选项的后面是`+` 表示至少有一个)
3. named/optional/y? (这个情况是不能匹配的,还是 `+` 号后面表示必须有一个,而 `?` 是非匹配集合中的一个 )
4. named/optional/ydfdff? (这个情况是可以匹配的, 原因是 `?` 是下一括号里面的匹配了)





 
 