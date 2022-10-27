---
layout:     post
title:      MySql检查数据库中所有同名的字段
subtitle:   MySql检查数据库中所有同名的字段
date:       2022-10-27
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - MySql
    - SQL
---
MySql通常我们使用外键时，并不创建外键索引。

时间久了，我们需要检查在一个数据库中，同样的字段名用到了多少个地方。

sql 如下 ：

```` sql

SELECT  COLUMN_NAME,TABLE_NAME  FROM information_schema.COLUMNS
where TABLE_SCHEMA = (select database())
AND COLUMN_NAME = 'search_column_name';

````
