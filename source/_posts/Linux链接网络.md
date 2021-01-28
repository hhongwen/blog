---
title: Linux链接网络
url: linux-connect-net
date: 2018-12-13 22:32:15
tags: [Linux, 网络]
categories: 运维
---

Linux在安装系统结束后总要面临一个主要问题，网络连接。习惯了Windows的图形界面连接网络是轻而易举的事，但是对于Linux命令操作很不容易上手，在此做记录方便后期使用也供大家参考。

## 查看网卡

> 输入命令后，打开网卡所在文件


``` bash
    #/etc/sysconfig/network-scripts/
    #vi ifcfg-eth0
    
    EVICE=eth0
    HWADDR=6E:D6:7D:EE:4D:93
    TYPE=Ethernet
    UUID=08212c73-eb02-4ad7-9969-ed3bc7a352ed
    ONBOOT=no
    NM_CONTROLLED=yes
    BOOTPROTO=dhcp
```

> HWADDR:Linux的MAC地址
> ONBOOT=yes:开启网络连接
> 网卡自动获取IP配置将ONBOOT=no 改为ONBOOT=yes即可
> 编辑、保存、然后重启网卡

``` bash
    # service network restart
```

## 存在多个网卡
> 如果存在多个网卡会在/etc/sysconfig/network-scripts/ 目录下存在多个ifcfg-eth0文件，如ifcfg-eth1、ifcfg-eth2、ifcfg-eth3等，具体哪个网卡好用需要挨个试探，只保持其中一个网卡的配置为ONBOOT=yes，然后重启网卡。

## 网卡配置静态IP
> 配置静态IP同样修改好用的网卡，如ifcfg-eth0，具体配置如下

``` bash
    #/etc/sysconfig/network-scripts/
    #vi ifcfg-eth0
    
    DEVICE=eth0
    HWADDR=2E:8A:C5:9A:AC:70
    TYPE=Ethernet
    UUID=439416c6-5dc9-43e0-b0de-12986a2cc25b
    #开启自动启用网络连接
    ONBOOT=yes
    NM_CONTROLLED=yes
     #启用静态IP地址
    BOOTPROTO=static
    #设置IP地址
    IPADDR=172.18.15.195
    #设置子网掩码
    NETMASK=255.255.224.0
    #设置网关
    GATEWAY=172.18.10.1
    #设置主DNS
    DNS1=172.18.12.62
    DNS2=
    #禁止IPV6
    IPV6INIT=no
    USERCTL=no
```

> 修改后执行下列名重启网卡

``` bash
    service ip6tables stop   #停止IPV6服务
    chkconfig ip6tables off  #禁止IPV6开机启动
    service network restart  #重启网络连接
```
    
## 报错总结
>  Device eth2 has different MAC address than expected, ignoring.
> 错误原因：实际Mac地址与网卡内配置的HWADDR地址不同导致，查看正确Mac地址

``` bash
    #ifconfig -a
    #HWaddr即为eth0对应的Mac地址，只要找到对应网卡的Mac地址修改对应的网卡文件的HWADDR即可
    eth0      
          Link encap:Ethernet  HWaddr 2A:B4:AD:CE:5A:57
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)
          Interrupt:35
```

> 修改后重复上述重启网卡即可




