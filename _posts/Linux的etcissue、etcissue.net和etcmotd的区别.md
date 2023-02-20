---
title: Linux的/etc/issue、/etc/issue.net和/etc/motd的区别
date: 2021-10-08 16:49:59
categories: Linux
tags: [Linux]
---

![12.png](https://github.com/Direct5dom/imageDB/blob/main/DB/12.png?raw=true)

树莓派的默认登陆提示。

<!--more-->

Linux使用三个文件：`/etc/issue`、`/etc/issue.net`和`/etc/motd`来控制本地及远程登录前后的信息显示。新版本的还有动态motd：`/run/motd.dynamic`以及PAM模块来控制。

`/etc/issue`和`/etc/issue.net`这2个文件是在登录之前显示的，区别一个负责本地登录（TTY）前显示，一个负责网络登录（PTS）前显示。

> `/etc/issue.net`不支持转义字符

`/etc/motd`：这个文件是在登录之后显示的，不管是 TTY 还是 PTS 登录，也不管是 Telnet 或 SSH 都显示这个文件里面的信息。

> 在较新的Linux发行版中，这个功能被扩展了，有了动态motd和静态motd的区别，在Ubuntu 16.04.01 LTS中，仅仅启用了动态motd，而未启用静态motd

对于动态motd，无法直接修改。因为它是由`/etc/update-motd.d/`下的几个脚本文件来动态生成的。所以可以通过`/etc/update-motd.d/`下的脚本来控制信息的生成。

那么如何禁用该动态motd功能呢？方法是将`/etc/update-motd.d/`下的脚本移除或者去掉可执行权限。
