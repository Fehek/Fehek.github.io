---
title: 完善blog1
categories: blog相关
abbrlink: 129da0a3
date: 2020-04-29 14:50:00
tags: APlayer
index_img:
---
添加APlayer播放器
```ejs
<!-- 在\theme\fluid\layout\layout.ejs里添加 -->
<!-- 引用依赖 -->
<link rel="stylesheet" 
  href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/meting@1.2.0/dist/Meting.min.js"></script>

<!-- 我使用的APlayer本体 -->
<div class="aplayer" 
  data-id="445994750" 
  data-server="netease" 
  data-type="playlist" 
  data-fixed="true" 
  data-autoplay="false" 
  data-order="random" 
  data-volume="0.7" 
  data-theme="#cc543a" 
  data-preload="auto" >
  </div>
<!--如果将本体放在body里面导致页面加载出现问题，请尝试放到body体后面-->
```
