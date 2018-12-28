---
title: Linux定时任务Crontab使用
url: linux-crontab-use
date: 2018-12-28 10:45:19
tags: [Crontab, Linux]
categories: Linux
---

> crontab文件包含送交cron守护进程的一系列作业和指令。每个用户可以拥有自己的crontab文件；同时，操作系统保存一个针对整个系统的crontab文件，该文件通常存放于/etc或者/etc之下的子目录中，而这个文件只能由系统管理员来修改。
crontab文件的每一行均遵守特定的格式，由空格或tab分隔为数个领域，每个领域可以放置单一或多个数值。

<!--more-->

> crontab文件包含送交cron守护进程的一系列作业和指令。每个用户可以拥有自己的crontab文件；同时，操作系统保存一个针对整个系统的crontab文件，该文件通常存放于/etc或者/etc之下的子目录中，而这个文件只能由系统管理员来修改。
crontab文件的每一行均遵守特定的格式，由空格或tab分隔为数个领域，每个领域可以放置单一或多个数值。

&#160; &#160; &#160; &#160;好记性不如烂笔头，之前的项目中做过一次crontab的定时任务，当时是用springboot写的一个etl任务，用Crontab配置定期做etl工作。但是由于时间久远这次再用的时候已经很难回忆起当时是怎么做的，所以在这里做个记录也希望可以帮助大家。

## Crontab文件

上面有说道该文件通常存放于/etc或者/etc之下的子目录中，查看crontab文件命令`vi /etc/crontab`，文件内容大致如下（下面是我的机器Ubuntu上内容）：

``` vim
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

### **说明：**

- **SHELL：** 任务执行方式指定本地sh，一般还有bash的
- **PATH：** 系统执行命令的路径

## 时间设置

``` vim
# 文件格式说明
#  ——分鐘（0 - 59）
# |  ——小時（0 - 23）
# | |  ——日（1 - 31）
# | | |  ——月（1 - 12）
# | | | |  ——星期（0 - 7，星期日=0或7）
# | | | | |
# * * * * * 被执行的命令

```

### **注：**

1. 在“星期域”（第五个域），0和7都被视为星期日。
2. 不很直观的用法：如果日期和星期同时被设置，那么其中的一个条件被满足时，指令便会被运行。
3. 前5个域称之分时日月周，可方便个人记忆。
4. 从第六个域起，指明要运行的命令。

## 操作符号

在一个区域里填写多个数值的方法：

- 逗号（','）分开的值，例如：“1,3,4,7,8”
- 连词符（'-'）指定值的范围，例如：“1-6”，意思等同于“1,2,3,4,5,6”
- 星号（'*'）代表任何可能的值。例如，在“小时域”里的星号等于是“每一个小时”，等等

某些`cron`程序的扩展版本也支持斜线（'/'）操作符，用于表示跳过某些给定的数。例如，“*/3”在小时域中等于“0,3,6,9,12,15,18,21”等被3整除的数；

## 设置任务

由于我是要做备份操作，所以设置每周六执行一下shell脚本，对数据进行备份，在crontab文件添加内容如下：

``` vim

```