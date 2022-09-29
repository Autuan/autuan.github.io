---
layout:     post
title:      this condition is null
subtitle:   
date:       2022-09-29
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - 错误记录
    - SpringBoot
---

> If the plan doesn't work,change the plan , but never the goal.

## 错误描述

org.springframework.context.ApplicationContextException: Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException: Cannot invoke "org.springframework.web.servlet.mvc.condition.PatternsRequestCondition.getPatterns()" because "this.condition" is null  

## 解决方法
1.  SpringBoot版本降级到 `2.5.8`  

2.  maven 中取消 `spring-aot-maven-plugin` 的依赖
