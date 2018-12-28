---
layout: post
title:  "cat、more、less、tail、head"
categories: linux
tags: command
author: cjie
---
* content
{:toc}

![linux](https://pic.bianla.cn/presentation/image/20181228/18ba7fd5cc764ba0be8f26329805f9cd.jpg =720x405)
## 前言
linux系统中查看文档不像windows中有图形界面，需要用命令语句来操作，cat、more、less、tail、head都是查看文档的语句，下文来介绍各个语句的异同。





### 一：cat 显示文件连接文件内容的工具 
cat 是一个文本文件（查看）和（连接）工具，通常与more搭配使用，与more不同的是cat可以合并文件。查看一个文件的内容，用cat比较简单，就是cat后面直接接文件名。 
如:
```markdown
[bianla_lee@iZbp18ibt7pw5chl5sgzk5Z logs]$ cat catalina.out
```
#### 1：cat 语法结构：
    cat \[选项] \[文件]... 
      
    选项 
      -A, --show-all           等价于 -vET 
      -b, --number-nonblank    对非空输出行编号 
      -e                       等价于 -vE 
      -E, --show-ends          在每行结束处显示 $ 
      -n, --number             对输出的所有行编号 
      -s, --squeeze-blank      不输出多行空行 
      -t                       与 -vT 等价 
      -T, --show-tabs          将跳格字符显示为 ^I 
      -u                       (被忽略) 
      -v, --show-nonprinting   使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外 
          --help     显示此帮助信息并离开 
      
#### 2:cat 查看文件内容实例： 
 ```markdown
    [root@localhost ~]# cat /etc/profile    注：查看/etc/目录下的profile文件内容； 
    [root@localhost ~]# cat -b /etc/fstab   注：查看/etc/目录下的profile内容，并且对非空白行进行编号，行号从1开始； 
    [root@localhost ~]# cat -n /etc/profile    注：对/etc目录中的profile的所有的行(包括空白行）进行编号输出显示； 
    [root@localhost ~]# cat -E /etc/profile     注：查看/etc/下的profile内容，并且在每行的结尾处附加$符号； 
      
    cat 加参数-n 和nl工具差不多，文件内容输出的同时，都会在每行前面加上行号； 
    [root@localhost ~]# cat -n /etc/profile 
    [root@localhost ~]# nl /etc/profile 
    
    cat 可以同时显示多个文件的内容，比如我们可以在一个cat命令上同时显示两个文件的内容； 
    [root@localhost ~]# cat /etc/fstab /etc/profile 
      
    cat 对于内容极大的文件来说，可以通过管道|传送到more 工具，然后一页一页的查看； 
    [root@localhost ~]# cat /etc/fstab /etc/profile | more 
```
#### 3:cat 的创建、连接文件功能实例： 
 cat 有创建文件的功能，创建文件后，要以EOF或STOP结束: 
 ```markdown
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat > test.txt <<EOF
    > heheh
    > lalal
    > xixixi
    > zzzz
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test.txt 
    heheh
    lalal
    xixixi
    zzzz
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ rm test.txt 
```
cat 还有向已存在的文件追加内容的功能:
```markdown
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat > test.txt <<EOF
    > hehehlalalaxixixi
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat >>test.txt << EOF 
    > zezeze
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test.txt 
    hehehlalalaxixixi
    zezeze
```
cat 连接多个文件的内容并且输出到一个新文件中:
```markdown
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat >test1.txt <<EOF
    > 111111111
    > 11111111
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat > test2.txt <<EOF
    > 22222222
    > 2222222
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat> test3.txt <<EOF
    > 3333333
    > 333333
    > EOF
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test1.txt test2.txt test3.txt >test4.txt
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test4.txt 
    111111111
    11111111
    22222222
    2222222
    3333333
    333333
```
cat 把一个或多个已存在的文件内容，追加到一个已存在的文件中:
```markdown
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test1.txt test2.txt test3.txt >>test4.txt 
    [bianla_lee@iZbp18ibt7pw5chl5sgzk5Z log]$ cat test4.txt 
    111111111
    11111111
    22222222
    2222222
    3333333
    333333
    111111111
    11111111
    22222222
    2222222
    3333333
    333333
```

**警告：我们要知道 > 意思是创建，>> 是追加。千万不要弄混了。造成失误可不是闹着玩的；**  



### 二：more 文件内容或输出查看工具
#### 1.more的用法
      more \[参数选项] \[文件] 
      
      参数如下： 
      +num   从第num行开始显示； 
      -num   定义屏幕大小，为num行； 
      +/pattern   从pattern 前两行开始显示； 
      -c   从顶部清屏然后显示； 
      -d   提示Press space to continue, 'q' to quit.（按空格键继续，按q键退出），禁用响铃功能； 
      -l    忽略Ctrl+l （换页）字符； 
      -p    通过清除窗口而不是滚屏来对文件进行换页。和-c参数有点相似； 
      -s    把连续的多个空行显示为一行； 
      -u    把文件内容中的下划线去掉退出more的动作指令是q 
      

#### 2.more 的参数应用举例：
  ```markdown
    [root@localhost ~]# more -dc /etc/profile    注：显示提示，并从终端或控制台顶部显示； 
    [root@localhost ~]# more +4 /etc/profile      注：从profile的第4行开始显示； 
    [root@localhost ~]# more -4 /etc/profile      注：每屏显示4行； 
    [root@localhost ~]# more +/MAIL /etc/profile     注：从profile中的第一个MAIL单词的前两行开始显示； 
```


#### 3.more 的动作指令： 
      我们查看一个内容较大的文件时，要用到more的动作指令，比如ctrl+f（或空格键） 是向下显示一屏，ctrl+b是返回上一屏； Enter键可以向下滚动显示n行，要通过定，默认为1行； 
      
      我们只说几个常用的； 自己尝试一下就知道了； 
      
      Enter       向下n行，需要定义，默认为1行； 
      Ctrl+f    向下滚动一屏； 
      空格键          向下滚动一屏； 
      Ctrl+b  返回上一屏； 
      =         输出当前行的行号； 
      :f      输出文件名和当前行的行号； 
      v      调用vi编辑器； 
      ! 命令            调用Shell，并执行命令； 
      q     退出more当我们查看某一文件时，想调用vi来编辑它，不要忘记了v动作指令，这是比较方便的； 
      
      
#### 4.其它命令通过管道和more结合的运用例子：
      比如我们列一个目录下的文件，由于内容太多，我们应该学会用more来分页显示。这得和管道 | 结合起来，比如： 
      [root@localhost ~]# ls -l /etc |more 
      
      
      
### 三：less 查看文件内容 工具 
less 工具也是对文件或其它输出进行分页显示的工具，应该说是linux正统查看文件内容的工具，功能极其强大；您是初学者，我建议您用less。由于less的内容太多，我们把最常用的介绍一下； 
#### 1.less的语法格式： 
      less [参数] 文件 
        
      常用参数 
      
      -c 从顶部（从上到下）刷新屏幕，并显示文件内容。而不是通过底部滚动完成刷新； 
      -f 强制打开文件，二进制文件显示时，不提示警告； 
      -i 搜索时忽略大小写；除非搜索串中包含大写字母； 
      -I 搜索时忽略大小写，除非搜索串中包含小写字母； 
      -m 显示读取文件的百分比； 
      -M 显法读取文件的百分比、行号及总行数； 
      -N 在每行前输出行号； 
      -p pattern 搜索pattern；比如在/etc/profile搜索单词MAIL，就用 less -p MAIL /etc/profile 
      -s 把连续多个空白行作为一个空白行显示； 
      -Q 在终端下不响铃； 
        
      比如：我们在显示/etc/profile的内容时，让其显示行号； 
      [root@localhost ~]# less -N    /etc/profile 
      
      
#### 2.less的动作命令： 
        进入less后，我们得学几个动作，这样更方便 我们查阅文件内容；最应该记住的命令就是q，这个能让less终止查看文件退出； 
        
        动作： 
        
        回车键 向下移动一行； 
        y 向上移动一行； 
        空格键 向下滚动一屏； 
        b 向上滚动一屏； 
        d 向下滚动半屏； 
        h less的帮助； 
        u 向上洋动半屏； 
        w 可以指定显示哪行开始显示，是从指定数字的下一行显示；比如指定的是6，那就从第7行显示； 
        g 跳到第一行； 
        G 跳到最后一行； 
        p n% 跳到n%，比如 10%，也就是说比整个文件内容的10%处开始显示； 
        /pattern 搜索pattern ，比如 /MAIL表示在文件中搜索MAIL单词； 
        v 调用vi编辑器； 
        q 退出less 
        !command 调用SHELL，可以运行命令；比如!ls 显示当前列当前目录下的所有文件； 
        
        
        
### 四.head 工具，显示文件内容的前几行 
    head 是显示一个文件的内容的前多少行； 
    
    用法比较简单； 
    head -n 行数值 文件名； 
    
    比如我们显示/etc/profile的前10行内容，应该是： 
    [root@localhost ~]# head -n 10 /etc/profile 
  
  
  
  
### 五.tail 工具，显示文件内容的最后几行 
      tail 是显示一个文件的内容的最后多少行； 
      
      用法比较简单； 
      tail   -n 行数值 文件名； 
      
      比如我们显示/etc/profile的最后5行内容，应该是： 
      [root@localhost ~]# tail -n 5 /etc/profile 
      
      tail -f /var/log/syslog 显示文件 syslog 的后十行内容并在文件内容增加后，且自动显示新增的文件内容。 
  **备注：最后一条命令tail非常有用，尤其在监控日志文件时，可以在屏幕上一直显示新增的日志信息**
