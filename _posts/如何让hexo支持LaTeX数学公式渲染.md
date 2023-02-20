---
title: 如何让hexo支持LaTeX数学公式渲染
date: 2021-09-04 23:19:58
tags: [hexo, 博客搭建]
categories: hexo
mathjax: true
---

![Hexo+LaTeX.png](http://github.com/Direct5dom/imageDB/blob/main/DB/Hexo+LaTeX.png?raw=true)

Hexo + $\LaTeX$

<!--more-->

hexo的默认渲染器是`marked`，并不支持用于渲染数学公式的`mathjax`。

想要让hexo支持数学公式的渲染，就需要更换渲染器。

可更换的渲染器有很多，我这里只列举个人试验成功且自认为最简单的方案。

# 安装过程

## 安装新渲染器

`kramed`是在`marked`基础上修改的，支持了`mathjax`。你的hexo工程目录下的node_modules中可以找到对应的渲染器文件夹。

可以在你的工程目录下用以下命令卸载`marked`安装`kramed`：

```shell
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

## 修改配置文件

到主题配置文件中`_config.yml`，找到`mathjax`的部分，将`mathjax`的`enable`设为`true`。

```yaml
...
# Math Formulas Render Support
math:
  # Default (true) will load mathjax / katex script on demand.
  # That is it only render those page which has `mathjax: true` in Front-matter.
  # If you set it to false, it will load mathjax / katex srcipt EVERY PAGE.
  per_page: true

  # hexo-renderer-pandoc (or hexo-renderer-kramed) required for full MathJax support.
  mathjax:
    enable: true
    # See: https://mhchem.github.io/MathJax-mhchem/
    mhchem: false

  # hexo-renderer-Markdown-it-plus (or hexo-renderer-Markdown-it with Markdown-it-katex plugin) required for full Katex support.
  katex:
    enable: false
    # See: https://github.com/KaTeX/KaTeX/tree/master/contrib/copy-tex
    copy_tex: false
...
```

## 文章渲染标签

为加快渲染速度，渲染器只会在标签中有`mathjax: true`的文章中使用利用`mathjax`渲染。

```
title: 如何让hexo支持LaTeX数学公式渲染
date: 2021-09-04 23:19:58
tags: [hexo, 博客搭建]
categories: hexo
mathjax: true
```

# 使用

$\LaTeX$公式属于Markdown的扩展语法，主要有两种使用方式：**内联公式**和**行间公式**

## 内联公式

主要是在行内所显示的公式，比如可以使用$\LaTeX$语法写$y=kx+b$。

上面这行的Markdown源码为：

```markdown
主要是在行内所显示的公式，比如可以使用$\LaTeX$语法写$y=kx+b$。
```

即：单个`$`所括起来的内容便是内联公式。

## 行间公式

主要是在行间所显示的公式，比如可以：
$$
\LaTeX =kx+b
$$
上面的Markdown源码为：

```markdown
$$
\LaTeX =kx+b
$$
```

即：两个`$`所括起来的内容便是行间公式。

# 问题解决

## 大括号渲染出错

可能是正则表达式的问题，导致

```LaTeX
\{ \}
```

无法正常转义。

想要解决这个问题，只需要用别的LaTeX定界符即可：

```LaTeX
\lbrace \rbrace
```

## 多行公式无法换行

同样是正则表达式的问题，`\\`被转译成了`\`。

简单粗暴的解决办法就是把`\\`换成`\\\\`，来暴力解决。

# Try and have Fun

最后就可以开心使用了：
$$
\lbrace \LaTeX \rbrace \\\\
$$
