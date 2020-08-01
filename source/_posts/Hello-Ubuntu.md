---
title: Hello Ubuntu
date: 2019-05-26 16:12:27
tags: Linux
---

# Hello Ubuntu

## 安装Ubuntu 18.04

这个网上教程有很多，我就不详述了。我来说下我遇到的一些坑（Win7 + Ubuntu），双显卡（Nvidia + Inter）。大一的时候未能成功，就是因为 Nvidia 的驱动问题。一开始遇到了安装时卡在 logo 界面的问题，Ubuntu 18.04 与前面不同的好像是选择`Try Ubuntu without install ` `Install Ubuntu`时有了界面。这一下把我搞懵了，因为原来选择的时候按 `e` 可以编辑 `grub` 参数，这次不知道去哪里编辑了，尴尬。后面摸索了下发现 `F6` 有个其他选项，里面有个 `nomodeset`，勾选后便解决了这个问题。<!--more-->

## 更换阿里源

这个是我每次安好Ubuntu必做的，网上教程一堆，复制粘贴下镜像地址就好了。

## 禁用nouveau驱动

先解释下`nouveau`是什么，我的理解（猜测）是个显卡驱动，但性能不高。（我估计刚刚安装好的时候用的就是这东西，卡得一比，差点让我放弃 Ubuntu），我电脑有Nvidia显卡，所以要先禁用下`nouveau`，然后安装`Nvidia`驱动。

这个教程网上也一堆，大概就是将它加入黑名单，之后重新生成` kernel initramfs`。

## 安装 Nvidia 驱动

参考[ Ubuntu下安装nvidia显卡驱动（安装方式简单）](<https://blog.csdn.net/linhai1028/article/details/79445722>)

不过看别人的一开始要删除下之前的 `Nvidia`驱动残余，（这一点我不清楚新机需不需要，加上吧，反应加上没问题）

```
 sudo apt-get remove --purge nvidia*
```

## 删除nomodeset

这是我这次遇到的最大的困难。在我安装好 `Nvidia`驱动后重启，却一直报错！定眼看一下是什么`pcieport`balabala之类的，我是慌了。在网上搜索也没找到能解决的办法，就在这一直不停地重启、玄学测试、报错......为些还删掉系统重装了一次，然后又搞一遍，依旧报错。在我用恢复模式删除`Nvidia`驱动、重启`nouveau`后，又可以打开。于是猜测是显卡驱动的问题。

看网上的方法没有用，于是只好靠自己意淫了。。我尝试着删去了 `nomodeset`，启动成功！而且系统变得流畅极速！整个人一下振奋了起来。一个多年的夙愿终于在今天完成了！

后来我查了下 `nomodeset`的意义，看了[acpi_osi=linux、 nomodeset是什麼意思? 功能?]([https://medium.com/caesars-study-review-on-web-development/acpi-osi-linux-nomodeset%E6%98%AF%E4%BB%80%E9%BA%BC%E6%84%8F%E6%80%9D-%E5%8A%9F%E8%83%BD-42d8e2c444c3](https://medium.com/caesars-study-review-on-web-development/acpi-osi-linux-nomodeset是什麼意思-功能-42d8e2c444c3)) 这篇文章，下面复制些其中的内容：

> ```
> nomodeset
> 不載入所有關於顯示卡的驅動
> nouveau.modeset=0
> 關閉nvidia顯卡的驅動，反之 =1為開啟
> i915.modeset=0
> 關閉Intel顯卡的驅動，挺好奇對於Intel內顯會有什麼影響
> xforcevesa 或 radeon.modeset=0 xforcevesa
> 跟AMD顯卡(ATI)有關的設定，我猜也是關閉吧，反正我不會買A牌顯卡...
> acpi=off
> 回歸舊時代，電源相關設定，OS無法控管，交給bios處理
> acpi功能失效，有不少硬體上奇怪的問題，可以用這參數解決
> ```

这样就理解些了吧。一开始，我们没有安装`Nvidia`驱动，所以要加上`nomodeset`，后面我们禁用了`nouveau`，只剩下了`Nvidia`驱动，所以要删去它。

## 更改主机名后sudo命令会很慢

因为觉得默认计算机名很难看，于是我换成了 `lover`，快乐。后面发现使用 `sudo` 命令时特别慢，要延迟好一会，而且用 `Ctrl + C` 好像还不能终止。网上查了下，原来是 因为没有将本机的名字加到`etc/hosts`中，导致执行`sudo`命令的时候，会读`/etc/sudoers`文件，向DNS服务器发起了请求并等待响应，耽误了时间。 

参考[linux为什么sudo命令执行变慢](https://blog.csdn.net/caiqiiqi/article/details/81069639)

于是在 `/etc/hosts` 中将原来的默认名字改过来，ok！

## 软件

### Rime

我在`windows`上面的输入法。叭说了，被`Byvoid`称为神级的输入法。谁用谁知道，我是离不开它了。

### Albert

从 `windows` 转向`ubuntu`最难受的是要离开自己喜欢的`wox`吧。在`Ubuntu`上找了些类似的快速启动器，试了`gnome-pie`,`gnome-do`,`ELaunch`，都差强人意。后面遇到了`Albert`，果断卸载了前面的。虽然比`wox`还差一些，但整体还是ok的。

参考了[Ubuntu安装Albert (替代 Mac Spotlight)](<https://blog.csdn.net/jankin6/article/details/80430223>)

```
sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install albert
```

### Chrome

不述

### QQ、微信

QQ的话用了`Wine`大法，整体还可以。教程网上一堆。

微信的话我用的`electronic-wechat`，[electronic-wechat](<https://github.com/geeeeeeeeek/electronic-wechat>)，附一篇教程[Ubuntu 18.04 安装微信（Linux通用）](https://www.cnblogs.com/dotnetcrazy/p/9124658.html)。

### Visual Studio Code

不述

### Hexo多端维护博客

尝试了下，失败了。。后来发现双系统的话可以访问另一个系统的文件，那我干嘛还要找麻烦呢（手动狗头

### ShadowSocks

连接是能连接上，但不能上网，也不知道为啥，弄清楚后再更。

### 图床工具

暂时没有发现啥比较好的图床工具，于是自己用七牛云的`qshell`写了个，能用是能用，不知道为啥上传速度慢。除了这点还是蛮棒的！

## 美化

`Ubuntu 18.04 LTS`好像使用的 GNOME，感觉棒棒呀，加入了扩展。于是像我这种人，怎么能不美化呢。暂时美化了一些东东：

- 首先要下载个 Gnome-tweak-tool
- 主题：Arc
- 图标：Ultra-Flat
- bash：Oh-my-zsh
- 登录界面背景
- 一些扩展：Dash to dock、system monitor、Openweather

美化后效果：

![](http://cmhblog.cfzhao.com/2019-05-25%2018-54-14%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

![](http://cmhblog.cfzhao.com/2019-05-25%2019-49-08%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)![](http://cmhblog.cfzhao.com/2019-05-25%2019-49-11%20%E7%9A%84%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

太爽了！爱上了，我的天呐，这也太美了。

# Note

非常开心终于搞好了 Ubuntu 的环境，快捷、流畅，直接就爱上了。好久的愿望终于在今天实现了，以后要多用鸭。

