---
layout:     post
title:      SQL连接错误
subtitle:   总有时会忽略这些
date:       2020-08-27
author:     Autuan.Yu
header-img: https://i.loli.net/2020/08/27/jFXCrMgxzHWGpcP.jpg
catalog: true
tags:
    - SQL
    - MyBatis
---

> 所谓完美，不是指不能再添加别的东西了，而是指没有东西可以从其中拿掉了。

## 简介
The server time zone value ' й   ׼ʱ  ' is unrecognized or represents more than one time zone.

## 解决
在连接字符串后面加上?serverTimezone=UTC  

## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
