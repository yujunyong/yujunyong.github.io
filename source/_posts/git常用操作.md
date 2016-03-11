title: "git常用操作"
date: 2015-06-02 10:47:37
categories: command
tags: [tool,git]
---

# 文件操作
## 添加
* 将所有文件添加到git中: `git add -A`

## 删除
* 删除已经在git中的文件: `git rm --cached <name_of_file>`
* 删除没有添加到git的文件：`git clean -df`

# 分支操作
## 删除
* 删除本地分支: `git branch -d <local_branch>`
* 删除远程分支: `git push origin <:the_remote_branch>`
