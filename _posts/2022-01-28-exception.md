---
layout:     post
title:      Consider defining a bean of type 'top.autuan.example.mapper.MemberMapper' in your configuration.
subtitle:   Mapper中的bean不在Spring管理类中
date:       2022-01-28
author:     Autuan.Yu
header-img: https://cdn.colorhub.me/5pO4fIZ7A2WlXhb-Wiqk9nsAlV-i-Y2kgdOhlSX2HPg/rs:auto:0:500:0/g:ce/bG9jYWw6Ly8vODEv/MzQvMzc2NGJlMjE5/MTBhODQyOWJlYjdh/N2E0ZjMzYzU4NDE2/ZWUyODEzNC5qcGVn.jpg
catalog: true
tags:
    - Thymeleaf
    - 错误记录
---
> 正确的结果，是从大量错误中得出来的；没有大量错误作台阶，也就登不上最后正确结果的高座。

## 错误描述
```` 
Action:
Consider defining a bean of type 'top.autuan.example.mapper.MemberMapper' in your configuration. 
````  

## 解决    
检查启动类 `@MapperScan` 或配置文件中的 `Mapper` 扫描，位置 `top.autuan.example.mapper` 包位置是否正确。
