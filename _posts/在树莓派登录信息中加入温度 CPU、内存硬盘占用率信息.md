---
title: 在树莓派登录信息中加入温度 CPU、内存硬盘占用率信息
date: 2021-10-08 16:56:59
categories: 树莓派
tags: [树莓派, Linux]
---

![13.png](https://github.com/Direct5dom/imageDB/blob/main/DB/13.png?raw=true)

更换成功的效果

<!--more-->

作为树莓派的重度用户，能在越多的地方显示树莓派的状态信息当然是一件好事。

控制树莓派登录欢迎信息的文件有两处

1. 位于`/etc/update-motd.d/`目录下的 shell 脚本
2. 上述脚本执行完毕后打印`/etc/motd`文本

新建一个欢迎脚本：

```zsh
$ sudo vim /etc/update-motd.d/11-info
```

部分源自 Adafruit 的开源项目修改而来（已停止维护）

```
#!/bin/sh
uptime | awk '{printf("\nCPU Load: %.2f\t", $(NF-2))}'
free -m | awk 'NR==2{printf("Mem: %s/%sMB %.2f%%\n", $3,$2,$3*100/$2)}'
cat /sys/class/thermal/thermal_zone0/temp|awk '{printf("CPU Temp: %.2f\t",$1/1000)}'
df -h | awk '$NF=="/"{printf "Disk: %.1f/%.1fGB %s\n\n", $3,$2,$5}'
```

添加执行权限：

```zsh
$ sudo chmod +x /etc/update-motd.d/11-info
```

取消原始静态欢迎信息（可选）：

```zsh
$ sudo mv /etc/motd /etc/motd.sample
```

更换后效果：

![13.png](https://github.com/Direct5dom/imageDB/blob/main/DB/13.png?raw=true)

---

参考资料：

[在树莓派登录信息中加入温度 CPU、内存硬盘占用率信息 | 树莓派实验室 (nxez.com)](https://shumeipai.nxez.com/2021/08/25/add-memory-and-disk-occupancy-info-to-login-info.html)
