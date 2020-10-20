---
layout:     post
title:      快速部署项目:alibaba cloud toolkit简单使用说明
subtitle:   人最大的优点就是懒
date:       2019-09-20
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2015/07/28/20/55/tools-864983__340.jpg
catalog: true
tags:
    - 博客搭建
    - Postman
---

> 软件供应商在努力尝试让他们的软件更'易于操作'…迄今为止，他们最好的办法就是翻出所有的老手册，然后在封面盖上'易于操作'这几个字。

## 前言
我们开发程序时,会需要将我们的Web程序部署到测试/线上服务器上,而部署项目,如果有专门的运维还好,如果没有运维,估计是很多小伙伴非常头痛的一件事,今天为大家推荐一个插件,可以快速帮我们部署项目到服务器上

## Cloud Toolkit
[Cloud Toolkit](https://cn.aliyun.com/product/cloudtoolkit)是本地 IDE 插件，帮助开发者更高效地开发、测试、诊断并部署应用。  

下面跟着博主一步步来使用这个效率插件吧!
本文以Intellij Idea作为示范,使用 Eclipse 开发程序的伙伴们请前往官网查询教程
### 升级
Cloud Toolkit 对 Intellij Idea 的版本支持为最低 **2018.1**,已为此版本或更高版本的小伙伴请跳过此步:
打开[idea 下载网站](https://www.jetbrains.com/idea/download/?fromIDE=#section=windows)下载最新的安装包进行安装

### 安装
#### 直接安装
打开`设置`,在插件市场中搜索`Alibaba Cloud Toolkit`并安装
![直接安装](https://yqfile.alicdn.com/b2bb78733c0be47a6f610794d1330194756a27d8.png)
#### 本地安装
通过idea plugin网站下载[toolkit插件](https://plugins.jetbrains.com/plugin/11386-alibaba-cloud-toolkit/versions)  

本地安装:
![本地安装](https://i.loli.net/2019/09/19/urZYLg71OyI9eJQ.png)  

安装完成后重启IDE,如果能在工具栏看见tookit的图标,就说明安装成功了:  

![安装成功](https://i.loli.net/2019/09/19/xQaV3zC5p1RekiY.png)

### 配置
如果弹出用户邀请码的话,请填写 `4ZKYXP` 这个码.

添加SSH链接信息
![链接](https://i.loli.net/2019/09/19/VLaMW819BqfriIO.png)

![添加 host 分组](https://i.loli.net/2019/09/19/ruEPR9a3dvn7HkU.png)

![添加HOST信息](https://i.loli.net/2019/09/19/F1aBU7Jtkmz9oLG.png)

![选择上传](https://i.loli.net/2019/09/19/QknIwZyo9du5cSK.png)

![确认上传](https://i.loli.net/2019/09/19/GPgT3tBYbKvVs1M.png)
