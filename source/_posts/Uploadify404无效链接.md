title: Uploadify404无效链接
date: 2014-11-04 19:26:24
tags: [Javascript]
categories: frontend
---
[Uploadify404无效链接](http://www.foreverpx.cn/2014/11/04/Uploadify404无效链接)

在使用Jquery Uploadify插件的時候，会发现在请求中有个返回值为404的请求。

假如现在的location为www.aa.com/bugs/more.
html,在这个页面中进行了Uploadify初始化，这个时候可以在浏览器的调试工具中看到一个404的多余请求。这个请求的地址一般为www.aa.com/bugs/，截取了最后一个“/”后的字符串并请求。

<!--more-->


想要去除这个无效请求，就得去修改源码。在未经压缩的源码中找到如下代码

```js
    this.settings.button_image_url =  SWFUpload.completeURL(this.settings.button_image_url)
```
改为

```js
    this.settings.button_image_url= this.settings.button_image_url ? SWFUpload.completeURL(this.settings.button_image_url) : this.settings.button_image_url  
```

我们在初始化的配置中如果没有配置button_image_url，则会产生这个无效链接。所以加一个判断，当button_image_url有配置时才使用completeURL方法来创建请求链接，这样就能避免发送一个无效链接


参考资料：
[uploadify 初始化访问url](http://www.oschina.net/question/96568_117454)

文章作者：foreverpx
文章原文链接：[Uploadify404无效链接](http://www.foreverpx.cn/2014/11/04/Uploadify404无效链接)