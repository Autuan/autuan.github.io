---
layout:     post
title:      SpringBoot外配配置文件
subtitle:   在项目外存放配置文件
date:       2020-11-25
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Java
    - SpringBoot
---

>  十月江南天气好，可怜冬景似春华

## 简介
众所周知，SpringBoot 的配置文件默认在`src/main/resource/` 目录下。  

但在实际使用时，我们往往不愿意将真实的配置文件放在这里：  

放在此处的配置文件会随着版本控制工具一同更新。  

往往会有一些配置信息会因为环境的不同而需要分别配置：特别是在开发环境上  

所以将配置信息外配是我们常见的一种需求。


## IDE 配置
此处，以 Intellij Idea 为例：

	1. 打开 Run/Debug Configurations 窗口
	2. 找到该项目的启动信息
	3. 在 Environment 下的 VM options 中填入下列信息： -Dspring.config.location=D:\config\application.yml 

这样子，在启动时，就会以 D 盘 config 目录下的 application.yml 作为配置文件

## Jar 包启动
SpringBoot 在单机部署往往使用jar包部署：可以在 jar 命令启动时设置启动参数：

```` 
 java -jar -Dspring.config.location=D:/config/application.yml project.jar
````

即可

## 参考  
[1]阿里云. https://cn.aliyun.com/  
