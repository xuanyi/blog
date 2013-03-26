---
title: Javascript深度克隆對像
date: '2013-03-26'
description:
categories: 'Javascript'
---

#### 為什麼要深度克隆對像

摟一眼下面的代碼：

	var xiaoming = {name: '明小舟', age: 26};
	var xiaozhou = xiaoming;
		xiaozhou.sex = '男';
	console.log(xiaoming);
	// 輸出：Object {name: "明小舟", age: 26, sex: "男"}

從代碼執行結果可以看出，`xiaozhou`只是`xiaoming`的一個引用，當改變`xiaozhou`的值`xiaoming`的值也同步改變。而有的時候我們不需要這種同步，那麼就需要生成一個副本。

#### 深度克隆

	function cloneObject(object){
		var newObject = object.constructor === Object ? {} : [];
		if(newObject.constructor === Array){
			newObject = newObject.concat(object);
		}else{
			for(var i in object){
				newObject[i] = object[i];
			}
		}
		return newObject;
	}

如果克隆的對像是數組，則使用內置`concat`方法生成一個新數組，如果是對像則將對像的屬性、方法賦給新對像。

測試下：

	// 對像
	var xiaoming = {name: '明小舟', age: 26};
	var xiaozhou = cloneObject(xiaoming);
	xiaozhou.sex = '男';
	console.log(xiaozhou);
	console.log(xiaoming);
	// 輸出：Object {name: "明小舟", age: 26, sex: "男"}
	// 輸出：Object {name: "明小舟", age: 26}

	// 數組
	var arr1 = [1,2,3,4,5];
	var arr2 = cloneObject(arr1);
	arr2[2] = 9;
	console.log(arr2);
	console.log(arr1);
	// 輸出：Array [1, 2, 9, 4, 5] 
	// 輸出：Array [1, 2, 3, 4, 5] 

#### 使用JSON對像的stringify()和parse()方法

	// 對像
	var xiaoming = {name: '明小舟', age: 26};
	var xiaozhou = JSON.parse(JSON.stringify(xiaoming));
	xiaozhou.sex = '男';
	console.log(xiaozhou);
	console.log(xiaoming);
	// 輸出：Object {name: "明小舟", age: 26, sex: "男"}
	// 輸出：Object {name: "明小舟", age: 26}

	// 數組
	var arr1 = [1,2,3,4,5];
	var arr2 = JSON.parse(JSON.stringify(arr1));
	arr2[2] = 9;
	console.log(arr2);
	console.log(arr1);
	// 輸出：Array [1, 2, 9, 4, 5] 
	// 輸出：Array [1, 2, 3, 4, 5] 


#### 最終的方式

	function cloneObject(object){
		var newObject = object.constructor === Object ? {} : [];
		if(typeof JSON === 'object'){
			var _string = JSON.stringify(object);
			newObject = JSON.parse(_string);
		}else{
			if(newObject.constructor === Array){
				newObject = newObject.concat(object);
			}else{
				for(var i in object){
					newObject[i] = object[i];
				}
			}
		}
		return newObject;
	}