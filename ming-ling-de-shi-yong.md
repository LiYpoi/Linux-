# 命令的使用

* `type`：显示命令的类型
* `which`：显示可执行程序的位置
* `man`：显示程序的手册页
* `apropos`：显示一系列合适的命令
* `info`：显示命令的info 条目
* `whatis`：显示一条命令的简述
* `alias`：创建一条命令的别名

#### 识别命令

Linux 为了识别命令的情况，提供了下面两个方法来识别命令类型。

**type 显示命令的类型**

基本语法：`type command`

type 命令是一个shell 内置命令，根据指定的命令名显示 shell 要执行命令的类型，例如：

```text
liypoi@liypoi-virtual-machine:~$ type cd
cd 是 shell 内建
liypoi@liypoi-virtual-machine:~$ type ls
ls 是 `ls --color=auto' 的别名
```

**which 显示可执行程序的位置**

which 命令**只适用于可执行程序**，不适于内置命令和命令别名，例如：

```text
liypoi@liypoi-virtual-machine:~$ which ls
/bin/ls
liypoi@liypoi-virtual-machine:~$ which cd
liypoi@liypoi-virtual-machine:~$ 
```

执行 cd 命令时，没有响应。

#### 获得命令文档

通过以下命令，可以查看每一类命令的可用文档。

**help 获得shell内置命令的帮助文档**

基本语法：`help command`

```text
liypoi@liypoi-virtual-machine:~$ help ls
bash: help: 没有与 `ls' 匹配的帮助主题。尝试 `help help' 或 `man -k ls' 或 `info ls'。
liypoi@liypoi-virtual-machine:~$ help cd
cd: cd [-L|[-P [-e]] [-@]] [目录]
    改变 shell 工作目录。
    
    改变当前目录至 DIR 目录。默认的 DIR 目录是 shell 变量 HOME
    的值。
    
    变量 CDPATH 定义了含有 DIR 的目录的搜索路径，其中不同的目录名称由冒号 (:)分隔。
    一个空的目录名称表示当前目录。如果要切换到的 DIR 由斜杠 (/) 开头，则 CDPATH
    变量不会被使用。
    
    如果路径找不到，并且 shell 选项 `cdable_vars' 被设定，则参数词被假定为一个
    变量名。如果该变量有值，则它的值被当作 DIR 目录。
    
    选项：
        -L  强制跟随符号链接: 在处理 `..' 之后解析 DIR 中的符号链接。
        -P  使用物理目录结构而不跟随符号链接: 在处理 `..' 之前解析 DIR 中的符号链接。
        -e  如果使用了 -P 参数，但不能成功确定当前工作目录时，返回非零的返回值。
        -@  在支持拓展属性的系统上，将一个有这些属性的文件当作有文件属性的目录。
    
    默认情况下跟随符号链接，如同指定 `-L'。
    `..' 使用移除向前相邻目录名成员直到 DIR 开始或一个斜杠的方式处理。
    
    退出状态：
    如果目录改变，或在使用 -P 选项时 $PWD 修改成功时返回 0，否则非零。
```

help 命令只能查看内置命令的文档。

大多数可执行程序支持`--help`选项，`--help`选项描述了命令支持的语法和选项，例如：

```text
liypoi@liypoi-virtual-machine:~$ mkdir --help
用法：mkdir [选项]... 目录...
Create the DIRECTORY(ies), if they do not already exist.
​
必选参数对长短选项同时适用。
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z                   set SELinux security context of each created directory
                         to the default type
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
      --help        显示此帮助信息并退出
      --version     显示版本信息并退出
​
GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
请向<http://translationproject.org/team/zh_CN.html> 报告mkdir 的翻译错误
```

**man 显示程序手册**

基本语法：`man program`

查看手册页后，退出时按 q 键。

**apropos 显示合适的命令**

基本语法：`apropos 搜索命令`

使用apropos 命令查看所搜命令的相关命令，就像搜索引擎一样，给出类似命令的匹配，例如：

```text
liypoi@liypoi-virtual-machine:~$ apropos mkdir
gvfs-mkdir (1)       - (未知的主题)
mkdir (1)            - make directories
mkdir (2)            - create a directory
mkdirat (2)          - create a directory
```

**whatis 显示命令的简述**

例如：

```text
liypoi@liypoi-virtual-machine:~$ whatis mkdir
mkdir (1)            - make directories
mkdir (2)            - create a directory
```

**info 显示程序的info条目**

目前未能理解该命令，暂贴上别人的链接[Linux 命令 info](%20https://www.cnblogs.com/mayou18/p/9166036.html%20)

#### 使用别名创建自己的命令

使用`alias`命令可以创建属于自己命令。

在使用命令行中，可以在多个命令之间添加分号，就可同时执行。例如：

```text
liypoi@liypoi-virtual-machine:~$ ls;cd playground/;ls -l
公共的  视频  文档  音乐  Ok.txt      snap
模板    图片  下载  桌面  playground  text.txt
总用量 16
drwxr-xr-x 2 liypoi liypoi 4096 1月  21 18:14 dir1
drwxr-xr-x 2 liypoi liypoi 4096 1月  21 18:14 dir2
-rw-r--r-- 4 liypoi liypoi 2635 1月  21 18:09 fun
-rw-r--r-- 4 liypoi liypoi 2635 1月  21 18:09 fun-hard
```

在创建自己的命令前，最好先使用 `type`命令测试一下该命令是否存在了。

基本语法：创建别名`alias name='string'`；删除别名`unalias name`

下面创建一个名为 liy01的命令，使它有上面命令的功能：

```text
liypoi@liypoi-virtual-machine:~/playground$ type liy01
bash: type: liy01: 未找到
liypoi@liypoi-virtual-machine:~/playground$ alias liy01='ls;cd playground/;ls -l'
liypoi@liypoi-virtual-machine:~/playground$ cd
liypoi@liypoi-virtual-machine:~$ liy01
公共的  视频  文档  音乐  Ok.txt      snap
模板    图片  下载  桌面  playground  text.txt
总用量 16
drwxr-xr-x 2 liypoi liypoi 4096 1月  21 18:14 dir1
drwxr-xr-x 2 liypoi liypoi 4096 1月  21 18:14 dir2
-rw-r--r-- 4 liypoi liypoi 2635 1月  21 18:09 fun
-rw-r--r-- 4 liypoi liypoi 2635 1月  21 18:09 fun-hard
liypoi@liypoi-virtual-machine:~/playground$ 
```

**注意：**当关闭当前终端后，创建的命令会消失，下次打开终端后并不能使用之前创建的命令。

