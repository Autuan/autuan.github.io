---
layout:     post
title:      Postman简单的介绍与使用
subtitle:   总要有个东西发发网络请求吧?
date:       2019-8-23
author:     Autuan.Yu
header-img: https://pixabay.com/zh/photos/%E9%82%AE%E6%94%BF%E4%BF%A1-%E7%95%99%E8%A8%80%E4%BF%A1%E7%AE%B1-%E4%BF%A1%E7%9B%92-%E4%BF%A1%E4%BB%B6-2828146/
catalog: true
tags:
    - Postman
---

> 女人和小孩可以粗心大意,但男人不行

## 前言
如果写完了代码不进行测试,那么,这个代码是不能说已经**完成** 了,只有当测试也通过了,才能说这个功能块完成了,本文主要简单介绍一下Postman

## 使用什么工具
作为一名Web开发,当接口写完之后,还有一次黑盒测试,或许有些程序员因为种种原因(懒) 没有写单元测试,但是接口测试一定是免不了的

依稀记得,我刚开发Web的时候,进行接口测试的方法就是打开我电脑上的火狐,然后把参数一拼,就直接在浏览器里看响应结果了,那时,真的是年少无知啊,连接口要加个签名都不知道, 还觉得自己已经超牛了,真是初生牛犊不怕虎啊

当然,我也不是因为什么签名方式而不使用浏览器了,只是因为第二天,我学到了Post请求┑(￣Д ￣)┍

后来我就走上了~女装~使用工具的不归路

1. 浏览器
很多人谈到接口测试工具的时候,总是会忽略浏览器,确实,它也不适合做接口测试.  
 博主也不推荐使用浏览器进行测试.  
 写在这里的主要原因是希望如果有萌新还在使用浏览器,然后又看见了这篇文章,希望尽快从浏览器测试的苦海中脱离出来  :-)
2. Postman
一个功能强大但又直观易懂的软件,博主很喜欢用这个软件,因为不需要配置很多东西,我做事向来怕麻烦,而postman,就满足了我这个需求
3. Jmeter
一个真正意义上的接口测试工具,和Jmeter比直接来,Postman就像过家家一样.
完全使用Java开发,要什么功能有什么功能,只有你想不到,没有测不了,就是这么强
不过也正是因为其专业性,如果不是要走测试或者全栈开发的,博主认为没有必要学这个
如果对JMeter感兴趣的,可以去[官网](https://jmeter.apache.org/)下载


## Postman
### 下载
首页[点此链接](https://www.getpostman.com/downloads/)下载Postman,并安装到本机,这一步相信每位玩过电脑的同学都了解这件事儿,不需要特意截图说明

### 界面简介
![基本界面介绍](https://i.loli.net/2019/08/27/cVHSonYRskp2DUz.png)  

### 使用
首页,我们先简单地创建一个可以响应数据请求的接口  

请求实体:  
![请求实体](https://i.loli.net/2019/08/27/j46cGnqa1HMRoNJ.png)  

响应实体:  
![响应实体](https://i.loli.net/2019/08/27/NTaRPKoQrdqVFZe.png)  

接口:  
![简单的接口](https://i.loli.net/2019/08/27/uVRpcZA1TSOsh7y.png)  

写完这些定义之后就可以发送请求了:  
![发送请求](https://i.loli.net/2019/08/27/ovANb7QBiL4fFsr.png)  

当然只是这种请求,浏览器也是可以请求的:
![浏览器请求](https://i.loli.net/2019/08/27/5hLmJg7QFEv6C3p.png)

但是如果根据REST风格接口改为只能使用Post接口的话  
![只接收post](https://i.loli.net/2019/08/27/vCphrIuzdjl7Yit.png)  

浏览器就会提示405错误  
![405错误](https://i.loli.net/2019/08/27/8CnaxJcDyoep4GY.png)  

通过 Postman工具的话,我们只需要通过一个按钮直接修改请求风格就可以了:  
![改为Post接口](https://i.loli.net/2019/08/27/auGLbd1v2qlmprM.png)  


非常的简单,易懂!  

### 使用环境变量
我们每次开发的时候,有很多接口在很多信息都是一致的:  
比如说这个接口请求地址的前缀
``
localhost:8070/rest
``
,如果每个接口都要写这么一长串的内容,真的是相当占用我们的时间啊,那么有没有比较简单的方法呢?  
答案当然是 :**有!**  
那就是Postman的环境变量  
选择右上角的`设置齿轮`    
![设置](https://i.loli.net/2019/08/27/3HCKPfoaQYFNTJ6.png)  

新增  
![新增](https://i.loli.net/2019/08/27/i6rz1EJyQgHm5nS.png)  

![变量](https://i.loli.net/2019/08/27/D4rRNEzK1OMA7sq.png)  

新增好之后,应用环境变量  
![选择](https://i.loli.net/2019/08/27/dwGHh1pqRPFkvUA.png)  

将请求前缀修改为我们定义好的键,请注意格式为:

``
{{你的键名}}
``  

![改名](https://i.loli.net/2019/08/27/5bSasQMKWqc2FpH.png)

再次发送请求,完美!  

![发送请求](https://i.loli.net/2019/08/27/4aPNxXftUvesAyH.png)

### 保存请求
发送成功后我们要保存我们的请求,这样一来,当下次还需要测试的话,我们就不需要再次输入请求路径和参数了  

点击保存按钮  
![保存](https://i.loli.net/2019/08/27/I1gd8YKMjoW4H9V.png)  
![输入名称及简介](https://i.loli.net/2019/08/27/Q3DNoC4qypnLjPm.png)  

创建集合,此处的集合推荐以项目划分,这样的话,可以和右上角的环境变量有着非常好的配合
![创建集合](https://i.loli.net/2019/08/27/EcPAZ5xbRwNDsvy.png)

集合创建后还可以新增文件夹,此处的文件夹推荐以功能划分,比如会员登录模块,因为博主是介绍功能使用的,就不再创建文件夹了,最后点击右下角的 Save to 便可  
![创建功能模块的文件夹](https://i.loli.net/2019/08/27/Aq7iGpu6tQSHfRm.png)
