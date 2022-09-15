---
layout:     post
title:      docker安装RabbitMq环境
subtitle:   
date:       2022-09-15
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - RabbitMq
    - docker
---

> 君子生非异也，善假于物也。

### 安装
````
docker search rabbitmq

docker pull rabbitmq
````
### 运行
````
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq
````

如果是在服务器上，记得开放端口。  

到这一步，我们就已经成功启动了 RabbitMq ,服务端口是 5672  

下面，让我们启动图形化管理界面：  

````
// 查看部署的mq容器id
docker ps -a

// 进入 RabbitMq 容器
docker exec -it 容器id /bin/bash

// 执行命令
rabbitmq-plugins enable rabbitmq_management
````
执行完成之后可以通过 ip:15672 访问 RabbitMq 的管理网页


### 修改密码
登录 RabbitMq 的管理页，默认账号及密码是 guest / guest

登录之后，在上方的二级菜单中，选择 `Admin` 选项卡。 

选择后点击页面下方的 `Add User` 新增用户。

最后，删除 guest 用户即可。