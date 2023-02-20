---
title: 常见网络摄像机(摄像头)的端口及RTSP地址
categories: 监控
tags: [监控]
date: 2023-01-16 18:35:29
---


转载自：[常见网络摄像机(摄像头)的端口及RTSP地址 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/366528096)

<!--more-->

> 根据评论区反应，这写地址并不完全，有些过时。
>
> YOOSEE的可以成功连接我自己的监控，此博文只做备份。

## 海康威视

默认IP地址：192.168.1.64/DHCP 用户名admin 密码自己设
端口：“HTTP 端口”(默认为 80)、“RTSP 端口”(默认为 554)、“HTTPS 端 口”(默认 443)和“服务端口”(默认 8000)，ONVIF端口 80。
RTSP地址：rtsp://[username]:[password]@[ip]:[port]/[codec]/[channel]/[subtype]/av_stream
说明：
username: 用户名。例如admin。
password: 密码。例如12345。
ip: 为设备IP。例如 192.0.0.64。
port: 端口号默认为554，若为默认可不填写。
codec：有h264、MPEG-4、mpeg4这几种。
channel: 通道号，起始为1。例如通道1，则为ch1。
subtype: 码流类型，主码流为main，辅码流为sub。
例如，请求海康摄像机通道1的主码流，Url如下
主码流：
rtsp://admin:12345@192.0.0.64:554/h264/ch1/main/av_stream
子码流：
rtsp://admin:12345@192.0.0.64/mpeg4/ch1/sub/av_stream

## 大华

默认IP地址：192.168.1.108 用户名/密码：admin/admin
端口：TCP 端口 37777/UDP 端口 37778/http 端口 80/RTSP 端口号默认为 554/HTTPs 443/ONVIF 功能默认为关闭，端口80
RTSP地址：rtsp://username:password@ip:port/cam/realmonitor?channel=1&subtype=0
说明:
username: 用户名。例如admin。
password: 密码。例如admin。
ip: 为设备IP。例如 10.7.8.122。
port: 端口号默认为554，若为默认可不填写。
channel: 通道号，起始为1。例如通道2，则为channel=2。
subtype: 码流类型，主码流为0(即subtype=0)，辅码流为1(即subtype=1)。
例如，请求某设备的通道2的辅码流，Url如下
rtsp://admin:admin@10.12.4.84:554/cam/realmonitor?channel=2&subtype=1

## 雄迈/巨峰

默认IP地址：192.168.1.10 用户名admin 密码空
端口：TCP端口：34567 和 HTTP端口：80，onvif端口是8899
RTSP地址：rtsp://10.6.3.57:554/user=admin&password=&channel=1&stream=0.sdp?
10.6.3.57这个是被连接的设备的IP
554这个是RTSP服务的端口号，可以在设备的网络服务里面更改
user=admin这个是设备的登录用户名
password= 密码空
channel=1 第一通道
stream=0.sdp？主码流
stream=1.sdp？副码流
图片抓拍地址：http://ip/webcapture.jpg?command=snap&channel=1

## 天视通

默认IP地址：192.168.0.123 用户名admin 密码123456
端口：http端口80 数据端口8091 RTSP端口554 ONVIF端口 80
RTSP地址：主码流地址:rtsp://192.168.0.123:554/mpeg4
子码流地址:rtsp://192.168.0.123:554/mpeg4cif
需要入密码的地址： 主码流 rtsp://admin:123456@192.168.0.123:554/mpeg4
子码流 rtsp://admin:123456@192.168.0.123:554/mpeg4cif
图片抓拍地址：http://ip/snapshot.cgi

## 中维/尚维

默认IP地址：DHCP 默认用户名admin 默认密码 空
RTSP地址：rtsp://0.0.0.0:8554/live1.264(次码流)
rtsp://0.0.0.0:8554/live0.264 (主码流)

## 九安

RTSP地址：rtsp://IP:port(website port)/ch0_0.264(主码流)
rtsp://IP:port(website port)/ch0_1.264(子码流)

## 技威/YOOSEE

默认IP地址：DHCP 用户名admin 密码123
RTSP地址：主码流：rtsp://IPadr:554/onvif1
次码流：rtsp://IPadr:554/onvif2
onvif端口是5000
设备发现的端口是3702

## V380

默认IP地址：DHCP 用户名admin 密码空/admin
onvif端口8899
RTSP地址：主码流rtsp://ip//live/ch00_1
子码流rtsp://ip//live/ch00_0

## 宇视

默认IP地址： 192.168.0.13/DHCP 默认用户名 admin 和默认密码 123456
端口：HTTP 80/RTSP 554/HTTPS 110(443)/onvif端口 80
RTSP地址：rtsp://用户名:密码@ip:端口号/video123 123对应3个码流

## 天地伟业

默认IP地址：192.168.1.2 用户名“Admin”、密码“1111”
onvif端口号“8080”
RTSP地址：rtsp：//192.168.1.2

## 巨龙/JVT

默认IP地址：192.168.1.88 默认用户名 admin 默认密码admin
RTSP地址：
主码流地址:rtsp://IP地址/av0_0
次码流地址:rtsp://IP地址/av0_1
onvif端口 2000

图片抓拍地址：http://ip/capture/webCapture.jpg?channel=1&FTpsend=0&checkinfo=0
(http://ip/cgi-bin/images_cgi?channel=1&user=admin&pwd=admin)
————————————————
版权声明：本文为CSDN博主「大白菜！」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_45061258/article/details/111399841
