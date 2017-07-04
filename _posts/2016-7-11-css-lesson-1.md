---
layout: post
title:  CSS学习一
date:   2016-7-11 13:28:03
author:  "Yvan"
header-img: "img/post-bg-02.jpg"
---
HTML、CSS、JS，前端必备三件套，上大学的时候就学过，无奈三天打鱼两天晒网，直到现在还是半桶水。现在重新拾起，并且写出来，教给别人是理解最深刻的。

<h2>CSS(Cascading Style Sheets层叠样式表)</h2>

CSS用来控制HTML的显示与布局。

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明:

	h1{color:blue;font-size:12px;}

CSS声明以分号结束，用大括号括起来

<h4>id和class选择器</h4>

HTML元素以id属性来设置id选择器,CSS 中 id 选择器以 "#" 来定义。

	#para1{
		text-align:center;
		color:red;
	}
class选择器用于描述一组元素的样式，class可以在多个元素中使用。

	.para2{
		text-align:center;
	} 

所有class=para2都居中

	<h1 class="para2">标题居中</h1>
	<p class="para2">段落居中。</p> 
	
也可以指定特定的HTML元素使用class

		p.para2{
			text-align:center;
		}

<h4>插入CSS有三种：</h4>
<ol>
<li>外部样式表
<br/>每个页面使用&lt;link&gt;标签链接到样式表。 &lt;link&gt; 标签在（文档的）头部
</li>
<li>内部样式表
<br/>当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用&lt;style&gt; 标签在文档头部定义内部样式表
</li>
<li>内联样式表
<br/>仅需要在一个元素上应用一次时采用
</li>
</ol>
