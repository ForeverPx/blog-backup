title: Select2插件IE下autofocus的解决方法
date: 2015-08-11 10:24:09
tags: [Javascript]
categories: frontend
---

##描述：
Select2插件在IE中，当页面加载完毕后会自动的focus，弹出本来应该点击input才会有个提示语。

##原因：
在select2的源码中第1849行左右，有一段监听input事件的代码，如下：

```js
this.$selection.on('keyup.search input', '.select2-search--inline',function (evt) {
      self.handleSearch(evt);
});
```
可以看到这里是使用HTML5的input事件去监听input中值的变化的，并没有使用传统的change或者keydown等事件。

<!-- more -->

原因就出在这里，input事件在IE中是有bug的。在IE中，如果一个input带有placeholder属性，那么IE会自动触发这个input的input事件，而这是在其他浏览器中不会发生的。

##解决方案
写一个对于IE的兼容性方法，在判断为IE的时候将input事件替换为keydown事件。

```js
var isIE = (function () {
    var ua = window.navigator.userAgent.toLowerCase();
    if (ua.indexOf("msie") > 0 || ua.indexOf("trident") > 0 ) {
        return true;
    }

    else {
        return false;
    }

}());

var input_event = !isIE ? 'input' : 'keydown';

this.$selection.on('keyup.search '+input_event, '.select2-search--inline',function (evt) {
      self.handleSearch(evt);
});
```

文章作者：forevercjl
文章原文csdn链接：[www.foreverpx.cn](http://www.foreverpx.cn/2015/08/11/Select2%E6%8F%92%E4%BB%B6%20IE%E4%B8%8B%20autofocus%20bug%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)
转载请注明出处。