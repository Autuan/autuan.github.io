---
layout:     post
title:      linux安装mysql环境
subtitle:   
date:       2020-11-20
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - linux
---

> 机会是留给有准备的人

## 安装

1.  执行以下命令，下载安装mysql
```` 
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm &&
yum -y install mysql57-community-release-el7-10.noarch.rpm &&
yum -y install mysql-community-server
```` 
2.  启动mysql
```` 
systemctl start mysqld.service
```` 
3.  查看初始密码
```` 
grep "password" /var/log/mysqld.log
```` 
4. 登录数据库
```` 
mysql -uroot -p
```` 
5. 修改数据库密码
```` 
set global validate_password_policy=0;  #修改密码安全策略为低（只校验密码长度，至少8位）。
ALTER USER 'root'@'localhost' IDENTIFIED BY '12345678';
```` 
6. 设置远程管理权限
```` 
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '12345678';
```` 
7. 退出数据库
```` 
exit
```` 


## 参考  
[1]阿里云. https://cn.aliyun.com/  
