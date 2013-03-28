---
title: 一段巧妙判断IE浏览器版本的js代码
date: '2013-03-28'
description:
categories: 'Javascript'
---

转自：[www.jsmix.com](http://www.jsmix.com/blog/javascript/a-short-snippet-for-detecting-versions-of-ie-in-javascript.html)

下面这段代码使用IE可以识别html注释的特性，可以准确的判断IE浏览器的版本号，没有使用诸多怪异的测力检测方法，超级简洁。 原作者的代码地址在[这里](https://gist.github.com/527683)，我做了一点小改动，文中有注释，可以到[这里](http://jsfiddle.net/Doomin/df8Hw/)运行测试。

	/**
	 * ie equals one of false|6|7|8|9 values, ie5 is fucked down.
	 * Based on the method: https://gist.github.com/527683
	 */
	var ie = function () {
	    var v = 4, //原作者的此处代码是3，考虑了IE5的情况，我改为4。
	        div = document.createElement('div'),
	        i = div.getElementsByTagName('i');
	    do {
	        div.innerHTML = '<!--[if gt IE ' + (++v) + ']><i></i><![endif]-->';
	    } while (i[0]);
	    return v > 5 ? v : false; //如果不是IE，之前返回undefined，现改为返回false。
	}();

