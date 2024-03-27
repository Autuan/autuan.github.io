---
layout:     post
title:      JetBrains系列IDE配置缓存日志目录
subtitle:   JetBrains系列IDE配置缓存日志目录
date:       2024-03-27
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - JetBrains
---

在 windows 平台，idea 默认会存放大量数据到C盘。  

然后过一段时间，就会痛苦地发现C盘报红，需要清理。  

而Idea则占用了超大的空间 T-T  

本文告诉大家如何修改 idea 的默认数据放置内容。  

当然，不仅是idea , 只要是同一框架打造的IDE, 如 `WebStorm`、`DataGrip` 都可以通过这种形式进行配置。  

右键我们桌面上idea的图标  

选择 `属性`，然后打开文件位置  

一般情况下，我们会进入 bin 目录  

在同目录下，我们找到 `idea.properties` 这个文件，并打开编辑  

在文件头部，我们可以看到相关的注释内容。 主要是如下三个：  

- user.home
- idea.config.path
- idea.system.path

我们将这三行取注并配置我们想要的路径便可，例：

```properties
user.home=E:/jetbrains/user_home
idea.config.path=E:/jetbrains/config
idea.system.path=E:/jetbrains/system

```

注意事项：
1. 这三个配置需要是不同的目录
2. 在 windows 平台下，我们要使用`/` ,不能使用 `\`

修改完成后，保存重启便可。  

例：
```
user.home=E:/jetbrains/user_home
idea.config.path=E:/jetbrains/config
idea.system.path=E:/jetbrains/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the settings directory.
#---------------------------------------------------------------------
idea.config.path=${user.home}/.IntelliJIdea/config

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the caches directory.
#---------------------------------------------------------------------
idea.system.path=${user.home}/.IntelliJIdea/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the user-installed plugins directory.
#---------------------------------------------------------------------
idea.plugins.path=${idea.config.path}/plugins

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the logs directory.
#---------------------------------------------------------------------
idea.log.path=${idea.system.path}/log
```

如果你启动的时候报了如下的类似错误：  

>  java.lang.IllegalArgumentException: 'config' and 'system' paths should point to different directories
    

说明你把某些配置配成了相同的目录，修改后重启便可。   


