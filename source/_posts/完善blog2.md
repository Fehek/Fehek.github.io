---
title: 完善blog2
categories: blog相关
abbrlink: 659a9035
date: 2020-04-30 11:40:00
tags: 
- 链接格式
- 博客排序
- 上传博客源文件
index_img:
---
一些小改进。
<!-- more -->

# 修改永久链接
安装插件
```bash
$ npm install hexo-abbrlink --save 
```
打开根目录下的 \_config.yml文件，修改为
```yaml
# permalink: :year/:month/:day/:title/
permalink: posts/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

# 修改排序配置
top值越高，排序越在前，不设置top值得博文按照时间顺序排序。
打开node_modules/hexo-generator-index/lib/generator.js文件，添加
```js
posts.data = posts.data.sort(function(a, b) {
if(a.top && b.top) {                             // 两篇文章top都有定义
    if(a.top == b.top) return b.date - a.date;   // 若top值一样则按照文章日期降序排
    else return b.top - a.top;                   // 否则按照top值降序排
}
else if(a.top && !b.top) {                       // 只有一篇文章top有定义，那么将有top的排在前面
    return -1;
}
else if(!a.top && b.top) {
    return 1;
}
else return b.date - a.date;                    // 都没定义按照文章日期降序排
});
```
最终显示
```js
'use strict';

var pagination = require('hexo-pagination');

module.exports = function(locals) {
  var config = this.config;
  var posts = locals.posts.sort(config.index_generator.order_by);

  posts.data = posts.data.sort(function(a, b) {
      if(a.top && b.top) {
          if(a.top == b.top) return b.date - a.date;
          else return b.top - a.top;
      }
      else if(a.top && !b.top) {
          return -1;
      }
      else if(!a.top && b.top) {
          return 1;
      }
      else return b.date - a.date;
  });

  var paginationDir = config.pagination_dir || 'page';

  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```
更新：此主题fluid已自带置顶，加上sticky: Num，数字越大，排序在前。 

# 源文件备份
```
将source文件上传到仓库分支，便于不同电脑编辑
在github上新建一个hexo分支，选择此分支分支为默认分支
（这样每次同步的时候就不用指定分支）

将其克隆到本地，因为默认分支已经设成了hexo，所以只克隆了hexo分支
git clone git@github.com:Fehek/Fehek.github.io.git

在克隆到本地的Fehek.github.io中，把除了.git 文件夹外的所有文件都删掉

git add 
git commit –m "XXX"
git push
上传完毕
```
```
	更换电脑，安装git和nodejs后
	ssh-keygen -t rsa -C "youremail"    //设置ssh key，生成后填到github和coding上
	ssh -T git@github.com               //验证是否成功
	ssh -T git@git.coding.net           //没有coding账号可省略
	sudo npm install hexo-cli -g        //安装hexo  
	
	//在任意文件夹下克隆
	git clone git@………………                
	//在克隆文件夹下安装
	npm install                         
	npm install hexo-deployer-git --save  
	
	//生成及部署
	hexo g && hexo d 

	//源文件上传
	git add .
	git commit –m "xxxx"
	git push  

	//已经编辑过的电脑有clone文件夹了，和远端同步
	git pull
```
# 加入emoji:sunglasses:
下载插件 `npm install hexo-filter-github-emojis --save`

修改根目录下的_config.yml文件，启用插件
```yml
githubEmojis: 
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:
```
