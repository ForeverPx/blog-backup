title: Mongodb之安装配置
date: 2014-10-15 14:56:12
tags: [mongodb]
categories: server
---
[Mongodb之安装配置](http://www.foreverpx.cn/2014/10/15/Mongodb之安装配置)

###安装
Mongodb的下载地址为[Mongodb官网](http://www.mongodb.org/)。下载时，你可以选择是安装包或者是压缩包。

下载完成后，双击安装包并安装。

安装完成后，你可以在安装目录看到下图中所见的目录

<!--more-->

![截图1](http://ww2.sinaimg.cn/large/005yyi5Jjw1elbv1iz27oj30mn09ojsz.jpg)

###配置环境变量
目录中exe可以理解为mongodb提供给开发者的工具集，为了更方便的使用这些工具，我们将这个目录加入到环境变量中，这样我们就可以在全局运行这些工具。
![截图2](http://ww3.sinaimg.cn/large/005yyi5Jjw1elbv9xrctgj309x041jrj.jpg)

###建立存储目录
在启动mongodb服务之前，我们要先指定mongodb数据存储的地方。
在E盘建立存放目录 **E:\mongodb\data\ **

###启动mongo服务
打开CMD，输入 mongod.exe --dbpath=E:\mongodb\data\  ， 回车开始启动服务
如果启动成功会显示如下图：
![截图3](http://ww4.sinaimg.cn/large/005yyi5Jjw1elbvgl3e46j30i008atbs.jpg)
可以看到图中的log显示，服务已开启并监听27017端口。
在浏览器中输入 http://localhost:27017
如果会出现
**It looks like you are trying to access MongoDB over HTTP on the native driver port.**
则表示服务启动成功。

###进入mongodb命令行
新开一个CMD窗口，输入 mongo.exe即可进入命令行
![截图4](http://ww3.sinaimg.cn/large/005yyi5Jjw1elbvkxbpxvj30am01qjre.jpg)

文章作者：foreverpx
文章原文链接：[Mongodb之安装配置](http://www.foreverpx.cn/2014/10/15/Mongodb之安装配置)