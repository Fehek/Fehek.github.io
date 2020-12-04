---
title: 完善blog4
categories: 生活
abbrlink: 129da0a3
date: 2020-04-29 14:50:00
tags: APlayer播放器
index_img:
---
网站终于有了小锁，打开速度也还可以了，功能完善就暂时告一段落了。

开启了音乐插件，加入了魔圆的歌。音乐跳转页面后会再次重新播放且没有专辑页面，待解决。

部署完成后，不能进入绑定的域名，然后发现每次都自动解绑了域名。
解决方法：在source文件夹下新建一个CNAME文件。
且附带一个效果：进入usename.github.io时会直接跳转绑定域名。

显示的很慢，怀疑之前快的都是缓存，修改了一下offline config。还有在线页面根本没有音乐，解决中。

没有用主题自带的音乐插件，换成了Aplayer官方的。要利用pjax使页面跳转不中断播放，懒得搞了，就这样吧。

评论暂时不想开启，因为这只是我的一个发泄情绪的小站点，目的不是交流。有人在下面写评论我每次看自己的文章还都会看到，影响观感。真想评论可以去我的其他账号私信。~~话说这个网站真的有人能搜到吗~~

又双叒叕看不到音乐。


~~我，真是个笨蛋啊~~
诸多因素的巧合造成了我以为音乐在网站上看不到。
1.在谷歌上用localhost，正常
2.由于谷歌装了可以访问部分网址（谷歌商店，邮箱，可以用谷歌搜索引擎）的插件，导致域名完全打不开（为什么？）
3.edge打开太慢，用了VPN，看不到音乐
4.认为是无线网太慢，去用家里的台式机
5.用台式机的非主流浏览器还是打不开音乐
——>Q.E.D没有配置好

解决方案：
1.在台式机上安装了谷歌浏览器，打开，正常工作
2.回到笔记本，谷歌浏览器还是打不开
3.关掉VPN，让edge慢慢打开，等了~很久~一会儿，歌声传出
P.S.由于台式机是win7，没有edge
P.S.S.ie浏览器也显示不出歌曲，至少win7的老版ie不行（我电脑用ie打不开）

得出结论：下次买笔记本一定要带网络接口的
2020.5.27号更新：并不是，也可以买拓展坞。还有今天买了一个USB有线网卡。

```ejs
添加APlayer播放器：
在\theme\fluid\layout\layout.ejs里添加
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