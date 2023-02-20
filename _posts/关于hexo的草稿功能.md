---
title: 关于hexo的草稿功能
date: 2021-09-04 17:06:10
tags: [hexo, 博客搭建]
categories: hexo
---

```zsh
hexo new draft <title>
```

<!--more-->

使用`hexo new <title>`创建的文章，会存储在`source/_posts`下，当使用`hexo g`时，该目录下的所有Markdown文件会被编译成HTML并存储在`public`下，再使用`hexo d`则会把`public`下的文件部署到GitHub。这是一般文章的部署流程。

但这种方式的缺点在于，如果文章尚未完成也会随着`hexo g`构建，并随着`hexo d`被部署到GitHub。

不过`hexo`提供了一个草稿功能，供未完成博文的存储。

# 创建文章草稿

使用命令：

```zsh
hexo new draft <title>
```

这样创建出的文件将会存在`source/_drafts`下，当使用`hexo g`时，该目录的文件不会被编译，因此再使用`hexo d`时也不会部署到GitHub。

# 本机预览草稿

虽然`hexo`不会编译`source/_drafts`下的文件，但是`hexo`提供了一个预览的方法，就是：

```zsh
hexo s --draft
```

这样就可以预览还在草稿状态的博文。

# 正式发布博文

使用命令：

```zsh
hexo p <filename>
```

其中`<filename>`不包含`.md`后缀，该命令的原理也不过是将文章从 `source/_drafts` 移动到 `source/_posts` 而已。

> 若日后想将正式文章转为为草稿，只需手动将文章从 `source/_posts` 目录移动到 `source/_drafts` 目录即可。

