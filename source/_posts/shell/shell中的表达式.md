title: "shell中的表达式"
date: 2015-09-26 12:52:56
categories: unix
tags: [shell,tool]
---

# 文件表达式
| 表达式              | 返回True的条件                                    |
| :------------------ | :------------------------------------------------ |
| *file1* -ef *file2* | file1和file2是同一个文件（link之类的文件）        |
| *file1* -nt *file2* | file1比file2新                                    |
| *file1* -ot *file2* | file1比file2旧                                    |
| -e *file*           | file存在                                          |
| -r *file*           | file存在并且可读                                  |
| -w *file*           | file存在并且可写                                  |
| -x *file*           | file存在并且可执行/可搜索                         |
| -s *file*           | file存在并且size大于0                             |
| -S *file*           | file存在并且是网络Socket                          |
| -d *file*           | file存在并且是文件夹                              |
| -f *file*           | file存在并且是普通文件                            |
| -b *file*           | file存在并且是[块文件](#块文件Block_File)         |
| -c *file*           | file存在并且是[字符文件](#字符文件Character_File) |
| -g *file*           | file存在并且被设置为[SGID](#SGID)                 |
| -u *file*           | file存在并且被设置为[SUID](#SUID)                 |
| -k *file*           | file存在并且被设置为[Sticky Bit](#Sticky_Bit)     |
| -G *file*           | file存在并且和调用程序是同一个用户组              |
| -O *file*           | file存在并且和调用程序是同一个拥有者              |
| -p *file*           | file存在并且是命名管道                            |
| -L *file*           | file存在并且是符号链接文件                        |

<!-- more -->
## 例子

``` sh
FILE=~/.bashrc

if [ -e "$FILE" ]; then
  if [ -f "$FILE" ]; then
    echo "$FILE is a regular file."
  fi

  if [ -d "$FILE" ]; then
    echo "$FILE is a directory."
  fi

  if [ -r "$FILE" ]; then
    echo "$FILE is readable."
  fi

  if [ -w "$FILE" ]; then
    echo "$FILE is writable."
  fi

  if [ -x "$FILE" ]; then
    echo "$FILE is executable/searchable."
  fi
else
  echo "$FILE does not exist"
  exit 1
fi
exit
```

# 字符表达式
| 表达式                                            | 返回True的条件         |
| :--------------------------------------------     | :--------------------- |
| *string*                                          | string不是null         |
| -n *string*                                       | string长度大于0        |
| -z *string*                                       | string长度等于0        |
| *string1* = *string2* <br> *string1* == *string2* | string1等于string2     |
| *string1* != *string2*                            | string1不等于string2   |
| *string1* > *string2*                             | string1排序后于string2 |
| *string1* < *string2*                             | string1排序先于string2 |

## 例子

``` sh
ANSWER=maybe

if [ -z "$ANSWER" ]; then
  echo "There is no answer." >&2
  exit 1
fi

if [ "$ANSWER" = "yes" ]; then
  echo "The answer is YES."
elif [ "$ANSWER" = "no" ]; then
  echo "The answer is NO."
elif [ "$ANSWER" = "maybe" ]; then
  echo "The answer is MAYBE."
else
  echo "The answer is UNKNOWN."
fi
```

## [[]]新增的特性
[[ *expression* ]]的格式中新增了两种表达式

*string* =~ regex, string满足右边的正则表达式，则返回true, 如：

``` sh
INT=-5

if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
  echo "$INT is an integer."
else
  echo "$INT is not an integer." >&2
  exit 1
fi
```

*string1* == *string2*, 支持通配符, 如：

``` sh
FILE=foo.bar

if [[ $FILE == foo.* ]]; then
  echo "$FILE matches pattern 'foo.*'"
fi
```

# 数字表达式
| 表达式                    | 返回True的条件             |
| :---                      | :---                       |
| *integer1* -eq *integer2* | integer1等于integer2       |
| *integer1* -ne *integer2* | integer1不等于integer2     |
| *integer1* -le *integer2* | integer1小于或等于integer2 |
| *integer1* -lt *integer2* | integer1小于integer2       |
| *integer1* -ge *integer2* | integer1大于或等于integer2 |
| *integer1* -gt *integer2* | integer1大于integer2       |

## 例子

``` sh
INT=-5

if [ -z "$INT" ]; then
  echo "INT is empty." >&2
  exit 1
fi

if [ $INT -eq 0 ]; then
  echo "INT is zero."
else
  if [ $INT -lt 0 ]; then
    echo "INT is negative."
  else
    echo "INT is positive."
  fi

  if [ $((INT % 2)) -eq 0 ]; then
    echo "INT is even."
  else
    echo "INT is odd."
  fi
fi
```

# 逻辑表达式
| 操作 | test 命令 | [[]]和(())中 |
| :--- | :---      | :---         |
| and  | -a        | &&           |
| or   | -o        | &#x7c;&#x7c; |
| not  | !         | !            |

## 例子
``` sh
MIN_VAL=1
MAX_VAL=100
INT=50

if [[ INT -ge MIN_VAL && INT -le MAX_VAL ]]; then
  echo "$INT is within $MIN_VAL to $MAX_VAL."
else
  echo "$INT is out of range."
fi

if [ $INT -ge $MIN_VAL -a $INT -le $MAX_VAL ]; then
  echo "$INT is within $MIN_VAL to $MAX_VAL."
else
  echo "$INT is out of range."
fi

if [[ ! (INT -ge MIN_VAL && INT -le MAX_VAL) ]]; then
  echo "$INT is outside $MIN_VAL to $MAX_VAL."
else
  echo "$INT is in range."
fi

if [ ! \( $INT -ge $MIN_VAL -a $INT -le $MAX_VAL \) ]; then
  echo "$INT is outside $MIN_VAL to $MAX_VAL."
else
  echo "$INT is in range."
fi
```

# 说明
## 普通文件*Regular File*
普通意义上的文件，如数据文件、可执行文件等.

## 设备文件*Device File*
### 字符文件*Character File*
以字符的形式进行读/写的硬件文件，如：键盘，鼠标，打印机。如果有用户在使用字符文件，其它用户就不能再访问这个字符文件。访问字符文件是同步进行的。

### 块文件*Block File*
以块的形式进行读/写的硬件文件，如：硬盘，USB，光盘。多个用户可以同时访问块文件，访问块文件是异步进行的。

## 文件特殊权限
### SUID
当s权限在User的x时，也就是类似-r-s--x--x, 称为Set UID, 简称为SUID，UID表示User的ID，而User表示这个程序的拥有者。
当有程序执行SUID的文件时，这个程序会得到SUID文件拥有者的权限，来执行这个文件。

### SGID
当s权限在User用户组的x时，也就是类似-r-x--s--x, 称为Set GID，简称为SGID，GID表示User的用户组ID，而User表示这个程序的拥有者。
当有程序执行SGID的文件时，这个程序会得到SUID文件用户组的权限，来执行这个文件。

### Sticky Bit
Sticky Bit只对目录有效对文件没有效果。
SBit对目录的作用是：在具有SBit的目录下，用户若在该目录下具有w及x权限，则当用户在该目录下建立文件或目录时，只有文件拥有者与root才有权力删除。
