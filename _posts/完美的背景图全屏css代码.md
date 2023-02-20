---
title: 完美的背景图全屏CSS代码
tags:
  - CSS
  - HTML
categories: HTML/CSS
date: 2022-05-23 22:04:32
---


![background.jpg (5760×2775) (direct5dom.github.io)](https://direct5dom.github.io/images/background.jpg)

> 此博客的背景图采用CSS3.0方式实现

<!--more-->

在写主题样式的时候经常会碰到用背景图铺满整个背景的需求，这里分享下使用方法

需要的效果

1. 图片以背景的形式[铺满整个屏幕](https://huilang.me/perfect-full-page-background-image/)，不留空白区域
2. 保持图像的纵横比（图片不变形）
3. 图片居中
4. 不出现滚动条
5. 多浏览器支持

以图片bg.jpg为例

## CSS3.0

归功于css3.0新增的一个属性[background-size](https://huilang.me/perfect-full-page-background-image/)，可以简单的实现这个效果，这里用fixed和center定位背景图，然后用background-size来使图片铺满，具体css如下

```css
html {
  background: url(bg.jpg) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
```

这段样式适用于以下浏览器

- Safari 3+
- Chrome
- IE 9+
- Opera 10+ (Opera 9.5 支持background-size属性 但是不支持cover)
- Firefox 3.6+

这里你会发现ie8及以下版本不支持，这些蛋疼浏览器则需要添加下面的css来设置兼容

```css
filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='.bg.jpg', sizingMethod='scale');
-ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='bg.jpg', sizingMethod='scale')";
```

这个用滤镜来兼容的写法并不是很完美，首先是图片路径，这里只能是相对于根目录的路径，或者用绝对路径；然后是图片纵横比改变了，是拉伸铺满的形式。尽管如此，总比留空白好多了吧（如果背景图bg.jpg的宽高够大，则可以不用这段，变成简单的平铺，比图片变形效果好写，大家可以尝试下）

## 用img形式来实现背景平铺效果

首先在html中加入以下代码

```html
<img src="bg.jpg" class="bg">
```

然后通过css来实现铺满效果（假设图片宽度1024px）

```css
img.bg {
    min-height: 100%;
    min-width: 1024px;
    width: 100%;
    height: auto;
    position: fixed;
    top: 0;
    left: 0;
}
```

下面这个是为了屏幕小于1024px宽时，图片仍然能居中显示（注意上面假设的图片宽度）

```css
@media screen and (max-width: 1024px) {
  img.bg {
    left: 50%;
    margin-left: -512px;
  }
}
```

兼容以下浏览器

- 以下浏览器的所有版本: Safari / Chrome / Opera / Firefox
- IE9+
- IE 7/8: 平铺效果支持，但是在小于1024px的屏幕下居中效果失效

## **JQ模拟的方法**

html部分

```html
<img src="bg.jpg" id="bg" alt="">
```

css部分

```css
#bg { position: fixed; top: 0; left: 0; }
.bgwidth { width: 100%; }
.bgheight { height: 100%; }
```

js部分

```js
$(window).load(function() {
    var theWindow        = $(window),
        $bg              = $("#bg"),
        aspectRatio      = $bg.width() / $bg.height();
    function resizeBg() {
        if ( (theWindow.width() / theWindow.height()) < aspectRatio ) {
            $bg
                .removeClass()
                .addClass('bgheight');
        } else {
            $bg
                .removeClass()
                .addClass('bgwidth');
        }
    }
    theWindow.resize(resizeBg).trigger("resize");
});
```

