---
title: Linux 命令
excerpt: |
  Linux 命令
category: Linux
feature_image: "https://picsum.photos/2560/600?image=872"
---
#### Linux返回上一次目录

有时候千辛万苦进入了一个很深层的目录，一不小心输入了cd并回车，有什么办法快速回到刚才所在的目录呢？对于bash来说，只需要很管理的一个命令：

cd -

该命令等同于`cd $OLDPWD`，关于这一点在bash的手册页(可使用命令man bash访问其手册页)中有介绍：

```
An argument of - is equivalent to $OLDPWD. 
```

并且它还会返回上一次目录的物理路径。
#### 拷贝输出的目录

##### 第一种方式
利用grep先查找到文件，再利用pbcopy 将文件名保存到缓冲区(clipboard buffer)，

ls | grep -i vim | pbcopy

再利用pbpaste获得上一步的结果

vim `pbpaste`

注意：
1. grep -i 可以忽略大小写，case-insensetive
2. pbpaste的两边要加上“`” 符号，位于左上边1 键的左边

##### 第二种方式

find . -iname *vim* -exec vim {} +

注意：
1. -exec是find命令的一个参数，具体可以  man find

##### 第三种方式

vim $(ls | grep -i vim)

##### 第四种方式

ls | grep -i vim | xargs vim

注意：
第四种方式会产生一个问题，退出后命令不再显示，如下有解释


Kote Dekuur wrote: 

> Have anyone seen this error before? 
> 
> Vim: Warning: Output is not to a terminal 
> 
> I'm writing a bash script to do merges in Mercurial and using vimdiff as 
>  the merge tool.   When there are merge conflicts that requires manual 
> interactions it will open up vimdiff of the two files, but it seems to 
> hang on 
> 
> Vim: Warning: Output is not to a terminal   and the terminal is stuck. 
> 
> I have no problems executing vimdiff <file1> <file2> 
> manually.  This only happens when running it through a bash script.  I'm 
>  not sure if I'm missing an environment setting somewhere.

If you redirect Vim input it will read commands from there.  If the 
stdin actually contains text use the "-" argument, e.g.: 

        ls . | vim - 

If you redirect Vim output you get the message you mentioned.  You then 
can't see what you are doing, but you can still edit. 

Try typing ":qa!".  If only output was redirected it should get you out 
of the stuck situation. 

If both input and output are redirected, you are really stuck.
