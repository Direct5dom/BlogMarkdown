---
title: 关于VNC Viewer连接树莓派无法调整分辨率以及指针变成X
date: 2021-09-09 03:07:21
categories: 树莓派
tags: [树莓派]
---

<img src="https://github.com/Direct5dom/imageDB/blob/main/DB/raspberry-pi-logo-1920.png?raw=true" alt="raspberry-pi-logo-1920.png" style="zoom:25%;" />

使用VNC Viewer连接树莓派，但分辨率太低，修改树莓派本身的分辨率并reboot后依然是默认的低分辨率。

<!--more-->

# 解决方法

`raspi-config`中修改的是树莓派的分辨率，修改成功后树莓派本身的分辨率（包括连接显示器时的分辨率）会发生改变，但不会影响VNC端的分辨率。

修改VNC端的分辨率需要`vncserver`这条命令后添加参数，但和这条命令本身一样，每次开机均需要再输入一次：

```bash
vncserver -geometry 1920x1080 #修改分辨率，并开启:1端口
```

关于指针问题，在VNC远程桌面上，打开终端输入：

```bash
lxappearance #解决连接后指针变成X的问题
```

