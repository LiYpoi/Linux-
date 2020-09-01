# 软件包管理

软件包管理是一种在系统上安装、维护软件的方法。决定Linux发行版本质量最重要的因素是软件包系统和支持该发行版本社区的活力。

不同的Linux发行版用的是不同的软件包系统，这些软件包系统之间基本不兼容。主流的发行版采用的技术分两种：Debian系的.deb技术和Red hat系的.rpm技术。

目前所有的主流Linux发行版都提供图形化操作界面，这使得软件包管理变简单，但命令行进程可以执行许多图形化程序难以完成的任务。

#### 软件包系统工作方式

**软件包文件**

包文件是组成软件包系统的基本软件单元，它由组成软件包的文件压缩而成。包文件及既包含了安装文件，又包含了有关包自身的文本说明，还有一些安装软件包前后执行配置任务的安装脚本。

**库**

中心库，包含许多软件包，每一个软件包都是专门为该发行版本建立和维护的。Linux用户可以从其所使用的Linux版本的中心库中获得软件包。

**依赖**

程序之间相互依赖彼此完成既定工作，一些共有的操作，由多个程序共享的例程执行。这些例程存储在共享库中，共享库的文件为多个程序提供必要的服务。

**软件包工具**

软件包管理系统通常包含两类工具：执行安装、删除软件包文件等任务的低级工具和进行元数据搜索及提供依赖解决的高级工具。

| 发行版本 | 低级工具 | 高级工具 |
| :---: | :---: | :---: |
| Debian系 | dpkg | apt-get、aptitude |
| Red hat系 | rpm | yum |

#### 常见软件包管理任务

**查找库中的软件包**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | apt-cache search 文件名 |
| Red hat系 | yum search 文件名 |

```text
liypoi@liypoi-virtual-machine:~$ apt-cache search emacs
```

**安装库中的软件包**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | apt-get install 包名 |
| Red hat系 | yum install 包名 |

```text
root@liypoi-virtual-machine:~# apt-get install emacs
```

**删除软件包**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | apt-get remove 包名 |
| Red hat系 | yum erase 包名 |

```text
root@liypoi-virtual-machine:~# apt-get remove emacs
```

**更新库中的软件包**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | apt-get yudate;apt-get upgrade |
| Red hat系 | yum update |

```text
root@liypoi-virtual-machine:~#  apt-get update
```

**列出已安装的软件包**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | dpkg --list |
| Red hat系 | rpm -qa |

```text
liypoi@liypoi-virtual-machine:~$ dpkg --list
```

**判断软件包是否安装**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | dpkg --status 包名 |
| Red hat系 | rpm -q 包名 |

```text
liypoi@liypoi-virtual-machine:~$ dpkg --status python3
```

**显示已安装软件包的信息**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | apt-cache show 包名 |
| Red hat系 | yum info 包名 |

```text
liypoi@liypoi-virtual-machine:~$ apt-cache show python3
```

**查看某个文件由哪个软件包安装得到**

| 发行版 | 命令 |
| :---: | :---: |
| Debian系 | dpkg --search 文件名 |
| Red hat系 | rpm -qf 文件名 |

```text
liypoi@liypoi-virtual-machine:~$ dpkg --search /usr/bin/pydoc3
python3: /usr/bin/pydoc3
```

