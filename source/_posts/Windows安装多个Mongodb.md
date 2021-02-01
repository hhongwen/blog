---
title: Windows安装多个Mongodb
url: windows-install-multiple-mongodb
date: 2018-12-27 15:33:19
tags: [MongoDB, Windows]
categories: 数据库
---

![mongodb-header-img](/images/mongodb-header-img.png)项目需要测试备份Mongodb的dump文件是否兼容，需要安装一个Mongodb3.6版本的库，由于之前已经安装过Mongodb4.0版本，所以这里讲一下Windows怎么安装第二个Mongodb，其中遇到了哪些坑。

<!-- more -->

## MongoDB下载

这里就不对Mongodb进行介绍了，因为第一次安装的Mongodb是安装包版本的，很简单直接下一步下一步就可以了，这次搞一个压缩版的进行安装，下载地址[MongoDB下载](https://www.mongodb.com/download-center/community)，选择自己要安装的版本进行下载即可。

![mongodb_download](/images/mongodb_download.png)

## MongoDB安装&配置

由于是压缩版，所以安装过程主要是就是配置和调试，主要讲的就是配置文件编写。

### 安装

将下载的ZIP压缩包解压到你的目标目录即可，文件路径大概如下，不同版本可能稍有不同：

``` folder
.
├── bin
├── LICENSE-Community.txt
├── MPL-2
├── README
└── THIRD-PARTY-NOTICES
```

### 创建文件夹

这里主要有**数据存放**以及**日志存放**文件夹，为了存放数据和日志。

1. 创建**数据存放**目标文件夹，例如：`E:\mongodata\3.6\data`。
2. 创建**日志存放**目标文件夹，例如：`D:\Programs\MongoDB\Server\3.6\log`。

**注：**OK，基本就这两个文件夹，位置可以随意设置，log一般是与解压在同一个目录下方便查找，data不需要一定与解压在同一个目录下，比如我安装时上述的例子就不在同一位置。这个主要看你的磁盘大小与数据量期望大小确定，如果你安装目录磁盘就50G，而后期数据大小可能100G那就自己选择了。

### 添加配置文件

配置是重中之重，在解压的bin目录下创建一个`mongo.cfg`的文件，然后填入如下内容：

``` yml
# Where and how to store data.
storage:
  dbPath: E:\mongodata\3.6\data
  journal:
    enabled: true

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path:  D:\Programs\MongoDB\Server\3.6\log\mongod.log

# network interfaces
net:
  port: 27018
  bindIp: 127.0.0.1
```

#### **配置项说明**

- dbPath：数据库路径
- systemLog.path：日志路径
- net.port：端口号，由于我是安装第二个Mongo所以就设置成`27018`，默认是`27017`
- 其他配置项默认即可

### 启动测试

这里就不需要多说了，以管理员身份打开cmd切到bin目录下，然后输入`mongo --config mongo.cfg`即可启动了，我想你已经装了Studio 3T，然后链接配置端口即可测试成功。

**但是**cmd的那个窗口一直开着，如果关了的话服务也就停止了，想到了Linux有fork参数，可以后台以守护进程的方式启动，试了一下果然在Windows上不好使！！！经过查阅说是需要注册成Windows服务，所以坑就接着来了，接着往下看。

## 注册Windows服务

cmd切换到bin目录下，然后执行下述命令，注册服务使用命令如下这个是正确的完整版：

``` bash
mongod.exe --install --serviceName "MongodbServer3" --serviceDisplayName "MongodbServer3" --config "D:\Programs\MongoDB\Server\3.6\bin\mongo.cfg"
```

这个时候你会发现在服务中生成了一个`MongodbServer3`的服务，然后右键启动即可。到这里就全部结束了，是不是很easy，下面记录一下安装过程中遇到的错误。

## 遇到的问题

前人种树后人乘凉，也是通过网上前人的资料解决的问题。在注册服务的时候遇到问题：

``` log
2018-12-27T15:18:58.785+0800 I CONTROL  [main] ***** SERVER RESTARTED *****
2018-12-27T15:18:59.047+0800 I CONTROL  [main] Trying to install Windows service 'MongodbServer3'
2018-12-27T15:18:59.048+0800 I CONTROL  [main] Error creating service: 名称已用作服务名或服务显示名称。 (1078)
```

明明没有`MongodbServer3`的服务，但是报错提示`名称已用作服务名或服务显示名称`，后来通过[【日积月累】MongoDB 注册为 Window 服务时遇到的问题总结](https://blog.csdn.net/qq_24598601/article/details/83420984)博客解决，发现 Windows 系统注册服务时有参数 `serviceDisplayName`，在此做一下记录也感谢此博主。

## 小结

此次安装过程还算是很顺利，现在正在测通过`4.0.5`版本的dump命令导出的数据能否还原到`3.6.9`版本，结果还是不错的暂时没有报错，也希望这篇博客能给你带来帮助。