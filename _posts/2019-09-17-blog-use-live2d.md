---
layout:     post                    
title:      GithubPages个人博客使用live2d看板娘               
subtitle:   图片存哪里好呢?
date:       2019-9-17             
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2019/03/12/20/27/kanban-4051777__340.jpg
catalog: true                      
tags:                               
    - 博客搭建
    - live2d
---

>男人变态有什么错

## 前言
相信很多小伙伴在看其他博客的时候或多或少见过看板娘
![别人家的看板娘](https://i.loli.net/2019/09/18/l17rIfEQSqazYKB.png)
有没有想要自己的博客也要一个呢?本博文主要讲解如何在Github Pages上使用此功能

## 鸣谢
本文主要来自[猫与向日葵](https://imjad.cn/archives/lab/add-dynamic-poster-girl-with-live2d-to-your-blog-02)博客,在此感谢!

## 引入依赖
本文章里的依赖在本人的项目中都有使用,你可以直接[download](https://github.com/Autuan/autuan.github.io)本人的项目,从中直接复制你需要的文件

### css样式
首先,我们需要引入看板娘的样式,可以在这里[复制](https://github.com/Autuan/autuan.github.io/blob/master/css/hwaifu.css)
获得css文件中,将它放入项目的`css`文件夹下
### js文件
#### jquery
在 `js` 文件中放入 jquery.js , 这个文件就不提供了,网络上一搜一大堆
#### live2d

live2d.js 是看板娘的核心,可以在这里[复制](https://github.com/Autuan/autuan.github.io/blob/master/js/live2d.js),如果你下载了本人的开源项目,在文件的/js/目录下可以找到
#### waifu-tips
看板娘如果要说话的话,就要引入这个JS,可以在这里[复制](https://github.com/Autuan/autuan.github.io/blob/master/js/waifu-tips.js),如果你下载了本人的开源项目,在文件的/js/目录下可以找到

### json
#### model.json
关于看板娘的配置文件,可以在这里[复制](https://github.com/Autuan/autuan.github.io/blob/master/js/model.json),如果你下载了本人的开源项目,在文件的/js/目录下可以找到
#### waifu-tips.json
关于看板娘提示语的配置文件,可以在这里[复制](https://github.com/Autuan/autuan.github.io/blob/master/js/waifu-tips.json),如果你下载了本人的开源项目,在文件的/js/目录下可以找到


### html 引入
打开项目中的`_includes`文件夹,然后打开`head.html`文件
将下列内容粘贴进去(请注意前后顺序):
```html
<link rel="stylesheet" href="{{ "/css/hwaifu.css" | prepend: site.baseurl }}">

<input type="hidden" id="baseurl" value="{{ site.baseurl }}">
<div class="waifu">
    <div class="waifu-tips"></div>
    <canvas id="live2d" width="280" height="250" class="live2d"></canvas>
</div>

<script async src="{{ site.baseurl }}/js/jquery.min.js"></script>
<script async src="{{ site.baseurl }}/js/waifu-tips.js"></script>
<script src="{{ site.baseurl }}/js/live2d.js"></script>
<script type="text/javascript">
    loadlive2d("live2d","/js/model.json");
</script>
```
## 引入model
前往github上的[开源项目](https://github.com/xiazeyu/live2d-widget-models)中,挑选一个你喜欢的看板娘,本博主使用的是初音  

在`js`目录下新建一个目录,名称为`textures`,用于存在看板娘的服装  

找到你想要的看板娘模型,在`/assets/moc/`中,将.moc文件复制到`/js/`文件目录下,然后将`/assets/moc/{看板娘的名称}`中的png图片复制到 /js/textures 中,改名为`default-costume.png`

## 查看效果
将所有的修改提交到io的开源项目中,然后刷新  
![效果](https://i.loli.net/2019/09/18/Gg2PpfYNS7iDHos.png)  
看板娘已经可以看到了!  

## 后记
本文只是简单地介绍了在GithubPages中使用看板娘,其他功能尚未讲解  

其他诸如`换肤`,`留言板`等功能都未说明,如果有小伙伴想要了解的话,我再看情况更新几篇和live2d有关的内容
