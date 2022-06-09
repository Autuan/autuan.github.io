---
layout:     post
title:      SpringBoot中swagger不能用
subtitle:   包含swagger的扩展插件：knife4j
date:       2022-06-09
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - SpringBoot
---

## 问题描述
在 SpringBoot 2.6.x 版本上，swagger-starter 无法正常使用(包含swagger的扩展插件：knife4j).  

springboot 仓库  版本：  
https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent  
版本： 2.6.x 

issue:  
https://github.com/springfox/springfox/issues/3934  

## 快速解决方案
将SpringBoot的版本降级至 2.5.x  
