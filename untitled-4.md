# vi 和 vim 编辑器

#### 基本介绍

vi 是UNIX传统核心软件，最初版由Sun公司的创始人之一 Bill Joy 编写，所有Linux系统都会内置 vi 文本编辑器。虽然图形界面的编辑器易于使用，但在没有图形界面的时候，vi 就发挥了极大的作用，而且 vi 是轻量级软件，运行速度快。

vim 可以看做 vi 的增强版，可以以字体颜色辨别语法的正确性，方便程序设计。

#### vi 和 vim 的三种常见模式

**正常模式**

以 vim 打开一个文档就直接进入一般模式了，在这个模式中，可以使用方向键移动光标，可以使用删除字符或删除整行来处理内容，也可以使用快捷键进行复制粘贴。

**插入模式**

按下i,I,o,O,a,A,r,R等任何一个字母后进入编辑模式，在此模式下，可以输入内容。

**命令行模式**

在此模式下，根据提供的指令，可以完成读取、存盘、替换、退出vim、显示行号等动作。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E5%9B%BE%E4%BE%8B1.png)

#### 基本编辑

**添加文本**

首先使用 vim创建一个text.txt文件并打开。

```text
liypoi@liypoi-virtual-machine:~$ vim text.txt
```

打开后界面如下，光标停留在首行。**此时为命令行模式，不能写入内容。**

```text
~                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                              
"text.txt" [新文件]                                          0,0-1        全部
```

**按下`i`进入插入模式，可以编辑内容。**

```text
这是一个vim测试文件
用来
测试一些
基本的
快捷指令                                                                              
~                                                                              
~                                                                              
~                                                                              
~                                                                                        
~                                                                              
~                                                                              
~                                                                              
-- 插入 --                                                   1,1          全部
```

**保存所编辑的内容**，先按`Esc`键返回到正常模式，按下 : 键后，一个冒号会出现在屏幕底部，再输入 w 即`:w`后，文件会写入硬盘，输入`:q`退出vim，`:wq`命令可以合并使用，在没有保存修改过的文件时，vim 不能正常退出，此时使用`:q!`。简单来说，当你只想打开一个文件看一看，使用`:q`退出；当你看完后想修改一下，修改一大堆后不想保存了，再改回去太麻烦，就用`:q!`退出；当你修改后觉得很满意，使用`:wq`保存退出。

**移动光标**

在命令行模式下，无法对text.txt文件的内容进行修改，但除了使用方向键移动文档中的光标外，还可以使用`H J K L`键移动光标，其分别对应左、下、上、右。数字0会将光标移至本行开头。`Shift-G`跳转至文件最后一行，`gg`跳转至文件首行。

**撤销与删除**

在插入模式进行编辑后，退至命令行模式下，使用`:u`可以撤销之前的操作。**注意：**vi 只能取消一次操作，vim 可取消多次操作。

| 命令 | 删除内容 |
| :---: | :---: |
| x | 当前字符 |
| 3x | 当前字符和之后2个字符 |
| dd | 当前行 |
| 3dd | 当前行和之后2行 |
| dG | 当前行到文件末尾 |
| d3G | 当前行到文件第3行 |

**剪切、复制和粘贴**

命令d不只是删除文本，每次用户使用d命令后，都会复制删除的内容，类似于剪贴板。

命令y是复制文本内容。

| 命令 | 复制内容 |
| :---: | :---: |
| yy | 当前行 |
| 5yy | 当前行和之后4行 |
| yG | 当前行到文件末尾 |
| y5G | 当前行到文件第5行 |

命令p是粘贴文本，准确的说是将文本粘贴到光标之后，P命令是将光标粘贴到光标之前。

**显示行号**

在对文件编辑时，当文件内容较多，处理起来比较麻烦，（命令行模式下）使用`:set nu`和`:set nonu`即可显示和撤销行号。搭配`Shift-G`命令，跳转至第4行，先`:set nu`显示行号，再输入 4 ，然后输入`Shift-G`就可以跳转至第4行。

#### 遇到的问题

在对text.txt文件不正常关闭后，每次打开都会出现以下内容。

```text
E325: 注意
发现交换文件 ".text.txt.swp"
            所有者: liypoi    日期: Sun Jan 26 20:16:23 2020
            文件名: ~liypoi/text.txt
            修改过: 否
            用户名: liypoi      主机名: liypoi-virtual-machine
           进程 ID: 4205 (仍在运行)
正在打开文件 "text.txt"
              日期: Sun Jan 26 20:30:04 2020
      比交换文件新！

(1) Another program may be editing the same file.  If this is the case,
    be careful not to end up with two different instances of the same
    file when making changes.  Quit, or continue with caution.
(2) An edit session for this file crashed.
    如果是这样，请用 ":recover" 或 "vim -r text.txt"
    恢复修改的内容 (请见 ":help recovery")。
    如果你已经进行了恢复，请删除交换文件 ".text.txt.swp"
    以避免再看到此消息。

交换文件 ".text.txt.swp" 已存在！
以只读方式打开([O]), 直接编辑((E)), 恢复((R)), 退出((Q)), 中止((A)):
```

**出现原因如下：**

在用vim打开一个文件时，其会产生一个cmd.swap文件，用于保存数据，当文件非正常关闭时，可用此文件来恢复，当正常关闭时，此文件会被删除，非正常关闭时，不会被删除，所以提示存在.swap文件

**解决方法如下：**

方法1

```text
vim -r text.txt
```

恢复以后把.swap文件删掉，在打开时就不会用提示良，注意.swap文件是个隐藏文件。可用：la查看。以.开头的是隐藏文件。

方法2

ls -a 查询隐藏文件，将后缀名为.swp的文件删除

```text
liypoi@liypoi-virtual-machine:~$ ls -a
.       音乐           .gnupg           snap
..      桌面           .ICEauthority    .sudo_as_admin_successful
公共的  .bash_history  .local           text.txt
模板    .bash_logout   .mozilla         .text.txt.swp
视频    .bashrc        Ok.txt           .viminfo
图片    .cache         playground
文档    .config        .profile
下载    .dbus          .python_history
```

```text
rm -f .text.txt.swp
```

