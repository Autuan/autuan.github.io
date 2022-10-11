---
layout:     post
title:      bean/mapper始终为空
subtitle:   bean/mapper始终为空
date:       2022-10-11
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - 错误记录
    - SpringBoot
---

> If the plan doesn't work,change the plan , but never the goal.

## 错误描述

SpringBoot Mapper注入为空

### 异常代码情况
```` java
@Service
public class DemoServiceImpl implements DemoService {
    @Autowired
    private DemoMapper demoMapper;


    public DemoServiceImpl(){
        // !important  此处 demoMapper 为空
        demoMapper.doingSomething();
    }

}

````



## 解决方法
如果在构造方法中执行了需要bean 的操作，需要通过属性注入的方式注入bean.  

如下：  

```` java
@Service
public class DemoServiceImpl implements DemoService {
    
    private DemoMapper demoMapper;


    public DemoServiceImpl(@Autowired DemoMapper demoMapper){
        this.demoMapper = demoMapper;
        // 正常执行
        demoMapper.doingSomething();
    }

}

````


## 原因
构造器的优先级要比属性注入优先级高，执行构造器方法时，`demoMapper`尚没有注入，故为空。

