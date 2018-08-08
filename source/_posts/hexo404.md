title: HEXO 指定404页面
date: 2014-09-23 19:52:54
tags: [Hexo]
categories: frontend
---
[HEXO 指定404页面](http://www.foreverpx.cn/2014/09/23/hexo404)

由于Hexo托管于github，所以加404页面非常容易，只需要在xxx.github.io下面新建一个404页面即可。代码如下：

<!--more-->

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>404</title>
</head>
<body>
	自定义
</body>
</html>
```

我们也可以使用腾讯的公益[404页面](http://www.qq.com/404/)
将页面中的javascript代码拷贝到我们的代码中就可以了

```html
<!DOCTYPE html>
<html lang="en">
<head>]\
	
	<meta charset="UTF-8">
	<title>404</title>
</head>
<body>
	<script type="text/javascript" src="http://www.qq.com/404/search_children.js" charset="utf-8"></script>
</body>
</html>
```

如下图：
![404](http://t.williamgates.net/image-F272_54215F39.jpg)

这时候你会发现，当你再次部署hexo的时候，github上面的404页面又被删除了。这是因为你hexo在你本地的文件夹内并没有这个文件，所以一同步就没了。即使你在本地根目录加了，hexo也会删除。


所以，这个404的页面要放到source目录下面。（注意，不要放到source下的post里面）
放在source下的文件会被上传但不会被解析到文章里面，source目录结构如下

![source](http://t.williamgates.net/image-10AB_5421616C.jpg)

此时就大功告成了

在你的域名下输入个不存在的，就能看到结果

文章作者：foreverpx
文章原文链接：[HEXO 指定404页面](http://www.foreverpx.cn/2014/09/23/hexo404)