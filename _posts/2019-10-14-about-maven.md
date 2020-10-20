---
layout:     post
title:      聊一聊 maven
subtitle:   Java Web 程序员都熟悉的工具
date:       2019-10-14
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - maven
---

> 知其然,知其所以然

## 前言
大概每一位JavaEE开发人员都知道有`maven`这个工具,可是,又有多少人能够在空闲的时候,主动去了解关于maven的各个细节呢?  

## 什么是 maven ?  
maven是Java领域一款优秀的构建管理解决方案.   

与 maven 类似的构建管理解决方案还有`Make`,`Ant`等.它们之间的区别如下:  
* Make  
Make使用一个名为Makefile的脚本文件驱动,通过一系列目标和依赖将整个构建过程串联起来,并利用本地命令完成每个目标的实际行为.  
Make的最大长处就是可以利用所有的系统本地命令,从而快速高效地完成任务.  
而它的缺点就是因为调用系统本地命令,从而把自己与操作系统绑定在一起了. 意思就是,使用Make,就很难进行跨平台的项目构建,这对于JaveEE开发者来说,是一个很重要的问题.  
而根据其社区回馈,Makefile的语法也有问题,很容易因为空格而导致构建失败.   
* Ant  
Ant(Another Neat Tool),是因Make的种种缺点,而重新开发的.使用xml格式文件进行配置. 和Ant相比,它已经进行了许多.  
主要的问题在于:  
Ant是过程式的,而Maven是声明式的,很多Maven仅需要声明之后就可以自动执行的功能,在Ant里,需要开发者显示地指明每一个目标.  
此外,Ant需要在其他插件的配合下,才能使用依赖管理,而maven是自带依赖管理的.  
最后一点,maven拥有一个几乎包含全世界所有第三方依赖的中央仓库,这一点对于大多数开发者而言,都是非常重要的.  

## 坐标
我们在使用Mavan的时候,在项目里一定要配置这些内容:  
```xml
<groupId>top.autuan</groupId>
<artifactId>my_project</artifactId>
<packaging>war</packaging>
<version>0.0.1-SNAPSHOT</version>
```
而这些标签(`groupId`/`artifactId`/`packaging`/`version`)就构成了maven的坐标.  

那么定义这些坐标有什么用呢?  

可能很多刚接触maven的小伙伴们觉得这个东西的非常的冗余,但是如果是有接触过多模块开发的小伙伴就知道它的重要性了.  

当你所写的程序需要作为一个依赖,让其他项目使用的时候,其他项目需要知道依赖在哪.  

就像快递员送快递的时候要根据收货地址去送一样.  

而这些坐标,就是maven中的收货地址,正确填写之后,当其他项目需要使用这个项目的时候,就可以根据这个地址(坐标)非常方便地找到并引用.  

## maven 依赖的配置  
写在pom.xml里的maven依赖配置信息,如下:  
``` xml
<!-- 所有对其他项目的依赖,都要写在 dependcies 里面 -->
<dependcies>
  <!-- 每一个依赖项,就是一个 dependcy -->
  <dependcy>
    <groupId>坐标:组ID,必填</groupId>
    <artifactId>坐标:项目名,必填</artifactId>
    <version>坐标:版本号,必填</version>
    <type>依赖类型,默认jar</type>
    <scope>依赖范围</scope>
    <optional>可选条件</optional>
    <exclusions>去除依赖里的指定依赖</exclusions>
  </dependcy>
</dependcies>
```

### maven的依赖范围
导入依赖时,<scope></scope>可以声明maven的依赖范围,很多小伙伴们都不知道scope的真正作用是什么,下面简单地介绍一下scope的各个属性值:  

* compile 全局生效  
compile 是maven的默认依赖范围,如果不显式声明scope,则所有的依赖都是compile,标明compile的依赖表明其依赖服务无论在任何时候,都需要添加至classpath里.   

* test 只在测试时生效  
只有在执行单元测试时,相关依赖会加入到classpath里,在项目运行/编译时,都会忽略掉test依赖.  

* provided 在编译和测试时生效  
比如说spring-boot 项目里的 tomcat ,在编译和测试时需要有spring-boot-starter-tomcat依赖,但是在发布线上时,tomcat已经配置好了,就需要将项目里的Tomcat移除.  

* runtime 运行时生效  
只在项目运行时启用. 比如说数据库操作,只有在项目运行时,才需要具体地JDBC驱动.  

* system 系统依赖  
个人不推荐各位使用这个依赖范围. system 是和本机的操作系统绑定,会对项目的可移植性产生很大的影响.   

* import 导入  
maven 2.0.9及以上版本,导入依赖范围.  

### 可选条件  
有时可能一个项目依赖了两个其他项目,但是这两个项目在其中的特性上是互斥的,就需要可选条件来声明.  

## 依赖传递
我们所引用的依赖项目,或多或少都还会依赖其他项目,而其他项目也有可能又依赖了其他项目.   

为了避免我们引入过多无用的依赖或一个个引用所有依赖, maven使用了依赖传递功能.  

依赖传递是maven的重要功能之一,有了此功能,我们不必再担心引入了多余的依赖,而那些必要的间接依赖,则可以通过依赖传递来引入.  

## 依赖冲突  
如果我们的项目依赖了两个不同的项目,而这两个项目又引入了一个相同项目的不同版本,那么会引入哪个依赖呢?  
### 调解第一原则: 路径最近者优先
同一个项目,依赖路径短的优先.  

### 调解第二原则: 先声明的优先
如果使用第一原则还不能确定使用哪个版本,在maven 2.0.9及之后,会有第二原则作为补充.  

谁先在pom文件里声明,就先用哪一个.  
