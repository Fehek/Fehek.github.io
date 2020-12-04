---
title: 完善blog
categories: 生活
abbrlink: 2523834b
date: 2020-04-28 13:04:00
tags: 
- 加速Hexo博客
- InstantClick 
index_img:
---
改进网站纪录
<!-- more -->
今天研究了一下怎么加速blog，用来InstantClick（不知道有没有用），还用Hexo插件配置了一下缓存，~~虽然我觉得还是挺慢的~~

顺便加了一下RSS。

我就说为什么about页面加载的这么慢，头图竟然有14M，果断换了。


下载[InstantClick](http://instantclick.io/download)
将文件放入source文件夹，在\~/blog/themes/fluid/layout/layout.ejs 添加
```ejs
+ <script src="/instantclick.min.js" data-no-instant></script>
+ <script data-no-instant>InstantClick.init();</script>
</body>
</html>
```
在文件夹根目录安装
```bash
~/blog $ npm install
hexo-service-worker 
hexo-filter-optimize --save
```
在_config.yml中修改
```yaml
# offline config passed to sw-precache.
service_worker:
  maximumFileSizeToCacheInBytes: 5242880
  staticFileGlobs:
  - public/index.html
  - public/instantclick.min.js
  - public/about/index.html
  - public/archives/*
  - public/categories/*
  - public/tags/*
  - public/image/*
  - public/search.xml
  stripPrefix: public
  verbose: false
  runtimeCaching:
    - urlPattern: /**/*
      handler: cacheFirst
      options:
        origin: https://fehek.github.io/

filter_optimize:
  enable: true
  # remove static resource query string
  #   - like `?v=1.0.0`
  remove_query_string: true
  # remove the surrounding comments in each of the bundled files
  remove_comments: true
  css:
    enable: true
    # bundle loaded css file into the one
    bundle: true
    # use a script block to load css elements dynamically
    delivery: true
    # make specific css content inline into the html page
    #   - only support the full path
    #   - default is ['css/main.css']
    inlines:
    excludes:
  js:
    # bundle loaded js file into the one
    bundle: true
    excludes:
  # set the priority of this plugin,
  # lower means it will be executed first, default is 10
  priority: 12

1. staticFileGlobs 是首次加载时主动缓存的文件，请自行修改。
   hexo g 之后去 ~/blog/public/目录下查看生成的文件，需要主动缓存则加上。
2. origin 修改为你的博客域名。
3. 要使用 Service Worker 博客必须 HTTPS。
```
