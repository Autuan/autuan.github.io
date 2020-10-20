---
layout:     post
title:      layer.open缓存导致页面数据不更新
subtitle:   一个容易让人忽略的地方
date:       2020-01-10
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Git
---

> 坚持,不是说从未中断过,而是说,现在依旧在做

## 前言
最近在一个比较老的项目中碰到了由layui引起的缓存问题,写在这里,因为能给其他人一些帮助  

## layer.open的缓存  
使用如下方式打开layer的窗口:
```JavaScript
layer.open({
    type: 2,
    //弹窗标题
    title: 'title',
    //访问路径
    content: '/content/url',
});
```

如果没有其他特别的操作的话,打开`'/content/url'`时,只有第一次会从后台加载页面,后面的打开都会直接从layer的缓存中打开,不再请求后台  

这种方式加快了页面的打开速度,但如果`'/content/url'`上有需要实时从后台返回的数据的话,就不能这样用了.  

这时,我们便需要禁用layer.open的缓存策略

## 清除缓存
最开始,我通过layer的[官方文档](https://www.layui.com/doc/),直接在layer中的config中进行配置:  
```JavaScript
parent.layer.config({
   version: true
});
```
但是在项目中没有什么效果,也许是版本的问题吧? 我并没有深究  

既然官方的方式不好用,那就只能用我们的土方法了.  

看了一下layer的缓存机制,在layer.open中,它是以`content`作为键来缓存页面信息的. 那么我们只需要将原本固定内容的`content`改为动态内容就可以了.  

如下:  
```JavaScript
let content = '/content/url'
let random = Math.ceil(Math.random()*1000);
content += '?random='+random;
layer.open({
    type: 2,
    //弹窗标题
    title: 'title',
    //访问路径
    content: content,
});
```
给定一个随机参数来使每次调用layer.open时,`content`的内容都不一致.  

随机数的取值范围是1000以内,已经满足我这边的使用需求了.  

因为是随机数,也有可能两次random值是一致的,如果有这个需求的话,建议使用 `new Date()`来避免random值随机一致的情况.  

问题得到解决,撒花~~~~~

希望能帮到各位~~~

## 参考文档
[1] Layui.Layui开发使用文档. https://www.layui.com/doc/ .
