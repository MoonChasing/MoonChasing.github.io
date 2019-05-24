---
title: 远程连接阿里服务器Mysql的坑
date: 2019-03-20 18:20:03
tags: Mysql
categories: Mysql
---

今天突发奇想，想在阿里的服务器上搭个数据库玩，结果按网上步骤搭完后远程一直连不上。用 Windows 连接时会报 10060 错误（未知错误），用阿里自己远程连的时候会报 110 错误。用命令行搞了一下午防火墙，开放端口一类的，又重置了一次系统，结果还是连不上。

最后，我默默在阿里服务器的控制台发现了这个，orz。![mark](http://cmhblog.cfzhao.com/blog/20190320/MeJbwBxWVWOr.PNG)

这里还有个总的防火墙，要在这里开下Mysql的端口。

<!--more-->

顺便记录下在服务器上搭建 Mysql 数据库并远程连接的步骤吧。

1. 安装 Mysql

   ```shell
   sudo apt-get install mysql-server
   sudo apt-get install mysql-client
   sudo apt-get install libmysqlclient-dev
   ```

2.  允许远程访问

   Mysql 默认只允许本地访问，要修改下配置使之允许远程连接。

   ```Shell
   sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf 
   ```

   把其中bind-address = 127.0.0.1注释掉

3.  授权用户远程访问

   使用root进入mysql命令行，执行如下2个命令，示例中mysql的root账号密码：root

   ```
   grant all on *.* to root@'%' identified by '你的密码' with grant option;
   flush privileges;
   ```

4. 重启 Mysql

   ```She
   # /etc/init.d/mysql restart 或者 #sudo service mysql start
   ```

   