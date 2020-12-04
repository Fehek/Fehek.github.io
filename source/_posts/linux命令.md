---
title: 基础Linux命令
abbrlink: 82b18e43
tags: Linux
date: 2020-07-26 15:38:00
categories: 实用
index_img: /image/index/sudo.jpg
banner_img: /image/index/sudo.jpg
---

# 显示、打开目录 
命令|含义
:--:|:--:
ls|显示当前目录内容
ls -a|显示当前目录所有内容（包括隐藏文件）
ls -l|除文件名外，将其它信息也一起罗列
ls -lh|以列表且文件大小易读(如KB,MB)显示信息
ls -la(ll)|以列表形式显示包括隐藏文件信息
ls -i|查看id
pwd|显示当前所在目录的绝对路径
cd|切换当前目录至
cd ~|切换至home目录
cd \.\.|返回上级目录
cd \.\./\.\.|返回上上级目录
cd -|上次打开的目录

# 新建、删除文件
命令|含义
:--:|:--:
touch|创建一个文本文件，或改变文件最后修改时间
mkdir|创建一个文件夹
mkdir -p|创建文件夹及其子文件夹
rm|删除一个文件
rm -r|删除一个文件夹

# 移动、复制文件
命令|含义
:--:|:--:
mv 文件名 文件名|重命名
mv 文件名 目录名|文件移动到目录
mv 目录名 目录名|目标目录存在，移动；不存在则重命名
mv 目录名 文件名|错误
cp|复制文件
cp -r [目录]|复制目录

# 通配符
命令|含义
:--:|:--:
*|通配符，代表0个或多个字符
？|通配符，代表1个字符
[abcd]|通配符，abcd中一个字符
[a-z]|a到z中的一个字符

# 搜索文件名
命令|含义
:--:|:--:
find [目录] -name ‘’|搜索名称符合的文件，iname会忽略大小写
find [目录] -size +/-|搜索大/小于的文件
find [目录] -user|搜索所属某人的文件
file [目录] -type f|搜索所有文件
file [目录] -type d|搜索所有文件夹
file [目录] -type l|搜索所有软链接
find [目录] -mmin +/-n|在n分钟之前/内被更改的文件
find [目录] -mtime +/-n|在n天之前/内被更改的文件
locate|用数据库搜索，速度快，但是更新不及时，可手动更新sudo updatedb

注|含义
:--:|:--:
atime|访问时间(access time)：文件最后被读取的时间，可以使用touch命令更改为当前时间;
ctime|变更时间(change time)，文件本身最后被变更的时间，变更动作可以使chmod、chgrp、mv等等；
mtime|修改时间（modify time），文件内容最后被修改的时间，修改动作可以使echo重定向、vi等等；

# 搜索文本内容
命令|含义
:--:|:--:
grep [搜索内容] [文件或目录]|在文件或目录中搜索文本内容
grep -n|显示该行内容与行号
grep -v|显示不包含匹配文本的所有行
grep ^|显示以开头的所有行
grep $|显示以结尾的所有行
grep -i|忽略大小写

# 写入文本内容
命令|含义
:--:|:--:
[输出结果] > [文件]|覆盖掉原来文件内容 
[输出结果] >> [文件]|追加到原来文件内容

# 显示文件内容
命令|含义
:--:|:--:
cat|文本文件全部显示
cat -n|对所有汉书编号
cat -b|除空行外，对行数编号
more|以一页一页的形式显示文本文件

# 拆分文件内容
命令|含义
:--:|:--:
split [-b <字节>] [要切割的文件] [输出文件名]|将原文件切割为X个字节一个的文件
split [-<行数>] [要切割的文件] [输出文件名]|将原文件每X行切割为一个文件
-C<字节>|与参数"-b"相似，但是在切 割时将尽量维持每行的完整性

# 创建链接
命令|含义
:--:|:--:
ln -s [源文件或目录] [链接文件或目录]|创建软链接
ln [源文件] [链接文件]|创建硬链接

\~|软链接|硬链接
:--:|:--:|:--:
inode|与原文件不同，说明是两个不同文件|与原文件相同，指向同一个区块
创建目录链接|可以|不行
跨文件系统建立|可以|不行
删除原文件后|不能打开|仍可打开

# 连接符
命令|含义
:--:|:--:
-a|条件同时满足
-o|或者，满足一个
[输出结果] \| [输入]|把某个命令的输出结果作为另一个命令的输入，输入常用more，用来显示很长的输出结果

# 快捷键
按键|含义
:--:|:--:
Tab|输入前几个字符可自动补全
↑|上一个输入的命令
↓|下一个输入的命令
Ctrl + A|鼠标移至最前面
Ctrl + E|鼠标移至最后面
Ctrl + R|搜索命令历史
Ctrl + L|清屏

# 用户及组
命令|含义
:--:|:--:
sudo useradd XXX|添加用户
cat /etc/passwd|查看用户
sudo passwd XXX|为用户添加密码
su XXX|切换用户
sudo su root|切换至root用户
exit|用户退出
sudo userdel XXX|删除用户
sudo userdel -r XXX|删除用户和该用户目录
groupadd XXX|添加组
cat /etc/group|查看组
groupmod -n [新名字] [旧名字]|给组改名
groupdel XXX|删除组
usermod -g xxgroup xxuser|修改初始组
usermod -G xxgroup,xxgroup xxuser|修改附加组
usermod -s [shell] xxuser|修改shell
id|查看用户id信息
who|当前所有登录用户显示

# shell
命令|含义
:--:|:--:
cat /etc/shells|查看所有shell
chsh|修改shell

# 命令
内置命令|外部命令
:--:|:--:
例which cd没有显示路径|which ls显示路径为/bin/ls
在系统启动时就调入内存，执行效率高|系统的软件功能，用户需要时才从硬盘中读入内存
大部分内置在shell中，也有一些有单独的内存|
系统启动时，会把shell中内置命令及不在shell中内置命令加载到内存|用户需要时才加载到内存

# 权限
```
   u   g   o
- --- --- ---

第一个字符表示文件类型：
a 二进制文件（包括不限于文本文件）；
d 目录（文件夹）；
| （软链接文件）

u(user)  所有者
g(group) 所属组
o(other) 其他用户

文件权限：
r 查看文件内容；
w 修改文件内容；
x 执行运行文件

文件夹权限：
r 列出目录中文件列表（仅限名字）；
w 在目录中创建、删除文件（包括修改文件名字）；
x 可以进入目录（不能查看目录内容）

可执行文件：
linux一般是用来启动某个应用程序或者服务程序的shell脚本（或类型的脚本）

```
```
修改权限：
chmod [ugoa][+-=][rwx] [file]
	a 表示ugo三者皆是
	+ 表示增加权限；- 表示取消权限；= 表示设定权限

chmod abc (-R) [file]
	r=4，w=2，x=1
	a、b、c各为一个数字,分别表示User、Group、Other的权限
	如rwx属性是4+2+1=7；rw-属性是4+2=6；r-x属性是4+1=5
	-R表示修改当前目录所有文件与子目录
```
命令|含义
:--:|:--:
sudo chown xxuser xxfile|修改文件所属者
sudo chgrp xxgroup xxfile|修改文件所属组

# vi/vim编辑器
可以分为三种模式，命令模式（Command mode），输入模式（Insert mode）和底线命令模式（Last line mode）。

**命令模式：**
启动vi/vim，就进入了命令模式

命令|含义
:--:|:--
i|切换到输入模式
:|切换到底线命令模式
x|删除当前光标所在处字符
dd|删除光标所在的那一整行
ndd|n为数字，删除光标所在的向下n行
d1G|删除光标所在到第一行的所有数据
dG|删除光标所在到最后一行的所有数据
yy|复制游标所在的那一行
nyy|n为数字，复制光标所在的向下n行
y1G|复制游标所在行到第一行的所有数据
yG|复制游标所在行到最后一行的所有数据
p/P|将已复制的数据粘贴在光标下/上一行

**编辑模式：**

命令|含义
:--:|:--
i|在命令模式下进入输入模式，从目前光标所在处输入
a|在命令模式下进入输入模式，从目前光标所在的下一个字符处开始输入
o|在命令模式下进入输入模式，在目前光标所在的下一行处输入新的一行
r|进入取代模式(Replace mode)，取代光标所在的那一个字符一次
R|进入取代模式(Replace mode)，一直取代光标所在的文字
ESC|退出输入模式，切换到命令模式
u|撤销上一次操作

**底线命令模式：**

命令|含义
:--:|:--
:|在命令模式下进入底线命令模式
set nu|标出行号
set nonu|取消行号
数字|跳到该数字表示的行数
/关键字|查找字符，按 n 往后寻找下一个关键字
w|保存文件
q|退出程序
q!|不保存退出

```
另外，Ubuntu中vi出现方向键显示ABCD，且后退键不能删除
解决方法：

1.修改vimrc.tiny文件
sudo su root                        //进入root权限
vi /etc/vim/vimrc.tiny              //修改文件
set compatible -> set nocompatible  //改为非兼容模式
set backspace=2                     //加入这句，解决backspace键问题

2.安装vim full版本
sudo apt-get remove vim-common      //卸载旧版的vi
sudo apt-get install vim            //安装full版的vim

其中，我运行过程中出现了
E: dpkg 被中断，您必须手工运行 sudo dpkg --configure -a 解决此问题
用以下几行命令解决
sudo rm /var/lib/dpkg/updates/* 
sudo apt-get update                 //更新软件列表
sudo apt-get upgrade                //更新软件
```