---
title: "terminal中的快捷键"
date: 2014-11-18 14:58:30 +0800
categories: os
tags: 快捷键
---
## 光标移动命令
* Ctrl + A: 光标移动到行首
* Ctrl + E: 光标移动到行尾
* Ctrl + F: 光标向前移动一个字符
* Ctrl + B: 光标向后移动一个字符
* Alt + F: 光标向前移动一个单词
* Alt + B: 光标向后移动一个单词
* Ctrl + L: 清屏

<!-- more -->
## 编辑命令
* Ctrl + D: 删除光标所在字符
* Ctrl + T: 当前字符与前一个字符交换位置
* Alt + T: 当前单词与前一个单词交换位置
* Alt + L: 将从当前位置到单词结尾的字符设置成小写字符
* Alt + U: 将从当前位置到单词结尾的字符设置成大写字符
* Ctrl + K: 删除从当前位置到行尾的所有字符
* Ctrl + U: 删除从当前位置到行首的所有字符
* Alt + D: 删除从当前位置到单词结尾的所有字符
* Alt + Backspace: 删除从当前位置到单词开始的所有字符
* Ctrl + Y: 从kill-ring复制字符并插入到当前位置

## history 相关命令
* Ctrl + R: 向前查找使用过的命令
* Ctrl + J: 用`Ctrl + R`找到需要的命令时，用这个命令拷贝到命令行并执行
* Ctrl + P: 查找上一条命令
* Ctrl + N: 查找下一条命令
* Alt + <: 移动到history的开始
* Alt + >: 移动到history的结束
* Alt + P: 输入查找的关键字后，使用该命令向前查找history
* Alt + N: 输入查找的关键字后，使用该命令向后查找history
* Ctrl + O: 执行history在当前命令行中的命令，并定位到下一个命令

## 其它命令
* Ctrl + G: 退出
* Ctrl + C: 终止当前正在执行的命令
* Ctrl + D: EOF文本输入结束
* Ctrl + Z: 悬挂当前正在执行的命令
