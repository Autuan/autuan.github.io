---
layout:     post
title:      Redission 设置过期时间的一个注意点
subtitle:   有时，一个小小的细节，总让人摸不着头脑
date:       2021-02-19
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - Redis
---

> 细节决定成败

## 简介
使用redission ， 设置过期时间时，发现过期时间总是 -1,而不是我所设置的一天 :
````
obj.expire(1, TimeUnit.DAYS);
````

## 原因
如果 obj 为空的话，为此对象设置对象是失败的。  

事实上，  expire 方法是有返回值的： 

````
// obj is null
RList obj = redission.getList("null key");
boolean isObjSetExpire = obj.expire(1,TimeUnit.DAYS);
// isObjSetExpire = false

// obj2 is not null
RList obj2 = redission.getList("has val");
boolean isObj2SetExpire = obj2.expire(1,TimeUnit.DAYS);
// isObj2SetExpire = true
````


## 解决
在设置过期时间之前进行一次判空便可：

````
boolean isNotEmpty = MyObjectUtil.isNotEmpty(obj);
if(isNotEmpty){
  obj.expire(1,TimeUnit.DAYS);
}
````

问题解决！  

## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
