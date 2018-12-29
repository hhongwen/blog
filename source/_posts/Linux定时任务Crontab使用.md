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

上面有说道该文件通常存放于/etc或者/etc之下的子目录中，查看crontab文件命令`vi /etc/crontab`，此文件只能root用户可以使用，然后将各个人物指定给不同的用户，文件内容大致如下（下面是我的机器Centos7上内容）：

``` vim
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

```

### **说明：**

- **SHELL：** 任务执行方式指定本地sh，一般还有bash的
- **PATH：** 系统执行命令的路径
- **MAILTO：** 发送结果给指定用户

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

由于我是要做备份操作，所以设置每周六下午八点执行一下shell脚本，对数据进行备份，在crontab文件添加内容如下：

``` vim
0 20 * * 6 root sh +x /home/crontab_test/cron_test.sh
```

保存后即可自动执行。

## Crontab命令

`Crontab`命令是另一种定时任务执行方式，与上述形式相比只不过不需要指定用户执行。

- `crontab -e`命令编写cron任务
- `crontab -l`命令查看cron命令
- `crontab -r`命令移除所有cron命令

### 使用方式

执行`crontab -e`命令，后进行编辑即可插入内容，注意和上述不同没有`root`不需要指定用户。编辑后不同操作系统`crontab -e`命令使用方式不同，一般`Ctrl+x`保存退出，还有`Ctrl+z`退出的具体看情况。

`0 20 * * 6 sh +x /home/crontab_test/cron_test.sh`

查看任务执行`crontab -l`就能看到所有任务列表：

``` cron
*/1 * * * * /usr/local/qcloud/stargate/admin/start.sh > /dev/null 2>&1 &
0 0 * * * /usr/local/qcloud/YunJing/YDCrontab.sh > /dev/null 2>&1 &
```

## 小结

本文只简单介绍了crontab的使用，还有一些详细的内容没有阐述，比如日志，不同用户使用和cron表达式等相关介绍，**后期持续更新**。