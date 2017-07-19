---
layout: post
title: "最长公共字符窜"
keywords: 公共字符窜
description: 公共字符窜
date: 2017-07-18 10:00:00
categories: [js]
tags: [算法]
---


对象冻结

```js
  function find(str1,str2){  
                  if(str1.length>str2.length){  
                      shorter = str2;  
                      longer = str1  
                  }else{  
                      shorter = str1;  
                      longer = str2;  
                  }  
                  for(var subLength = shorter.length;subLength>0;subLength--){  
                      for(var i = 0;i+subLength<=shorter.length;i++){  
                          var subString =shorter.substring(i,i+subLength);  
                              // debugger;
                          if(longer.indexOf(subString)>=0){  
                              targetString = subString;  
                              return targetString;  
                          }  
                      }  
                  }  
              }  
              find("instritesting","string");  
  "stri"
```


[是否回文](http://blog.csdn.net/esir82/article/details/52179453)

  