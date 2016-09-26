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








 
 