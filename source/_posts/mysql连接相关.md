title: mysql连接相关
date: 2016-01-20 23:43:24
categories: mysql
tags: mysql
---
# 显示正在运行的线程
可以通过mysql命令`SHOW [FULL] PROCESSLIST`, shell命令`mysqladmin processlist`, `information_schema.processlist`表查看当前连接到mysql数据库的线程，如果你有`PROCESS privilege`的特权，则你可以查看所有的线程，否则只能查看当前用户的线程。

# 查看mysql允许的最大连接数
`show variables like '%max_connections%'`
