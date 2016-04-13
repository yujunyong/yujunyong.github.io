---
title: java命令
date: 2016-03-17 15:27:51
tags: [java, command]
---

# 进程
## `jps` 显示当前所有java进程的pid以及相关参数
### 原理
java程序在启动以后，会在`java.io.tmpdir`指定的目录下，就是临时文件夹里，生成一个类似于`hsperfdata_User`的文件夹，这个文件夹里（在Linux中为`/tmp/hsperfdata_{userName}/`），有几个文件，名字就是java进程的pid，因此列出当前运行的java进程，只是把这个目录里的文件名列一下而已。 至于系统的参数什么，就可以解析这几个文件获得。

# 线程
## `jstack` 显示当前时刻java虚拟机的线程快照, 制作线程Dump

# 显示内存
## `jmap` 显示内存映射，制作堆Dump
## `jhat` 内存分析工具
* 通过命令`jmap -dump:format=b,file=heapDump 62247`生成dump文件
* 通过命令`jhat heapDump`启动一个http服务，端口是7000, 在浏览器里面查看内存使用情况

# 控制台
## `jconsole` 简易的可视化控制台
## `jvisualvm` 功能强大的控制台

# 性能
## `jstat` 性能监控工具

# 代码分析
## `javap` java反编译工具
* `javap -c <ClassName>` 查看java文件的字节码信息
