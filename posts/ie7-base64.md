---
title: IE hasLayout问题总结
date: '2013-03-25'
description:
categories: 'IE'
---

#### 介绍

这篇文章是一个总结haslayout的文章。

**更新：haslayout在ie8的标准模式下已经被废弃了，但是在ie7的兼容版本以及以下的版本是仍然存在的。**


#### 什么是haslayout？

MSIE  有一个很早很早，过时的渲染引擎 Mosaic . 在表格布局的时代。几乎是所有的元素（除了内联内容）都是一个盒子。内容几乎不可能超过表格的单元格。表格的单元格不可能超出表格。

很多年过去了。微软开始采用Trident engine 来使用CSS，但是，CSS改变了最初的这个古老引擎的假设（最重要的一点就是任何元素都会包含它的内容）。

但是CSS允许内容超出元素(就是内容浮动的时候，或者内容太高、宽去适应包含盒子)

#### haslayout是怎么来的？

为了解决这个问题，微软的天才coder决定去修理他们的这个古老的引擎，因此haslayout这个属性就诞生了。每个元素都有haslayout这个属性去设置true或者false，如果它设置成了true，它就不得不去渲染它自己，因此元素不得不扩展去包含它的流出的内容。例如浮动或者很长很长的没有截断的单词，如果haslayout没有被设置成true，那么元素得依靠某个祖先元素来渲染它。这就是很多的ie bugs诞生的地方。

haslayout不是一个CSS属性，你不能这样的来设置它 haslayout:true;一个元素被设置成haslayout：true将被渲染成一个 having haslayout，反之。

#### 哪些元素本身就有haslayout属性


&lt;html&gt;, &lt;body>
&lt;table&gt;, &lt;tr&gt;, &lt;th&gt;, &lt;td&gt;
&lt;iframe&gt;, &lt;embed&gt; (non-standard element), &lt;object&gt;, &lt;applet&gt;
&lt;img&gt;
&lt;hr&gt;
&lt;input&gt;, &lt;button&gt;, &lt;select&gt;, &lt;textarea&gt;, &lt;fieldset&gt;, &lt;legend&gt;
&lt;marquee&gt; (don’t ever use this one, non-standard and annoying)


这个列表时不完善的。很多元素在微软的官方网站上没有提到，但是有一个方法很容易的测试到一个元素是否有layout，例如下代码：

<pre>
	<code>
		<div id="menu"> … </div>
	</code>
</pre>

为了判断这个div的haslayout属性值，我们可以在浏览器地址栏中输入如下代码：

<pre>
	<code>
		javascript:alert(menu.currentStyle.hasLayout);
	</code>
</pre>

运行了这个代码之后就会反映出这个div的haslayout的属性值

#### 如何设置haslayout

设置haslayout，换句话来说，就是给定一个布局，相对来说比haslayout等于false要简单。

以下属性和值将给定一个元素布局

<pre>
	<code>
		position: absolute
		float: left or right
		display: inline-block
		width: any value other than auto
		height: any value other than auto
		zoom: any value other than normal (see description below)
		writing-mode: tb-rl (see description below)
	</code>
</pre>

在ie7中, 也有一些属性 give “layout”:

<pre>
	<code>
		overflow: hidden or scroll or auto
		overflow-x: hidden or scroll or auto
		overflow-y: hidden or scroll or auto
		min-width: any value other than auto
		max-width: any value other than auto
		min-height: any value other than auto
		max-height: any value other than auto
	</code>
</pre>

在ie8的标准模式中，微软已经废弃了haslayout属性了，但是在ie7的兼容模式中，仍然存在着这个属性。

你可能对zoom 和write-mode这2个属性不太熟悉，他们都是微软的扩展属性。他们仅仅在ie中有效并且将来可能无效，因此我建议你把他们放入condcoms

write-mode属性在css3技术文档中已经出现了。zoom可能被提议，但是目前还没有。

zoom：1作者认为是最好的触发haslayout属性的组合，因为它对房前元素没有一点影响。

write-mode 就是字体排版布局的方式。

设置display：inline-block没有移除布局，这个技巧可以给元素设置成haslayout：true；

它相当于

<pre>
	<code>
		div { display: inline-block; } 
		div { display: block; }
	</code>
</pre>

设置contenteditable也给一个元素设置成了haslayout：true。例如：<p contenteditable=”true”>so lame</p>

你可能从来不用它来设置haslayout：true，写在这里只是为了一个信息的目的。不仅contenteditable是微软的一个属性，而且他还可以允许用户

编译元素的内容。这点有可能使用户困惑。

hasLayout 是一个可读的属性，不能通过js来修改它。

#### Bug 

ie下的80% 的bug都是由于元素没有haslayout

IE hasLayout bugs经常出现各种很奇怪的问题，如果ie有些很难解释的问题，第一件事情要做的就是给该元素一个layout。

[原文地址](http://www.cnblogs.com/yupeng/archive/2011/04/11/2012996.html)