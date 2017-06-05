---
layout: post
title: knowledge--闲着无聊
categories: knowledge
description: Knowledge changes destiny
keywords: knowledge, javaScript
---

> 使用DOM 操作添加元素

  1、创建元素

  createElement()
  使用createElement()方法创建一个新的元素节点。（该节点需要保存到变量中）

  2、设置内容

  createTextNode()
  创建一个新的文本节点（同理需要保到变量中）

  3、添加到DOM中

  appendChild()
  使用这个方法将其添加到DOM树中

  ---

  var newEl = document.createElement('li');		// 1

  var newText = document.createTextNode('提莫')		//2

  newEl.appendChild(newText) 		//3    <li>提莫</li>

  var k = document.getElementById("xxx");

  k.appendChild(newEl) 				// 3


  

