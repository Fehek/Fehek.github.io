---
title: Markdown语法
categories: 实用
abbrlink: 87f960c2
tags: 
- Markdown syntax
- Emoji
date: 2020-04-30 17:25:00
index_img: /image/index/markdown.png
---
Markdown语法及emoji输出
<!-- more -->

# Markdown语法
## 标题
```md
一级标题：#
二级标题：##
以此类推
```

## 字体
```md
加删除线：~~text~~
PS. text两边加中括号删除线不起作用，也可以用<del>text</del>
字体倾斜：*text*
字体加粗：**text**
斜体加粗：***text***
```

## 分割线
三个以上\-或\*
例如
```md
---
----
***
****
```

---
----
***
****

## 引用　
```md
引用内容：>　　　
引用引用的内容：>>　
以此类推
```

## 代码使用
+ 行内代码用两个反引号 \`kkkk\`
+ 多行代码用两个三反引号，反引号要单独一行。
\`\`\`C
int main()
{
&nbsp;&nbsp;&nbsp;&nbsp;printf("Hello, World!");
&nbsp;&nbsp;&nbsp;&nbsp;return 0;
}
\`\`\`

效果：
行内代码`kkkk`
多行代码
```c
int main()
{
   printf("Hello, World!");
   return 0;
}
```

## 空格与换行
缩进一个空格（半角空格）：添加`&ensp;`
缩进两个空格（全角空格）：添加`&emsp;`或者使用全角空格
P.S.一个汉字占两个空格

换行：`<br />`


## 列表
有序列表：`数字 + 点`
无序列表：`- / + / * + 文字`
嵌套只需要在符号前加 `Tab`

## 表格
```markdown
表头|表头|表头
-|:-:|-:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。
-可以有一个或多个
文字默认居左
两边加：文字居中
右边加：文字居右
```
例如
```markdown
姓名|分数|排行
:-:|-|-:
张三|56|2
刘四|45|3
李五|70|1
```
姓名|分数|排行
:-:|-|-:
张三|56|2
刘四|45|3
李五|70|1

## 插入图片
```markdown
![图片alt](图片地址 "图片title")

图片alt是描述图片的关键词，可不写。当图片因为某种原因不能被显示时而出现的替代文字。
图片title是图片的标题，可不写。当鼠标移到图片上时显示的内容。
```
例如：
```markdown
![百度搜索](https://www.baidu.com/img/bd_logo1.png "百度")
```
![百度搜索](https://www.baidu.com/img/bd_logo1.png "百度")

## 插入链接
```markdown
[超链接名](超链接地址 "超链接title")，title可加可不加
```
例如：
```markdown
[blog](https://fehek.xyz "Fehek的博客")
```
[blog](https://fehek.xyz "Fehek的博客")

# 输出emoji
```markdown
:laughing: :smirk: :exclamation: :question: :octocat:
```
:laughing: :smirk: :exclamation: :question: :octocat:
更多：https://www.webfx.com/tools/emoji-cheat-sheet/
https://www.einsition.com/tools/emojicheatsheet
