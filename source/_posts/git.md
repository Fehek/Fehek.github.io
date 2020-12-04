---
title: git、ssh等
abbrlink: c34887c6
tags:
  - Git
  - SSH
  - 系统变量
categories: 实用
date: 2020-09-24 15:50:00
index_img:
banner_img:
---

# 设置Path系统变量
控制面板 - 系统与安全 - 系统 - 高级系统设置 - 环境变量 - 系统变量Path - 编辑 - 添加需要的路径

# Git指令
指令|含义
:-:|:-:
git init|初始化一个仓库
git log|工作日志
git status|文件状态
git diff|工作区与暂存区的差异
git diff --cached  [<path>...]|比较暂存区与最新本地版本库（本地库中最近一次commit的内容）
git add .|将此文件夹下所有文件添加至暂缓区
git commit -m "XXX"|把暂存区的所有修改提交到分支，须输入描述信息
git commit -a -m "XXX"|add + commit功能（从未add过的文件不能使用）
git commot --amend|更改之前一次commit的描述信息
git remote add 名称 仓库地址|添加其它仓库
git remote --verbose|显示所有仓库
git push -u origin master|推送至默认仓库
git push gitee master|推送至其它仓库
git restore|将在工作空间但是不在暂存区的文件撤销更改
git restore --staged|将文件从暂存区撤出，但不会撤销文件的更改
git config --global user.name "username"|设置全局用户名
git config --global user.email you@example.com|设置全局邮箱
git config --global core.editor XXX|配置默认编辑器（须先加入系统变量）
git config --global alias st status|取别名，例如将status改成st

# SSH
## 创建一个SSH key
`ssh-keygen -t rsa -C "your_email@example.com"`
-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。
以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key 代码。
若不输入文件名，则会使用默认文件名，会生成 id_rsa 和 id_rsa.pub 两个秘钥文件。
接下来会提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码），
若不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上。

`ssh -T git@github.com`
检测是否已经绑定成功及当前账户。

## 添加一个新的SSH key
`ssh-keygen -t rsa -C "your_email@example.com" -f ~/.ssh/id_rsa_2`
新建一个SSH密钥，名字不要取默认，不然会把之前的覆盖掉。

`eval $(ssh-agent)` 或者 ```eval `ssh-agent` ```
开启ssh-agent，用来管理密钥。若成功，会输出`Agent pid xxxx`

`ssh-add ~/.ssh/id_rsa_2`
将新的SSH私钥添加至ssh-agent。


