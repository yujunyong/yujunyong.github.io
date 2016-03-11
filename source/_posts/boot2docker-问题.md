title: "boot2docker 问题"
date: 2015-07-04 11:00:51
categories: 虚拟化
tags: [docker,问题]
---

# docker命令无法正常执行
在执行docker命令时出现以下的错误：

`An error occurred trying to connect: Get https://192.168.59.107:2376/v1.18/containers/json?all=1: x509: certificate is valid for 127.0.0.1, 10.0.2.15, not 192.168.59.107`

这个问题在boot2docker 1.7.0版会出现

## 解决方法：

执行下这个命令`boot2docker ssh sudo /etc/init.d/docker restart`

github上有对这个问题的讨论，[boot2docker(v 1.4.1 and 1.5) bad cert issues on OSX 10.9.3](https://github.com/boot2docker/boot2docker/issues/824)

