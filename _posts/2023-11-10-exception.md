---
layout:     post                    
title:      uniapp 微信小程序报错 Bad attr data-event-opts with message
subtitle:   for循环对象$0
date:       2023-11-10              
author:     Autuan.Yu
header-img:     
catalog: true                      
tags:                               
    - 错误记录
    - uni-app
---

今天遇到了一个UniApp编译微信小程序的问题：

```
Bad attr `data-event-opts` with message
```

这个问题是因为我在for循环中使用了:key属性，并且用了表达式拼接：

```
<view class="label-section" v-for="(item, index) in list" :key="'item' + index"  
        @tap="doSomething(item, index, 'parent')" >
```

这样的写法会导致编译后的代码中，传入doSomething方法的不是item对象，而是一个字符串$0，就像这样：

{% raw %}
{% comment %} 

```
<view data-event-opts="{{[['tap',[['doSomething',['$0',index,'parent'],
```
 {% endcomment %}
{% endraw %}

解决办法很简单，就是把:key属性的值改成`index`就可以了：

```
:key="index"
```
