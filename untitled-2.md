# 用户权限

Linux，或者说Unix操作系统有着多用户、多任务处理的特点，在同一时间内可以有多个用户使用一台计算机。多用户功能基于个人计算机未大量普及的背景下的，当时，计算机体积庞大价格昂贵，在一所大学中只有一台计算机，其他的终端机都连接在这台计算机上。为防止用户之间操作不造成相互干扰，产生了多用户处理的功能。

下面介绍Linux 下用户权限的问题。

* `id`：显示用户身份表示。
* `chmod`：更改文件的模式。
* `umask`：设置文件的默认权限。
* `su`：以另一个用户的身份运行shell。
* `sudo`：以另一个用户的身份执行命令。
* `chown`：更在文件所有者。
* `chgrp`：更改文件所属组。
* `passwd`：更改用户密码。

### 用户

当一个用户拥有一个文件或目录时，他同时拥有了对该文件或目录的访问权限以及控制权；系统可以对有共性的多个用户进行统一的管理，这些用户归属于一个组，组下用户对其拥有文件或目录的权限，由组的所有者授予。

使用`id`命令可以查看用户身份信息。

```text
liypoi@liypoi-virtual-machine:~/playground$ id
uid=1000(liypoi) gid=1000(liypoi) 组=1000(liypoi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),118(lpadmin),129(sambashare)
```

### 读、写、执行

```text
liypoi@liypoi-virtual-machine:~$ ls -l foo.txt 
-rw-r--r-- 1 liypoi liypoi 0 2月   7 20:37 foo.txt
```

上面命令的执行结果分为以下几个部分：

| 字符 | 表示 |
| :---: | :---: |
| - | 文件类型为- |
| rw- | 文件所有者权限rw- |
| r-- | 文件所在组用户权限为r-- |
| r-- | 文件其他组的用户权限r-- |
| 1 | 如果是文件，表示硬链接的个数，如果是目录则表示该目录下子目录的个数 |
| liypoi | 文件所有者 |
| liypoi | 文件所在组 |
| 0 | 文件大小 |
| 2月   7 20:37 | 最后修改时间 |
| foo.txt | 文件名 |

**文件类型**

| 属性 | 文件类型 |
| :---: | :---: |
| - | 普通文件 |
| d | 目录文件 |
| l | 软链接 |
| c | 字符设备文件 |
| b | 块设备文件，例如硬盘驱动或光驱 |

**权限属性**

| 属性 | 文件 | 目录 |
| :---: | :---: | :---: |
| r | 可以读取、查看 | 可以读取，ls 查看目录内容 |
| w | 可以修改，但不代表可以删除，删除的前提是拥有该文件所在目录的写权限 | 可以修改，可在目录内创建、删除、重命名目录 |
| x | 可以被执行 | 可以进入目录 |

#### chmod：更改文件的模式

**八进制数字表示权限**

每个八进制数字对应着3个二进制数字，计算机用二进制的数字表示权限属性，所以读（r）、写（w）、执行（x）用八进制数字表示为4、2、1。

常用的几种权限表示：7（rwx）、6（rw-）、5（r-x）、4（r--）、0（---）。

之前创建的foo.txt文件权限为：

```text
liypoi@liypoi-virtual-machine:~$ ls -l foo.txt 
-rw-r--r-- 1 liypoi liypoi 0 2月   7 20:37 foo.txt
```

下面将其文件所有者权限改为rwx，文件所在组用户权限改为r-x，文件其他组用户权限改为r-x，使用`chmod`命令：

```text
liypoi@liypoi-virtual-machine:~$ chmod 755 foo.txt 
liypoi@liypoi-virtual-machine:~$ ls -l foo.txt 
-rwxr-xr-x 1 liypoi liypoi 0 2月   7 20:37 foo.txt
```

**符号表示法**

`chmod`命令支持一种符号表示法。

| 符号 | 含义 |
| :---: | :---: |
| u | 文件、目录所有者 |
| g | 文件所在组 |
| o | 其他用户 |
| a | ugo三种符号的组合 |
| + | 添加一种权限 |
| - | 删除一种权限 |
| = | 指定权限 |

给foo.txt文件的所有者读写执行权限，给所在组读执行权限，给其他组读执行权限：

```text
liypoi@liypoi-virtual-machine:~$ chmod u=rwx,g=rx,o=rx foo.txt 
liypoi@liypoi-virtual-machine:~$ ls -l foo.txt 
-rwxr-xr-x 1 liypoi liypoi 0 2月   7 20:37 foo.txt
```

符号表示的几组实例：

| 符号 | 含义 |
| :---: | :---: |
| u+x | 文件所有者添加执行权限 |
| u-x | 文件所有者删除执行权限 |
| +x | 文件所有者、文件所在组、其他用户添加执行权限，等价于a+x |

#### umask：设置文件的默认权限

下面新建一个moe.txt文件，查看其权限，发现默认权限为rw-、r--、r--，使用`umask`命令更改文件的权限属性。

```text
liypoi@liypoi-virtual-machine:~$ touch moe.txt
liypoi@liypoi-virtual-machine:~$ ls -l moe.txt 
-rw-r--r-- 1 liypoi liypoi 0 2月   8 18:34 moe.txt
```

先查看当前掩码值，直接输入命令`umask`，不添加任何参数，得到0022（常用掩码值还有0002）。

```text
liypoi@liypoi-virtual-machine:~$ umask
0022
```

删除moe.txt文件，设置掩码值0000，即关闭该功能，再次新建文件moe.txt。

```text
liypoi@liypoi-virtual-machine:~$ rm -rf moe.txt 
liypoi@liypoi-virtual-machine:~$ umask 0000
liypoi@liypoi-virtual-machine:~$ touch moe.txt
liypoi@liypoi-virtual-machine:~$ ls -l moe.txt 
-rw-rw-rw- 1 liypoi liypoi 0 2月   8 18:44 moe.txt
```

发现文件的默认权限已经改变，所有用户都有了写权限。

**掩码问题**

上述通过设置掩码值更改默认权限，掩码值对应不同的权限。以掩码值0000为例，它其实是掩码八进制的表示形式。

| 原文件权限 | --- | rw- | rw- | rw- |
| :---: | :---: | :---: | :---: | :---: |
| 掩码（二进制） | 000 | 000 | 000 | 010 |
| 结果 | --- | rw- | rw- | r-- |

从表中看出，（第一位为特殊掩码，暂不解释）掩码的二进制数值中每个出现1的地方，其对应属性都被取消。所以可以解释为什么在默认掩码0022时，创建文件的权限为`-rw-r--r--`，2的二进制值为010，原文件权限为rw-，在w 的位置出现1，w 权限就被去除，只剩下r 权限，相当于（rw-）-（w）= r。

通过`umask 掩码值`的方式更改默认权限，并不能永久改变，关闭终端后再次打开，默认权限还会变回去。永久更改要在 /etc/profile文件或/etc/bashrc文件中添加`umask 掩码值`。

### 切换用户

#### su：以另一个用户的身份运行shell

基本语法：`su -l user`

在`su`后如果有`-l`参数，那么会得到后面指定用户的登录界面，会登录指定用户。如果不指定用户，则默认为root 用户登录，`-l`此时可以直接写为`-`。

```text
liypoi@liypoi-virtual-machine:~$ su -
密码： 
：认证失败
```

为什么会登录认证失败？ 因为Ubuntu安装后root用户默认是被锁定的，不允许登录，也不允许 su 到 root，所以需要先设置一下。

```text
liypoi@liypoi-virtual-machine:~$ sudo passwd 
[sudo] liypoi 的密码： 
输入新的 UNIX 密码： 
重新输入新的 UNIX 密码： 
passwd：已成功更新密码
```

再次登录root 用户，登录成功，提示符的结尾字符从$变为\#。使用结束后输入`exit`退出root 用户。

```text
liypoi@liypoi-virtual-machine:~$ su -
密码： 
root@liypoi-virtual-machine:~# exit
注销
liypoi@liypoi-virtual-machine:~$
```

使用`su -c 'command'`可执行单个命令，单个命令行将被传送到一个新的shell 环境下执行。

#### sudo：以另一个用户的身份执行命令

基本语法：`sudo command`

使用`sudo`命令时，并不需要输入root 用户密码，只需输入用户自己的密码进行认证。`su`命令和`sudo`命令间的区别在于`sudo`命令不需要启动新的shell 环境，也不需要加载另一个用户的运行环境。

#### chown：更改文件所有者

基本语法：`chown newowner file` 改变文件的所有者；`chown newowner:newgroup file` 改变用户的所有者和所有组；如果改变的是目录，为使其下所有子文件或目录递归生效，在后面添加`-R`参数。

**实例1.把文件cat\_word.txt 的所有者从liypoi改为tom。**

```text
liypoi@liypoi-virtual-machine:~$ ls -l cat_word.txt 
-rw-r--r-- 1 liypoi liypoi 29 2月   4 12:20 cat_word.txt
```

切换至tom 用户。

```text
liypoi@liypoi-virtual-machine:/home$ cd tom/
```

执行`chown`命令，发现没有对应文件，因为tom 用户没有 liypoi 用户对文件的操作权限。

```text
liypoi@liypoi-virtual-machine:/home/tom$ chown tom cat_word.txt
chown: 无法访问'cat_word.txt': 没有那个文件或目录
```

在返回根目录后，查看家目录的权限，发现根用户对家目录有读写和执行的权限。

```text
liypoi@liypoi-virtual-machine:/$ ls -l
drwxr-xr-x   4 root root      4096 2月   8 23:30 home
```

查看家目录发现，家目录下的用户目录对于其他用户来说，有读和执行的权限。

```text
liypoi@liypoi-virtual-machine:/home$ ls -l
总用量 8
drwxr-xr-x 19 liypoi liypoi 4096 2月   8 22:43 liypoi
drwxr-xr-x  2 tom    tom    4096 2月   8 23:30 tom
```

查看liypoi 用户的cat\_word.txt文件权限，发现它对于其他用户来说，只有读的权限。

```text
-rw-r--r--  1 liypoi liypoi   29 2月   4 12:20 cat_word.txt
```

所以该实例并不能成功。

**实例2.把/playground目录下所有文件和目录所有者改为tom。**

首先要进入root 用户目录下，然后进入/playground 所在的目录/home/liypoi中进行操作。

```text
root@liypoi-virtual-machine:/home/liypoi# chown -R tom playground/
```

使用`-R`参数，将文件目录递归更改。

```text
root@liypoi-virtual-machine:/home/liypoi# ls -l
drwxr-xr-x  4 tom    liypoi 4096 2月   6 20:17 playground
root@liypoi-virtual-machine:/home/liypoi/playground# ls -l
总用量 20
drwxr-xr-x 2 tom liypoi 4096 1月  21 18:14  dir1
drwxr-xr-x 2 tom liypoi 4096 1月  21 18:14  dir2
-rw-r--r-- 1 tom liypoi    0 2月   6 20:17 'file type.txt'
-rw-r--r-- 4 tom liypoi 2635 1月  21 18:09  fun
-rw-r--r-- 4 tom liypoi 2635 1月  21 18:09  fun-hard
-rw-r--r-- 1 tom liypoi 1014 2月   4 10:59  ls-output.txt
```

目录及其下文件和子目录所有者都变成了tom。

#### chgrp：更改文件所属组

基本语法：`chgrp newgroup file` 改变文件的所有组

该命令只能改变所有组，和`chown`命令的使用方式几乎相同。

