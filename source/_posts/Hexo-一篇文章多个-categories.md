---
title: Hexo 一篇文章多个 categories
date: 2018-07-03 10:25:52
tags: 博客
---

## 前言

&emsp;&emsp;在很多情况下，我们希望在 Hexo 中写的一篇文章能够同时属于多个分类，例如我写一篇 [《Servlet笔记》](http://zhiyi.live/2018/06/29/Servlet%E7%AC%94%E8%AE%B0/)，我既想将它放在 [Java](http://zhiyi.live/categories/Java/) 这个分类中，又想将它放入 [Servlet](http://zhiyi.live/categories/Java/Servlet/) 这个分类。<!--more-->

&emsp;&emsp;按照官方的解释，`categories` 这个选项有两种配置方法（其实有三种）。那我们就来讲讲这三种配置方法。 

## 子分类

下面的分类会将该分章放到 `Java/Servlet`这个分类下。

```yaml
categories:
  - Java
  - Servlet
```

同样的作用我们也可以这样写。

```yaml
categories: [Java, Servlet]
```

上面两种方法最终效果一样，都是将文章放在了一个子分类的目录下，效果如图。![mark](http://cmhblog.cfzhao.com/blog/180703/9eJKE7b4Il.png)

## 多个分类

如果我们的要求是将文章同时分到多个不同的分类中呢，我们应该这样：

```yaml
categories:
  - [Java]
  - [Servlet]
```

这样，就可以将上面的文章分类到 `Java` 和 `Servlet` 这两个不同的目录中了。

扩展一下，如果我们将其分类到 `Java/Servlet` 和 `Programming` 两个不同的目录下，我们应该如下写：

```yaml
categories:
  - [Java, Servlet]
  - [Programming]
```

