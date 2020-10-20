---
layout:     post                    
title:      面试常见问题:maven的常见面试问题
subtitle:   maven也是面试常问的知识点呢
date:       2019-09-07           
author:     Autuan.Yu
header-img: img/post-bg-2015.jpg    
catalog: true                      
tags:                               
    - maven
    - 面试问题
---

> 有备无患

## 题目

### maven的常见依赖范围有哪些  
compile:编译依赖，默认的依赖方式，在编译（编译项目和编译测试用例），运行测试用例，运行（项目实际运行）三个阶段都有效，典型地有spring-core等jar。
test:测试依赖，只在编译测试用例和运行测试用例有效，典型地有JUnit。
provided:对于编译和测试有效，不会打包进发布包中，典型的例子为servlet-api,一般的web工程运行时都使用容器的servlet-api。
runtime:只在运行测试用例和实际运行时有效，典型地是jdbc驱动jar包。
system: 不从maven仓库获取该jar,而是通过systemPath指定该jar的路径。
import: 用于一个dependencyManagement对另一个dependencyManagement的继承

### maven常用的配置文件有哪些
config.xml : 配置中央仓库地址以及本地仓库位置
pom.xml : 配置项目中所使用的依赖

## P.S.
网络上关于maven的常见面试问题已经有很多了,我在博客上也写这些内容并非简单地重复造轮子,我会不定期更新,希望能把我的博客内容更新成精品

## 更新记录
2019.07  新建文章并添加一道面试题
2020.09 增加一道面试题
