---
layout:     post
title:      stream中findAny报null
subtitle:   jdk1.8
date:       2022-05-23
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2021/12/02/18/38/seagulls-6841129_960_720.jpg
catalog: true
tags:
    - Java
---  

在 JDK 1.8 版本中的 Stream 特性中，`findAny`是一个常用的api:

```` java  
list.stream()
    .findAny()
    .orElse(null);
````

但有时会碰到 NPE 异常： 在`findAny`抛出。  

可以在 stream 中增加一层判空来避免：

```` java  
list.stream()
    .filter(Objects::nonNull)
    .findAny()
    .orElse(null);
````
