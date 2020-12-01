---
layout:     post
title:      The Scheduler cannot be restarted after shutdown() has been called.
subtitle:   使用若依出现的问题
date:       2020-12-01
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - 错误记录
---

> 好好学习，天天向上

## 错误
使用若依框架时，在本地开发环境使用报了如下异常：  

The Scheduler cannot be restarted after shutdown() has been called.

### 控制台打印
```` 
Exception in thread "Quartz Scheduler [RuoyiScheduler]" org.springframework.scheduling.SchedulingException: Could not start Quartz Scheduler after delay; nested exception is org.quartz.SchedulerException: The Scheduler cannot be restarted after shutdown() has been called.
	at org.springframework.scheduling.quartz.SchedulerFactoryBean$1.run(SchedulerFactoryBean.java:753)
Caused by: org.quartz.SchedulerException: The Scheduler cannot be restarted after shutdown() has been called.
	at org.quartz.core.QuartzScheduler.start(QuartzScheduler.java:529)
	at org.quartz.impl.StdScheduler.start(StdScheduler.java:142)
	at org.springframework.scheduling.quartz.SchedulerFactoryBean$1.run(SchedulerFactoryBean.java:750)
````

## 原因
已有一个项目实例在运行

## 解决方法
Windows平台： 
  - 打开任务管理器
  - 找到后台进程中对应的线程
  - 右键->结束任务
  - 重启项目

## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
