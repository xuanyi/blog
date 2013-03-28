---
title: 使用JS處理HTML字符實體
date: '2013-03-28'
description:
categories: 'Javascript'
---

有的时候我们可能不得不使用 `js` 来处理 `html entities`，例如把 `&gt;` 这样的内容转化成 `>` 符号，或者是相反的操作。如果你不幸遇到了这样的需求，下面的内容也许会有所帮助。


`unescapeHtmlEntities` 的实现非常简单，创建一个节点元素，然后将字符串写入这个元素的 `innerHTML` 属性，最后返回这个元素的文本内容。调用下面的函数时传参`'&gt;'`，得到的结果将会是`'>'`：

	function unescapeHtmlEntities(str) {
		var tempEle = document.createElement("div");
		tempEle.innerHTML = str;
		var result = tempEle.childNodes[0].nodeValue;
		return result;
	}

`escapeHtmlEntities` 的过程刚好相反，创建元素，插入文本节点，读出元素的 innerHTML 时，得到字符的 `entity`。调用下面的函数时传参'>'，得到的结果将会是'&gt;'

	function escapeHtmlEntities(str) {
		var tempEle = document.createElement("div");
		tempText = document.createTextNode(str);
		tempEle.appendChild(tempText);
		var result = tempEle.innerHTML;
		return result;
	}

	alert(escapeHtmlEntities('>'));  // result : >

如果使用 `jQuery`，这个两个函数可以一句话搞定：

	function unescapeHtmlEntities(str) {
	    return $('<div />').html(str).text();
	}
	function escapeHtmlEntities(str){
	    return $('<div />').text(str).html();
	}

尽管如此，对于 `html entities` 的处理并不推荐在行为层中进行。事实上模板、后端语言更能更好地进行工作，例如 **PHP** 的 `htmlentities` 。

[點這裏試一下](http://tools.mingxiaozhou.com/html_to_entities/index.html)

[查看原文](http://www.jsmix.com/blog/javascript/htmlentities.html)