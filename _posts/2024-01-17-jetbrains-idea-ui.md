---
layout:     post
title:      Jetbrains idea 老版本切换回老ui不使用newui
subtitle:   idea版本切换时ui未切换问题处理
date:       2024-01-17
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - JetBrains
---

> 人是会犯错，但真要把事情搞得稀巴烂还得靠计算机。——保罗 R 厄尔林奇  

有时试用了新版本idea 后因为种种原因换回 2022.2 或更之前的其他版本时，却发现 ui 还是 new ui.  

new ui 当然不错，但是在没有支持的老版本上会出现各种问题，比如因为适配问题找不到功能了。  

那么要如何切换回原来的ui 呢？  

直接打开`设置`菜单，是看不到 new ui 的相关设置的。  


具体操作步骤如下：  

1. 双击 <keyboard>shift</keyboard> , 唤醒 `Search Everywhere`   
2. 查找并进入  `registry`.  
        ![修改](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/newui/ui_1.png?x-oss-process=style/toWebp)
3. 取消new ui 的相关配置，如果你嫌麻烦，也可以直接点击  `restore defaults` 全部改回默认值便可  
        ![修改](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/newui/ui_2.png?x-oss-process=style/toWebp)


    
