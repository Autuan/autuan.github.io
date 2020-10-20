---
layout:     post
title:      MySql:修改数据库列的修改时赋值
subtitle:   标题有点拗口呢~~~~~~
date:       2019-11-29
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Sql
    - MySql
---

> 各种各样性格的人都有可能成功，只不过是看你有没有利用自己的性格优势来做事情。

假设数据库在建立的时候设置错误
```` Sql
`create_time` datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '创建时间',
````

这样子的话,创建时间在每次数据修改的时候都会被更新,非常不符合我们最初的设计要求

这个时候,可以通过下列SQL将更新赋值取消:
````Sql
ALTER TABLE table_name CHANGE create_time create_time datetime NOT NULL DEFAULT CURRENT_TIMESTAMP;
````

有任何问题可以留言哦  :-)
