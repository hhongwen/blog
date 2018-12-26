---
title: Centos7安装Python3.7
url: centos7-install-pyhton3-7
date: 2018-12-13 22:32:47
tags: [Python, Linux]
categories: Python
---

全新Centos7系统安装 Python3.7，虽然在Centos7已经预先存在了python2.7版本，但是不要慌，编译安装Python3.7是和原先的旧版本没有任何冲突的，原有的Python是在/usr/bin目录下，是可以共存的。
下面介绍了安装步骤以及安装过程中遇到各种坑在此记录一下。T-T

<!--more-->

>全新Centos7系统安装 Python3.7，虽然在Centos7已经预先存在了python2.7版本，但是不要慌，编译安装Python3.7是和原先的旧版本没有任何冲突的，原有的Python是在/usr/bin目录下，是可以共存的。
下面介绍了安装步骤以及安装过程中遇到各种坑在此记录一下。T-T

----------

## 下载Python
> [官网下载Python](https://www.python.org/downloads/source/)，下载地址：https://www.python.org/downloads/source/， 选择要下载的版本，这里选择Download Gzipped source tarball 下载，不同时期可能下载页面不一样，请自行寻找，good luck for you。
> 下载后将文件用ssh传到centos即可，存放目录放在opt/python（自建哈）下就可以，然后执行下术命令

----------

## Python安装
> Python安装过程很简单，执行下述命令即可，但是在 make 和 make install 过程中会遇到很多问题，请先看**遇到问题**章节，可以预先要开错误。

``` linux
# 解压文件
$ tar -xvzf Python-3.7.0.tgz
# 进入解压后目录
cd Python-3.7.0
$ # 添加对应配置将要安装的目录  安装后就在/usr/python下
$ ./configure --prefix=/usr/python
# 执行安装
$ make
# 然后执行
$ make install
```

> 如果上述操作遇到问题参考下面**遇到问题**章节，如果成功install后即可发现在/usr目录下会有python文件夹，原有的在/usr/bin目录下。
系统中原来的python在/usr/bin/python，通过ls -l可以看到，python是一个软链接，链接到本目录下的python2.6。
我们可以把这个删除，也可以新建一个python3的软链接，只不过执行时python要改成python3，或者python脚本头部声明要改为#!/usr/bin/python3。
这里为了方便建议先重命名一下，然后建立个软链接就可以了，之前的程序头部也不用更改：

```linux
$ mv /usr/bin/python /usr/bin/python.bak
$ ln -s /usr/python/bin/python3 /usr/bin/python
```
----------

## 遇到问题
> 在安装过程中遇到了几个错误，在此记录下，错误都是需要相关依赖导致安装失败，详细内容如下：

### 错误： **configure: error: no acceptable C compiler found in $PATH**
> 此问题是执行./configure --prefix=/usr/python时编译缺少gcc环境，具体错误及解决如下：

``` linux
# 错误如下：
configure: error: in `/usr/local/src/pythonSoft/Python-3.7.0':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details

# 解决办法，安装gcc环境
$ yum install -y gcc

# 安装成功后重新执行
$ ./configure --prefix=/usr/python
```

### 错误： **can't decompress data; zlib not available**
> 在执行make命令安装过程中会遇到错误can't decompress data; zlib not available，是因为缺少zlib依赖导致，安装zlib即可，命令如下

``` linux
# 安装zlib依赖
$ yum -y install zlib*
# 然后再执行
$ make
$ make install
```

### 错误： **ModuleNotFoundError: No module named '_ctypes'**
> make install安装遇到ModuleNotFoundError: No module named '_ctypes'问题，执行如下命令：
``` linux
# Python3.7中缺少libffi-devel依赖
$ yum install libffi-devel -y
# 然后再执行即可
$ make install
```

## 小结
> **锣鼓喧天鞭炮齐鸣红旗招展人山人海**，到此Python终于安装结束了，互联网拯救了我，再次感谢如下博主资料：
https://www.cnblogs.com/jellydong/p/7724169.html
https://blog.csdn.net/qq_31306973/article/details/78538601
https://blog.csdn.net/qq_36416904/article/details/79316972
https://blog.csdn.net/blueheart20/article/details/72827666

