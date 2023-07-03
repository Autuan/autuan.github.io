
---
layout:     post
title:      IntellijIdea关闭项目自动重启
subtitle:   Edit Configuration Setting 配置 On frame deactivation
date:       2023-07-03
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - JetBrains
---
> 落红不是无情物，化作春泥更护花


# 前言
自打博主更新了JetBrains之后，惊奇的发现只要修改了源码之后，Intellij Idea 就会很`贴心`的为我重新启动  

自动化当然好了，可是有些项目在开发环境没必要这么频繁的重启的呀  

我只不过加了两行注释，整个服务就重启了诶  (╯‵□′)╯︵┻━┻  

下面是关闭自动重启的方法

博主使用的版本是
# 关闭


编辑项目的启动命令，博主示范的为一个单体项目:
![编辑启动配置](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/on-frame-deactivation/070301.png)

在弹出的 Edit Configuration Settings 中，会看到如下配置：  
On frame deactivation : Update classes and resources  

![修改自动更新](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/on-frame-deactivation/070302.png)
点击旁边的 `x` 或者在下拉菜单中选择 `Do nothing` 即可，当然，你也可以根据自己的需要选择 `Update Resources`


# 恢复
如果有一天，你又想要这个功能了，不要害怕找不到

还是在 Edit Configuration Settings ,在 Modify options 中找到 On frame deactivation 并选中你想要执行的操作便可以了

![编辑启动配置](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/on-frame-deactivation/070303.png)

