---
layout:     post
title:      SQLException Incorrect string value
subtitle:   mysql字符集的常见问题
date:       2020-08-10
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - SQL
    - MyBatis
---

> 所谓完美，不是指不能再添加别的东西了，而是指没有东西可以从其中拿掉了。

## 简介
java.sql.SQLException: Incorrect string value: '\xE5\x90\x8D\xE7\xA7\xB0' for column 'name' at row 1
; uncategorized SQLException; SQL state [HY000]; error code [1366]; Incorrect string value: '\xE5\x90\x8D\xE7\xA7\xB0' for column 'name' at row 1; nested exception is java.sql.SQLException: Incorrect string value: '\xE5\x90\x8D\xE7\xA7\xB0' for column 'name' at row 1

## 解决
这个问题是由数据库编码问题引起的.

如果提示的字符,有3个字节长度,如:'\xE5',那么检查数据库字符集格式是否是latin1,修改为`utf8`就可以了
如果提示的字符,有3个字节长度,如:'\xE53',那么需要将数据库字符集格式修改为`utf8m64`可以解决

## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
