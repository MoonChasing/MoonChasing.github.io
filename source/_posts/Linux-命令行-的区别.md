---
title: Linux 命令行 & ; && 的区别
date: 2019-03-21 10:06:20
tags: Linux
Categories: Linux
---

1. command1 & command2 & command3     

  三个命令同时执行 

2. command1; command2; command3         
  不管前面命令执行成功没有，后面的命令继续执行 

3. command1 && command2                         
   只有前面命令执行成功，后面命令才继续执行

