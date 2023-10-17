---
layout:     post
title:      IntellijIdea更换版本报错Error Could not find or load main class top.autuan.demo.DemoApplication
subtitle:   IntellijIdea更换版本报错Error Could not find or load main class top.autuan.demo.DemoApplication
date:       2023-10-16
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - JetBrains
---


# 问题描述
在实际开发中，我们有可能会因为种种原因回退我们的IDE版本（比如因为插件的支持版本、授权版本等原因）

而我们在新版本正常开发维护的项目回到旧版本启动时却无法正常启动，报错：

```
Error: Could not find or load main class top.autuan.demo.DemoApplication
```

# 解决方案
1. 关闭 idea
2. 删除 .idea 文件
    打开资源管理器，找到项目目录，删除 `.idea` 目录，如果你没有在项目目录中看到这个文件夹，先检查资源管理器中`显示隐藏的文件夹`选项是否开启
3. 修改 SDK 版本（如果必要）
    使用idea重新打开此项目，加载完成后进入项目配置，将SDK版本配置为此项目需要的SDK版本
4. 重新启动 错误解决
