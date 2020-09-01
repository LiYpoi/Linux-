# shell提示符

#### 提示符的组成

```text
liypoi@liypoi-virtual-machine:~$ 
```

上面是系统默认的提示符，由用户名、主机名和工作目录，这种顺序的提示符是定义在一个叫PS1的环境变量中，使用`echo`命令可以查看PS1的值。

```text
liypoi@liypoi-virtual-machine:~$ echo $PS1
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
```

不同的Linux发行版，有不同的输出结果，输出结果由大量转义字符组成。

| 转义字符 | 含义 | 转义字符 | 含义 |
| :---: | :---: | :---: | :---: |
| \a | ASII铃声，遇到该字符时，计算机会发出声音 | \l | 当前终端名称 |
| \d | 以星期 月 日显示当前日期 | \! | 当前命令的历史编号 |
| \h | 本地机器的主机名 | \r | 回车符 |
| \j | 完整的主机名 | \s | shell程序的名称 |
| \t | 当前时间\(24小时制\) | \u | 当前用户的用户名 |
| \T | 当前时间\(12小时制\) | \v | shell的版本号 |
| \@ | 当前时间\(12小时制，有AM/PM\) | \V | shell的版本号和发行号 |
| \A | 当前时间\(24小时制\)格式为：小时:分钟 | \w | 当前工作目录 |
| \$ | 非root下输出"$"，root用户下输出"\#" | \\[ | 标志一个或多个非打印字符的开始 |
| \\# | 当前shell会话中输入的命令数 | \\] | 标志非显示字符序列的结束 |

#### 修改提示符

既然原提示符是系统默认的，所以可以尝试更改一下，当然，要先备份当前的设置。

1.创建备份

将原PS1设置备份到ps1\_old中，使用`echo`命令查看配置成功。

```text
liypoi@liypoi-virtual-machine:~$ ps1_old="$PS1"
liypoi@liypoi-virtual-machine:~$ echo $ps1_old
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
```

2.设计提示符

可以在提示符中添加一个铃声，每次显示提示符时会发出声音。

```text
liypoi@liypoi-virtual-machine:~$ PS1="\a\$"
$
```

可以在提示符中添加主机名和当前时间信息。

```text
liypoi@liypoi-virtual-machine:~$ PS1="\a\$"
$ PS1="\A \h \$"
20:59 liypoi-virtual-machine $
```

还可以仿照默认样式。

```text
20:59 liypoi-virtual-machine $PS1="<\u@\h \W>\$"
<liypoi@liypoi-virtual-machine ~>$
```

![ys12](http://image-liypo.test.upcdn.net/Blog_Picture/ys12.png)

3.颜色设置

从上图看到，自定义的提示符没有颜色，通过shell文本颜色转义序列可以设置不同颜色的提示符。

设置蓝色的提示符

```text
<liypoi@liypoi-virtual-machine ~>$PS1="\[\033[0;34m\]<\u@\h \W>\$"
```

但此时输入的字体颜色也会变蓝，所以再对输入字体进行设置

```text
<liypoi@liypoi-virtual-machine ~>$PS1="\[\033[0;34m\]<\u@\h \W>\$\[\033[0m\]"
```

![ys13](http://image-liypo.test.upcdn.net/Blog_Picture/ys13.png)

4.恢复设置

```text
<liypoi@liypoi-virtual-machine ~>$PS1="$ps1_old"
```

以上命令可以合并使用，就是说可以同时设置字体颜色样式等等，如果想保存自己的设置，使用`export PS1`命令保存至`.bashrc`文件中。

