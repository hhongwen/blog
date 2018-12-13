---
title: Linux下安装JDK
date: 2018-12-13 22:31:56
tags: JDk
categories: Linux
---

Linux安装JDK是开发中部署中必要知识，在此做下记录。

<!--more-->

Linux安装JDK是开发中部署中必要知识，在此做下记录。

## 卸载JDK
    查看Linux是否安装了其他版本的JDK，如果安装了卸载原有JDK。

## 解压JDK安装
- 去官网下载需要版本的JDk；
- 解压JDK文件；
- 配置JDK环境变量；
- 重启Linux；

> 本文下载的是 jdk-7u79-linux-x64.tar.gz 包，上传到Linux的任意路径下，比如我上传到/opt/java/路径下

```linux
#cd ../opt/java
#tar xvf  jdk-7u79-linux-x64.tar.gz
```
> 解压后修改环境变量，修改 /etc/ 下的profile文件，在文件最后添加如下内容

    #vi /etc/profile
    #添加内容如下，环境变量路径为安装JDK的路径
    export JAVA_HOME=/opt/java/jdk1.7.0_79
    export JRE_HOME=/opt/java/jdk1.7.0_79/jre  
    export PATH=$PATH:/opt/java/jdk1.7.0_79/bin  
    export CLASSPATH=./:/opt/java/jdk1.7.0_79/lib:/opt/java/jdk1.7.0_79/jre/lib  

> 重启Linux系统，然后java -verson查看JDK版本

    [root@localhost java]# java -version
    java version "1.7.0_79"
    Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
    Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)


