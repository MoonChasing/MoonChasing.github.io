---
title: YouCompleteMe踩坑记
date: 2019-05-29 08:38:46
tags: [Vim, YouCompleteMe]
---
最近转移到了 `Ubuntu` 系统上，配了一波环境，逐步适应了些`Vim`，虽然一开始觉得没有补全无所谓的，可以练练实力，再安装插件觉得麻烦，但后面还是觉得没有补全少点什么，于是决定还是下个 `Vim`补全的插件。大约刻之前在赵老板博客上看到过一个关于`Vim`补全的插件[赵老板博文链接](http://www.cfzhao.com/2018/11/03/安利一个vim代码补全工具：youcompleteme/)，所以打算用那个，也就是 `YouCompleteMe`。据说这是`Vim`最难安装的插件？

安装步骤并不算麻烦吧：

1. 安装 `Vundle` （`Vim` 插件管理）
2. 用 `Vundle`安装编译`YouCompleteMe`
3. 配置 `YouCompleteMe`

## 前期工作

1. 确认你的vim版本在7.3.584以上。
2. 确认你的电脑支持Python 2.x。
3. 需要Vundle（vim的插件管理工具）来管理你的插件。
4. 安装脚本需要使用CMake。

以上几点大家如果没有配置好的话，网上有很多很详细的教程，这里就不细讲了。（完全照抄赵老板博客）<!--more-->

## 安装 Vundle

这个网上教程蛮多的，照抄就好了，我说几个坑点。

首先是 `Vundle`的语法，安装完`Vundle`后，我们要在`.vimrc`中加入一些东西，我当时是复制`Vundle`的[github](<https://github.com/VundleVim/Vundle.vim#quick-start>)中的原文，因为一开始，也并不知道这些东西是啥意思，后来猜出了一些语法后，才发现自己被这个官方的命令坑得不清。

```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

首先这里面 " 表示注释，以`Plugin`开始的每行是注明`Vundle`中有这个插件（后面安装的时候要下载）。这个文件是为了给我们演示语法。。所以这里面很多的 `Plugin` 可以不安装！里面有个`git`下了我不知多长时间......还有个本地仓库，555，当时不清楚，一直安装不成功，以为自己操作错了。后来看懂了语法，心想：woc，这什么傻x东西。遂注释掉。

## 安装`YouCompleteMe`

1. 在上面说的`.vimrc`的`Vundle`中加入`YouCompleteMe`

```
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'
call vundle#end()
```

2. 之后进入`Vim`，输入命令

   ```
   :BundleInstall
   ```

   或者在终端中

   ```
   vim +PluginInstall +qall
   ```

3. 之后我们进入`YouCompleteMe`文件夹

   ```
   cd   ~/.vim/bundle/YouCompleteMe/
   ```

   执行安装脚本，根据需要添加参数。由于我用vim是写C++的，所以选择添加了C-family的语言支持。其他语言版本参数的加法可以参照Github上README里的说明。

   ```
   ./install.py  --clang-completer
   ```

   但这一步它告诉我找不到`python 2.7`...怎么会，我明明安好了的，就在它说的文件夹。。神奇，后来我玄学了一波，安装了下`python 3`，本来以为没啥用。。结果...就好了，神奇。

4. 等待它编译成功。（这一步我没遇到啥问题）

## 配置`YouCompleteMe`

### `.ycm_extra_conf.py`文件

下面我们要配置下 `YouCompleteMe`，（即`.ycm_extra_conf.py`文件），让它发挥最大作用。

这个文件可以在 [Github](https://raw.githubusercontent.com/Valloric/ycmd/66030cd94299114ae316796f3cad181cac8a007c/.ycm_extra_conf.py) 中找到，我这里也链接了一下。为了方便，我把他放在了`~/.vim`文件夹中。后来发现这个文件有很多没用的(那一些'-isystem', 'cpp/...'这些路径)，遂删之，留下了比较重要的东西。然后在`.vimrc`的适当位置（就刚才那段里面）写上

```
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'
let g:ycm_global_ycm_extra_conf = '~/.vim/.ycm_extra_conf.py'
call vundle#end()
```

最重要的一步呢，是要根据你电脑在`.ycm_extra_conf.py`中的`flags`加一些头文件包含路径。比如我的是

```
 '-isystem',
'/usr/include/c++/7',
#'-isystem',
#'/usr/lib/gcc/x86_64-linux-gnu/7',
#'-isystem',
#'/usr/lib/gcc/x86_64-linux-gnu/7/include',
'-isystem',
'/usr/include/x86_64-linux-gnu',
'-isystem',
'/usr/include/x86_64-linux-gnu/c++/7'
'-isystem',
'/usr/include',
# '-isystem',
# './header',
```

添加的时候注意些格式，大概就是 ‘-isystem’然后有个逗号，下面加路径。

**注意这里有坑啊！我在这坑上调了好久才发现问题！** 这些路径的顺序是有一定讲究的，因为一开始顺序错了，`YCM`补全总有些问题，具体啥讲究我也不清楚，在网上看到 `/usr/include/c++/7`一定要在`/usr/include` 前面。啊，我在这上面找了好久的问题。。。这坑太大了。

### `.vim`文件`Vundle`中对`YCM`一些个性化配置

我总以为`YCM`会对库函数有提示补全什么的，结果默认是没有的（害我以为我配置有问题，捣鼓了好久）。对 `YCM`的一些个性化配置我主要参考了[YouCompleteMe 中容易忽略的配置](https://zhuanlan.zhihu.com/p/33046090)。包括

* 库函数补全
* 语义补全自动触发
* 补全对话框颜色的修改
* 关闭补全时分屏提示
* 关闭代码检查诊断信息
* 设置一下：g:ycm_filetype_whitelist 白名单