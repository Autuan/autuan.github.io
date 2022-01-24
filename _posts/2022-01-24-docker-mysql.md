---
layout:     post
title:      docker安装mysql
subtitle:   docker安装mysql
date:       2022-01-24
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - docker
---

> 学习从来无捷径，循序渐进登高峰。


#### 1 搜索 mysql 服务的版本
````
docker search mysql
````

#### 2 拉取镜像
````
docker pull mysql:latest
````
如果对版本号无要求，直接安装最新的便可；如果有要求，将`latest`替换成第1步查到的版本号，此处略

#### 3 启动容器
````
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=Autuan -d mysql
````
参数说明  


| 名称 | 作用 |
| --- | --- |
| -d | 常驻后台 |
| -p | 端口绑定,此处将容器3306端口绑定到主机3306端口上 |
| --name | 为服务起一个好记的名字，这里还用`mysql`的名称 |
| -e MYSQL_ROOT_PASSWORD | 配置mysql连接密码，此处为 ‘Autuan’ |

 

#### 4 检查容器
正常情况下，第3步操作完之后 mysql 就已经启动并可以使用了。  

如果还是不放心，可以通过这条命令看一下：  
````
docker ps
````

