---
layout: post
title: knowledge-函数
categories: knowledge
description: Knowledge changes destiny
keywords: knowledge, javaScript
---

> 前言

  本来还想着说点别的什么事情，结果上了一趟脉脉，感觉中国最不缺的就是牛人，我就想啊，牛人这么多，为啥我不是呢。。。 好了，还是好好的自己默默地磨刀吧。。。先把基础的弄好在飞，先把自己本分的东西学好再说。。

* 基础——函数

  函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。

  关键字 function

  简单的函数：

  ```
  function xxx(){
	内容
  }
  ```

  有时候，函数需要特定的信息来执行任务，在这种情况下，声明函数需要给它提供形参。在函数内部，形参的行为类似于变量。

  如果函数需要一些信息，在函数名称后面的括号内指定
  括号中的这些项即函数的形参。在函数体重他们的行为和变量一样。

  ```
  function getArea(width, height){
	return width * height
  }	
  getArea(3,5)
  ```
  调用这些带有形参的函数时，需要在函数名后面的括号中指定一些值，这些值就是形参，可以像变量一样被赋值。

  变量作为实参
	无需再调用函数时指定实际的值，可以在其位置使用变量
  ```
  wallWidth = 3;
  wallHeight = 5;
  getArea(wallWidth,wallHeight);
  ```

  形参 VS 实参
	
	形参：它不是实际存在的变量，是在定义函数名和函数体的时候使用的参数,目的是用来接收调用该函数时传入的参数.在调用函数时，实参将赋值给形参。

	实参：实参可以是常量、变量、表达式、函数等， 无论实参是何种类型的量，在进行函数调用时，它们都必须具有确定的值， 以便把这些值传送给形参。


  匿名方法和函数表达式

  表达式会产生值，这些值可以被用于所期望的位置，如果函数被置于浏览器期待看到表达式的地方，那么函数将被当做表达式一样对待。

  将函数放在本该表达式呆的位置，它将被当做表达式对待，这称为函数表达式。在函数表达式中，名字经常被省略。没有名字的函数被称为匿名函数。

  ```
  var area = function(width,height){
  	return width * height;
  };
  
  var size = area(3,4);
  ```  

  在函数表达式中，解释器到达这条语句时函数是不会执行的。这意味着在解释器发现这条语句之前，不能调用此函数。这也意味着在那个点之前出现的任何代码都可能修改函数的内容。

* 执行上下文与提升
  脚本每次进入一个新的执行上下文时，都会经历两个阶段。

  1.准备
  ———— 创建新的作用域
  ———— 创建变量、函数和参数
  ———— 确定this 关键字的值

  2.执行
  ———— 现在可以给变量赋值了
  ———— 引用函数来执行其代码
  ———— 执行语句

  理解这两个阶段所发生的事情有助于理解提升(Hoisting)这个概念

  已经知道
  	在声明函数之前就调用改函数（假设他们是使用的函数声明来创建的，而不是使用函数表达式）
  	复制给还没有声明的变量

  这是因为每个执行上下文中的任何变量和函数都会在执行之前被创建好

  收集所有变量和函数，并将它们提升到执行上下文的顶部。

  每个执行上下还会创建它自己的variables对象。这个对象包含了改执行上下文中所有变量、函数和参数的细节。

  
	 ```
	 var greeting = greetUser();
	 function greetUser(){

	 };
	 ```

 它能正常的执行，因为该函数和第一条语句位于同一个执行上下文中，所以，它会被解析成：

	 ```
	 function greetUser(){

	 }
	 var greeting = greetUser();
	 ```

  下面这个代码则会出错，因为greetUser()是在getName()函数的上下文中创建的

	  ```
	   var greeting = greetUser();
	   function getName(){
			function geetUser(){

			}
	   }
	  ```

* Hoisting

	变量的提升，顾名思义，就是把下面的东西提到上面。
	在JS中，就是把定义在后面的东东（变量或函数）提升到前面中定义。 

	举例：
	```
		var a='Hello World'; 
		alert(a); 
	``` 
	弹出 Hello World

	```
	var v='Hello World'; 
	(function(){ 
		alert(v); 
	})() 
	```
	弹出 Hello World

	```
	var v='Hello World'; 
	(function(){ 
		alert(v); 
		var v='I love you'; 
	})() 
	```
	弹出undefined

	原因：

	```
	var v='Hello World'; 
	(function(){ 
	 	var v;
		alert(v); 
		var v='I love you'; 
	})() 
	```

* 函数作用域（Function Scoping）
	 JavaScript 没有块级作用域（Block Scoping），只有函数作用域（Function Scoping）

	```
	 var a = 1;
 
	function foo() {
	  if (!a) {
	    var a = 2;
	  }
	  alert(a);
	};
	 
	foo();	
	```
	==
	```
	var a;   //声明
 	a = 1;	 //定义
	function foo() {
	  var a;
	  if (!a) {
	    var a = 2;
	  }
	  alert(a);
	};
	 
	foo();	
	```
	当解析器读到 if 语句的时候，它发现此处有一个变量声明和赋值，于是解析器会将其声明提升至当前作用域的顶部（这是默认行为，并且无法更改），这个行为就叫做 Hoisting。

	创建新的作用域
```
	var a = 1;
 
	function foo() {
	  if (!a) {
	    (function() {    // 它会创建一个新的函数作用域
	      var a = 2;    // 并且该作用域在 foo() 的内部，所以 alert 访问不到
	    }());        // 不过这个作用域可以访问上层作用域哦，这就叫：“闭包”
	  };
	  alert(a);
	};
	 
	foo();
```


> 充实自己，使自己成为你想成为的人