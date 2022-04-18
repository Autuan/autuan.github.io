---
layout:     post
title:      GenericObjectPoolConfig Exception
subtitle:   ClassNotFoundException org.apache.commons.pool2.impl.GenericObjectPoolConfig
date:       2022-04-18
author:     Autuan.Yu
header-img: https://i0.hippopx.com/photos/1017/475/750/image-picture-color-aberration-color-error-preview.jpg
catalog: true
tags:
    - maven
    - SpringBoot
---

## 问题
ClassNotFoundException: org.apache.commons.pool2.impl.GenericObjectPoolConfig

## 环境
spring-boot-starter-parent  2.5.8

## 解决
添加以下依赖
```` xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
````
