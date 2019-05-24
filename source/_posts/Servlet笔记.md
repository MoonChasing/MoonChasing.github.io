---
title: Servlet笔记
date: 2018-06-29 15:58:13
tags: [Servlet]
categories: [Java, Servlet]
---

- 编写 Java 方法签名的惯例是， 对于与包含该方法类型不处于同一个包中的类型，要使用全类名。<br>如 void service(ServletRequest request, ServletResponse response) throws ServletException, java.io.IOException 中 javax.servlet.ServletException 的签名中(与 Servlet 位于同一个包中)是没有包信息的，而 java.io.IOException同是编写完整的名称。

