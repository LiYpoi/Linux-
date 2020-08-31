# 文件操作

#### 文件浏览

* `ls`：列出目录内容
* `file`：确定文件类型
* `less`：查看文件内容

ls 命令的常用选项

| 选项 | 含义 |
| :---: | :---: |
| -a | 列出所有文件，包括隐藏文件 |
| -d | 查看目录详细信息 |
| -l | 以长格式显示 |
| -t | 按修改时间排序 |
| -r | 以相反顺序显示 |

`file filename` 可确定文件类型。

less 命令是一种查看文本文件的程序，使用`less filename`可查看文本，如果文本内容不止一夜，可以上下滚动文件，按q键可退出 less 程序。

#### 操作文件与目录

* `cp`：复制文件和目录
* `mv`：移动或重命名文件和目录
* `mkdir`：创建目录
* `rm`：移除文件和目录
* `ln`：创建硬链接和符号链接
* `touch`：创建空文件

**cp 命令基本语法**

`cp item1 item2`：将单个文件或目录 item1 复制到文件或目录 item2 中。

`cp item... directory`：将多个项目复制到一个目录中。

**cp 命令选项**

| 选项 | 含义 |
| :--- | :---: |
| -a | 复制文件和目录及其属性、所有权、权限 |
| -i | 在覆盖一个已存在的文件前，提示用户进行确认。 |
| -r | 复制目录时使用 |
| -u | 从一个目录复制到另一个目录时，只复制目标目录不存在的文件以及已存在文件的更新文件 |

**mv 命令基本语法**

`mv item1 item2`：将单个文件或目录 item1移动或重命名为 item2 。

`mv item... directory`：将一个或多个条目从一个目录移动到另一个目录下

| 选项 | 含义 |
| :--- | :---: |
| -i | 在覆盖一个已存在的文件前，提示用户进行确认。 |
| -u | 将文件从一个目录移动到另一个目录时，只移动目标目录中不存在的文件或是已存在文件的更新文件 |

**mkdir 命令基本语法**

`mkdir dir1`：创建单个 dir 目录。

`mkdir dir1 dir2 dir3`：创建3个目录。

**rm 命令**

**使用 rm 命令要小心！**

**使用 rm 命令要小心！**

**使用 rm 命令要小心！**

常用选项

| 选项 | 含义 |
| :--- | :---: |
| -r | 删除一个目录，包括其子目录，删除目录时必须指定该命令 |
| -f | 删除时忽略不存在的文件并不需提醒确认（强制删除） |
| -i | 删除一个已存在的文件前，提醒用户确认 |

**ln 命令基本语法**

`ln file link`：创建硬链接。

`ln -s item link`：创建符号链接（软链接）。

**硬链接与软链接的联系与区别**（摘自 [理解Linux的硬链接与软连接](https://www.ibm.com/developerworks/cn/linux/l-cn-hardandsymb-links/index.html#listing3%20)）

我们知道文件都有文件名与数据，这在 Linux 上被分成两个部分：用户数据 \(user data\) 与元数据 \(metadata\)。用户数据，即文件数据块 \(data block\)，数据块是记录文件真实内容的地方；而元数据则是文件的附加属性，如文件大小、创建时间、所有者等信息。在 Linux 中，元数据中的 inode 号（inode 是文件元数据的一部分但其并不包含文件名，inode 号即索引节点号）才是文件的唯一标识而非文件名。文件名仅是为了方便人们的记忆和使用，系统或程序通过 inode 号寻找正确的文件数据块。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA3.jpg)

为解决文件的共享使用，Linux 系统引入了两种链接：硬链接 \(hard link\) 与软链接（又称符号链接，即 soft link 或 symbolic link）。链接为 Linux 系统解决了文件的共享使用，还带来了隐藏文件路径、增加权限安全及节省存储等好处。若一个 inode 号对应多个文件名，则称这些文件为硬链接。**换言之，硬链接就是同一个文件使用了多个别名**。硬链接可由命令 link 或 ln 创建。如下是对文件 oldfile 创建硬链接。

```text
link oldfile newfile 
ln oldfile newfile
```

由于硬链接是有着相同 inode 号仅文件名不同的文件，因此硬链接存在以下几点特性：

* 文件有相同的 inode 及 data block；
* 只能对已存在的文件进行创建；
* 不能交叉文件系统进行硬链接的创建；
* 不能对目录进行创建，只可对文件创建；
* 删除一个硬链接文件并不影响其他有相同 inode 号的文件。

软链接与硬链接不同，若文件用户数据块中存放的内容是另一文件的路径名的指向，则该文件就是软连接。软链接就是一个普通文件，只是数据块内容有点特殊。软链接有着自己的 inode 号以及用户数据块。软链接更像Windows下的快捷方式。软链接的创建与使用没有类似硬链接的诸多限制：

* 软链接有自己的文件属性及权限等。
* 可对不存在的文件或目录创建软链接。
* 软链接可交叉文件系统。
* 软链接可对文件或目录创建。
* 创建软链接时，链接计数 i\_nlink 不会增加。
* 删除软链接并不影响被指向的文件，但若被指向的原文件被删除，则相关软连接被称为死链接（即 dangling link，若被指向路径文件被重新创建，死链接可恢复为正常的软链接）。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA4.jpg)

软链接基本语法：`ln -s 原文件或目录 链接名`

```text
liypoi@liypoi-virtual-machine:~$ ln -s /usr/bin mybin
```

```text
liypoi@liypoi-virtual-machine:~$ ll -t
总用量 152
drwxr-xr-x 19 liypoi liypoi  4096 2月   4 18:15 ./
lrwxrwxrwx  1 liypoi liypoi     8 2月   4 18:15 mybin -> /usr/bin/
```

可以看到mybin 链接指向/usr/bin目录，打开mybin后应该进入所链接的/usr/bin目录下，但使用pwd查看后，仍然是软链接所在的目录。

```text
liypoi@liypoi-virtual-machine:~$ cd mybin/
liypoi@liypoi-virtual-machine:~/mybin$ pwd
/home/liypoi/mybin
```

**touch 命令基本语法**

`touch filename`：创建一个空文件

`touch filename...`：创建多个空文件

