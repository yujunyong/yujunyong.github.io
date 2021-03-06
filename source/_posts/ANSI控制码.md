---
title: "ANSI控制码"
date: 2014-10-29 23:30:24 +0800
categories: [command]
tags: [shell, terminal]
---

ANSI/3.64控制码标准
ANSI控制码均以 Esc[ 作为控制码的开始标志，其中，Esc 的ansi码为 27-十进制，33-八进制，所以在c中，可以使用 \033 表示。

## ANSI控制码
| 控制码              | 作用                   |
| ------------------- | ---------------------- |
| \033[0m             | 关闭所有属性           |
| \033[1m             | 设置高亮度             |
| \033[4m             | 下划线                 |
| \033[5m             | 闪烁                   |
| \033[7m             | 反显                   |
| \033[8m             | 消隐                   |
| \033[30m - \033[37m | 设置前景色             |
| \033[40m - \033[47m | 设置背景色             |
| \033[nA             | 光标上移n行            |
| \033[nB             | 光标下移n行            |
| \033[nC             | 光标右移n行            |
| \033[nD             | 渔村左移n行            |
| \033[2J             | 清屏                   |
| \033[K              | 清除从光标到行尾的内容 |
| \033[s              | 保存光标位置           |
| \033[u              | 恢复光标位置           |
| \033[?25l           | 隐藏光标               |
| \033[?25h           | 显示光标               |

<!-- more -->
### 前景颜色 30 - 37
| 控制码         | 颜色       |
| -------------- | ---------- |
| \033[30m       | 黑色       |
| \033[31m       | 红色       |
| \033[32m       | 绿色       |
| \033[33m       | 黄色       |
| \033[34m       | 蓝色       |
| \033[35m       | 紫色       |
| \033[36m       | 深绿色     |
| \033[37m       | 白色       |

### 背景颜色 40 - 47
| 控制码         | 颜色       |
| -------------- | ---------- |
| \033[40m       | 黑色       |
| \033[41m       | 红色       |
| \033[42m       | 绿色       |
| \033[43m       | 黄色       |
| \033[44m       | 蓝色       |
| \033[45m       | 紫色       |
| \033[46m       | 深绿色     |
| \033[47m       | 白色       |

### 范例
`echo '\033[37m\033[40m\033[4m白字，黑底，下划线'`

`echo '\033[37;40;4m白字，黑底，下划线'`

以m为标记的，是控制字符显示相关的设置，如果有多项设置，可以使用连续多个\033[nm，也可以使用分号将两个数字连接起来。
