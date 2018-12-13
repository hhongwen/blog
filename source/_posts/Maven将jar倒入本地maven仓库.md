---
title: Maven将jar倒入本地maven仓库
date: 2018-12-13 22:31:37
tags: Maven, jar
categories: Java
---

在Java项目开发过程中总会遇到自定义jar包或其他jar不存在maven仓库中，但是因为网络等原因无法更新maven仓库或者无法加载此jar包，那么可以通过下面方法将jar包引入到本地maven仓库中。

<!--more-->

在Java项目开发过程中总会遇到自定义jar包或其他jar不存在maven仓库中，但是因为网络等原因无法更新maven仓库或者无法加载此jar包，那么可以通过下面方法将jar包引入到本地maven仓库中。

## 环境变量配置maven

``` bash
    # 新建系统变量，保存maven所在位置
    key=MAVEN_PATH
    value=E:\ProgramFiles\apache-maven-3.2.3
    # Path 添加上面的 MAVEN_PATH 并指定到bin目录
    %MAVEN_PATH%\bin
    
    # 验证是否配置成功，cmd中执行 mvn -version
    C:\Users\Administrator>mvn -vesion
    Apache Maven 3.2.3 (xxxxxxxxxxxxxx; 2014-08-12T04:58:10+08:00)
    Maven home: E:\xxxx\xxxxx\apache-maven-3.2.3\bin\..
    Java version: 1.8.0_144, vendor: Oracle Corporation
    Java home: D:\xxxx\xxxxx\Java\jdk1.8.0_144\jre
    Default locale: zh_CN, platform encoding: GBK
    OS name: "windows 10", version: "10.0", arch: "amd64", family: "dos"
```
    

## 执行一下命令即可

``` bash
    # -Dfile 为本地jar包所在的地址
    # -DgroupId 为pom文件中引的groupId
    # -DartifactId 为pom文件中引的artifactId
    # -Dversion 为pom文件中引的version
    # 引入后pom配置需要和上述的名一致即可
    mvn install:install-file -Dfile=D:\myDoc\private\private_jars\javailp-1.2a\javailp-1.2a.jar -DgroupId=net.sf.javailp -DartifactId=javailp -Dversion=1.2a -Dpackaging=jar
```
