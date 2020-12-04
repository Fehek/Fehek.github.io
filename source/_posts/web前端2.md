---
title: web前端学习笔记2
categories: web前端学习笔记
abbrlink: d8ef0c96
tags:
  - web前端
  - CSS
date: 2020-06-14 22:25:00
index_img:
---

# 推荐网站
[MDN](https://developer.mozilla.org/zh-CN/)：Web文档
[W3school](https://www.w3school.com.cn/)：与MDN类似
[Can I use](https://caniuse.com/)：查询CSS在浏览器中的支持程度

# 基本
- 注释
`/*  */`
注意，CSS中没有`//`注释
- HTML5新增语义化标签
article，section，aside，header，footer，nav，main，template
- \<link>
`<link rel="stylesheet" href="style.css" media="print">`引入一个外部样式表。
media 属性用于为不同的媒介类型规定不同的样式，所有浏览器都支持"screen"、"print"、"all" 的 media 属性。

# 容器标签
标签|作用
:-:|:-:
span|一个容器标签，用于包裹一段文本，便于给文本增加样式
div|一个通用容器标签，可以包裹任何内容，也可以容器直接互相包裹。
text-align:center|让内部元素水平居中
margin:auto|让容器本身水平居中
background-color:black|设定背景颜色
font-size:30px|设定字体大小
color:white|设定字体颜色

```html
	<div style="color:lightslategray; margin:auto; width:500px;">
			<p style="text-align:center;">
				<span style="background-color:gray; color:white; font-size:24px">雪国</span>
			</p>
			<p>
				<b>穿过县界长长的隧道，便是雪国</b>。<span style="color:#9F9F9F";>夜空下一片白茫茫。火车在信号所前停了下来。</span>
			</p>
			<p>
				一位姑娘从对面座位上站起身子，把岛村座位前的玻璃窗打开。<b>一股冷空气卷袭进来。</b>姑娘将身子探出窗外，仿佛向远方呼唤似地喊道：“站长先生，站长先生！”
			</p>
			<p>
				一个把围巾缠到鼻子上、帽耳聋拉在耳朵边的男子，<span style="color:rosybrown";>手拎提灯，踏着雪缓步走了过来。</span>
			</p>
	</div>
```
显示效果：
<div style="color:lightslategray; margin:auto; width:500px;">
			<p style="text-align:center;">
				<span style="background-color:gray; color:white; font-size:24px">雪国</span>
			</p>
			<p>
				<b>穿过县界长长的隧道，便是雪国</b>。<span style="color:#9F9F9F";>夜空下一片白茫茫。火车在信号所前停了下来。</span>
			</p>
			<p>
				一位姑娘从对面座位上站起身子，把岛村座位前的玻璃窗打开。<b>一股冷空气卷袭进来。</b>姑娘将身子探出窗外，仿佛向远方呼唤似地喊道：“站长先生，站长先生！”
			</p>
			<p>
				一个把围巾缠到鼻子上、帽耳聋拉在耳朵边的男子，<span style="color:rosybrown";>手拎提灯，踏着雪缓步走了过来。</span>
			</p>
		</div>

# 选择器
## 选择器类别
一个空的div，默认宽度100%，高度为0
标签内部的叫行内样式，style内部的叫内部样式
- ID选择器（#box）
在页面中的id不允许重复，因此id选择器只能选择单个元素
- 类别选择器（.nav）
选择该class的多个元素
- 标签选择器（div）
选择对应的所有标签
- 通用选择器（\*）
针对页面上所有的标签生效
- 属性选择器

写法|含义
:-|:-
[attr]|存在某种属性
[attr=value]|属性值为 value 的
[attr~=value]|属性至少包含一个 value(必须是一个单词)
[attr\|=value]|属性以 value 或者 value- 开头，一般用于语言
[attr^=value]|属性以 value 开头
[attr$=value]|属性以 value 结尾
[attr\*=value]|属性至少包含一个 value
[attr operator value i]|添加一个用空格隔开的字母 i（或 I），匹配属性值时忽略大小写
[attr operator value s]|添加一个用空格隔开的字母 s（或 S），匹配属性值时区分大小写

- 伪类选择器

位置伪类|例子|含义
:-|:-|:-
:first-child|p:first-child|选择任意元素的第一个p子元素
:last-child|p:last-child|选择任意元素的最后一个p子元素
:only-child|p:only-child|选择所有仅有一个p子元素
:nth-child(n)|p:nth-child(n+2):nth-child(-n+5)|选择第2个到第5个p子元素；n为odd时，是第奇数个元素，n为even时，是第偶数个元素
:nth-last-child(n)|p:nth-last-child(2)|选择所有倒数第二个p子元素
:first-of-type|p:first-of-type|选择的每个p元素是其父元素的第一个p元素
:last-of-type|p:last-of-type|选择每个p元素是其母元素的最后一个p元素
:nth-of-type(n)|p:nth-of-type(2)|选择所有p元素第二个为p的子元素
:nth-last-of-type(n)|p:nth-last-of-type(2)|选择所有p元素倒数的第二个为p的子元素
:not(selector)|:not(a)|选择所有a以外的元素
:empty|没有子元素的元素，子元素只可以是元素节点或文本（包括空格）

链接/交互伪类|意义
:-|:-
:link|未被激活的链接
:visited|访问者已访问过的链接
:focus|获得焦点(Tab键)的链接（处于活动状态也会获得焦点）
:hover|光标悬浮在链接上时
:active|光标按下时
由于链接可能同时处于多种状态，按照link、visited、focus、hover、active（缩写为LVFHA）或者LVHFA来定义。

其它伪类|意义
:-|:-
::before|通常用作装饰，没有交互的元素。通过content:""来为一个元素添加修饰性的内容。此元素默认为行内元素。
::after|和::before基本相同
::selection|应用于文档中被用户高亮的部分
[:target](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target)|选择一个ID与当前URL片段匹配的元素

- 组合器

写法|含义
:-|:-
元素1`␣`元素2|后代选择器。择由元素1作为祖先元素的所有元素2（不要求是父子关系）
元素1 > 元素2|子选择器。选择作为元素1的直接后代(子元素)的元素2
元素1 \~ 元素2|兄弟选择器。选择元素1之后所有同层级元素2。
元素1 + 元素2|相邻兄弟选择器。当两个元素都是属于同一个父元素的子元素，且元素2紧跟在元素1之后，则选择元素2。

## 选择器权重
名称|写法|权重值
:--:|:--:|:--:
通用选择器|\*{...}|0       
标签选择器|div{...}|1
类选择器|.nav{...}|10
ID选择器|#box{...}|100
行内样式|<...style="...">|1000
重要|!important|权重最大
注：权重值仅作参考，并不表示实际值。
如果两个元素的权重完全相同，在样式表中后出现的一个会表示出来。
继承来的样式没有优先级。
选择器选择的范围越小越精确，优先级就越高。
display属性可以改变元素的显示角色：inline，block

## 例子
```html
<!DOCTYPE html>
<html style="background-color: antiquewhite;">
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body style="margin:0">
		<div id="banner">
			<img src="/image/1.jpg" style="width:100%; height:500px;">
		</div>
		<div id="navigation" style="height:80px; line-height:80px; text-align:center;">
			<a href="#" style="margin:0 15px;">首页</a>
			<a href="#" style="margin:0 15px;">归档</a>
			<a href="#" style="margin:0 15px;">分类</a>
			<a href="#" style="margin:0 15px;">标签</a>
			<a href="#" style="margin:0 15px;">关于</a>
			<a href="#" style="margin:0 15px;">友链</a>
		</div>
		<div id="bottom" style="height:40px; line-height:40px; text-align:center; background-color:#ddd; color:gray; font-size: 13px;">
			版权所有：FEHEK的学习&nbsp;技术支持懒懒散散实验室
		</div>
	</body>
</html>
```
改成用选择器：
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			html{
				background-color: antiquewhite;
			}
			body{
				margin:0;
			}
			#navigation{
				height:80px; line-height:80px; text-align:center;
			}
			#bottom{
				height:40px; line-height:40px; text-align:center; background-color:#ddd; color:gray; font-size: 13px;
			}
			.nav{
				style="margin:0 15px;
			}
			#banner img{
				width:100%; height:500px;
			}
		</style>
	</head>
	<body>
		<div id="banner">
			<img src="/image/1.jpg">
		</div>
		<div id="navigation">
			<a href="#" class="nav">首页</a>
			<a href="#" class="nav">归档</a>
			<a href="#" class="nav">分类</a>
			<a href="#" class="nav">标签</a>
			<a href="#" class="nav">关于</a>
			<a href="#" class="nav">友链</a>
		</div>
		<div id="bottom">
			版权所有：FEHEK的学习&nbsp;技术支持懒懒散散实验室
		</div>
	</body>
</html>

```
效果：<img src="/image/post/web前端2.jpg">

# 盒模型
content-box:
<img src="/image/post/content_box.jpg">
border-box:
<img src="/image/post/border_box.jpg">

# CSS属性
## 长度
绝对长度单位|意义
:--:|:--
in|inch，英寸(=2.54cm)
cm|厘米
mm|毫米
pt|点，1/72 inch
pc|12点活字（12pt），1/6 inch
大部分时候不准，取决于分辨率及系统设置，用的较少。
打印时比较准。

相对长度单位|意义
:--:|:--
px|像素
em|继承父元素的 font-size
rem|继承根元素（html）的 font-size。大多数浏览器中，默认值为16px。
ex|"x"字符的高度，有些浏览器会把它计算成0.5em
ch|"0"字符的宽度
vw|viewport width，1vw为视口宽度的1/100（包含滚动条）
vh|viewport height，1vw为视口高度的1/100（包含滚动条）
vmax|max(vw,vh)，视口宽或者高较大的那一个的1/100
vmin|min(vw,vh)，视口宽或者高较小的那一个的1/100

## 文本
功能|写法
:--|:--
文字颜色|color: black;
字体类型|font-family: "微软雅黑"
字体大小|font-size: 30px;
文字加粗|font-weight: bold;
文字倾斜|font-style: italic;
首行缩进|text-indent: 60px/2em;
水平对齐方式|text-align(-last): center/justify;
垂直对齐方式|vertical-align:bottom/baseline/middle（只在行内元素或表格单元格中生效）
行高|line-height: 100px;
宽度|[width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width):fit-content
文字方向|direction:ltr/rtl;
控制每个单词之间的间隔|word-spacing:1px;（单词间的间隔宽度是本身的空格加这个值，对中文无效）
控制字母之间的间隔|letter-spacing:1px;
背景颜色与高度|height: 100px; background-color: gray;
文本修饰|text-decoration: underline;
文字阴影|text-shadow:水平偏移 垂直偏移 模糊半径 颜色,下一组;
盒阴影|box-shadow:水平偏移 垂直偏移 模糊半径 扩散半径 颜色,下一组;（两个半径都可以不写）
元素中的空白|[white-space](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)
单词内断行|[word-break](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)

字体|功能
:--|:--
font-style: italic;|斜体
font-weight: bold;|加粗
font-family: arial,sans-serif;|字体种类
font-size: 20px;|字体大小
line-height: 30px;|行高
font: italic bold 20px/35px arial,sans-serif,"微软雅黑";|斜体，加粗，字号大小/行高，默认字体，备用字体，备用字体

## 边框
边框|功能
:--|:--
border-width|边框宽度
border-style|边框样式
border-color|边框颜色
border: 1px solid #009A4F;|宽度，样式，颜色
border: 2px dashed;|颜色默认为黑色，其它两属性不可省略
box-sizing: border-box|默认值是"cotent-box"。border-box表示设置的width包含border和padding的值。
[border-radius](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)|圆角
[outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline)|轮廓属性

边距|功能
:--|:--
margin-top|上边距
margin-right|右边距
margin-bottom|下边距
margin-left|左边距
margin: 10px 15px 10px 15px;|上，右，下，左边距
margin: 10px 15px 15px;|上，左右，下
margin: 10px 15px;|上下，左右
margin: 10px;|上下左右

填充|功能
:--|:--
padding-top|上填充
padding-right|右填充
padding-bottom|下填充
padding-left|左填充
padding: 10px 15px 10px 15px;|上，右，下，左填充
padding: 10px 15px 15px;|上，左右，下
padding: 10px 15px;|上下，左右
padding: 10px;|上下左右

## 背景
背景|功能
:--|:--
background-color|背景色
background-image|背景图片url()
[background-repeat](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)|背景平铺方式
[background-size](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)|cover图片由无穷大缩小到正好覆盖元素；contain图片由无穷小放大到正好被元素包围
[background-position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)|图片初始位置
[background-attachment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)|fixed图片固定；scroll背景不会随着元素的内容滚动；local背景会随着元素的内容滚动
[object-fit](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)|背景适应到其使用的高度和宽度确定的框
[background-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-origin)|背景的原点位置。border-box，padding-box，content-box
[background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)|只显示里面的背景。border-box，padding-box，content-box，text
[background](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background)|背景
[background-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode)|背景颜色混合
[isolation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/isolation)|背景混合时独立
[clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)|使用裁剪方式创建元素的可显示区域

## 颜色 
颜色|功能
:--|:--
color: white;|
color: rgba(255,151,0,0.1);|
color: #FF9700|rgb转化成16进制
[linear-gradient()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient)|线性渐变
[radial-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient)|辐射渐变
[conic-gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/conic-gradient)|圆锥渐变
[mix-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mix-blend-mode)|颜色混合
[filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)|滤镜

## 缓动
属性|意义
:-|:-
[transition-property](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-property)|过渡属性
[transition-duration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-duration)|过渡时间
[transition-timng-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-timing-function)|[缓动](https://developers.google.com/web/fundamentals/design-and-ux/animations/the-basics-of-easing?hl=zh-cn)：linear线性变化，steps [number]跳跃变化，ease-out缓出，ease-in缓进，ease-in-out缓进缓出。
[transition-delay](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-delay)|延迟时间
[transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)|`<transition-property> | <transition-duration> | <transition-timng-function> | <transition-delay> `

## 动画
属性|意义
:-|:-
[animation-name](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name)|动画名称
[animation-duration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-duration)|一个动画周期的时长
[animation-timing-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-timing-function)|在每一动画周期中执行的节奏
[animation-delay](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-delay)|动画延迟
[animation-iteration-count](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-iteration-count)|播放次数
[animation-direction](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-direction)|播放方向
[animation-fill-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-fill-mode)|动画在执行之前和之后如何将样式应用于其目标
[animation-play-state](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-play-state)|动画是否运行或者暂停
[animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)|`@keyframes name | duration | timing-function | delay | iteration-count | direction | fill-mode | play-state`

## 变换
属性|意义
:-|:-
[transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)|旋转，缩放，倾斜或平移给定元素
[transform-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function)|对元素的显示做变换
[transform-origin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)|元素变形的原点
[rotate()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate)|旋转
[translate()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translate)|(translateX,translateY)平移，是自己的宽高百分比，而不是包含块的百分比
[scale()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/scale)|缩放
[skew()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/skew)|倾斜
[matrix()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/matrix)|由指定的 6 个值组成的 2D 变换矩阵
[perspective](https://developer.mozilla.org/zh-CN/docs/Web/CSS/perspective)|景深
[translate3d()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translate3d)|3d变换
[transform-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-style)|preserve-3d/flat
hover相关：
hover之前的transform属性不会和hover时的transform同时起效。
若hover之前的transform和之后的属性和顺序相同，会做指定变换。若不相同，会沿着最短距离直接到达hover状态。

# 布局
## 水平布局
块元素的水平布局：
元素的宽度不受内容的影响，仅受以下规则的影响。
1. 水平方向的七个属性（margin-left、border-left、padding-left、width、padding-right、border-right、margin-right）之和要等于包含块的content-box宽度
2. width、margin-left、margin-right可以取值为auto。其余属性必须设置为特定值，或者默认宽度为0。若这3个属性都设置为非auto的某个值，这时格式化属性过分受限（overconstrained），总会把margin-right强制为auto。
3. margin可能为负。margin-left的计算结果不能为负，但可以声明为负值。
4. 关于auto
 - 0个auto：margin-right 被重置为 auto
 - 1个auto：计算出它
 - 2个auto：
	- 2个都在 margin 上，则两边 margin 相同
	- 其中1个在 width 上，则 width 占据尽量多的空间，另一个 auto 为0。
 - 3个auto：width 占据尽量多的空间，margin-l/r 都为0
5. 替换元素
- 替换元素用`style="dispalay:block;`可将其变成块级元素。
- 若width为auto，元素的宽度是内容的固有宽度。
- 若设置了width，height保持auto，则height也会成比例变化。
6. 应用
```css
#main {
  width: 600px;
  margin: 0 auto; 
}
```
设置块级元素的 width 可以防止它从左到右撑满整个容器。设置左右外边距为 auto 来使其水平居中。
唯一的问题是，当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面，文本不会自动换行。
这时使用 max-width 替代 width 可以使浏览器更好地处理小窗口的情况。

## 垂直格式化
1. 若指定高度大于显示内容所需高度：`<p style="height: 10em;>`，多余的高度会好像有额外的内边距一样。
若指定高度小于显示内容所需高度：`<p style="height: 3em;>`，浏览器会提供某种方法来查看所有内容，而不是增加元素框的边框。也可用[overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow)指定查看方式。
2. 有7个相关的属性：margin-top、border-top、padding-top、height、padding-bottom、border-bottom、margin-bottom。
3. 其中，height、margin-top、border-bottom可设置成auto。在正常流中一个块元素的margin-top或margin-bottom设置成auto，会自动计算成0。
4. 合并垂直外边距，两个外边距中较小的一个会被较大的一个合并。（只应用与外边距，如果元素有内边距和边框，它们不会被合并）
5. 正负外边距合并。若两个margin都是负数，取较小的数；两个都是正数，取较大的数；一个正数一个负数，相加。

## 行内布局
1. **非替换元素的padding、border、margin对行内元素或其生成的框没有垂直效果。**
2. 行间距（leading）是font-size和line-height值之差。
对于非替换元素，元素行内框的高度等于line-height的值。对于替换元素，行内框的高度等于内容区的高度，因为行间距不应用到替换元素。
3. 行框包含该行出现的行内框的最低点和最小框。
4. 行内布局基本上只有与line-height和[vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)(垂直对齐)有关。
5. 行内块`inline-block`
```css
.box2 {
  display: inline-block;
  width: 200px;
  height: 100px;
  margin: 1em;
}
```
行内元素通过使用`display: inline-block;`可变成行内块元素。
它既有块元素的部分特性（支持width/height/maigin），又有行内元素的部分特性（不换行）。
有时可取代 float 元素。

6. 用`inline-block`布局时，需要注意
	- vertical-align 属性会影响到 inline-block 元素（默认为基线对齐 vertical-block：baseline），可能会把它的值设置为 top 或 bottom。
	- 需要设置每一列的宽度。
	- 如果HTML源代码中元素之间有空格，那么列与列之间会产生空隙。
7. 解决行内水平布局之间有空格的问题
	1. 直接将HTML代码行内元素之间的换行去掉
	2. 设置整个块元素 font-size: 0; ，设置行内元素为 font-size: initial; ，将最后一行元素设置为 margin-bottom :0;
	3. 用 word-spacing（不推荐，因为要量出像素值）

## 定位布局
**[positon](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)**
- static不定位
元素的默认定位属性。
位置看display属性即可。
这个值可以用来给已定位的元素重置为不定位。
- relative相对定位
相对于自身本来的位置定位。
无论是否进行移动，元素仍然占据原来的空间。因此，移动元素会导致它覆盖其它框。
- fixed固定
被移出常规流。
元素相对于窗口定位，不随窗口内容的滚动而滚动。
定位的时候定的是元素的margin-box，所以也是m-b的边与定位box的边对齐。
元素水平和垂直方向上分别的9个属性之和要等于定位box对应方向上的尺寸。
存在过分受限的情况时，重置右方或下方的margin。
- abosolute绝对定位
被移出常规流。
元素的定位box是最近的定位祖先（position不为static的祖先）的padding-box。
其它与fixed一样。
【relative和absolute结合使用。可将父级元素设为relative，此时再设置absolute属性即可不再参照浏览器定位，而参照父级元素定位，更加方便。】
- sticky粘黏定位
可以认为是融合了fixed定位和relative定位方式。
元素的四周都在窗口以内的时候，看起来就在常规流里。
当元素的某一边从窗口的对应边离开时，触发该边的fixed定位以及该边的方位距离的生效。
而元素本身必须出现在其包含块内部，如果包含块要离开窗口，也会把它带走。

**[z-index](https://developer.mozilla.org/zh-CN/docs/Web/CSS/z-index)**
- 该属性只能用在定位元素上。
- 只能为整数。范围是 -2147483648 到 2147483647。
- 当元素之间重叠的时候，z-index 较大的元素会覆盖较小的元素在上层进行显示。
- 当 z-index 相同时，元素的叠放顺序为其在dom中的出现顺序，出现的越晚，叠放的越高。
- 当 z-index 为负的时候，元素会出现在常规流元素的下方。
- 如果父子元素都定位的话，子元素无法通过z-index跑到父元素的下方。

基本上，如果页面中有元素发生了明显的重叠，那么肯定要用到定位。

## 表布局
- border-collapse：
collapse相邻的单元格共用同一条边框。
separate默认值。每个单元格拥有独立的边框。
- hover一行/列：
行：`tr:hover{background-color: cornflowerblue;}`
列：不能直接用:hover属性。先在`<td></td>`间加上`<span></span>`,设置`td{position:relative;}`，再设置`td:hover span{position:absolute;}`使之相对 td 定位，再加上选定范围以及定位位置`width:100%; height:9999px top:-9999px; left:0; background-color:plum; z-index、:-10;`，最后将表格的`overflow: hidden;`。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    table {
      border-collapse: collapse;
      overflow: hidden;
    }

    tr:hover {
      background-color: cornflowerblue;
    }

    td {
      position: relative;
    }

    /* td:hover {
      background-color: plum;
    } */

    td:hover span {
      position: absolute;
      width: 100%;
      height: 9999px;
      background-color: plum;
      top: -9999px;
      left: 0;
      z-index: -10;
    }
  </style>
</head>

<body>
  <table border="1">
    <col>
    <col>
    <col>
    <tbody>
      <tr>
        <td>1<span></span></td>
        <td>2<span></span></td>
        <td>3<span></span></td>
      </tr>
      <tr>
        <td>4<span></span></td>
        <td>5<span></span></td>
        <td>6<span></span></td>
      </tr>
      <tr>
        <td>7<span></span></td>
        <td>8<span></span></td>
        <td>9<span></span></td>
      </tr>
      <tr>
        <td>10<span></span></td>
        <td>11<span></span></td>
        <td>12<span></span></td>
      </tr>
      <tr>
        <td>13<span></span></td>
        <td>14<span></span></td>
        <td>15<span></span></td>
      </tr>
      <tr>
        <td>16<span></span></td>
        <td>17<span></span></td>
        <td>18<span></span></td>
      </tr>
    </tbody>
  </table>
</body>

</html>
```

- 表格自适应宽度
更改第一个单元格的内容，它的单元格能自适应它自己内容的长度。
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>Document</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px blue;
    }

    table {
      width: 100%;
    }

    td:first-child {
      width: 1px;
      white-space: nowrap;
    }
  </style>
</head>

<body>
  <table>
    <td>我可是很灵活的！</td>
    <td>fffffffff</td>
  </table>
</body>

</html>
```

## 浮动布局
- 浮动元素也会生成块框。
常规流中的块元素感质不到它，行内元素感知得到它并且避开它来摆放。
所以说浮动元素同时处于流内和流外。
- 浮动元素在排列时，只参考前一个元素位置。
浮动元素不会覆盖文字/图片/表单内容。
- 清除浮动：某个块框通过向下移动，使其两边没有浮动元素。
clear: left/right/both;（只对块级元素起作用）

### 触发[BFC](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
- 作用
子元素的margin不会跑出去
它会包住自己所有的浮动后代（即闭合浮动）
自动变窄以避开与浮动元素的重叠
- 方法
`dispalay: inline-block;`
`dispalay: table-cell/table-caption;`
`dispalay: flow-root;`
`position: absolute/fixed;`
`overflow: hidden;（除了visible的值）`
`float: left;(不是none)`
`.clearfix:after{display: block; clear: both; content: "";}`

## [flex](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)布局
display： flex/inline-flex
- 主轴
[flex-direction](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-direction) :row/row-reverse/column/column-reverse
- 交叉轴
交叉轴垂直于主轴，所以如果flex-direction (主轴) 设成了 row 或者 row-reverse，交叉轴的方向就是沿着列向下的。
如果主轴方向设成了 column 或者 column-reverse，交叉轴就是水平方向。
- 折行
[flex-wrap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-wrap): nowrap/wrap/wrap-reverse;
- 简写
flex-flow: \<flex-direction> || \<flex-wrap>
<hr>

- 一行或一列的元素在主轴方向上的摆放
[justify-content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-content): center/start/end/space-around/space-between;
- 行或列在交叉轴方向上的摆放
[align-content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-content): stretch/baseline/start/end/center;
- 为所有盒中的项目定义了默认的 justify-self
[justify-items](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-items): stretch/start/end/center;
- 行里的元素在交叉轴方向上的垂直对齐
[align-items](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items): stretch/center/start/end;
<hr>

- 设置在子元素上
	- 某个元素自己的排列方式
[align-self](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-self): stretch/center/start/end;
[justify-self](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-self): stretch/center/start/end;
	- flex子元素的展示顺序
[order](https://developer.mozilla.org/zh-CN/docs/Web/CSS/order): 5
<hr>

- 每一行主轴方向上多余的空间分配权重，这个属性不会改变元素在不同行的分布
[flex-grow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-grow)
- 每一行主轴方向上空间不够时元素的收缩系数，收缩权重还要拿flex-shrink乘以元素的宽度或高度，收缩只会发生在不wrap的情况下
[flex-shrink](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-shrink)
- 在主轴方向上的初始大小
[flex-basis](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-basis)
相当于width或者height，当主轴水平时相当于width，当主轴垂直时相当于height。
如果width/height一起用，它为auto，则width/height生效；不为auto，它生效。
```html
<!-- 简例 -->
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>flex_basis_demo</title>
  <style>
    * {
      background-color: rgba(0, 0, 0, 0.1);
      box-shadow: 0 0 1px red;
    }

    @media (max-width: 600px) {
      section {
        flex-direction: column;
      }
    }

    section {
      display: flex;
      height: 100px;
    }

    div {
      flex-basis: 70%;
    }

    span {
      flex-basis: 30%;
    }
  </style>
</head>

<body>
  <section>
    <div>a</div>
    <span>b</span>
  </section>
</body>

</html>
```
- 简写
flex: \<flex-grow> | \<flex-shrink> | \<flex-basis>
<hr>

flex布局可以实现双向居中
flex父元素的匿名文本也将成为一个flex-item
flex子元素的margin auto在可能的情况下将会占据剩余的空间

# 其它
## 计数器
[counter-increment](https://developer.mozilla.org/zh-CN/docs/Web/CSS/counter-increment)
```css
/*  对段落进行编号  */
/*  每遇到一个这样元素的，计数器变量para加1  */
p {
  counter-increment: para;
}
h1 {
  counter-reset: para;
}
P::before {
  content: counter(para) ". ";
}
```
```css
/*  二进制按钮  */
[name="b16"]:checked{
	counter-increment: bin 16;
}
[name="b8"]:checked{
	counter-increment: bin 8;
}
[name="b4"]:checked{
	counter-increment: bin 4;
}
[name="b2"]:checked{
	counter-increment: bin 2;
}
[name="b1"]:checked{
	counter-increment: bin 1;
}

span::after{
	counter: counter(bin);
}
```
```css
/*  对列表进行编号  */
ul{
	list-style-type: none;
}
li{
	counter-increment: a;
}
li::before{
	content: counters(a, ".") "."
}
ul{
	counter-reset: a;
}
```

## 媒体设置
[@media](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media)
设置宽度不同时显示不同，可适应各种不同的设备。
@media (min-width:300px) and (max-width: 600px){}

## 字体
[@font-face](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)
```css
/*  指定字体  */
@font-face{
  font-family: AAA;
  src: url();
}
/*  使用字体  */
div{
  font-family: AAA;
}

/*  使用字体图标  */
.star {
  font-family: FontAwesome;
  font-size: 30px;
}
.star::before {
  	content: "\f006";
}
```
图标选择：
- 图标方案
直接用图片
每个小图片都得单独下载，流量增大
每次使用都要书写图片的地址
可以用动图
图标放大都会不清晰
  
- css sprite
所有图片都在一个文件里，只下载一次，节省流量
需要使用背景相关的属性裁剪出大图中的小图
不能用动图
不好维护
图标放大都会不清晰

- 字体图标
矢量图，放大不失真
由于图标其实是文字，所以可以随意改变颜色
但由于一个图标是一个字，颜色单一
所有的符号形状在一个字体文件里，体积小，省流量
不好维护
图标也是不能动的
不能是彩色的
	- SVG图标：
	图标可以是彩色的
	还可以是带动画的

# 其它布局属性
## 多列布局
属性|意义
:-|:-
[column-count](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count)|元素的列数
[column-width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-width)|每列的宽度
[columns](https://developer.mozilla.org/zh-CN/docs/Web/CSS/columns)|`<'column-width'> || <'column-count'>`
[column-fill](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-fill)|如何对列进行填充
[column-gap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-gap)|元素列之间的间隔
[column-rule](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-rule)|`<'column-rule-width'> || <'column-rule-style'> || <'column-rule-color'>`
[break-inside](https://developer.mozilla.org/zh-CN/docs/Web/CSS/break-inside)|多列布局页面下的内容盒子如何中断【更符合想要效果h1,h2{[column-span](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-span): all;}】

## 文字换行
属性|意义
:-|:-
[text-overflow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow)|ellipsis;文字溢出排版
[hyphens](https://developer.mozilla.org/zh-CN/docs/Web/CSS/hyphens)|连字符
\&shy;|Soft hyphen

## 手机布局
viewport声明：`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
只被手机浏览器识别。不被电脑浏览器识别。
它的作用是设定手机浏览器使用多宽的窗口（即初始包含块的宽度）来渲染页面。
手机上页面一旦渲染，窗口的大小是不再发生变化的，双指缩放也不会让页面重新布局。
手机在特定宽度的浏览器上渲染完成后会将结果正好铺满手机屏幕。

所有的手机都支持viewport的width写成device-width，该值在不同手机上不一样，典型的大小是360，415，425，390，320。
一般来讲屏幕越大device-width实际生效的值也越大。安卓5.0以上支持将viewport的width写成具体数值，这样一来开发者就相当于面对了相同宽度的手机浏览器窗口。

针对不同的网站设计，需要使用不同的方案：
- 不同手机上都是等比的，如小米商城移动版。
1. 如果网站用户的浏览器都支持将viewport的width写成具体数值，那么就直接将width写成视觉稿的宽度（不用带单位），然后整个页面就用从视觉稿里量出来的数据配上px单位。
2. 如果网站用户的浏览器有比较旧的，不支持将viewport的width写成数值，而只能写成device-width。
我们想要的效果其实就是视觉稿宽度就是屏幕宽度
假设视觉稿宽度为X，则Xrem = 100vw
于是 html {font-size: calc(100vw / X)}
开始时使用视觉稿中量出来的尺寸配上rem单位即可。
	1. 但是，由于Chrome浏览器默认不支持小12px的字号，所以html元素的字号放大100倍，视觉稿中测量出来的数值则要缩小100倍再加上rem单位。
	2. 更旧的浏览器甚至不支持calc/vw，所以使用js读取出窗口宽度的px单位的长度，然后计算出结果并设置到html元素上。
- 对于内容更多以文字为主，没有太多需要保持比例的布局的网站，如github的首页。
那么直接将viewport设置为device-width，让不同手机有不同窗口宽度，使用流式布局风格+media query来实现。

- 对于布局想要保持比例，但文字内容跟屏幕大小呈正相关的网站。
可以使用device-width，让文字保持默认大小即16px，那么空间越大字就越多。但页面的布局及元素的大小，依然使用rem等比缩放布局。

# 回流和重绘
## 回流(reflow,relayout)
- 定义
浏览器为了重新渲染部分或全部的文档而重新计算文档中元素的位置和几何结构的过程。简单来说就是当页面布局或者几何属性改变时就需要reflow。
- 触发回流
盒模型相关的属性: width，height，margin，display，border，etc
定位属性及浮动相关的属性: top,position,float，etc
改变节点内部文字结构: text-align, overflow, font-size, line-height, vertival-align，etc
除开这三大类的属性变动会触发reflow，以下情况也会触发：
调整窗口大小
样式表变动
元素内容变化，尤其是输入控件
dom操作
css伪类激活
计算元素的offsetWidth、offsetHeight、clientWidth、clientHeight、width、height、scrollTop、scrollHeight

尽量不使用回流，如果要用的话，尽量把回流限制在一定范围之内。

## 重绘(repaint)
- 定义
当页面中的元素只需要更新样式风格不影响布局，这个过程就是重绘。
- 触发重绘
页面中的元素更新样式风格相关的属性时，background，color，cursor，visibility，etc
