---
title: Centos7安装MongoDB4.0
url: centos7-install-mongodb4
date: 2018-12-13 22:33:19
tags: MongoDB
categories: Linux
---

由于项目需要使用MongoDB，所以记录一下在Centos7下安装MongoDB的方法。
MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

<!--more-->

由于项目需要使用MongoDB，所以记录一下在Centos7下安装MongoDB的方法。
MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

## 安装包下载

> [MongoDB点击跳转下载地址](https://www.mongodb.com/download-center#community)，打开地址后选择Community Server,然后选择Linux下载对应的版本安装包即可，但是下拉列表中有很多Linux安装包，Centos选择带RHEL的安装包，这里选**择RHEl 7 Linux 64-bit x64**版本的安装包即可，下载后上传到对应Linux服务器。

## MongoDB安装

> 在安装时均使用root用户，如果非root用户则在命令前加sudo命令，用来以root身份运行

``` bash
# 1.在usr/local/下创建mongodb文件夹
cd /usr/local/
mkdir mongodb

# 2.解压文件
tar -xzvf mongodb-linux-x86_64-rhel70-4.0.1.tgz

# 3.将解压后的文件下所有内容移动到mongodb文件夹下
# 注意这里不是将mongodb-linux-x86_64-rhel70-4.0.1文件夹移动到创建好的mongodb下，而是文件下的内容
mv mongodb-linux-x86_64-rhel70-4.0.1/*  /usr/local/mongodb/

# 4.添加mongodb的环境变量
vi /etc/profile

# 5.在文件末尾插入如下内容
export MONGODB_HOME=/usr/local/mongodb  
export PATH=$PATH:$MONGODB_HOME/bin

# 6.修改保存后要重启系统配置，执行命令如下
source /etc/profile
```

> 经过上述步骤，基本的配置已经完成了，接下来创建mongodb数据文件和日志文件的存放位置，并且对启动项进行配置，启动项配置其中包含数据库文件路径和日志文件路径，填写上述将要创建的文件夹或文件路径。具体步骤如下：

``` bash
# 1.创建数据库文件存放路径
cd /usr/local/mongodb
mkdir -p data/db
chmod -r 777 data/db

# 2.创建日志文件
cd /usr/local/mongodb
mkdir logs
cd logs
touch mongodb.log

# 3.创建启动文件
cd /usr/local/mongodb/bin
touch mongodb.conf

# 4.编辑启动文件
vi mongodb.conf

# 5.在文件中插入如下内容
dbpath = /usr/local/mongodb/data/db  #数据文件存放目录
logpath = /usr/local/mongodb/logs/mongodb.log #日志存放目录
port = 27017 #端口
#fork = true #以守护程序的方式启用，即在后台运行
logappend = true
maxConns = 5000
storageEngine = mmapv1

```

## 启动数据库

> 经过配置后即可启动数据库了，启动数据库文件在bin目录下执行下术命令

``` bash
# 切换到bin目录下
cd /usr/local/mongodb/bin

# 启动数据库
./mongod --config mongodb.conf

# 访问数据库
./mongo

```

## 补充

在Ubuntu上安装的时候遇到了一些问题，做如下补充：

1. 之前启动数据库写错了，由`./mongod  mongodb.conf`修改为`./mongod --comfig mongodb.conf`。

2. `mongodb.conf`配置文件注释掉`fork`参数，后期解决后再做补充。

## 小结

> 安装mongodb和配置是很简单的，过程还是比较顺利。大家一起学习！