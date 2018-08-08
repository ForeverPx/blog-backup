title: 解决Mac_java.net_Local_host_name_unknown_error的方法
date: 2015-05-22 20:09:14
tags: [Java,Tomcat]
categories: frontend
---
解决Mac java.net Local host name unknown error的方法

#现象
在Mac上启动tomcat时，报了如下错误：

*Error: Exception thrown by the agent : java.net.MalformedURLException: Local host name unknown: java.net.UnknownHostException: XXXX: XXXX: nodename nor servname provided, or not known*

<!--more-->

#解决方法
查看 */ect/hosts* 文件的内容:
*127.0.0.1 localhost*
好像没什么异常，但是通过
```bash
scutil ––get HostName 
```
命令查看返回的确实空，所以只有手动设置默认的host了
```bash
scutil ––set HostName "localhost"
```
此处的localhost必须存在于hosts文件中。

文章作者：forevercjl
文章原文csdn链接：[http://www.foreverpx.cn/2015/05/22/解决Mac_java.net_Local_host_name_unknown_error的方法](http://www.foreverpx.cn/2015/05/22/解决Mac%20java.net%20Local%20host%20name%20unknown%20error的方法)
转载请注明出处。

