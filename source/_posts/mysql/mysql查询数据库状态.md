---
title: mysql查询数据库状态
date: 2016-06-08 16:43:15
tags:
---

# 通过information_schema查询信息
## 查询数据库连接信息
`select user, substring_index(host, ':', 1) as h, command, count(*) from processlist group by user, h, command;`

## 查询数据库大小
`select (sum(data_length)+sum(index_length))/1024/1024/1024 as size(G) from tables where table_schema='database_name'`

# 通过mysql命令
## 查看最大连接数
`show variables like 'max_connections';`
