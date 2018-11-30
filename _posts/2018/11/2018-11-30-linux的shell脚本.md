---
layout: post
title:  "linux——shell脚本"
categories: linux
tags:  crontab 定时任务 
author: watermelon
---
* content
{:toc}

## 前言
部署一个项目可能一键启动、快速重启和打印报告、需要指定jdk环境，或者控制台的日志打印log地址、一键启动等，就需要有一个脚本，类似于windows的.bat脚本



### 1、写过的一个脚本
```xml
#!/bin/sh  
log_dir='/home/my_dir/myPrject/log'
day=$(date +%Y%m%d)

export JAVA_HOME=/home/my_dir/jdk1.8.0_191
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
export PATH=$JAVA_BIN:$PATH

id=$(ps -ef | grep /home/my_dir/bianlaWeb/myPrject-1.0.0-main.jar   | grep -v 'grep' | awk '{print $2}')
echo $id
kill -9 $id
nohup java -jar /home/my_dir/bianlaWeb/myPrject-1.0.0-main.jar >> ${log_dir}/myPrject{day}.log
```

* 1.有些应用程序需要手动关闭进程，才能重启它，所以给变量id赋值该应用的进程号，kill执行关闭，下一行再次启动该应用。
* 2.没有 nohup，如果关闭了命令控制窗口，该进程就自动关闭了，所以为了保持进程运行，