---
layout:     post
title:      前端Get请求时间报错endTime Failed to convert property value of type 'java.lang.String' to required type 'java.time.LocalDateTime' for property 'endTime'
subtitle:   使用注解形式解决ConversionFailedException
date:       2023-06-05
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2016/03/23/12/53/clock-1274699_640.jpg
catalog: true
tags:
    - SpringBoot
    - 错误记录
---

# 问题描述
使用Get请求时请求时，路径&参数如下：
http://localhost:8080/meber/query?startTime=2023-05-07T09:21:50.830Z&endTime=2023-06-05T09:21:50.830Z

报错如下：
```
endTime Failed to convert property value of type 'java.lang.String' to required type 'java.time.LocalDateTime' for property 'endTime'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [@io.swagger.annotations.ApiModelProperty java.time.LocalDateTime] for value '2023-06-05T09:21:50.830Z'; nested exception is java.lang.IllegalArgumentException: Parse attempt failed for value [2023-06-05T09:21:50.830Z]
```

# 解决方式：
使用`@DateTimeFormat` 注解来解决

``` java
public class MemberQueryParam {  
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME)  
    private LocalDateTime orderStartTime;  
      
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME)  
    private LocalDateTime orderEndTime;  

    // ... 省略其他属性和方法
}

```
