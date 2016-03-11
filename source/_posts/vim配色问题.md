---
title: "vim配色问题"
date: 2014-11-04 01:05:44 +0800
categories: os
tags: [vim,tool]
---

# 在终端为vim配置使用solarized theme
1. 设置环境变量TERM：`export TERM=screen-256color`

2. 在vim配置文件中添加如下代码

``` vim
" 设置配色主题
if $TERM =~ '^xterm' || $TERM =~ '^screen' || $TERM=~ '256color$'
	set background=dark
	let g:solarized_termcolors = 256
	colorscheme solarized
elseif has('gui_running')
	set background=dark
	let g:solarized_termcolors = 256
	colorscheme solarized
elseif $TERM =~ 'cons25'
	colorscheme default
endif
```

<!-- more -->
# 可能出现的问题
环境变量也可以设置成：`export TERM=xterm-256color`。

但是如果你在tmux中使用vim的话会出现如下图的情况：
![tmux bug](/images/vim-solarized-theme-tmux-bug.png)

而tmux的default-terminal只能配置成screen-256color或screen，为了兼容tmux也就只能`export TERM=screen-256color`了。

配置正确后vim如下图所示
![terminal solarized](/images/vim-solarized-theme-terminal-normal.png)

