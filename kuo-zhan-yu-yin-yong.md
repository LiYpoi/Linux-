# 扩展与引用

* `echo`：显示一行文本

#### 扩展

在输入命令后按下Enter键，bash 会在执行之前对文本进行多重处理，这个处理的过程称为**扩展**。

```text
liypoi@liypoi-virtual-machine:~$ echo text
text
```

echo 命令会将传入的参数打印出来。

```text
liypoi@liypoi-virtual-machine:~$ echo *
公共的 模板 视频 图片 文档 下载 音乐 桌面 
cat_word.txt ls-error.txt ls-output.txt 
ls.txt mybin Ok.txt playground snap text.txt
```

但传入的参数是“\*”号时，却不会输出“\*”，因为“\*“表示匹配文件名中的任意字符。shell 把”\*“扩展了其他功能，上例中被扩展为当前工作目录下的所有文件名。

**路径名扩展**

```text
liypoi@liypoi-virtual-machine:~$ echo l*
ls-error.txt ls-output.txt ls.txt
liypoi@liypoi-virtual-machine:~$ echo *t
cat_word.txt ls-error.txt ls-output.txt ls.txt Ok.txt text.txt
```

上例会输出以“l”开头的文件以及以“t”结尾的文件。

```text
liypoi@liypoi-virtual-machine:~$ echo /usr/*/share
/usr/local/share
```

上例会查看除主目录以外的目录。

echo \* 这类命令并不能显示出以“.”开头的隐藏文件。

**波浪线扩展**

```text
liypoi@liypoi-virtual-machine:~$ echo ~
/home/liypoi
```

"~"如果用在一个单词的开头，它将会被扩展为指定用户的主目录；如果没指定用户名，则扩展为当前用户的主目录。

**算术扩展**

可以把算术扩展当简易计算器。

```text
liypoi@liypoi-virtual-machine:~$ echo $((2+2-2))
2
```

算术扩展格式：`$((expression))`

算术扩展只支持整数运算，可以计算加、减、乘、除、取模、取幂（\*\*），表达式之间可以嵌套。

```text
liypoi@liypoi-virtual-machine:~$ echo 中国目前人口超过$((((2**3)+6)))亿
中国目前人口超过14亿
```

**花括号扩展**

花括号扩展可以创建多种文本字符串。

```text
liypoi@liypoi-virtual-machine:~$ echo 三年级{1,2,3,4}班
三年级1班 三年级2班 三年级3班 三年级4班
```

花括号中的内容可以是数字、字母、字符，并且支持嵌套。

```text
liypoi@liypoi-virtual-machine:~$ echo 我们{学校有{一,二,三},还有{1,2,3}}年级
我们学校有一年级 我们学校有二年级 我们学校有三年级 我们还有1年级 我们还有2年级 我们还有3年级
```

花括号可以应用在批量处理文件上，例如：创建2020年1月每一天的目录，以日期命名。

```text
liypoi@liypoi-virtual-machine:~/图片$ mkdir 2020-01-{1..31}
```

```text
liypoi@liypoi-virtual-machine:~/图片$ ls
2020-01-1   2020-01-15  2020-01-20  2020-01-26  2020-01-31  2020-01-9
2020-01-10  2020-01-16  2020-01-21  2020-01-27  2020-01-4
2020-01-11  2020-01-17  2020-01-22  2020-01-28  2020-01-5
2020-01-12  2020-01-18  2020-01-23  2020-01-29  2020-01-6
2020-01-13  2020-01-19  2020-01-24  2020-01-3   2020-01-7
2020-01-14  2020-01-2   2020-01-25  2020-01-30  2020-01-8
```

**命令替换**

命令替换可以把一个命令的输出作为一个扩展模式使用。

```text
liypoi@liypoi-virtual-machine:~/playground$ echo $(ls)
dir1 dir2 fun fun-hard ls-output.txt
```

ls 命令的运行结果作为echo 命令的参数。

#### 引用

shell 提供了一种称为引用的机制，用来有选择地避免不想要的扩展。

```text
liypoi@liypoi-virtual-machine:~/playground$ echo 衬衫的价格是$100元
衬衫的价格是00元
```

"$1"是一个未定义的变量，所以参数扩展把它替换成了空字符。

**双引号**

除“ $ ”（美元符号）、“ \ ”（反斜杠）、“ ’ ”（单引号）外，双引号中的其他字符都会失去它们原本的含义，被视为普通字符。如果有的文件命名为“file type.txt”，直接对该文件操作时会报错，“file”和“type”之间的空格不会被省略，此时要用双引号引用。

```text
liypoi@liypoi-virtual-machine:~/playground$ less file type.txt
file: 没有那个文件或目录
type.txt: 没有那个文件或目录
liypoi@liypoi-virtual-machine:~/playground$ less "file type.txt"
liypoi@liypoi-virtual-machine:~/playground$ 
```

这里说一下单词分割机制。默认情况下，单词分割会先查找是否有空格、制表符、换行符，然后把它们当作单词的界定符，没有被双引号包围的空格、制表符、换行符都不会被当成文本的一部分，但如果加上双引号，单词分割机制会失效，空格会被当作参数的一部分。

```text
liypoi@liypoi-virtual-machine:~/playground$ echo "$(cal)"
      二月 2020         
日 一 二 三 四 五 六  
                   1  
 2  3  4  5  6  7  8  
 9 10 11 12 13 14 15  
16 17 18 19 20 21 22  
23 24 25 26 27 28 29  
                      
liypoi@liypoi-virtual-machine:~/playground$ echo $(cal)
二月 2020 日 一 二 三 四 五 六 1 2 3 4 5  6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29
```

**单引号**

单引号可以抑制所有扩展。

```text
liypoi@liypoi-virtual-machine:~/playground$ echo $USER $(date)
liypoi 2020年 02月 06日 星期四 20:27:01 CST
liypoi@liypoi-virtual-machine:~/playground$ echo '$USER $(date)'
$USER $(date)
```

**转义字符**

如果文件中含特殊字符例如“$”、“!”空格等，可以用转义字符消除文件名中某个字符的特殊含义。

```text
liypoi@liypoi-virtual-machine:~/playground$ file file\ type.txt 
file type.txt: empty
```

使用转义字符转义“file type.txt”中的空格。

