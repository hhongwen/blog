---
title: Shell脚本备份Mongodb数据库
url: linux-shell-backup-mongodb
date: 2018-12-26 15:47:19
tags: [MongoDB, Linux]
categories: 数据库
---

项目需要对Mongodb的数据进行定期备份，以免出现什么差错难以追回，但是数据量比较大，本机硬盘不够用，还要异地备份，于是乎通过通过dump远程链接目标库，然后将数据dump到另一个位置，暂时还在执行中，如果有错误再进行修改。

---------

## 环境还原

- Mongodb服务器`A`（Linux），备份机器`B`（Windows），备份移动硬盘C。
- `B`机器可以远程`A`服务器，由于`A`和`B`服务器的硬盘剩余空间都不够备份，在`B`机器插入移动硬盘C。
- 在B机器上通过VirtualBox创建`Linux虚拟机`，与`B`机器的移动硬盘`C`进行共享文件夹`D`。
- `Linux虚拟机`创建shell脚本，将数据备份到共享文件夹`D`。
- `Linux虚拟机`需要安装Mongodb数据库，如何安装数据库请参考[Centos7安装MongoDB4.0](/20181213/centos7-install-mongodb4)，以便执行`mongodump`命令。

## 环境创建

`Linux虚拟机`在共享目录`<folder>`下创建导出dump的文件夹和将dump文件压缩的目标文件夹，命令如下：

**注：执行的时候请将下面提到的所有`<folder>`替换成你的目标目录，比如`/media/sf_mongobak/`**

``` bash
# 切换到你的目标目录
$ cd <folder>

# 创建文件夹
$ mkdir -p dump_bak
$ mkdir -p tar_bak
```

## 编写shell脚本

创建好文件夹后只需要编写shell脚本就可以了，shell脚本里会用到上述创建的文件夹，如果写错了执行的时候会报错，请注意。

### 准备文件

``` bash
# 切换到你的目标目录
$ cd <folder>

# 创建shell文件
$ sudo touch mongobak.sh

# 编写文件
$ vi mongobak.sh

```

### 创建shell脚本

在`mongobak.sh`文件中插入如下内容，为了方便理解下面写的路径都是临时编写，请将`/media/sf_mongobak`自行修改成你的目录。

``` bash
# 在文件中编写如下内容
## 指定到你安装的Mongodb bin目录下的mongodump
dump=/usr/local/mongodb/bin/mongodump
## 填写你创建dump_bak文件的绝对路径
out_dir=/media/sf_mongobak/dump_bak
## 填写你创建tar_bak文件的绝对路径
tar_dir=/media/sf_mongobak/tar_bak
## 记录备份时间
sysdate=`date +%Y_%m_%d`
db_user=***
db_pass=***
## 设置删除期限，删除10天前的备份
days=10
## 设置最终压缩的文件名称，带有日期
tar_bak="mondodb_bak_$sysdate.tar.gz"

cd $out_dir
# 删除之前的dump文件
sudo rm -rf $out_dir
# 创建新的文件夹存放dump文件
sudo mkdir -p $out_dir/$sysdate
# 导出172.18.9.123机器上的masterdata库的所有表到$out_dir/$sysdate文件夹
$dump -h 172.18.9.123 -u $db_user -p $db_pass -d masterdata -o $out_dir/$sysdate
# 压缩$out_dir/$sysdate到目标文件夹
sudo tar -zcvf $tar_dir/$tar_bak $out_dir/$sysdate
# 删除指定期限钱的备份文件
sudo find $tar_dir/ -mtime +$days -delete
# 退出
exit

```

**注：上面插入的内容请注意空格，比如`date +%Y_%m_%d`获取时间`date`和`+`号之间就有空格。**

导出dump数据的时候根据自己的需求自行修改，dump命令可参考：[Mongo的备份和恢复（mongodump 和mongorestore ）](https://www.cnblogs.com/xiaotengyi/p/6393972.html)

### 执行shell脚本

给文件添加权限，然后shell命令执行。

``` bash
# 添加权限
$ chmod +x mongobak.sh

# 执行shell脚本
sh +x mongobak.sh
```

在执行过程中遇到错误`error running listCollections`,还有其他的错误一般就是`mongodump`命令使用不对，这里参考了一篇文章：[Not able to run mongodump](https://stackoverflow.com/questions/29699906/not-able-to-run-mongodump)，其他的请自行百度或者Google。

## 进阶版

以上满足了备份的脚本化，但是如果要做定时任务定期执行，那么就要结合linux的crontab完成，具体使用可参考文章[Linux定时任务Crontab使用](https://hhongwen.cn/20181228/linux-crontab-use/)

## 感谢

参考来源：**[用shell脚本实现MongoDB数据库自动备份](http://blog.51cto.com/13362895/2150200)**