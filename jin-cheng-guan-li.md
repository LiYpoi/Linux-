# 进程管理

现代操作系统普遍支持多任务处理，多任务处理指系统通过快速切换运行中的程序实现多任务同时执行。Linux内核使用进程管理多任务，**进程是Linux用来安排不同程序等待CPU调度的一种组织方式。**

* `ps`：显示当前所有进程。
* `top`：试试显示当前所有任务资源占用情况。
* `jobs`：列出所有活动作业的信息。
* `bg`：在后台中运行作业。
* `fg`：在前台中运行作业。
* `kill`：发送信号给某个进程。
* `killall`：杀死指定名字的进程。

#### 进程如何工作

Linux中，每个指定的程序都被称为一个**进程**，每个进程都分配一个ID号。每一个进程都会对应一个父进程。每个进程基本以两种方式存在。**前台与后台**，前台进程就是用户在屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中，直到关机才结束。

系统启动时，内核先把它的一些程序初始化为进程，然后运行一个称为 init 的程序，init程序将依次运行一系列称为脚本初始化的shell脚本，这些脚本将启动所有的系统服务。一个程序的运行可以触发其他程序的运行，这被称为父进程创建子进程。

内核会给每个进程分配一个进程ID，即PID。进程ID是按递增的顺序来分配的，init进程的PID始终为1。

**使用ps 命令查看进程信息**

ps 命令是用来查看当前系统中，有哪些正在执行的进程，以及其执行情况。

```text
liypoi@liypoi-virtual-machine:~$ ps
   PID TTY          TIME CMD
  4099 pts/0    00:00:01 bash
  9549 pts/0    00:00:00 ps
```

不添加任何参数时，输出和当前终端会话相关的进程信息，即进程4099和进程9549，分别对应bash 命令和ps 命令。输出结果中，TTY是_teletype_的缩写，代表进程的控制终端；TIME表示进程消耗的CPU时间总和。

```text
liypoi@liypoi-virtual-machine:~$ ps x
   PID TTY      STAT   TIME COMMAND
  1716 ?        Ss     0:01 /lib/systemd/systemd --user
  1729 ?        S      0:00 (sd-pam)
  1755 ?        Sl     0:00 /usr/bin/gnome-keyring-daemon --daemonize --login
  1763 tty2     Ssl+   0:00 /usr/lib/gdm3/gdm-x-session --run-script env GNOME_
  1765 tty2     Sl+    2:34 /usr/lib/xorg/Xorg vt2 -displayfd 3 -auth /run/user
  1777 ?        Ss     0:03 /usr/bin/dbus-daemon --session --address=systemd: -
  1781 tty2     Sl+    0:01 /usr/lib/gnome-session/gnome-session-binary --sessi
  1907 ?        Ss     0:00 /usr/bin/ssh-agent /usr/bin/im-launch env GNOME_SHE
  1909 ?        Ssl    0:00 /usr/lib/at-spi2-core/at-spi-bus-launcher
  1914 ?        S      0:00 /usr/bin/dbus-daemon --config-file=/usr/share/defau
  1916 ?        Sl     0:01 /usr/lib/at-spi2-core/at-spi2-registryd --use-gnome
  1934 tty2     Rl+    6:29 /usr/bin/gnome-shell
​
...
```

在ps后面添加x 参数，将得到系统运行情况。TTY中列出的“？”表示没有控制终端。除此之外，输出结果中多了一个STAT的新列，表示进程的当前状态。

| 进程状态 | 含义 |
| :---: | :---: |
| R | 正在运行或准备运行 |
| S | 睡眠状态 |
| T | 暂停状态 |
| D | 不可终端的睡眠，进程在等待I/O操作 |
| Z | 无效进程 |

```text
liypoi@liypoi-virtual-machine:~$ ps -a
 PID TTY          TIME CMD
  1765 tty2     00:02:37 Xorg
  1781 tty2     00:00:01 gnome-session-b
  1934 tty2     00:06:36 gnome-shell
  1966 tty2     00:00:34 ibus-daemon
  1970 tty2     00:00:00 ibus-dconf
  1971 tty2     00:00:08 ibus-extension-
  1974 tty2     00:00:00 ibus-x11
...
```

`ps -a`：显示当前终端的所有进程信息。

```text
liypoi@liypoi-virtual-machine:~$ ps -u
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
liypoi     1763  0.0  0.2 188016  5412 tty2     Ssl+ 00:00   0:00 /usr/lib/gdm3
liypoi     1765  0.2  2.4 338648 49824 tty2     Sl+  00:00   2:37 /usr/lib/xorg
liypoi     1781  0.0  0.6 536728 12620 tty2     Sl+  00:00   0:01 /usr/lib/gnom
liypoi     1934  0.5  9.4 2784588 189660 tty2   Sl+  00:00   6:36 /usr/bin/gnom
...
```

`ps -u`：以用户的格式显示进程信息。

```text
liypoi@liypoi-virtual-machine:~$ ps -x
   PID TTY      STAT   TIME COMMAND
  1716 ?        Ss     0:01 /lib/systemd/systemd --user
  1729 ?        S      0:00 (sd-pam)
  1755 ?        Sl     0:00 /usr/bin/gnome-keyring-daemon --daemonize --login
  ...
```

`ps -x`：显示后台进程运行的参数。

```text
liypoi@liypoi-virtual-machine:~$ ps -aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.4 123420  9308 ?        Ss   2月10   0:11 /sbin/init s
​
```

`ps -aux`是上述三个命令的组合，其中输出又多出几个新列。

| 标题 | 含义 |
| :---: | :---: |
| USER | 用户名 |
| %CPU | 占用的CPU百分比 |
| %MEM | 占用内存百分比 |
| VSZ | 使用的虚拟内存 |
| RSS | 使用的物理内存 |
| START | 进程的状态 |

`ps -ef`：查看当前所有进程的父进程。

`ps -ef | grep command`：查看某个命令的父进程。

**使用top 命令动态查看进程信息**

ps 命令提供在其被执行时刻机器状态的一个快照，要查看机器实时运行情况，使用`top`命令。

```text
liypoi@liypoi-virtual-machine:~$ top
​
top - 19:39:50 up 19:41,  1 user,  load average: 0.06, 0.08, 0.03
任务: 250 total,   1 running, 249 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.0 us,  1.4 sy,  0.0 ni, 97.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1969.9 total,    110.7 free,   1053.6 used,    805.6 buff/cache
MiB Swap:    947.2 total,    934.0 free,     13.3 used.    720.1 avail Mem 
​
进程号 USER      PR  NI    VIRT    RES    SHR    %CPU  %MEM     TIME+ COMMAND  
  1765 liypoi    20   0  338648  49824  28028 S   1.0   2.5   2:43.52 Xorg     
  4089 liypoi    20   0  675512  42928  30040 S   1.0   2.1   
...
```

顶部信息中的字段

| 字段 | 含义 | 字段 | 含义 |
| :---: | :---: | :---: | :---: |
| top | 程序名 | 19:39:50 | 当前时间 |
| up 19:41 | 正常运行时间 | 1 user | 有1个用户已登录 |
| load average | 负载均值 | 1.0 us | 1%的CPU时间被用户进程占用 |
| 1.4 sy | 1.4%的CPU被系统进程占用 | 0.0 ni | 0%的CPU时间被低优先级进程占用 |
| 97.6 id | 97.6%是CPU时间是空闲的 | 0.0 wa | 0%的CPU时间用来等待I/O操作 |
| MiB Mem | 物理RAM使用情况 | MiB Swap | 虚拟内存使用情况 |

负载均衡：值等待运行的进程数，即共享CPU资源的处于可运行状态的进程数。显示的三个值分别对应不同的时间段。第一个对应的是前一分钟的均值，下一个对应的是前5分钟的均值，最后一个对应的是前15分钟的均值，均值小于1.0表示机器不忙。

按k键，可选择PID杀死程序。

#### 控制进程

**中断进程**

```text
liypoi@liypoi-virtual-machine:~$ xlogo
liypoi@liypoi-virtual-machine:~$ 
​
```

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA6.png)

使用`xlogo`命令可以调出一个包含X标识的可缩放窗口，此时shell 正在等待xlogo程序结束，按Ctrl-C可结束中断程序，大多数程序可以通过该方法实现中断。

**使进程在后台运行**

启动程序时让程序在后台运行，这样可以实现shell 提示符返回，但xlogo 程序不终止。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA7.png)

命令执行后出现xlogo窗口，shel l提示符返回，同时出打印出“\[1\] 10331”数字信息。该信息是shell 中称为**作业控制**（job control）的特性表现。表示已启动的作业编号为1，对应PID为10331。此时执行ps命令，可查看到当前运行的进程。

```text
liypoi@liypoi-virtual-machine:~$ jobs
[1]+  运行中               xlogo &
​
```

使用`jobs`命令可以查看当前终端启动的所有作业。

**使进程回到前台运行**

后台运行的程序不会受到任何键盘输入的影响，包括中断进程的Ctrl-C。想要结束后台的进程，要先使进程返回到前台运行，使用`fg`命令。

```text
liypoi@liypoi-virtual-machine:~$ fg
xlogo
^C
liypoi@liypoi-virtual-machine:~$ fg
bash: fg: 当前: 无此任务
​
```

可以通过在`fg`命令后面加上百分比符号和作业编号实现该功能，如果后台只有一个任务，可以不带参数。

**暂停进程**

如果是暂停进程而不是中断进程，要把前台进程运行的进程移到后台运行，使用Ctrl-Z暂停前台进程。也可以使用`bg`命令让进程移到后台运行，具体参数设置与`fg`类似。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA8.jpg)

#### 信号

使用`kill PID`的方式，可以杀死进程。但准确说并不是“杀死”进程，而是给进程发送信号。信号是操作系统和程序间通信的方式之一，终端接收输入时，它将发送信号到前台进程。程序“监听”信号，并且可以按照信号指示进行操作。

**使用kill 命令发送信号到进程**

基本语法：`kill -signal PID`

如果命令行中没有指定的信号，则默认发送TERM（终止）信号。

| 信号编号 | 信号名 | 含义 |
| :---: | :---: | :---: |
| 1 | HUP | 挂起信号 |
| 2 | INT | 终端信号，等价于Ctrl-C |
| 3 | QUIT | 退出信号 |
| 9 | KILL | 杀死信号 |
| 15 | TERM | 终止信号 |
| 18 | CONT | 继续信号 |
| 19 | STOP | 暂停信号 |

实例：

```text
liypoi@liypoi-virtual-machine:~$ xlogo &
[1] 10492
liypoi@liypoi-virtual-machine:~$ kill -3 10492
liypoi@liypoi-virtual-machine:~$ jobs
[1]+  退出                  (核心已转储) xlogo
​
```

**使用killall 命令发送信号给多个进程**

基本语法：`killall -u user -signal name...`

可以给指定程序或者指定用户名的多个进程发送信号，必须有root权限，才能使用`killall`命令给不属于自己的进程发送信号。

```text
liypoi@liypoi-virtual-machine:~$ xlogo &
[1] 10496
liypoi@liypoi-virtual-machine:~$ xlogo &
[2] 10498
liypoi@liypoi-virtual-machine:~$ killall xlogo
[1]-  已终止               xlogo
[2]+  已终止               xlogo
```

