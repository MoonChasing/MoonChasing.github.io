---
title: Spring MVC笔记
date: 2018-07-13 21:05:56
categories: [Java, Spring MVC]
---

* ![mark](http://cmhblog.cfzhao.com/blog/180713/aBmGe3iall.png)
* MVC 的核心思想是业务数据抽取同业务数据呈现相分离。
* Model: 业务数据的信息表示，关注支撑业务的信息构成，通常是多个业务实体的组合。<br>View: 为用户提供UI，重点关注数据的呈现。<br>Controller: 调用业务逻辑产生合适的数据(Model)。<!--more-->
* ![DispatcherServlet](http://cmhblog.cfzhao.com/blog/180713/ghfAJDbkfD.png)
* ![HandlerAdapter](http://cmhblog.cfzhao.com/blog/180713/fghK0mcga5.png)
* ![HandlerMapping](http://cmhblog.cfzhao.com/blog/180713/aaghehi1i8.png)
* ![HandlerExecutionChain](http://cmhblog.cfzhao.com/blog/180713/ma0gbAKgjc.png)
* ViewResolver: Help DispatcherServlet to resolve the right view to render page.
* ![Spring MVC 流程](http://cmhblog.cfzhao.com/blog/180713/7KBb3hjjLj.png)