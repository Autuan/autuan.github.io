---
layout:     post
title:      分布式配置中心的优点及常见分布式配置中心选型
subtitle:   数数看,你认识几个?
date:       2020-09-05
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Thymeleaf
---
> 成人们对数字情有独钟。如果你为他们介绍一个朋友，他们从不会问你“他的嗓子怎么样？他爱玩什么游戏？他会采集蝴蝶标本嘛？”而是问“他几岁了？有多少个兄弟？体重多少？他的父亲挣多少钱？”他们认为知道了这些，就了解了这个人。

## 简介
这几天和其他人沟通,聊到了分布式相关的知识.发现这方便博主还是比较短板的,所以请上网搜索了一些资料.  

一方面希望能够加深印象,另一方面也希望能够帮助到其他人.  

## 为什么要使用分布式配置中心?  

* 方便  
  在集群情况下,如果不使用分布式配置中心,那么当我们需要修改一下公共配置时,需要逐一修改每个单机的配置文件,非常繁琐.  
* 实时  
  通过分布时配置中心,我们可以实时地修改配置值并生效,不需重启服务.  
* 安全
  通过配置中心,我们可以通过身份角色等安全措施,防止配置信息值外泄.  

## 常见的分布式配置中心有哪些?  

| 名称 | 面世时间 | 所属公司/组织 | 开发语言 | 备注 |
|:---:|:---:|:---:|:---:|:---:|
|[zookeeper](https://zookeeper.apache.org/)| - | apache | Java | 老牌子,不说会用至少见过这个名字 |
|Diamond|2011|阿里巴巴|Java|已停止更新|
|[Disconf](https://disconf.readthedocs.io/zh_CN/latest/)|2015|百度|Java|..|
|[Apollo](https://github.com/ctripcorp/apollo)|2016|捷程|Java|国内强势|
|[Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/reference/html/)|-|Spring|Java|Spring家族,中文站在[这里](https://www.springcloud.cc/spring-cloud-config.html)|
|[config-toolkit](https://github.com/dangdangdotcom/config-toolkit)|2014|当当|Java|-|
|[QConf](https://github.com/Qihoo360/QConf)|2014|360|C++|-|
|Lion|2013|点评|Java|-|
|[Nacos](https://nacos.io/zh-cn/index.html)|2018|阿里巴巴|Java|-|

怎么样 ? 是不是超过意料的多?  

因为分布式配置中心作为一个大型项目必用的产品,不同公司的不同产品,可能会有不同的需求.  

这也直接导致了百花齐放的配置中心轮子.  

当然实际使用中,我们只要使用任一即可.  

博主个人比较推荐 `ZooKeeper` 和 `Nacos` 两款,因为这两个不仅可以作为配置中心,也可以作为注册中心.

其次则是 `Spring Cloud Config` 和 `Apollo`,这两款都经受了市场和时间的考验,是比较安心的选择.  

其他几款截止此文发布时,博主未深入了解,不敢造次,若有其他小伙伴了解,欢迎赐教. ❤
