---
title: 基于Aplayer在hexo博客上添加音乐播放器并全局适用不中断播放
date: 2021-09-07 04:28:10
tags: [hexo, 博客搭建]
categories: hexo
---

hexo博客建好了，除了视觉美化最好还有声觉美化，也就是BGM。早在当年QQ空间时代就已经开始弄BGM了，到了hexo这里自然不能落下。

今天用到的一个js组件是aplayer，一个非常漂亮好用的HTML5音乐播放器，和dplayer师出同门。

<!--more-->

## 下载部署

虽然理论上可以通过npm安装，但是我这里通过npm安装无法实现想要的效果，因此选择提取dist文件的方式部署。

在[APlayer GitHub发布页面](https://github.com/DIYgod/APlayer/releases)下载源码，将其中的`dist`文件夹解压到`hexo/themes/next/source`，即Next主题的`source`文件夹下。

## 创建播放器样式

新建播放器样式的js文件，在`hexo/themes/next/source/dist`文件夹中新建`music.js`，在其中添加样式代码。

播放器样式代码完整调用及说明：

```js
const ap = new APlayer({
    container: document.getElementById('player'),	//播放器HTML容器元素
    mini: false,		//迷你模式开关
    autoplay: false,	//自动播放开关（Chromium现已禁止自动播放）
    theme: '#FADFA3',	//主题色
    loop: 'all',		//音频循环播放, 可选值: 'all'全部循环, 'one'单曲循环, 'none'不循环
    order: 'random',	//音频循环顺序, 可选值: 'list'列表循环, 'random'随机循环
    preload: 'auto',	//预加载，可选值: 'none', 'metadata', 'auto'
    volume: 0.7,		//默认音量，请注意播放器会记忆用户设置，用户手动设置音量后默认音量即失效
    mutex: true,		//互斥，阻止多个播放器同时播放，当前播放器播放时暂停其他播放器
    listFolded: false,	//列表默认折叠
    listMaxHeight: 90,	//列表最大高度
    lrcType: 3,			//歌词传递方式
    audio: [			////音频信息,包含以下
        {
            name: 'name1',			//音频名称
            artist: 'artist1',		//音频艺术家
            url: 'url1.mp3',		//音频外链
            cover: 'cover1.jpg',	//音频封面
            lrc: 'lrc1.lrc',		//音频歌词，配合上面的lrcType使用
            theme: '#ebd0c2'		//切换到此音频时的主题色，比上面的 theme 优先级高
        },
        {		//如果只有一首歌，删掉这一块，如有更多歌曲按此格式逐渐往下添加
            name: 'name2',
            artist: 'artist2',
            url: 'url2.mp3',
            cover: 'cover2.jpg',
            lrc: 'lrc2.lrc',
            theme: '#46718b'
        }
    ]
});
```

更多内容请参考[APlayer中文文档](https://aplayer.js.org/#/zh-Hans/?id=安装)

## 部署到博客

按照上面的设置，已经将播放器样式设置好了，这时候只需要将播放器部署到合适的位置即可。

下面是部署到页面的代码：

```html
<link rel="stylesheet" href="/dist/APlayer.min.css">
<div id="aplayer"></div>
<script type="text/javascript" src="/dist/APlayer.min.js"></script>
<script type="text/javascript" src="/dist/music.js"></script>
```

可以将其放在`hexo/themes/next/layout/***.swig`，在不同.swig文件中的效果也不同。

个人建议放置在`hexo/themes/next/layout/_partials/header/brand.swig`中，因为brand是全局存在的，这样设置的播放器全局都能看到：

```html
{%- if theme.favicon.apple_touch_icon %}
  <link rel="apple-touch-icon" sizes="180x180" href="{{ url_for(theme.favicon.apple_touch_icon) }}">
    <!--音乐播放器-->
      <link rel="stylesheet" href="/dist/APlayer.min.css">
      <div id="aplayer"></div>
      <script type="text/javascript" src="/dist/APlayer.min.js"></script>
      <script type="text/javascript" src="/dist/music.js"></script>
      <!--音乐播放器-->
{%- endif %}
```



## 利用`pajx`实现全局播放不暂停

在切换博文页面的时候，一般情况下都会整页刷新，这导致我们的播放器组件每次切换页面的时候都被”重置“了一次，为了避免这个重置，我们要让浏览器的部分内容（音乐播放器）在切换页面的时候不刷新，换个角度来说就是每次只让页面发生部分内容更新。

`pjax`是一个jQuery插件，它的工作是让页面上的页面资源不被重新执行或应用，并且每次切换页面都部分重新渲染页面。这样就可以避免我们正在播放的音乐播放器随着页面切换被刷新掉了。

我这里同样将`pajx`部署在`hexo/themes/next/layout/_partials/head/head.swig`中：

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=2">
<meta name="theme-color" content="{{ theme.android_chrome_color }}">
<meta name="generator" content="Hexo {{ hexo_version }}">
  <!--pjax：防止跳转页面音乐暂停-->
  <script src="https://cdn.jsdelivr.net/npm/pjax@0.2.8/pjax.js"></script> 
```

> 网上很多教程部署在了`_layout.swig`中，但实测这样并没有全局应用，从部分页面进入的时候`pajx`并没有部署好。因此和播放器本身一样，我将其都部署在了head中，因为每个页面都存在head，所以部署在head中的内容从那个页面开始访问都是会被调用出来的。

## 参考

[1] [hexo上的aplayer应用 | Y's BLOG (yleao.com)](https://blog.yleao.com/2018/0902/hexo上的aplayer应用.html)

[2] [APlayer中文文档](https://aplayer.js.org/#/zh-Hans/?id=安装)
