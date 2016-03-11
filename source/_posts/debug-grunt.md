---
title: "debug grunt"
date: 2014-11-26 00:59:34 +0800
categories: webdesign
tags: [js,tool]
---

##Grunt Javascript世界中的构建工具

### Grunt配置文件的debug方法

Grunt是基于node.js的工具，配置文件是项目根目录下的Gruntfile.js，这是一个普通的js文件。
因此调试node.js的方式也同样可以用在调试grunt上面。

<!-- more -->
* 在终端输入`npm install -g node-inspector`命令安装node-inspector插件
* 在终端输入`node-debug $(which grunt)`命令开始调试(此时当前目录下需要包含Gruntfile.js文件)
* 在自动打开的Chrome或Opera浏览器(程序会打开电脑上面默认的浏览器，需要注意的是node-inspector插件只能工作在Chrome或Opera浏览器上面)上进行调试，
* 工具首先进入的是grunt这个js文件，通过调试命令进入Gruntfile.js文件中调试。
