---
title: Windows与Windows之间传输数据
categories: 实用
abbrlink: 5d108ff3
tags:
  - 数据传输
date: 2020-08-01 21:57:00
index_img: /image/index/transfer.jpg
---
**设置共享文件**
+ 设置 → 网络和Internet → 网络和共享中心 → 更改高级共享选项 → 来宾或公用 → 启用网络发现、启用文件和打印机共享 → 所有网络 → 启用共享 → 保存更改
+ 需要共享的文件或盘符 → 右键“属性” → 安全 → 编辑 → 添加 → 输入“everyone” → 确定 → 点击“Everyone” → 下方“Everyone的权限”根据需要修改
+ 属性里的“共享”选项卡 → 高级共享 → 勾选“共享此文件” → 权限 → 根据需要选择

**访问共享文件**
+ 在文件资源管理器地址栏输入 `\\计算机IP`