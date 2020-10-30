---
layout:     post                    
title:      取消博客的fork              
subtitle:   欢迎建立友情链接 
date:       2019-8-16              
author:     Autuan.Yu
header-img: img/post-bg-2015.jpg    
catalog: true                      
tags:                               
    - 博客搭建
---

> 我们对采摘不到的葡萄，不但想象它酸，也很可能想象它是分外地甜。
## 前言
如果通过本博客的教程建立过基于GithubPages的个人博客，相信各位都已经拥有自己的博客，没事可以在上面写点文章，如果有其他人看到，也可以在基于issue的条件下评论。    
但相信也有一些小伙伴注意到了：既然是基于Github 仓库的更新，为什么在我们的GitHub贡献图上，看不到我们的博客记录呢？
![空空如也的贡献图 T-T](https://i.loli.net/2020/10/30/UJ4NEdRmQiCt2ly.png)
## 不更新的贡献图的原因

## 解决方案
既然知道了原因，那么解决方案也呼之欲出了：我们把这个仓库改成自己的不就好了？  

改为我们自己的项目有很多种，比如说：把项目clone然后直接上传到新项目中。  
在本文中，使用的是 template 方案，都大同小异,如果小伙伴们打算直接重传项目，我相信本文也可以做个参考。 

### 修改  io 名称
进入 github上fork的博客项目地址，并打开 ` setting ` 选项卡。

首先是每一栏：修改本仓库的名称，io 是github用来区别GithubPage的一处位置，我们把名字改掉，博主这边是直接添加了后缀名： `remove.github.io.fork`  
![仓库名称](https://i.loli.net/2020/10/30/dZu9FvCVK6lOcEr.png)  


### 改为template
在项目名 template 选项，将此项目作为template项目。
![修改template](https://i.loli.net/2020/10/30/26y5ZAqdVbQRXwW.png)


### 删除老项目的域名
如果你没有为自己的项目设置单独域名，可以跳过此步
#### 设置中删除 
向下拉，找到 Github Page的设置，将这些开关关闭：
![GithubPage 关闭](https://i.loli.net/2020/10/30/xqlh4Kjcz6iHOL5.png)

#### 或者删除CNAME
如果不想在这里设置，也可以直接删除仓库里的CNAME文件，将文件内容全部删除也可以  
![CNAME文件](https://i.loli.net/2020/10/30/7gjRupvsoAiD5Pt.png)

### 创建新项目
从template创建新项目，项目名就是之前的仓库名，博主这里是 `autuan.github.io`。

### 域名重新定义
如果你没有为自己的项目设置单独域名，可以跳过此步。  

打开仓库中的CNAME文件，修改其中的映射值为你的域名。
![CNAME文件](https://i.loli.net/2020/10/30/7gjRupvsoAiD5Pt.png)

如果你的域名没有变化，也是要改一下再改回来的：
博主就是   `autuan.top` -> `www.autuan.top` -> `autuan.top`  修改一遍
还有，修改完成后不要忘记提交哦

## 再发布文章
修改完成后，再上传一篇博客，查看你的贡献图吧！  
