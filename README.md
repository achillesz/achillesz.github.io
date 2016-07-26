touch 总结
======================


本地无景点解决
----

```
	/home/q/www/piao.qunar.com/cache/qconfig
	cat application.properties.merged
```

chrome浏览器模拟APP
----

```
qunariphone/80018888
Mozilla/5.0 (iPad; CPU OS 9_1 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Version/9.0 Mobile/13B137 Safari/601.1 qunariphone
```
上面随便填的似乎有问题，要尽量选一个之后，再加入 `qunariphone`

detail页面打不开 报500错误
----

需要重启报价

打开景点 `detail` 没有数据
----
刷报价


用手机调试
----

保证

刷 host
----

```
ipconfig /flushdns
```



本机开无线
----

[参考](http://jingyan.baidu.com/article/335530da4f774019cb41c3eb.html) 
能连上，但不能用

fiddler用于手机测试
----

1. 开启fiddler 本地所有的请求都被监听
2. 手机测试要保证统一网段，
3. 要让 `fiddler` 监听需要设置代理 `IP` 和端口

![代理关系图]('https://pic1.zhimg.com/45e27880b2543da77d9c44a43d740988_b.jpg')


## 安卓包下载地址

http://192.168.9.253/out

