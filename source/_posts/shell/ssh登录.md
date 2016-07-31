title: ssh登录
date: 2016-03-04 15:09:47
categories: shell
tags: ssh
---

# 公钥指纹
当第一次使用ssh登录远程主机时，会出现如下的一段话

``` plain
The authenticity of host 'host (12.18.429.21)' can't be established.
RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.
Are you sure you want to continue connecting (yes/no)?
```
里面包含128位的用16进制表示的***公钥指纹***`98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d`。
通过对比远程主机上的公钥指纹可以登录的机子是否正确，防止中间人攻击

查看主机公钥指纹的命令`ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key`

<!-- more -->

# 公钥登录
使用公钥登录可以省去输入密码的步骤。

## 原理
用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码。

## 生成公钥
如果没有现在的公钥可以通过命令生成。

### 生成本机的公钥和私钥
运行命令`ssh-keygen`，系统会出现一系列提示，可以一路回车。运行结束后，在`~/.ssh`目录下，会新生成两个文件：id_rsa.pub和id_rsa。前者是你的公钥，后者是你的私钥。

### 将公钥传送到远程主机上
运行命令`ssh-copy-id user@host`，执行完后公钥就拷贝到远程主机上面了。

远程主机将用户的公钥，保存在`~/.ssh/authorized_keys`文件中. 公钥就是一段字符串，只要把它追加在authorized_keys文件的末尾就行了。

因此也可以用下面的命令保存公钥：
``` sh
ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```

### 验证远程登录
执行完上面的步骤后运行命令`ssh user@host`就可以直接登录到远程主机上面了，不再需要输入密码。

如果不不行，就打开远程主机的/etc/ssh/sshd_config这个文件，检查下面几行前面"#"注释是否取掉。
``` plain
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
然后，重启远程主机的ssh服务
``` sh
# ubuntu系统
service ssh restart

# debian系统
/etc/init.d/ssh restart
```
