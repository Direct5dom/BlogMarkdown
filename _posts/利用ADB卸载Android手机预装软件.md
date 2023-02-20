---
title: 利用ADB卸载Android手机预装软件
tags:
  - Android
categories: Android
date: 2021-10-30 19:24:23
---

国内手机预装软件乱象越来越严重，预装且不能卸载的软件也越来越多。这些预装的软件可能到手机报废都不会点开，但是其隐私问题、安全问题确实不可忽视的。恰逢最近软件洁癖犯了，便顺手清理了手头两部手机（小米11、荣耀V20）的全部用不到的预装软件。

<!--more-->

## 准备工作

### `ADB`

Android调试桥（Android Debug Bridge，简称ADB）是用于链接Android手机进行开发调试的工具。其随Android Studio附带，也可单独部署使用。

下载链接：[Android 调试桥 (ADB)  | Android Open Source Project](https://source.android.google.cn/setup/build/adb?hl=zh-cn#download-adb)

### 手机开发者模式

进入手机的`设置` -> `系统` -> `关于手机` -> 连续点击手机的版本号即可解锁。

然后在手机的`设置` -> `开发者选项`中，开启`USB调试`。

然后将手机通过USB数据线连接到电脑。

### 连接设备

使用ADB命令：

```zsh
adb devices
```

并收到类似下方提示代表成功：

```zsh
adb devices
List of devices attached
3c272e2e        device
```

### 监控软件动态

ADB命令卸载软件是通过软件包名来识别的，软件包名并非软件名，因此我们需要通过监视器命令来获取：

```zsh
adb shell am monitor
```

当你输入完以上命令后，随便运行一个软件，其运行状态和包名就会显示出来：

```
Monitoring activity manager...  available commands:
(q)uit: finish monitoring
** Activity starting: com.miui.player
** Activity starting: com.miui.player
** Activity starting: com.miui.player
** Activity starting: com.miui.player
** Activity resuming: com.miui.player
```

### 卸载软件

使用ADB命令：

```zsh
adb shell pm uninstall --user 0 <软件包名>
```

如果输入后回复`Success`，则代表卸载成功。

---

## 多个设备的情况

在连接设备的时候可能出现有多个设备的情况：

```zsh
adb devices
List of devices attached
3c272e2e        device		# 手机
127.0.0.1:58526 device		# WSA
```

这就要求我们在之后的命令中指明对哪个设备进行操纵，例如：

```zsh
adb -s 3c272e2e shell am monitor
```

## 列出所有应用

```sh
adb shell pm list packages
```

