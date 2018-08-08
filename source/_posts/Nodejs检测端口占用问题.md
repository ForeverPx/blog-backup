title: Nodejs检测端口占用问题
date: 2018-08-04 12:11:24
tags: [Nodejs]
categories: frontend
---

## 问题：

在Windows机器上装了我们的PC端（Electron）应用（TcpServer默认监听20000端口），随后启动应用，发现安卓端登录学生账号后发现无法正常连接pc端（Tcp通信）。


## 系统环境：

Win7 64位

 <!--more-->

## 原因：

对nodejs中net.createServer的listen方法认知不全面，导致20000端口被其他应用程序占用，但同时PC端的端口检查更换机制既没有生效，也没有报错。

Listen方法可以指定host。如果你不指定，程序会创建 0.0.0.0 和 172.18.96.46 两个ip对于port的监听，此时如果 0.0.0.0:port 端口被其他程序占用，API并不会抛出异常



## 解决方案：

显示在listen端口的时候，指定host

const server = net.createServer().listen(port, '0.0.0.0');



## 过程：

通过在PC端的log中我们发现，Android端并没有与PC端的TcpServer创建socket连接，根据经验来看，有以下两种可能

1.一体机和安卓平板不在一个内网段，无法互相ping通

2.一体机防火墙未关闭


经过了互ping和查看防火墙设置，排除了上述两个问题点。


接下来对tcp请求进行抓包，找到了一些规律

![这里写图片描述](https://img-blog.csdn.net/20180508205154343?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZvcmV2ZXJDamw=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

PS： 172.18.96.46为PC端IP      172.18.102.211为Android端IP

根据图中可以看出，Android端在与PC端建立连接之后，不久之后就断开了。一直重复这个过程。

在每次断开之前，两个端互发了一条数据，长度为114和135。

![这里写图片描述](https://img-blog.csdn.net/20180508205229960?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZvcmV2ZXJDamw=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

第一个请求是Android端向PC端发送，数据内容如下：

![这里写图片描述](https://img-blog.csdn.net/20180508205250807?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZvcmV2ZXJDamw=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

数据内容为Android端向PC端发送是哪个学生在请求连接，所以这里没问题，再看下一个请求内容。

![这里写图片描述](https://img-blog.csdn.net/20180508205311890?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ZvcmV2ZXJDamw=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里发现了问题，这个含有 customError 的消息内容，并不是我们PC端程序发送的。。

所以可以判断出，20000端口被其他程序使用了，Android的消息发到了其他程序。

通过 netstat -ano 和 tasklist 命令发现如下信息

ip:port            pid

0.0.0.0:20000  5444


pid            name

5444         Service.exe

找到了占用20000端口的进程，是另一个服务，进程名称为Service.exe。


在原有的程序设计中，我们在创建TcpServer之前，会通过 net.createServer().listen(port)的方法去检查默认端口20000是否被占用。

如果被占用，程序会抛出错误，我们在捕获错误的时候，会在待选端口列表中选出下一个再进行尝试，以此类推，知道有一个端口可用。


在之前对这个功能的测试中，我们通过 net.createServer().listen(port) 方法先创建了一个tcpserver后，如果再调用该方法监听同一个端口，发现我们的策略是有效的。

但是这里却失效了，20000被占用后，tcpserver依然正常创建并监听，但是收不到消息。


通过查阅Nodejs net.createServer().listen 方法文档，发现listen方法可以指定host。如果你不指定，程序会创建 0.0.0.0 和 172.18.96.46 两个ip对于port的监听，此时如果 0.0.0.0:port 端口被其他程序占用，API并不会抛出异常。

所以解决方案是在调用listen方法的时候，显示指定host。此时如果端口被其他程序占用，就会抛出异常。

const server = net.createServer().listen(port, '0.0.0.0');

server.on('error', function(){

    //try next port

})