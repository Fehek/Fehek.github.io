---
title: Hexo升级
categories: 实用
abbrlink: ff1c9b2c
tags:
  - Hexo
date: 2020-10-29 15:09:00
index_img:
---

1. 全局升级hexo-cli
`hexo version`查看当前版本，然后`npm i hexo-cli -g`，完成后再次`hexo version`查看是否升级成功。
2. 升级插件
`npm install -g npm-check`和`npm-check`，检查系统中安装的插件以及是否有更新。
`npm install -g npm-upgrade`和`npm-upgrade`，升级系统中的插件。
3. 更新hexo及所有插件
`npm update -g`和`npm update --save`