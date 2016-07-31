title: "shell中的特殊变量"
date: 2015-09-25 17:02:24
categories: unix
tags: [shell,tool]
---

# Positional Parameters
## 参数表示
* `$#`: 添加到Shell的参数个数
* `$0`: Shell本身的文件名
* `$1 - $n`: 添加到Shell的各参数值。$1是第1参数、$2是第2参数...

新建文件positional-parameters.sh

``` sh
echo "
Number of arguments: $#
\$0 = $0
\$1 = $1
\$2 = $2
\$3 = $3
\$4 = $4
\$5 = $5
\$6 = $6
\$7 = $7
\$8 = $8
\$9 = $9
"
```

在shell中输入`sh positional-parameters.sh {1..10}`, 输出如下

``` plain
Number of arguments: 10
$0 = position-parameters.sh
$1 = 1
$2 = 2
$3 = 3
$4 = 4
$5 = 5
$6 = 6
$7 = 7
$8 = 8
$9 = 9
```

<!-- more -->
## 参数扩展
* `$*`: 所有参数列表，如果`$*`用双引号括起来，则以`$1 $2 $3 ... $n`的形式输出
* `$@`: 所有参数列表，如果`$@`用双引号括起来，则以`$1` `$2 $3 ... $n`的形式输出

新建文件expand-param.sh

``` sh
print_params () {
  echo "\$1 = $1"
  echo "\$2 = $2"
  echo "\$3 = $3"
  echo "\$4 = $4"
}

pass_params () {
  echo '$* :'; print_params $*
  echo
  echo '"$*" :'; print_params "$*"
  echo
  echo '$@ :'; print_params $@
  echo
  echo '"$@": '; print_params "$@"
}

pass_params "word" "words with spaces"
```

在中输入`sh expand-param.sh`, 输出如下
``` plain
$* :
$1 = word
$2 = words
$3 = with
$4 = spaces

"$*" :
$1 = word words with spaces
$2 =
$3 =
$4 =

$@ :
$1 = word
$2 = words
$3 = with
$4 = spaces

"$@":
$1 = word
$2 = words with spaces
$3 =
$4 =
```

## 参数移动
可以使用`shift`的命令移动参数

新建文件shift-param.sh

``` sh
count=1

while [[ $# -gt 0  ]]; do
  echo "Argument $count = $1"
  count=$((count + 1))
  shift
done
```
在shell中输入命令`sh shift-param.sh {1..20}`, 输出如下

``` plain
Argument 1 = 1
Argument 2 = 2
Argument 3 = 3
Argument 4 = 4
Argument 5 = 5
Argument 6 = 6
Argument 7 = 7
Argument 8 = 8
Argument 9 = 9
Argument 10 = 10
Argument 11 = 11
Argument 12 = 12
Argument 13 = 13
Argument 14 = 14
Argument 15 = 15
Argument 16 = 16
Argument 17 = 17
Argument 18 = 18
Argument 19 = 19
Argument 20 = 20
```
## 其它
* `$$`, Shell本身的pid
* `$!`, Shell最后运行的后台进程的pid
* `$?`, Shell最后运行的命令结束代码
