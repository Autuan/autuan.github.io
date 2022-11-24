---
layout:     post
title:      在distinct的情况下使用PageHelper分页
subtitle:   distinct或distinctrow
date:       2022-11-24
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - MyBatis
---

在 distinct 情况下分页
PageHelper 是 mybatis 相关的知名分页插件，在国内可以说是相当流行。

一般情况下，我们并不需要特别配置或写一些代码即可完成分页的操作。  

但随着业务的开发，我们总归会碰到一些非一般情况。  

有时，业务的需要可能会使我们使用`distinct`或者`distinctrow`来完成去重操作，情况可能类似如下 ： 

```` xml
<select id="listSomeUser" resultMap="someUserMap">
	select distinct user_name,user_avatar
	from sys_user
	where del_flag = 0
</select>
````

假设去重后的数据量是 100 条，如果我们直接使用基于 PageHelper 的分页插件下，可能会发现响应的`total`和这个数量是对不上的：可能是200条或是其他数目。  

其原因是，PageHelper 计算统计时，会替换掉 select 中的内容，生成如下语句

```` sql
select count(0) from sys_user where del_flag = 0
````

知道了原因,那自然就很好改了，事实上 PageHelper 也注意到了可能会有自定义count语句的需求，并提供了[文档]()

我们只需要在 xml 文件里重新提供一个 count 方法即可
```` xml
<select id="listSomeUser_COUNT" resultType="long">
	select COUNT(distinct user_name)
	from sys_user
	where del_flag = 0
</select>
````
- 在xml新增一个select方法,id为原本的查询方法id后加后缀`_COUNT`
- `resultType` 返回 `long`
- interface 可以不提供 count 接口


#### 参考
https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/Changelog.md#504---2017-08-01
