---
title: 在树莓派4B上使用HC-SR04超声波测距模块
date: 2021-10-31 20:30:30
tags:
  - 树莓派
categories: 树莓派
---


<img src="https://github.com/Direct5dom/imageDB/blob/main/DB/raspberry-pi-logo-480.png?raw=true" alt="raspberry-pi-logo-1920.png" style="zoom: 50%;" />

<!--more-->

# HC-SR04技术原理

![HC-SR04](https://github.com/Direct5dom/imageDB/blob/main/DB/HC-SR04.png?raw=true)

## 简介

HC-SR04 模块可以测量 3cm – 4m 的距离，精确度可以达到 3mm。这个模块包括 超声波发射器、超声波接收器和控制电路三部分。有 4 个引脚。

## 参数表

| 项目                | 参数         |
| ------------------- | ------------ |
| 工作电压            | DC 5V        |
| 工作电流            | 15mA         |
| 最短测量距离        | 3cm          |
| 最长测试距离        | 4m           |
| 测量角度            | 15°          |
| Trigger引脚输入信号 | 10us TTL脉冲 |
| Echo引脚输出信号    | 5V 脉冲信号  |

## 引脚定义

| 引脚 | 功能                     |
| ---- | ------------------------ |
| VCC  | 控制电压，DC 5V          |
| Trig | 接收来自树莓派的控制信号 |
| Echo | 发送测距结果给树莓派     |
| Gnd  | 接地                     |

## 物理原理

![超声波测距物理原理](https://github.com/Direct5dom/imageDB/blob/main/DB/%E8%B6%85%E5%A3%B0%E6%B3%A2%E6%B5%8B%E8%B7%9D%E7%89%A9%E7%90%86%E5%8E%9F%E7%90%86.png?raw=true)

## 电子原理

1. 树莓派向Trig引脚发送一个持续10us的脉冲信号。

2. HC-SR04接收到树莓派发送的脉冲信号，开始发送超声波（start sending ultrasoun），并把Echo置为高电平。然后准备接收返回的超声波。

3. 当HC-SR04接收到返回的超声波（receive returned ultrasound）时，把Echo置为低电平。

从上述过程可以看出， Echo高电平持续的时间就是超声波从发射到返回所经过的时间间隔。

![HC-SR04电子原理](https://github.com/Direct5dom/imageDB/blob/main/DB/HC-SR04%E7%94%B5%E5%AD%90%E5%8E%9F%E7%90%86.png?raw=true)

# 接线与程序

## 接线

![](https://github.com/Direct5dom/imageDB/blob/main/DB/HC-SR04%E8%BF%9E%E6%8E%A5%E6%A0%91%E8%8E%93%E6%B4%BEGPIO-2.png?raw=true)

> GPIO可以自由选择，由程序决定。

## 程序

> 程序来自：[树莓派上使用HC-SR04超声波测距模块 | 树莓派实验室 (nxez.com)](https://shumeipai.nxez.com/2019/01/02/hc-sr04-ultrasonic-ranging-module-on-raspberry-pi.html)

```python
#导入 GPIO库
import RPi.GPIO as GPIO
import time
  
#设置 GPIO 模式为 BCM
GPIO.setmode(GPIO.BCM)
  
#定义 GPIO 引脚
GPIO_TRIGGER = 23
GPIO_ECHO = 24
  
#设置 GPIO 的工作方式 (IN / OUT)
GPIO.setup(GPIO_TRIGGER, GPIO.OUT)
GPIO.setup(GPIO_ECHO, GPIO.IN)
  
def distance():
    # 发送高电平信号到 Trig 引脚
    GPIO.output(GPIO_TRIGGER, True)
  
    # 持续 10 us 
    time.sleep(0.00001)
    GPIO.output(GPIO_TRIGGER, False)
  
    start_time = time.time()
    stop_time = time.time()
  
    # 记录发送超声波的时刻1
    while GPIO.input(GPIO_ECHO) == 0:
        start_time = time.time()
  
    # 记录接收到返回超声波的时刻2
    while GPIO.input(GPIO_ECHO) == 1:
        stop_time = time.time()
  
    # 计算超声波的往返时间 = 时刻2 - 时刻1
    time_elapsed = stop_time - start_time
    # 声波的速度为 343m/s， 转化为 34300cm/s。
    distance = (time_elapsed * 34300) / 2
  
    return distance
  
if __name__ == '__main__':
    try:
        while True:
            dist = distance()
            print("Measured Distance = {:.2f} cm".format(dist))
            time.sleep(1)
  
        # Reset by pressing CTRL + C
    except KeyboardInterrupt:
        print("Measurement stopped by User")
        GPIO.cleanup()
```

