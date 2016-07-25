---
layout: post
title: "e6toe5 class extends"
keywords: ecmascript6 
description: ecmascript6
date: 2016-07-24 13:52:36
categories: [ecmascript6]
tags: [ecmascript6]
---

 [参考地址](http://es6.ruanyifeng.com/#docs/let)
 
 es6

{% highlight javascript %}

   /*Boy Class*/
   var People = require('./people');
   
   class Boy extends People {
   
       constructor(name, older, sex) {
           super();
           this.sex = sex;
       }
   
       sex(sex) {
           this.sex = sex;
       }
   }
   
   module.exports = Boy;

{% endhighlight %}
 
   
es5

```javascript


        'use strict';

        var _createClass = function () {
            function defineProperties(target, props) {
                for (var i = 0; i < props.length; i++) {
                    var descriptor = props[i];
                    descriptor.enumerable = descriptor.enumerable || false;
                    descriptor.configurable = true;
                    if ("value" in descriptor) descriptor.writable = true;
                    Object.defineProperty(target, descriptor.key, descriptor);
                }
            }

            return function (Constructor, protoProps, staticProps) {
                if (protoProps) defineProperties(Constructor.prototype, protoProps);
                if (staticProps) defineProperties(Constructor, staticProps);
                return Constructor;
            };
        }();

        function _classCallCheck(instance, Constructor) {
            if (!(instance instanceof Constructor)) {
                throw new TypeError("Cannot call a class as a function");
            }
        }

        function _possibleConstructorReturn(self, call) {
            if (!self) {
                throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
            }
            return call && (typeof call === "object" || typeof call === "function") ? call : self;
        }

        function _inherits(subClass, superClass) {
            if (typeof superClass !== "function" && superClass !== null) {
                throw new TypeError("Super expression must either be null or a function, not " + typeof superClass);
            }
            subClass.prototype = Object.create(superClass && superClass.prototype, {
                constructor: {
                    value: subClass,
                    enumerable: false,
                    writable: true,
                    configurable: true
                }
            });
            if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
        }

        /*Boy Class*/
        var People = __webpack_require__(5);

        var Boy = function (_People) {
            _inherits(Boy, _People);

            function Boy(name, older, sex) {
                _classCallCheck(this, Boy);

                var _this = _possibleConstructorReturn(this, Object.getPrototypeOf(Boy).call(this));

                _this.sex = sex;
                return _this;
            }

            _createClass(Boy, [{
                key: 'sex',
                value: function sex(_sex) {
                    this.sex = _sex;
                }
            }]);

            return Boy;
        }(People);

        module.exports = Boy;

```
    
    


 