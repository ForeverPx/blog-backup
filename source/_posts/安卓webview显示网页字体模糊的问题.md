title: 安卓webview显示网页字体模糊的问题
date: 2018-08-10 17:14:54
tags: [Javascript,Css]
categories: frontend
---

## 问题
先看下如下代码
```html
<div id='A'>绝对定位区域</div>
<div id='B'>内容正文区域</div>
```

<!-- more -->

```css
#A{
  position: absolute;
  left: 0;
  right: 0;
  box-shadow: 0 0 6px 0 rgba(0,0,0,0.10);
}
```
上面的代码在PC和Mobile的浏览器中文字的显示都是正常的。
但是安卓Mobile的webview中，一旦满足以下条件，A元素的字体将会变得模糊。

 - A元素为绝对（absolute）或固定定位（fixed）元素
 - A元素有设置box-shadow属性，且blur值为偶数
 - B元素的内容过多使得浏览器产生了竖向的滚动条
 
## 解决方案
大多数情况下，B元素的内容多少是无法控制的。同时也无法避免的要使用到绝对定位的元素。所以只能采用下列方法解决：

在安卓webview下将box-shadow的blur值设置为奇数