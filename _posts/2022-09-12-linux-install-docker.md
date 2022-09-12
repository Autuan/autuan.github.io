---
layout:     post
title:      linux安装docker环境
subtitle:   
date:       2022-09-12
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - linux
    - docker
---

> 时不利兮骓不逝。  

### linux 安装 docker

下面来看看 Linux 中如何安装 Docker，这里以 CentOS7 为例。

在测试或开发环境中，Docker 官方为了简化安装流程，提供了一套便捷的安装脚本，执行这个脚本后就会自动地将一切准备工作做好，并且把 Docker 的稳定版本安装在系统中。

````
// 下载脚本
curl -fsSL get.docker.com -o get-docker.sh

// 执行脚本
sh get-docker.sh --mirror Aliyun
````

安装完成后直接启动服务：  
````
systemctl start docker
````

推荐设置开机自启，执行指令：  
````
systemctl enable docker
````

