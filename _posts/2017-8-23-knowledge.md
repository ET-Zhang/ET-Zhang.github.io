---
layout: post
title: knowledge--对象
categories: knowledge
description: Knowledge changes destiny
keywords: knowledge, javaScript
---

> 创建对象
  
  var arr = [a, b, c]

  原型链：

  arr ---> Array.prototype ---> Object.prototype ---> null

  prototype  >  原型

  prototype 属性可以向对象添加属性和方法

  Object.prototype.name = vale 

  函数也是对象

  function foo () {
    return 0;
  }

  foo ---> Function.prototype ---> Object.prototype ---> null 

> 构造函数

  定义一个构造函数

  function people(name) {
    this.name = name;
    this.hello = function () {
      alert('hello' + this.name);
    }
  }

  var xiaoming = new people('小名');
  xiaoming.name;       // 小名
  xiaoming.hello();    // hello 小名

  共享函数

  people.prototype.hello = function () {
    alert('hello' + this.name);
  }