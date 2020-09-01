# 环境

shell在调用环境期间会存储大量信息。程序使用存储在环境中的数据来确定配置，大多数系统程序使用配置文件存储程序设置，但有一些程序会查找环境中存储的变量来调整自己的行为。因此，用户可以使用环境来自定义shell。

* `printenv`：打印部分或全部的环境信息。
* `set`：设置shell选项。
* `export`：将环境导出到随后要运行的程序中。
* `alias`：为命令创建一个别名。

#### 环境中的存储

shell在环境中存储了两种基本类型的数据，分别是环境变量和shell 变量。shell变量是由bash存放的少量数据，环境变量是除此之外的所有其他变量。除变量外，shell还存储了一些编程数据，即别名和shell函数。

**检查环境**

`set`命令会同时显示shell变量和环境变量，而`printenv`只会显示环境变量。其包含的内容一般都很长，所以查看时把这两个命令的输出以管道形式重定向到`less`命令中。

```text
liypoi@liypoi-virtual-machine:~$ printenv | less
输出内容...
```

`printenv`命令也可以列出特定变量的值。

```text
liypoi@liypoi-virtual-machine:~$ printenv USER
liypoi
```

使用`set`命令时，如果不带任何参数，只会显示shell变量、环境变量、已定义的shell函数。

```text
liypoi@liypoi-virtual-machine:~$ set | less
输出内容...
```

`set`命令输出的结果是按字母顺序排列的。`set`和`printenv`命令都不能显示一个环境元素的别名。查看别名直接使用`alias`。

**环境变量**

某些环境变量会因发行版的不同而有所差异，下面列举一些变量例子。

| 变量 | 说明 |
| :--- | :---: |
| DISPLAY | 运行图形界面时界面的名称 |
| SHELL | 本机shell名称 |
| HOME | 本机家目录的路径名 |
| PATH | 目录列表，当用户输入可执行程序名称时，会查找该目录列表 |
| PWD | 当前工作目录 |
| USER | 用户名 |
| TERM | 终端类型名称 |

#### 环境的建立过程

用户登录系统后，bash会启动并读取称为启动文件的配置脚本，这些脚本定义了所有用户共享的默认环境。然后bash会读取存储在主目录下用于定义个人环境的启动文件，而这些步骤的执行顺序是由shell会话类型决定的。

**login 和non-login shell**

shell会话有两种类型，分别为`login shell`会话和`non-login shell`会话。前者会提示用户输入用户名和密码，如虚拟控制台会话；在GUI中启动的终端会话是`non-login shell`会话。

**login shell 的启动文件**

| 文件 | 说明 |
| :--- | :--- |
| /etc/profile | 所有用户的全局配置脚本 |
| ~/.bash\_profile | 用户的个人启动文件 |
| ~/.bash\_login | 若个人启动文件缺失，则读取此脚本 |
| ~/.profile | 若上两行文件都缺失，则读取此文件 |

**non-login shell 的启动文件**

| 文件 | 内容 |
| :--- | :--- |
| /etc/bash.bashrc | 所有用户的全局配置脚本 |
| ~/.bashrc | 用户的个人启动文件 |

启动文件通常是隐藏的，查看时使用`-a`选项。

#### 修改环境

在PATH中添加目录定义额外环境变量，需要将这些更改放入.bash\_profile文件中，或其他等效的文件，这取决于系统的发行版，Ubuntu系统使用.profile文件，其他的改变则录入.bashrc文件中，一般普通用户只需要对主目录下的文件作出修改即可。

**激活修改**

在对`.bashrc`做出修改后，关闭shell并重启才能生效，因为shell在启动时读取`.bashrc`文件。可以使用命令强制执行读取`.bashrc`文件。

```text
liypoi@liypoi-virtual-machine:~$ bash .bashrc
```

**文本编辑器**

Linux系统可以使用的文本编辑器很多。文本编辑器大致分两类，图形界面和基于文本的。GNOME（网络对象模型环境）和KDE（K桌面环境）都有图形界面编辑器。GNOME配备的编辑器叫gedit，通常被称为文本编辑器。

常用文本编辑器有nano、vi、vim、emacs等。

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA9.png)

![](http://image-liypo.test.upcdn.net/Blog_Picture/%E6%BC%94%E7%A4%BA10.png)

