---
layout: post
title: "core"
keywords: core
description: slark core
date: 2016-09-27 10:31:36
categories: [js]
tags: [slark]
---

core

属性:

```text
initTime: 1475028608660, router: t, isLowDevice: false, transform: "-webkit-transform", animationEnd: "webkitAnimationEnd"
```

方法

1. 直接声明
2. from `util` 模块

```js

define(function(){
	var util = {};

	util.isLowDevice = (function(){
        var ua = navigator.userAgent.toLowerCase();
        //(screen.width < 720) || (screen.height < 720) ||
        //android低端机
        if( ua.indexOf('iemobile') !== -1  || ((ua.indexOf('android') !== -1) && 
                    ((/ucbrowser\/[2-9]\./.test(ua)) ||  (/android [1-2]\./.test(ua))))){
            return true;
        }

        return false;
    })();

    util.supports = function(key,value){
        //key 没有value时，key如下格式 string格式 eg："display:flex" | "(display:flex) and (transition:initial)" | "(display:flex) or (transition:initial)" 
        //兼容opera
        //如果不支持support也会返回flase
        if(util.hasSupport() === false) return false;
        if(typeof value === "undefined") return CSS.supports(key)||(window.supportsCSS&&supportsCSS(key));
        else return CSS.supports(key,value)||(window.supportsCSS&&supportsCSS(key,value));
    };
    util.hasSupport = function(){
        //android4.4，IOS9以上版本支持；
        return !!((window.CSS && window.CSS.supports) || window.supportsCSS || false);
    };

    util.animationEnd = (function(){
        var t;

        if(util.hasSupport()){
            var animations = {
                '-webkit-transform': 'webkitAnimationEnd',
                '-o-transform': 'oAnimationEnd',
                '-moz-transform': 'AnimationEnd',
                '-ms-transform': 'msAnimationEnd',
                'transform': 'animationEnd'
            }
            for (t in animations) {
                if(util.supports(t,"initial")){
                    util.transform = t;
                    return animations[t];
                }
            }
        }
        else{
            var el = document.createElement('element');
        
            var animations = {
                'WebkitTransform': 'webkitAnimationEnd',
                'OTransform': 'oAnimationEnd',
                'MozTransform': 'AnimationEnd',
                'MsTransform': 'msAnimationEnd',
                'transform': 'animationEnd'
            }
            for (t in animations) {
                if (el.style[t] !== undefined) {
                    util.transform = t;
                    return animations[t];
                }
            }
        } 
    })();

    util.transitionEnd = (function(){
        var t;

        if(util.hasSupport()){
            var transitions = {
                '-webkit-transition': 'webkitTransitionEnd',
                '-o-transition': 'oTransitionEnd',
                '-moz-transition': 'transitionend',
                '-ms-transition': 'msTransitionEnd',
                'transition': 'transitionend'
            }
            for (t in transitions) {
                if(util.supports(t,"initial")){
                    util.transition = t;
                    return transitions[t];
                }
            }
        }
        else{
            var el = document.createElement('element');
            
            var transitions = {
                'WebkitTransition': 'webkitTransitionEnd',
                'OTransition': 'oTransitionEnd',
                'MozTransition': 'transitionend',
                'MsTransition': 'msTransitionEnd',
                'transition': 'transitionend'
            }
            for (t in transitions) {
                if (el.style[t] !== undefined) {
                    util.transition = t;
                    return transitions[t];
                }
            }
        }
    })();


    util.moveWidget = function(widget, from, to,cb){
        //animation don't support android 2.3, so use transition
        if (util.isLowDevice || !util.animationEnd){
            widget.addClass("page-on-" + to);
            widget.removeClass("page-on-" + from);
            cb && cb();
        }else{
            widget.bind(util.animationEnd, function(){
                widget.addClass("page-on-" + to);
                widget.removeClass("page-on-" + from);
                widget.removeClass("page-from-" + from + "-to-" + to);
                cb && cb();
                widget.unbind(util.animationEnd);
            });

            widget.removeClass("page-on-"+from).addClass("page-from-" + from + "-to-" + to);
        }
        
    }




	//去除日期中前面的0，在部分原生浏览器下parseInt('08') 和 parseInt('09') 会等于0
    util.removeZeroOfDate = function(date) {
        return date.charAt(0) == '0' ? date.substring(1) : date;
    }    

    util.getElementLeft = function(element){
　　　　var actualLeft = element.offsetLeft;
　　　　var current = element.offsetParent;

　　　　while (current !== null){
　　　　　　actualLeft += current.offsetLeft;
　　　　　　current = current.offsetParent;
　　　　}
　　　　return actualLeft;
　　}

　　util.getElementTop = function(element){
　　　　var actualTop = element.offsetTop;
　　　　var current = element.offsetParent;

　　　　while (current !== null){
　　　　　　actualTop += current.offsetTop;
　　　　　　current = current.offsetParent;
　　　　}
　　　　return actualTop;
　　}

    util.getElementVisibleLeft = function(element){
　　　　var actualLeft = element.offsetLeft;
　　　　var current = element.offsetParent;

　　　　while (current !== null){
　　　　　　actualLeft += current.offsetLeft;
            actualLeft -= current.scrollLeft;
　　　　　　current = current.offsetParent;
　　　　}

        if($(current).hasClass('page')) {
            actualLeft -= current.scrollLeft;
        }

　　　　return actualLeft;
　　}

　　util.getElementVisibleTop = function(element){
　　　　var actualTop = element.offsetTop;
　　　　var current = element.offsetParent;

　　　　while (current !== null){
　　　　　　actualTop += current.offsetTop;
            actualTop -= current.scrollTop;
　　　　　　current = current.offsetParent;
　　　　}

        if($(current).hasClass('page')) {
            actualTop -= current.scrollTop;
        }

　　　　return actualTop;
　　}

    util.getLanguage = function(key,$LANG, param){
        if(!$LANG) {
            return "";
        }

        var value = $LANG[key];

        if(!value) {
            return "";
        } else if(!!param && param.length > 0) {
            for(var i = 0; i < param.length; i ++) {
                value = value.replace(new RegExp('\\{' + (i) + '\\}',"g"), param[i]);
            }
        }

        return value;
    }

    util.getHash = function(name){
        var params = location.hash.substring(1).split('&');

        for(var i = 0; i < params.length; i ++) {
            var tmp = params[i].split("=");

            if(tmp.length > 1 && name == tmp[0]) {
                return tmp[1];
            }
        }

        return null;
    }

    util.setHash = function(opt){
        if(!opt || !opt.length) return;

        var hash = location.hash;
        var hash2 = hash;

        var _setHash = function(name, value){
            var start = hash.indexOf(name);
            var end = hash.charAt(start + name.length);

            if(!value) return;

            if(hash.length === 0) {
                //hash长度为0，直接添加
                hash = name + '=' + value;
                return;
            } else if(start > 0 && (end == '=' || end == '&' || end === '')) {
                //hash里包含该参数
                var oldValue = util.getHash(name);

                //旧值不存在替换整个键值对
                var tmp = hash.substring(start);
                var end = tmp.indexOf('&');
                tmp = tmp.substring(0, end > 0 ? end : tmp.length);

                hash = hash.replace(tmp, name + '=' + value);
                return;
            } else {
                //hash中不包含参数
                hash += hash.charAt(hash.length - 1) == '&' ? '' : '&' + name + '=' + value; 
                return;
            }
        }

        for(var i = 0; i < opt.length; i ++){
            var param = opt[i];

            _setHash(param.name, param.value);
        }

        if(hash.charAt(0) !== '#') {
            hash = '#' + hash;
        }
        if (hash2 != hash){
            History.replaceState(this.getActiveId(), document.title, location.pathname + location.search + hash);
        }
        
    }

    util.clone = function(obj) {
        var newObj = null;

        if(!obj) {
            newObj = obj;
        } else if (typeof obj != 'object') {
            newObj = obj;
        } else {
            newObj = obj.constructor == Array ? [] : {};

            for (var i in obj) {
                newObj[i] = util.clone(obj[i]);
            }
        }

        return newObj;
    }

    util.smartFloat = function(num){
        num = Math.round(num * 100);
        return num / 100;
    }

    util.countTimer = {};
    util.countTimer.set = function(){

    };
    util.countTimer.clear = function(){

    };

    util.getParam = function(param){
        var paramArr = location.search.substring(1).split('&');

        if(paramArr.length == 1 && paramArr[0] == '') return;

        for(var i = 0; i < paramArr.length; i ++) {
            var keyValue = paramArr[i].split('=');

            if(keyValue[0] == param) return keyValue[1];
        }

        return;
    }

    util.handleBlankPaint = function(dom,id,data,plugin){

        // var appCache = window.applicationCache;
        // appCache.update(); // Attempt to update the user's cache.

        // if (appCache.status == window.applicationCache.UPDATEREADY) {
        //   appCache.swapCache();  // The fetch was successful, swap in the new cache.
        // }

        // //window.addEventListener('load', function(e) {
          window.applicationCache.addEventListener('updateready', function(e) {  
            if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {
              // Browser downloaded a new app cache.
              // Swap it in and reload the page to get the new hotness.
              window.applicationCache.swapCache();
              if (confirm('存在最新数据，是否刷新')) {
                window.location.reload();
              }
            } else {
              // Manifest didn't changed. Nothing new to server.
            }
          }, false);
        // //}, false);
    };

    util.testVersion = function(version){
        if(ElongBridge.RUNTIME_VERSION == 0) return false;

        var runtimeArr = ElongBridge.RUNTIME_VERSION.split('.');
        var verArr = version.split('.');
        var canSetNav = false;

        for(var i = 0; i < runtimeArr.length; i ++ ){
            runtimeArr[i] = parseInt(runtimeArr[i]);
            verArr[i] = parseInt(verArr[i]);
        }

        //判断版本是否大于9.10.0
        if(runtimeArr.length > 2) {
            if(runtimeArr[0] > verArr[0]) {
                canSetNav = true;
            } else if(runtimeArr[0] == verArr[0]) {
                if(runtimeArr[1] > verArr[1]) {
                    canSetNav = true;
                } else if(runtimeArr[1] == verArr[1] && runtimeArr[2] >= verArr[2]) {
                    canSetNav = true;
                }
                
            } 
        }

        return canSetNav;
    }

    //如果在APP中，且版本>9.10.0，设置导航栏title
    util.setAppNavBar = function(){
        //判断是否在APP中
        if(typeof ElongBridge !== 'undefined' && util.testVersion('9.10.0')) {
            ElongBridge.setNavbar({
                data : [
                    {
                        key : 'title',
                        content : document.title,
                        priority : 0
                    }
                ],
                onsuccess : function(){
                    console.log('set navbar title success');
                }, 
                onfail : function(){
                    console.log('set navbar title faile');
                }
            });
        }

    }

    return util;
});
   

```

window.slark = core;

blend = Blend.ui;

core.ui = blend;

main.js 里面声明返回了 `Blend`

```js



  define(['./blend','./Layer','./LayerGroup'], function (blend, layer,layergroup) {
      "use strict";
  
      blend = blend||{};
      
  	//layer
      blend.Layer = layer;
      blend.LayerGroup = layergroup;
      
      // window.Blend = blend;
      window.Blend = window.Blend || {};//初始化window的blend 对象 ， 将 blend 作为模块 绑定到 Blend.ui 上
      window.Blend.ui = blend;
  
      return blend;
      
  });  
    
```

上面的 `blend` 的首先赖在 `web/blend` 下;

下面是源码:

```js
    define(['../lib',"../web/events",'../web/api'],
        function(lib,events,api) {
    
    
            /**
             * @class blend
             * @singleton
             */
            var blend = {};
            var controls = {};
            var cbs={};//临时存储，blend.layerInit 的 layerId对应的执行函数
    
    
            /**
             * 版本信息
             *
             * @property {String} version info
             */
            blend.version = 'alpha';
    
            /**
             * 开放的Api接口entend到blend中
             *
             * @property {Object} Api接口
             */
    
            lib.extend(blend,api);
    
    
            // {};
            // //main.api.core.removeSplashScreen
            // var noop = function(){};
            // blend.api.core={};
            // blend.api.core.removeSplashScreen = noop;
    
            // blend.api.layer = {};
            // blend.api.layer.on = events.on;
            // blend.api.layer.off = events.off;
            // blend.api.layer.fire = events.fire;
            // blend.api.layer.once = events.once;
    
            // blend.api.layerStopRefresh = function(id){
            //     Layer.prototype.endPullRefresh(blend.get(id));
            // };
    
            
            
            /**
             * 开放的Api接口entend到blend中
             *
             * @property {Object} Api接口
             */
            blend.layerInit = function(layerId,callback){
                
                cbs[layerId] = callback;
    
                var layer = blend.get(layerId);
    
                if (layer && layer.isRender() ){
    
                    callback && callback.call(layer,layer.main,layer.controllerData);
                }
            };
            $(document).on("onrender",function(eve){
                var detail=typeof eve.originalEvent !== 'undefined' ? eve.originalEvent.detail : eve.detail;
                if (detail && cbs[detail]) {
                    var layer = blend.get(detail);
                    //native 无法传递 layer 对象，所以无法使用 this
                
                    cbs[detail].call(layer,layer.main,layer.controllerData);
                    
                }
                if (layer) {
                    layer.isRender(true);
                }
                
            });
    
            blend.ready = function(cb){
                if (/complete|loaded/.test(document.readyState) && document.body) 
                    cb();
                else 
                    //$(document).on('DOMContentLoaded', function(){ cb(); }, false);
                    document.addEventListener("DOMContentLoaded", function(){cb()},false);
            };
            blend.ready(function(){
                events.fire("blendready");
                /**
                 * 当前的active apge 记录到blend中
                 *
                 * @property {Object} activeLayer
                 */
                blend.activeLayer = $('.page');
            });
    
    
            /**
             * 是否处于Runtime环境中
             *
             * @property {boolean} inRuntime
             */
            blend.inRuntime = function() {
                return false;
            };//runtime.inRuntime();
    
    
            // var config = {
            //     DOMPrefix: 'data-ui',
            //     classPrefix: {
            //         'ui' : 'ui',
            //         'skin' : 'skin',
            //         'state' : 'state'
            //     }
            // };
    
    
            // *
            //  * 设置config
            //  *
            //  * @property {Object} info
             
            // blend.config = function(info) {
            //     lib.extend(config, info);
            // };
    
            // /**
            //  * 获取config
            //  *
            //  * @property {String} name
            //  */
            // blend.getConfig = function(name) {
            //     return config[name];
            // };
    
            /**
             * 获取currentLayerid
             *
             * @property {String} name
             */
            
            blend.getLayerId = function(){
                return Blend.ui.activeLayer.attr("data-blend-id");
            };
    
            /**
             * 从ID获取Control
             *
             * @param {String} element
             *
             * @return {Control} control
             */
            blend.getUI = function(element) {
                element = $(element)[0];
                do {
                    //如果元素是document
                    if (!element || element.nodeType == 9) {
                        return null;
                    }
                    if (element.getAttribute('data-blend')) {
                        return controls[element.getAttribute('data-blend-id')];
                    }
                }while ((element = element.parentNode) != document.body);
            };
    
            /**
             * 注册控件到系统中
             *
             * @param {Control} control 控件实例
             * @return null
             */
            blend.register = function(control) {
                console.log('reg: ' + control.id);
                controls[control.id] = control;
            };
    
            /**
             * 注销控件
             *
             * @param {Control} control 控件实例
             * @return null
             */
            blend.cancel = function(control) {
                //console.log("reg: " + control.id);
                delete controls[control.id];
            };
    
            blend.create = function(type, options) {
    
            };
            blend.canGoBack = function(){
                return blend.layerStack.length;
            };
    
            /**
             * 根据id获取实例
             *
             * @param {string} id 控件id
             * @return {Control}
             */
            blend.get = function(id) {
                if (id === "0") {
                    if (!controls[id]) {
                        controls[id] = new blend.Layer({id:"0"});
                        if ($(".page").length){
                            controls[id].main = $(".page")[0];
                        }else{
                            console.warn(" '0' page need to have classes .pages>.page>.page-content ");
                        }
                        
                    }
                }
                return controls[id];
            };
    
            blend.on = events.on;
            blend.once = events.once;
            blend.off = events.off;
            blend.fire = events.fire;
    
            
    
            blend.layerStack = [];
            
    
            // blend.configs = configs;
    
            return blend;
        }
    );

``




 




 






 
 