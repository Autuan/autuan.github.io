---
layout:     post
title:      linux安装nginx
subtitle:   
date:       2020-12-25
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - linux
---

> 机会是留给有准备的人

## 安装

###  安装Nginx运行所需要的插件。

####  安装gcc
gcc是Linux下的编译器，它可以编译C、C++、Ada、Object C和Java等语言。
````
yum -y install gcc
````
#### 安装pcre
pcre是一个perl库，Nginx的HTTP模块使用pcre来解析正则表达式。

````
yum install -y pcre pcre-devel
````
#### 安装zlib
zlib是一个文件压缩和解压缩的库，Nginx使用zlib对HTTP数据包进行gzip压缩和解压。
````
yum install -y zlib zlib-devel
````

###  下载Nginx安装包。
````
wget http://nginx.org/download/nginx-1.17.10.tar.gz
````
###  解压Nginx安装包。
````
tar -zxvf nginx-1.17.10.tar.gz
````

###  编译安装Nginx
````
cd nginx-1.17.10
./configure
make && make install
````
### 启动Nginx。
````
cd /usr/local/nginx/
sbin/nginx
````
### 测试Nginx启动
在浏览器地址栏输入IP,显示nginx提示则成功


## 参考  
[1]阿里云. https://cn.aliyun.com/  
