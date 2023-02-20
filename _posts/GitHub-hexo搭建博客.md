---
title: GitHub+hexo搭建博客
date: 2021-09-03 16:15:55
categories: hexo
tags: [hexo, GitHub, 博客搭建]
---

参考：[【保姆级】利用Github搭建自己的个人博客，看完就会 - Python研究者 - 博客园 (cnblogs.com)](https://www.cnblogs.com/chenlove/p/15058170.html)

## 1、环境准备

### 1.安装Windows Terminal

在Windows Store下载安装Windows Terminal（个人更加喜欢在Windows Terminal下工作，也可以不使用）

### 2.安装Git

1. 在[Git官网](https://git-scm.com/)下载安装Git。

2. 安装好后打开Windows Terminal，可以看到Git Bash的选项。在Git Bash中输入`git --version`，显示版本号即代表Git安装成功。


<!--more-->

### 3.安装`node.js`

1. 在[node.js官网](https://nodejs.org/en/download/)下载安装`node.js`。
2. 安装好后在终端输入`node -v`，出现版本号即代表`node.js`安装成功。

### 4.安装hexo及其依赖

1. 安装Hexo。在终端输入：`npm install hexo -g`
2. 安装好后在终端输入`hexo -v`，出现版本号即代表hexo安装成功。
3. 安装hexo依赖。在终端输入：`npm install --save hexo-deployer-git`

## 2、Git配置SSH Key

>为什么要配置SSH key？
>
>目的：可以免密的将本地的源码和资源上传到github，无需要每次都输账号和密码。

可以通过命令`cd ~/.ssh`先看本地是否配置好SSH key，若没配置好则会提示`No such file or directory`。

>备注：
>
>因为ssh配置好之后是保存到 c:/用户/Administrator/.ssh

### 1.配置SSH

1. 生成SSH Key。

   ```shell
   ssh-keygen -t rsa -C "邮件地址"
   ```

   > 这里的邮件地址是github账号绑定的邮件地址

   输入命令后连续按三次回车即可。

2. 此时在`C:/User/你的用户名/`生成了一个`.ssh`文件夹，打开这个文件夹可以看到一个名为`id_rsa.pub`，这里面存有你需要的SSH Key。

3. 打开GitHub主页，点击个人设置，点击左侧的SSH and GPG keys，点击New SSH key

4. 将`id_rsa.pub`的内容粘到Key中，Title可以随意取，最后点击Add SSH Key即可。

5. 接下来可以查看是否成功，输入命令：

   ```shell
   ssh -T git@github.com
   ```

   接着输入`yes`，若成功可以看到如下提示：

   ```
   Hi Direct5dom! You've successfully authenticated, but GitHub does not provide shell access
   ```

6. 配置账号和密码：

   ```shell
   $ git config --global user.name "你的github用户名" 
   $ git config --global user.email "你的github注册邮箱" 
   ```

## 3、搭建个人博客

### 1.新建博客

新建一个目录用于保存博客（**注意不是GitHub仓库的目录**），并通过Git Bash进入该目录。

#### 1.初始化个人博客

```shell
hexo init
```

> 生成静态网页
>
> ```shell
> hexo g
> ```
>
> 预览
>
> ```
> hexo s
> ```
>
> ##### 报错解决：hexo g报错,line.mathALL is not funciton问题解决
>
> 原因：nodejs版本低于12
>
> 解决：两种方法
>
> 方法1）请将nodejs升级到高于12.0.0的版本
>
> 方法2）config.xml中的 highlight->enable的值从true更改为false，这样可以避免异常。

#### 2.部署到GitHub

##### 1.新建GitHub库

仓库名称格式**特别注意不能乱起名**： 用户名.github.io

这个仓库名称将作为你GitHub博客的访问地址。

##### 2.编辑`_config.yml`

`_config.yml`在博客存放目录下

在里面找到`deploy`的部分，改为：

```yaml
deploy:
  type: git
  repository: git@github.com:xxx/xxx.github.io.git
  branch: main
```

repository仓库地址改为自己的。

branch看自己的github仓库是master还是main，按照自己的情况填写。

##### 3.发布到GitHub

```shell
hexo d
```

然后在浏览器访问`xxx.github.io`，成功访问即代表部署成功。

> 若出现报错`ERROR Deployer not found: git`，则代表依赖项没安装好，输入命令：
>
> ```
> npm install --save hexo-deployer-git
> ```

## 4、编写博客

### 1.新建博文

```shell
hexo new '博文标题'
```

然后会在`./source/_posts`下生成一个`.md`文件，打开即可编写。

### 2.部署到GitHub

#### 1.生成html文件

```
hexo g
```

#### 2.上传到github

```
hexo d
```

即可将博文同步到GitHub平台。

## 附录：

### heox的基本命令

```shell
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

对应的缩写，比如：

```powershell
hexo n == hexo new
hexo g == hexo generate
```
