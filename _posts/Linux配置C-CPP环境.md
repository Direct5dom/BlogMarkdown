---
title: Linux配置C/C++环境
tags:
  - Linux
  - C
  - CPP
categories: Linux
date: 2021-10-12 18:26:08
---

![Linux-CPP.png](https://github.com/Direct5dom/imageDB/blob/main/DB/Linux-CPP.png?raw=true)

<!--more-->

在Windows下，拥有宇宙第一IDE——Visual Studio，直接使用VS编写C++是很容易的事情。

但是不是所有的代码都在VS的配置下简单，而且如果使用Linux就用不上VS这个最强IDE了。

本篇主要是以Kali-Linux为例，搭建Linux下的C/C++开发环境。

# 编译调试工具

## 编译器

安装GCC和G++

```zsh
sudo apt install gcc g++
```

## 调试器

安装GDB

```zsh
sudo apt install gdb
```

## 测试

一个简单的`Hello World!`示例代码：

```c++
#include<iostream>
int main()
{
    std::cout << "Hello World!" << std::endl;
    return 0;
}
```

编译运行：

```zsh
g++ helloworld.cpp -o helloworld_cpp
./helloworld_cpp
```

# 项目构建工具

安装Make和Cmake

```zsh
sudo apt install make cmake
```

## Make

make 是一个构建工具，它解释 Makefile 中的规则。在 Makefile 文件中描述了整个工程所有文件的编译顺序、编译规则。

Makefile 有自己的书写格式、关键字、函数。而且在 Makefile 中可以使用系统 shell 提供的任何命令来完成想要的工作。但是 Makefile 的编写难度较高。

## CMake

CMake是一个跨平台的构建工具，可以用简单的语句来描述所有平台的安装、编译过程。他能够输出各种各样的 Makefile 或者 project 文件，能测试编译器所支持的 C++ 特性。CMake 的组态档取名为 CMakeLists.txt

Cmake 并不直接构建出最终的软件，而是产生标准的构建档（如 Unix 的 Makefile 或 Windows Visual C++ 的 projects/workspaces），然后再依一般的构建方式使用。这使得熟悉某个集成开发环境（IDE）的开发者可以用标准的方式建构他的软件，这种可以使用各平台的原生建构系统的能力是 CMake 和其他类似系统的区别之处。

CMake 可以脱离 IDE 构建项目，目前大部分常用的 IDE 直接支持 CMake。

