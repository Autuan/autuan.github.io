---
layout:     post
title:      linux安装tomcat环境
subtitle:   
date:       2020-12-09
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - linux
---

> 机会是留给有准备的人

## 安装

1.  执行以下命令，下载Tomcat压缩包。
```` 
wget https://mirror.bit.edu.cn/apache/tomcat/tomcat-10/v10.0.0/bin/apache-tomcat-10.0.0.tar.gz
````
如果不能下载，可以尝试下列源：  
````
https://mirrors.cnnic.cn/apache/tomcat/tomcat-10/v10.0.2/bin/apache-tomcat-10.0.2.tar.gz
````

2.  执行以下命令，解压刚刚下载Tomcat包。
```` 
tar -zxvf apache-tomcat-10.0.0.tar.gz 
````
3.  修改Tomcat名字
```` 
mv apache-tomcat-10.0.0 /usr/local/tomcat10.0
````
4. 为Tomcat授权
```` 
chmod +x /usr/local/tomcat10.0/bin/*.sh
````
5. 修改Tomcat默认端口号为80
    注：Tomcat默认端口号为8080
```` 
sed -i 's/Connector port="8080"/Connector port="80"/' /usr/local/tomcat10.0/conf/server.xml
````
6. 启动
```` 
/usr/local/tomcat10.0/bin/./startup.sh
````
7. 退出数据库
```` 
exit
````

8. 访问

   输入公网IP + 配置的端口号，访问页面

   



## 参考  

[1]阿里云. https://cn.aliyun.com/  
