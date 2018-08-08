title: Linux中npm出现npmlog找不到的解决方法
date: 2014-11-29 11:10:40
tags: [NodeJs,Npm,Linux]
categories: frontend
---
​[Linux中npm出现npmlog找不到的解决方法](http://www.foreverpx.cn/2014/11/29/Linux中npm出现npmlog找不到的解决方法)

##描述

今天在对nodejs项目进行服务器迁移的时，在新的linux服务器上输入 npm -v 命令后报 “cannot find module 'npmlog'....”的错误。找到的原因是拷贝过去的bin目录下的`npm`文件并没有链接到module目录下的`npm-cli.js`文件，所以导致在执行 `npm -v` 命令时，路径问题导致`npmlog`模块无法找到。

<!--more-->
##解决方法

解决的方法有两个，一个是删除bin目录下的npm文件，并用如下命令链接到`npm-cli.js`。

```bash
	ln -s ../lib/node_modules/npm/bin/npm-cli.js
```

经过试验发现，在服务器外解压缩nodejs官网下载的编译好的压缩包，再将解压的文件拷贝到服务器上时，bin下的`npm`是没有链接到`npm-cli.js`的。

第二种方式是将下载好的`node-v0.10.33-linux-x64.tar.gz`压缩包先放置在服务器上，再进行解压缩，此时`npm`是已经链接到`npm-cli.js`的。

看到命令行中显示如下则为成功。

```bash
[root@linux bin]# ll
npm -> ../lib/node_modules/npm/bin/npm-cli.js

[root@linux bin]# ./npm -v
v1.4.26
```

这两种拷贝方式导致不同的情况的原因不详，有待考究。


文章作者：foreverpx
文章原文链接：[Linux中npm出现npmlog找不到的解决方法](http://www.foreverpx.cn/2014/11/29/Linux中npm出现npmlog找不到的解决方法)