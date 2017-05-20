---
layout: post
title: knowledge-立即调用函数 、 variables 、 闭包
categories: knowledge
description: Knowledge changes destiny
keywords: knowledge, javaScript
---

> 立即调用函数 、 variables 、 闭包

  * 立即调用函数
    
    立即调用函数 = 立即执行函数

    通常是为了确保变量名不互相冲突(尤其是在一个页面中有多段脚本的时候)

    立即调用函数表达式(IIFE)
    在解释器经过它们时执行一次
    ( function(){…} )()和( function (){…} () )是两种javascript立即执行函数的常见写法
	
	```
		xxx();
		function xxx(){
			alert("1");
		};

		// 没有问题。因为在准备阶段已经创建了函数

		xxx();

		var xxx = function(){
			alert("1");
		};

		// 错误。因为 xxx 还没有对函数引用，函数调用当然出错。在执行阶段先给变量赋值，然后在引用函数执行代码。
	```

	立即执行函数在模块化中有大量的用处，用立即执行函数处理模块化可以减少全局变量造成的空间污染，构造更多的私有变量。

	```
	// 并不会像你想象那样的执行，因为i的值没有被锁住
	// 当我们点击链接的时候，其实for循环已经执行完了
	// 于是在点击的时候i的值其实已经是elems.length了
	var elems = document.getElementsByTagName( 'a' );

	for ( var i = 0; i < elems.length; i++ ) {

	  elems[ i ].addEventListener( 'click', function(e){
	    e.preventDefault();
	    alert( 'I am link #' + i );
	  }, 'false' );

	}

	// 这次我们得到了想要的结果
	// 因为在立即执行函数内部，i的值传给了lockedIndex，并且被锁在内存中
	// 尽管for循环结束后i的值已经改变，但是立即执行函数内部lockedIndex的值并不会改变
	var elems = document.getElementsByTagName( 'a' );

	for ( var i = 0; i < elems.length; i++ ) {

	  (function( lockedInIndex ){

	    elems[ i ].addEventListener( 'click', function(e){
	      e.preventDefault();
	      alert( 'I am link #' + lockedInIndex );
	    }, 'false' );

	  })( i );

	}

	// 你也可以这样，但是毫无疑问上面的代码更具有可读性
	var elems = document.getElementsByTagName( 'a' );

	for ( var i = 0; i < elems.length; i++ ) {

	  elems[ i ].addEventListener( 'click', (function( lockedInIndex ){
	    return function(e){
	      e.preventDefault();
	      alert( 'I am link #' + lockedInIndex );
	    };
	  })( i ), 'false' );

	}	
	```

	其实上面代码的lockedIndex也可以换成i，因为两个i是在不同的作用域里，所以不会互相干扰，但是写成不同的名字更好解释。以上便是立即执行函数+闭包的作用。
	转自 http://web.jobbole.com/82520/

	根据我百度查看，可能还会涉及到惰性行数，在以后补充上。


* variables
	
	关于JavaScript的变量的作用域，有两种。一种是定义在任何函数外面，为全局变量，其作用范围是全局的。另外一种是定义在函数里面，为局部变量，其作用范围是函数内部。

	局部变量：本变量声明的函数内部调用

	全局变量：整个代码中都可以调用的变量

* 闭包

	要理解闭包，首先必须理解Javascript特殊的变量作用域。
	变量的作用域无非就是两种：全局变量和局部变量。
	Javascript语言的特殊之处，就在于函数内部可以直接读取全局变量

	```
	var n=999;
　　function f1(){
　　　　alert(n);
　　}
　　f1(); // 999
	```

	另一方面，在函数外部自然无法读取函数内的局部变量。
	
	```
	function f1(){
　　　　var n=999;
　　}
　　alert(n); // error
	```

	这里有一个地方需要注意，函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明了一个全局变量！

	```
	function f1(){
　　　　n=999;
　　}
　　f1();
　　alert(n); // 999
	```

	出于种种原因，我们有时候需要得到函数内的局部变量。但是，前面已经说过了，正常情况下，这是办不到的，只有通过变通方法才能实现。
	那就是在函数的内部，再定义一个函数。

	```
	function f1(){
　　　　var n=999;
　　　　function f2(){
　　　　　　alert(n); // 999
　　　　}
　　}
	```

	在上面的代码中，函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1就是不可见的。这就是Javascript语言特有的"链式作用域"结构（chain scope），子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立。
	既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！

	```
	function f1(){
　　　　var n=999;
　　　　function f2(){
　　　　　　alert(n); 
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
	```

	各种专业文献上的"闭包"（closure）定义非常抽象，很难看懂。我的理解是，闭包就是能够读取其他函数内部变量的函数。
	由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。
	所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

	闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

	```
	function f1(){
　　　　var n=999;
　　　　nAdd=function(){n+=1}
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　nAdd();
　　result(); // 1000
	```

	在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。
	为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。
	这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。
	

	1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
	2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

	```
	var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};
　　　　}
　　};
　　alert(object.getNameFunc()());
	```

	```
	var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　var that = this;
　　　　　　return function(){
　　　　　　　　return that.name;
　　　　　　};
　　　　}
　　};
　　alert(object.getNameFunc()());
	```

	转自：http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

	两段代码值得深思。。。this 指向的值各不相同，第一段this 指向Windows，第二个是object。

> 充实自己，使自己成为你想成为的人