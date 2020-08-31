# 导航文件系统

#### 文件系统树

与Windows相同，Linux下的文件也是在树形结构目录中组织的，不同的是，在Windows中，每个存储设备（每个盘）都有一个独立的文件系统树，而在Linux中，只有一个文件系统树，作为系统管理员，可以设置存储设备挂载到文件系统树的不同位置。

#### 目录操作

现在要进行如下操作：

**1.显示当前工作目录并列出目录内容。**

```text
liypoi@liypoi-virtual-machine:~$ pwd
/home/liypoi
liypoi@liypoi-virtual-machine:~$ ls
公共的  模板  视频  图片  文档  下载  音乐  桌面  snap
```

`pwd`:打印当前的工作目录。

`ls`:列出目录内容。

**2.在/usr和/bin目录下，使用绝对路径和相对路径的方式更改当前工作目录。**

```text
liypoi@liypoi-virtual-machine:~$ cd /usr/bin
liypoi@liypoi-virtual-machine:/usr/bin$ 
```

绝对路径名：从根目录开始，后面跟着一个个文件名，Windows下的“E:\PyCharm\venv\Lib”，就是用的绝对路径名，当然，在Linux中路径名之间使用“/”。

```text
liypoi@liypoi-virtual-machine:/usr/bin$ cd ..
liypoi@liypoi-virtual-machine:/usr$ pwd
/usr
liypoi@liypoi-virtual-machine:/usr$ cd ./bin
liypoi@liypoi-virtual-machine:/usr/bin$ 
```

相对路径名：绝对路径是从根目录开始，通向目标目录，相对路径是从工作目录开始的。“.”代表工作目录，“..”代表工作目录的父目录。"./"可以省略，即`cd bin`。

**cd指令**

* `cd ..` 回到当前目录的上一级目录
* `cd ~`或`cd`回到家目录

