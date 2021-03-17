---
title: web前端学习笔记1——HTML
subtitle: HTML
categories: web前端学习笔记
abbrlink: 41e65d2c
tags:
  - web前端
  - HTML
date: 2020-06-14 19:17:00
index_img:
---

# 路径及注释
```
../               上一级目录
../../            上两级目录
<!-- 注释内容 -->  注释（快捷键Ctrl + /）
```

# 标题
```html
<h1>......</h1>    一级标题（就是Markdown里的#）
<h2>......</h2>    二级标题（##）
<h3>......</h3>	   三级标题（###）
<h4>......</h4>	   四级标题（####）
<h5>......</h5>	   五级标题（#####）
<h6>......</h6>	   六级标题（######）
```
# 字体及格式
```html
<i>............</i>          斜体 
<em>...........</em>         斜体（emphasized，内容的着重强调）     
<b>............</b>          加粗
<strong>.......</strong>     加粗（表示文本十分重要）
<del>..........</del>	     删除线   
<u>............</u>          下划线
<sup>..........</sup>	     上标
<sub>..........</sub>        下标
<center>.......</center>     居中显示  
<p>............</p>          段落标签
<blockquote>...<blockquote>  引用
<hr />                       水平线
<br />                       换行（自闭合标签）
&nbsp;	                     空格
&lt;                         左尖括号
&gt;                         右尖括号                  
```
# 常见标签
- `<abbr>`
缩写，使用 title 属性可以在鼠标放在上面时显示完整词语
`<abbr title="Cascading Style Sheets">CSS</abbr>`
- `<pre>`           
预定义格式文本，不改变原有格式排版。常与`<code>`搭配使用。
`<pre><code class="">code goes here</code></pre>`
- `<base>`
指定用于一个文档中包含的所有相对 URL 的根 URL。如果指定了多个 \<base> 元素，只会使用第一个 href 和 target 值, 其余都会被忽略。
```html
给定 <base href="https://example.com" target="_blank">
链接 <a href="#anchor">first</a>
链接指向 https://example.com/#anchor
```
- `<map>`
与`<area>`属性一起使用来定义一个图像映射
```html
<img ...usemap="#map1">
<map name="map1">
  <area title="" href="" shape="rect/circle/poly" coords="">
</map>

<!-- 矩形框，坐标以图片左上角为(0,0)-->
shape="rect" coords="左上角坐标,右下角坐标"  
<!-- 圆形框 -->
shape="circle" coords="圆心坐标,半径"  
<!-- 多边形框 -->
shape="poly" coords="顶点1坐标,顶点2坐标,..."  
```
- `<progress>`
显示为一个进度条
```html
<label for="file">File progress:</label>
<progress id="file" max="100" value="70"> 70% </progress>
```

# 常见属性
名称|功能
:-:|:-:
[tabindex](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/tabindex)|全局属性，指示其元素是否可以聚焦
[contenteditable](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/contenteditable)|表示元素是否可被用户编辑
[data-\*](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/data-*)|自己编写的属性，存储一些需要记录的信息


# 列表
## 无序列表
```html
<ul>
	<li>...</li>
	<li>...</li>
</ul>

另外
<ul type = "disc">      默认，实心圆
<ul type = "circle">    空心圆
<ul type = "square">    实心方块
```
## 有序列表
```html
<ol>
	<li>...</li>
	<li>...</li>
</ol>

另外
<ol type = "1">        默认，数字排序
<ol type = "a/A">      大/小写字母排序
<ol type = "ⅰ/Ⅰ">    大/小写罗马字母排序
```
## 自定义列表
```html
<dl>
  <dt>术语</dt>
  <dd>描述</dd>
</dl>
```
注. 不要将此属性用来在页面创建具有缩进效果的内容。
要改变描述列表中描述的缩进量，请使用 CSS margin 属性。
# 链接
## 超链接  
`<a href = "..." target = "_blank">名称</a>`
默认为target = "\_self"，在当前标签跳转。              
target = "\_blank" 表示在新页面跳转，不加则在当前页面跳转。
\_blank可以自定义名称，若两个标签此名称相同，则会占用同一个页面。
download = ""属性，只能下载来自自己网站的文件。
还有一个 download 属性，下载的文件名以它的值来命名。
## 外部资源链接[`<link>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link)
外部css`<link rel="stylesheet" href="theme.css" type="text/css">`
网站图标`<link rel="icon" href="favicon.ico" type="image/x-icon">`
## 图片插入
`<img src = "" title = "" alt = "" width = "" height = "">`
titlt 是鼠标滑上去出现的文字(tooltip)，alt 是加载失败后出现的文字，width 可以用"10px"或者100%。
## 页面插入
- `<iframe>`
```html
  <a target="frame1" href="https://www.baidu.com/">baidu</a>
  <a target="frame1" href="https://www.hao123.com/">hao123</a><br>
  <iframe width="500" height="300" src="" frameborder="1" name="frame1">您的浏览器不支持iframe，请升级浏览器</iframe> 
  target="_top"，在嵌套多层时，打开的窗口在顶层。
  target="_top"，在嵌套多层时，打开的窗口在它的父窗口。
```
只有当页面插入不能显示时，才会显示出中间的文字。
- `<object>`
`<object data=""></object>`
可以嵌入的资源格式很多，包括文档类，图片类等。
# 表格
输入tr\*m>td\*n，再按一下Tab可快速建立m行n列的表格。
```html
<table>......</table>           表格
<thead>......</thead>           表头
<tbody>......</tbody>           表格
<tfoot>......</tfoot>           表尾
<tr>.........</tr>              表示一行
<td>.........</td>              表示单元格
<col>                           表示一列
<caption>....</caption>         表格标题
border = "1px"                  表格边框属性
cellspacing = "0"               单元格空隙
align = "center/left/right"     左右对齐方式
valign = "center/left/right"    上下对齐方式
```
```html
<!-- 最基本的使用 -->
<table border = "1px" cellspacing = "0">
<caption>成绩表</caption>
	<col width = "100px">
	<col width = "100px">
	<col width = "100px">
	<col width = "100px">
	<tr align = "center">
		<td></td> 
		<td>张三</td>
		<td>李四</td>
		<td>王五</td> 
	</tr>
	<tr align = "center">
		<td>智育</td> 
		<td>30</td>
		<td>40</td>
		<td>50</td> 
	</tr>
	<tr align = "center">
		<td>体育</td> 
		<td>35</td>
		<td>56</td>
		<td>20</td> 
	</tr>
	<tr align = "center">
		<td>德育</td> 
		<td>56</td>
		<td>36</td>
		<td>47</td> 
		</tr>
	</table>
```
```html
colspan = ""     合并几列
rowspan = ""     合并几行
th               加粗并水平居中的td
colgroup         用来定义表中的一组列表
```
```html
<table border = "1px" cellspacing = "0">
	<colgroup span = "6" width = "100px">
	<colgroup span = "1" width = "200px">		
	<tr align = "center" height = "40px">
		<th colspan = '7'>个人简历</td> 
	</tr>
	<tr align = "center" height = "40px">
		<td>姓名</td> 
		<td></td>
		<td>性别</td>
		<td></td> 
		<td>年龄</td>
		<td></td>
		<td rowspan = "4">照片</td>
	</tr>
	<tr align = "center" height = "40px">	
		<td>学历</td> 
		<td></td>
		<td>籍贯</td>
		<td></td> 
		<td></td>
		<td></td>
	</tr>
	<tr align = "center" height = "40px">
		<td>电话</td> 
		<td></td>
		<td>政治面貌</td>
		<td colspan = "3"></td> 
	</tr>
	<tr align = "center" height = "40px">
		<td>毕业院校</td> 
		<td colspan = "5"></td>
	</tr>
	<tr align = "center" height = "40px">
		<td>求职意向</td> 
		<td colspan = "6"></td>
	</tr>
</table>
```
实际显示效果：
<table border = "1px" cellspacing = "0">
	<colgroup span = "6" width = "100px">
	<colgroup span = "1" width = "200px">		
	<tr align = "center" height = "40px">
		<th colspan = '7'>个人简历</td> 
	</tr>
	<tr align = "center" height = "40px">
		<td>姓名</td> 
		<td></td>
		<td>性别</td>
		<td></td> 
		<td>年龄</td>
		<td></td>
		<td rowspan = "4">照片</td>
	</tr>
	<tr align = "center" height = "40px">	
		<td>学历</td> 
		<td></td>
		<td>籍贯</td>
		<td></td> 
		<td></td>
		<td></td>
	</tr>
	<tr align = "center" height = "40px">
		<td>电话</td> 
		<td></td>
		<td>政治面貌</td>
		<td colspan = "3"></td> 
	</tr>
	<tr align = "center" height = "40px">
		<td>毕业院校</td> 
		<td colspan = "5"></td>
	</tr>
	<tr align = "center" height = "40px">
		<td>求职意向</td> 
		<td colspan = "6"></td>
	</tr>
</table>

# 表单
## `<form>`
用于向 Web 服务器提交信息。常见属性如下：
- action = ""
表单提交的地址
- method = "get/post"
表单数据提交的方式

get请求|post请求
:-:|:-:
通常表示获取数据|通常表示提交数据
发送的数据写在地址栏上，用户可见|发送的数据用户不可见
不能提交大量数据|可以提交大量数据，不要混用
- name = ""
表单的名称。该值必须是所有表单中独一无二的，而且不能是空字符串。（HTML 4中应使用 id 属性）

## `<input>`
- type = ""

功能|写法
:-|:-
文本输入|type = "text"
密码输入|type = "password"
数字输入|type = "number"
邮箱输入|type = "email"
网址输入|type = "url"
电话输入|type = "tel"
【属性】文本框|maxlength=""，minlength=""，placeholder=""(输入预期值的提示信息)，required(必须要填)，autofous(页面加载时自动聚焦到此表单控件)
单选框|type = "radio"
复选框|type = "checkbox"
【属性】选择框|checked，默认选中；disabled，不能选择
普通按钮|type = "button"(`<button>...</button>`)
提交按钮|type = "submit"
重置按钮|type = "reset"
提交按钮，形态是一张图片|type="image" src=""
隐藏|type = "hidden"
颜色|type = "color"
日期|type = "date"
当地日期|type = "datetime-local"
时/周/月|type = "time/week/month"
范围选择，滑条|type = "range" max="" min="" step=""
文件选择|type = "file"
【属性】文件默认选择格式|accept = "image/*"
【属性】文件可以选择多个|multiple，"email"和"file"可用
- name = ""
表单控件的名字。以名字/值对的形式随表单一起提交。
- value = ""
表单控件的值。以名字/值对的形式随表单一起提交。若选择框不写此属性，则选择默认value值为"on"。

## `<label>`
可以和一个\<input>标签关联起来，使用方法
`<label>Click <input type="text"></label>`
或者
```	html
<label for="username">Click me</label>
<input type="text" id="username">
```

## `<select>`
```html
<label for="birthdat">出生日期</label>

<select id="birthday" name="birth">
	<option hidden disabled selected>请选择</option>
	<optgroup label="91-92">
		<option value = "91" >1991</option>
		<option value = "92" >1992</option>
	<optgroup label="93-94">
		<option value = "93" disabled>1993</option>
		<option value = "94" >1994</option>
	......
</select>
```
上面展示了一个下拉菜单。。
\<option>如果不含 value 属性，则 value 值默认为元素中的文本
\<optgroup>分组。
selected 属性，使其默认被选中。
disabled 属性，禁用。
hidden 属性，隐藏。

## `<textarea>`
```html
<label for="proverb">你的座右铭：</label>

<textarea id="proverb" name="pb" cols="20" rows="3">Where there's a will there's a way
</textarea>
```
上面展示了一个多行文本框。
rows 和 cols 属性设置文本输入窗口的高度和宽度。

## `<fileset>`
```html
<fileset disabled>
	<legend>个人信息</legend>
	<input type="text">
</fileset>
```

## 实例
```html
<form name = "" method = "" action = "">
	<table>
		<tbody>
			<tr height = "40px">
				<td rowspan = "7" align ="center">总体信息</td>
				<td colspan = "2"></td>
			</tr>
			<tr height = "40px">
				<td align ="right">用户名：</td>
				<td>
					<input type = "text" name = "login"> 
				</td>
			</tr>
			<tr height = "40px">
				<td align = "right">密码：</td>
				<td>
					<input type = "password" name = "pwd"> 
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td  colspan = "2">
					男<input type = "radio" name = "gender">&nbsp;&nbsp;&nbsp;&nbsp;
					女<input type = "radio" name = "gender" checked>  
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td colspan = "2">出生年份：
					<select>
						<option>1996</option>
						<option>1997</option>
						<option>1998</option>
						<option>1999</option>
					</select> 
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td align = "right">个人简介：</td>
				<td>
					<textarea cols="24" rows="5"></textarea>
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td colspan = "2">
					<input type = "submit" value ="提交">
					<input type = "reset" value ="重置">
				</td>
			</tr>
		</tbody>
	</table>
</form>
```
显示效果：
<form name = "" method = "" action = "">
	<table>
		<tbody>
			<tr height = "40px">
				<td rowspan = "7" align ="center">总体信息</td>
				<td colspan = "2"></td>
			</tr>
			<tr height = "40px">
				<td align ="right">用户名：</td>
				<td>
					<input type = "text" name = "login"> 
				</td>
			</tr>
			<tr height = "40px">
				<td align = "right">密码：</td>
				<td>
					<input type = "password" name = "pwd"> 
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td  colspan = "2">
					男<input type = "radio" name = "gender">&nbsp;&nbsp;&nbsp;&nbsp;
					女<input type = "radio" name = "gender" checked>  
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td colspan = "2">出生年份：
					<select>
						<option>1996</option>
						<option>1997</option>
						<option>1998</option>
						<option>1999</option>
					</select> 
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td align = "right">个人简介：</td>
				<td>
					<textarea cols="24" rows="5"></textarea>
				</td>
			</tr>
			<tr height = "40px" align = "center">
				<td colspan = "2">
					<input type = "submit" value ="提交">
					<input type = "reset" value ="重置">
				</td>
			</tr>
		</tbody>
	</table>
</form>
