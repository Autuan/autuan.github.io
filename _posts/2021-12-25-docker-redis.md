---
layout:     post
title:      docker 安装 redis
subtitle:   docker 安装 redis
date:       2021-12-25
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - docker
---

> 学如逆水行舟，不进则退。

docker 是市面上常用的一种容器服务，使用它可以很方便的帮我们对linux服务器上的服务进行管理。本文不对docker进行过多描述。

前提你已经安装好docker，服务器docer安装本文不描述。 

#### 1 搜索 redis 服务的版本
````
docker search redis
````

#### 2 拉取镜像
````
docker pull redis:latest
````
如果对版本号无要求，直接安装最新的便可；如果有要求，将`latest`替换成第1步查到的版本号，此处略

### 3 启动容器
````
docker run -d -p 6379:6379 --name redis redis --appendonly yes --requirepass autuan
````
参数说明
| 名称 | 作用 |
| --- | --- |
| -d | 常驻后台 |
| -p | 端口绑定,此处将容器6379端口绑定到主机6379端口上 |
| --name | 为服务起一个好记的名字，这里还用`redis`的名称 |
| --appendonly | redis 配置持久化配置，如果不做持久化可以关闭 |
| --requirepass | redis 密码，此处设置密码为'autuan' |

特别注意：  
如果你的redis 是公网对外暴露的，一定要设置一个比较复杂的密码，不然服务器很容易被黑。  

### 4 检查容器
正常情况下，第3步操作完之后redis就已经启动并可以使用了。  

如果还是不放心，可以通过这条命令看一下：  
````
docker ps
````

