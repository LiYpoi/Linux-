# 重定向

* `cat`：合并文件
* `sort`：对文本排序
* `uniq`：报告或删除文件中的重复行
* `wc`：打印文件中的换行符、字、字节的个数
* `grep`：打印匹配行
* `head`：输出文件中的第一部分内容，默认显示前10行内容
* `tail`：输出文件的最后一部分内容，默认显示后10行内容
* `tee`：读取标准输入的数据，并将其内容输出到标准输出和文件中

#### 标准输入、标准输出、标准错误

对于很多程序来说，输出通常分两种类型。一种是程序运行的结果，即程序生成的数据；另一种是状态和错误信息。

程序的运行结果发送到了一个称为标准输出（stdout）的文件中，状态信息被发送到一个称为标准错误（stderr）的文件中，这两个文件都直接链接到屏幕上，不会保存在磁盘中；标准输入（stdin）连接到键盘上。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA5.jpg)

通过I/O重定向，可以改变输入、输出内容的来源和目的地。

**标准输出重定向**

语法：重定向`>` ;追加`>>`

**重定向**

先查看当前目录下所含的文件

```text
liypoi@liypoi-virtual-machine:~$ ls -l
总用量 48
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 1月  21 18:14 playground
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
```

使用重定向将上面内容输入到文件中，这个文件如果不存在，则会自动创建。

```text
liypoi@liypoi-virtual-machine:~$ ls -l > ls-output.txt
```

再次查看当前目录，发现多出了一个“ls-output.txt”的文件。

```text
liypoi@liypoi-virtual-machine:~$ ls -tl
总用量 52
-rw-r--r-- 1 liypoi liypoi  729 2月   4 10:44 ls-output.txt
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 1月  21 18:14 playground
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
```

查看该文件，发现内容已经定向进去了。

```text
liypoi@liypoi-virtual-machine:~$ more ls-output.txt 
总用量 48
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
-rw-r--r-- 1 liypoi liypoi    0 2月   4 10:44 ls-output.txt
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 1月  21 18:14 playground
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
```

如果不在重定向符前添加任何内容，或者指定不存在的目录，原文件内容会清空，变成空文件。

```text
liypoi@liypoi-virtual-machine:~$ > ls-output.txt 
liypoi@liypoi-virtual-machine:~$ file ls-output.txt 
ls-output.txt: empty
```

**追加**

如果不想清空原文件，仅仅想在文件末尾添加内容，可以使用追加。

还以上述文件为例，先将/home目录下的内容定向进入，然后再次在文件末尾追加该内容。

```text
liypoi@liypoi-virtual-machine:~$ ls -l > ls-output.txt 
liypoi@liypoi-virtual-machine:~$ more ls-output.txt 
总用量 48
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
-rw-r--r-- 1 liypoi liypoi    0 2月   4 11:02 ls-output.txt
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 2月   4 10:57 playground
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
```

```text
liypoi@liypoi-virtual-machine:~$ ls -l >> ls-output.txt 
liypoi@liypoi-virtual-machine:~$ more ls-output.txt 
总用量 48
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
-rw-r--r-- 1 liypoi liypoi    0 2月   4 11:02 ls-output.txt
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 2月   4 10:57 playground
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
总用量 52
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 公共的
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 模板
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 视频
drwxr-xr-x 2 liypoi liypoi 4096 1月  20 21:40 图片
drwxr-xr-x 6 liypoi liypoi 4096 1月  21 17:30 文档
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 下载
drwxr-xr-x 2 liypoi liypoi 4096 1月  13 22:17 音乐
drwxr-xr-x 2 liypoi liypoi 4096 1月  17 19:09 桌面
-rw-r--r-- 1 liypoi liypoi  729 2月   4 11:02 ls-output.txt
-rw-r--r-- 1 liypoi liypoi   30 1月  26 20:10 Ok.txt
drwxr-xr-x 4 liypoi liypoi 4096 2月   4 10:57 playground
drwxr-xr-x 4 liypoi liypoi 4096 1月  17 18:02 snap
-rw-r--r-- 1 liypoi liypoi   71 1月  26 20:30 text.txt
总用量 52
```

**标准错误重定向**

一个程序可以把生成的输出内容发送到任意文件流中，这些文件流中的三个分别对应标准输入文件、标准输出文件、标准错误文件。在shell 内部它们的索引为0、1、2。通过使用文件描述符编号，来表示重定向错误。

```text
liypoi@liypoi-virtual-machine:~$ ls -error 2> ls-error.txt 
liypoi@liypoi-virtual-machine:~$ more ls-error.txt 
ls: 不适用的选项 -- e
Try 'ls --help' for more information.
```

**标准输出和标准错误重定向到同一个文件**

使用下面两种方法，将所有输出内容放到一个文件中。

```text
liypoi@liypoi-virtual-machine:~$ ls -l > ls-output.txt 2>&1
```

上面方法将执行两个重定向操作，先会重定向标准输出到“ls-output.txt”文件中，然后使用标记符“2&gt;&1”把标准错误重定向到标准输出中。但这种方法写起来太长，所以有以下方法。

```text
liypoi@liypoi-virtual-machine:~$ ls -l &> ls-output.txt 
```

**处理不想要的输出**

把输出重定向到/dev/null中，不会得到任何输出，这个文件称为位桶（bit bucket），它接受任何输入，但不对输入进行处理，也不能读取到任何内容。

**标准输入重定向**

`cat`命令读取一个或多个文件，并把它们复制到标准输出文件中。除此功能外，`cat`命令可以用来显示文件，功能与`more`和`less`类似。

一些大的视频文件经常会拆分成多个部分，使用`cat`可以将这些文件拼接在一起，如果这些文件原名称为“mv01.mp4、mv02.mp4、mv03.mp4”，使用下命令会将这些文件拼接。

```text
liypoi@liypoi-virtual-machine:~$ cat mv.0* > mv
```

**输入重定向**

如果只输入`cat`命令，而不添加任何参数，它将会从标准输入读取内容，标准输入默认连接到键盘，所以此时它正等待从键盘输入的内容。

```text
liypoi@liypoi-virtual-machine:~$ cat
liy
liy
nihao
nihao
​
```

在缺少文件名的情况下，cat 将把标准输入的内容复制到标准输出文件中，所以会出现重复的文本。按Ctrl+D会结束输入。

使用cat 可以创建短文本文件，下例将先创建cat\_word.txt文件，然后就可以在键盘上输入文件内容，最后按Ctrl+D关闭文件。

```text
liypoi@liypoi-virtual-machine:~$ cat > cat_word.txt
miao miao miao
miao miao
miao
```

使用cat 命令查看文件内容。

```text
liypoi@liypoi-virtual-machine:~$ cat cat_word.txt 
miao miao miao
miao miao
miao
```

使用cat 进行重定向，把标准输入源从键盘变成cat\_word.txt文件。

```text
liypoi@liypoi-virtual-machine:~$ cat < cat_word.txt 
miao miao miao
miao miao
miao
```

#### 管道

使用管道符“\|”可以把一个命令的标准输出传送到另一个命令的标准输入中，通过该方法，可以检查任意一条生成标准输出的命令的运行结果。

使用less 命令接收标准输入。

```text
liypoi@liypoi-virtual-machine:~$ ls | less
```

**过滤器**

把多条命令合并成一个管道，这样可以处理复杂的操作，这种方式中用到的命令通常被称为过滤器。

过滤器接受输入，按照某种方式对输入进行改变，然后再输出。

先接受/bin和/usr/bin目录下文件列表的输入，使用`sort`对输入顺序排序，然后用`less`命令将结果发送到标准输出。

```text
liypoi@liypoi-virtual-machine:~$ ls /bin /usr/bin | sort | less
```

**uniq 报告或忽略文件中重复的行**

`uniq`通常和`sort`结合使用，默认情况下，该命令删除来自sort命令输出内容中的任意重复行。

```text
liypoi@liypoi-virtual-machine:~$ ls /bin /usr/bin | sort | uniq | less
​
```

如果想查看重复行的列表，可以在uniq命令后添加-d选项。

**wc 打印行数、字数、字节数**

默认情况下，`wc`可以显示出一个文件的行数、字数、字节数。

```text
liypoi@liypoi-virtual-machine:~$ wc Ok.txt 
 6  6 30 Ok.txt
```

如果只想单一显示，使用man查看其手册，发现有几个可选参数。

```text
        -c, --bytes
              print the byte counts
​
       -m, --chars
              print the character counts
​
       -l, --lines
              print the newline counts
              
        -L, --max-line-length
              print the maximum display width
              
       -w, --words
              print the word counts
```

**grep 打印匹配符**

`grep`可以在文件中查询匹配文本。

查询文件名中含有zip的所有文件。

```text
liypoi@liypoi-virtual-machine:~$ ls /bin /usr/bin | sort | uniq | grep zip
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
gunzip
gzip
mzip
preunzip
prezip
prezip-bin
unzip
unzipsfx
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipnote
zipsplit
```

**head/tail 打印文件的开头/结尾部分**

这两个命令默认输出前10行和后10行内容，可以使用-n选项来调整输出行数。

```text
liypoi@liypoi-virtual-machine:~$ head -n 2 Ok.txt 
ok11
ok12
liypoi@liypoi-virtual-machine:~$ tail -n 2 Ok.txt 
ok15
ok16
```

**实时查看日志文件**

`tail -f 文件`：实时追踪该文档的所有更新。

**tee 的使用**

Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。

tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。

```text
liypoi@liypoi-virtual-machine:~$ ls /bin /usr/bin | tee ls.txt | grep zip
bunzip2
bzip2
bzip2recover
gunzip
gzip
funzip
gpg-zip
mzip
preunzip
prezip
prezip-bin
unzip
unzipsfx
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipnote
zipsplit
```

