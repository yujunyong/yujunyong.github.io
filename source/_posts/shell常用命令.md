title: "shell常用命令"
date: 2015-09-25 17:57:17
categories: unix
tags: [shell,tool]
---

# 创建文件/文件夹
## 创建文件
* 创建内容为空的文件, `touch abc.txt 123.txt`
* 创建一行内容的文件, `echo "hello world!" >> hello-world.txt`
* 创建多行内容的文件, `cat > cat.txt`, Ctrl+D结束输入; `> cat.txt`, Ctrl+C结束输入

## 创建文件夹
* 自动创建父文件夹，`mkdir -p /tmp/fold1/fold2`

# 显示
## 显示文件大小
* 显示当前文件夹大小, `du -sh`
* 显示当前文件夹下面所有文件的大小, `du -sh *`

## 显示系统信息
* 显示当前操作系统的信息, `lsb_release -a`

## 显示文件夹中的信息
* 显示文件夹中的所有文件信息，`ls -alh`
* 只显示文件夹中的直接子文件夹，`ls | grep ^d`
* 只显示文件夹中的文件，`ls | grep -v ^d`
* 显示子文件夹的信息，而不是子文件夹中的内容，`ls -dl dir`

<!-- more -->

# 输入/输出
## 输出
* 输出错误信息到文件，`ls -l /bin/usr 2> ls-error.txt`
* 输出标准输出和错误信息到同一文件，`ls -l /bin/usr > ls-output.txt 2>&1`, `ls -l /bin/usr &> ls-output.txt`
* 不输出错误信息， `ls -l /bin/usr 2> /dev/null`


# 进程
* 查看某个进程信息，`ps aux | grep nginx`
* 后台远行程序，`xlogo &`, `&`放在后台表示在后台运行

# 网络
## telnet
* 退出telnet，先按下Ctrl+]，再输入quit
* 空行表示EOF, 一次内容传入完成。

## 端口
* 查看商品使用情况: `netstat -ant | grep LISTEN`
* 查看端口被哪个程序占用：`lsof -i:1099`
