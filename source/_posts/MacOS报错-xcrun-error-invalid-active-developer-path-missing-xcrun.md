---
title: 'MacOS报错 xcrun: error: invalid active developer path, missing xcrun'
date: 2021-04-20 17:09:57
tags: Mac
---

今天配置 VS Code 中 include 路径时，想查看下自己 C++ 相关的 include 文件夹在哪，于是使用下面命令查看。

```bash
gcc -v -E -x c++ -
```

但它竟然报错：

> xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

震惊，使用 `gcc --version` 发现报同样错误，这意味着什么，意味着我整个 gcc 不能使用了，这还了得？

于是上网搜索解决方法，大多数人都说使用 `xcode-select --install` 就行，我试了试，还真解决了，大概几分钟就 OK 了。

唉，看来 `MacOS` 还是有点坑的，大家都说之前自己系统升级失败了出现了这个问题，我之前也是升级失败了。

