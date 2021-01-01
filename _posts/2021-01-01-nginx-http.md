---
layout:     post
title:      使用nginx代理http子域名
subtitle:   nginx反向代理的使用
date:       2021-01-01
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - linux
    - nginx
---

>  江南腊尽，早梅花开后，分付新春与垂柳

## 启程
首先，我们有一个项目绑定到了服务器的 8081 端口:

![8081端口](https://i.loli.net/2021/01/01/YlbZv9BwOPd3hjT.png)

输入端口号访问项目实在是太令人不满了，我希望能够直接通过一个子域名来访问：

比如说 ： `demo.autuan.top`

那么，就需要通过 nginx 来进行反向代理我们的服务了。

## nginx
要使用 nginx ,我们得先确认服务器上有nginx.  
安装 nginx 的教程博主之前已经有写，这里不再重复，不明白的小伙伴可以[访问这里](http://autuan.top/2020/12/25/linux-nginx/)进行 nginx 的安装  .  

## 修改域名解析
在域名解析处，将我们想要代理的域名解析至服务器所在地址。  

![修改域名解析](https://i.loli.net/2021/01/01/Hsr9pFOJoUD3gCn.png)

## 修改代理设置
### 打开nginx配置文件地址
```` 
cd /usr/local/nginx/conf
````
### 使用vim命令开始修改
````
vim nginx.conf
````
### 添加配置值
新增一条 server 记录 :
````
server {
        listen 80;
        server_name demo.autuan.top;

        location / {
            proxy_pass http://127.0.0.1:8081/;
        }
    }
```` 

### 重启nginx 服务
```` 
/usr/local/nginx/sbin/nginx -s reload
````    

### 访问
大功告成！  

![代理成功](https://i.loli.net/2021/01/01/xf6U8Rks7E9KcT3.png)
