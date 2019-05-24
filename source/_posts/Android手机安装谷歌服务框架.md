---
title: Android手机安装谷歌服务框架
date: 2018-11-22 13:23:16
tags: Android
categories: Android
---

程序员果真离开了自己的电脑就不会用电脑。新换了手机，还好小米有着一键换机，可以把一些数据转移过去，不然不知道自己要纠结多久才能适应新的手机。

当然一键换机并不能把全部的设置都配置过去，在谷歌服务框架上就出了些问题。新手机上并没有安装好谷歌服务框架，我怎么忍受得了。而且 Go 谷歌安装器好像在 Android 8 上并不能用。于是只好自己手动安装服务框架。

所谓的谷歌服务框架，其实就是谷歌三件套（Google Services Framework、Google Account Manager、Google Play services），和一个可选的 Google Play 商城。

自己像原来一样在网上分别下载了这些的 APK,不过打开 Google Play 商城的时候，要登录账号（这时候的 UI 非常之丑），一直说我无法与服务器建立可靠的数据连接。确保自己 SSR 能用的情况下，多次尝试还是不行，于是觉得是安装的 APK 的锅。

所以在这里说下我重新安装（下载 APK 的地方）的方法。（墙外）

1. 首先打开 [www.apkmirror.com](https://www.apkmirror.com/)

2. 在网页上搜索 com.google.android.gsf，这个是 Google Services Framework。（版本最好和自己 Android 相合，我猜的）

   com.google.android.gsf.login，这个是 Google Account Manager。

   com.google.android.gms，这个是 Google Play services。

   com.android.vending， 这个是 Google Play 商城。

安装好后打开 Google Play 商城，发现账号登录界面已经是新的了，很现代的 UI。当时心里就知道成了。果然。毫无问题的登录成功。

现在想来，应该是之前安装的一些 APK 版本太旧了吧。