---
layout:     post
title:      Thymeleaf中在fragment中的js中使用变量
subtitle:   js调取字符串变量
date:       2020-07-15
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Thymeleaf
---

> 有些烦恼是我们凭空虚构的，而我们却把它当成真实去承受.

## 简介
在一些[网址](https://www.jianshu.com/p/89e1b4b2d66f)上关于thymeleaf 中调取变量的描述如下:
````JavaScript
 var message=[[${message}]];
````
但事实上,如果`message`变量是字符串的话(如  String messag = "msg";),在实际使用中,会报异常如下

Uncaught ReferenceError: msg is not defined

所以如果要让 JS 正确使用变量的话,需要如下方法

```` JavaScript
 var message= '[[${message}]]';
````

## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
