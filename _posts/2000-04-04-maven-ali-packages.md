---
layout:     post
title:      使用阿里云创建自己的maven私有仓库
subtitle:   云效制品仓库
date:       2022-04-04
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - maven
---

# 前言
maven 可以说是 java 开发人员的必备技能了。  

当然，除了众所周知的各种公开仓库，我们也会用到私有化仓库。  

常见的私有化仓库有下面几种：

* 服务器搭建 Nexus 
* 通过 github 托管 
* 阿里云云效仓库


本文主要描述一下使用**云效仓库**的方式。

# 准备
使用阿里云的云效工具，需要拥有阿里云的账号。   
此处不再赘述。  

# 配置云端
注册登录云效工作台后，找到制品仓库并点击进入。  
（可以通过左上角的更多进入）  

如果你的公司已经在使用了云效，请注意组织机构的选择。 

![选择制品仓库](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-ali-packages/maven-ali-packages-1.png)

制品仓库默认已经为我们创建了两个目录：
云效 Packages 为您自动创建了两个 Maven 仓库，一个 release 库和一个 snapshot 库。
分别对应稳定版本和快照版本。

而我们要做的，是去复制上传密钥。
在制品仓库左下角的设置中，可以打开看到密码。
![设置](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-ali-packages/maven-ali-packages-2.png)
![查看密码](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-ali-packages/maven-ali-packages-3.png)

# 配置 maven 
## 服务器配置
编辑 maven 的 setting.xml  

加入服务器的配置和密钥
```` xml
<servers>
  <server>
    <id>rdc-releases</id>
    <username>************************</username>
    <password>************</password>
  </server>
  <server>
    <id>rdc-snapshots</id>
    <username>************************</username>
    <password>************</password>
  </server>
</servers>
````
## 上传配置
如果只需要下载，不需要配置这些。  

上传地址可以前往

仓库-> 仓库指南 -> Maven 方式查看。  

同样是 setting.xml :

```` xml
<profiles>
  <profile>
    <id>rdc</id>
    <properties>
      <altReleaseDeploymentRepository>
        rdc-releases::default::https://packages.aliyun.com/maven/repository/autuan/
      </altReleaseDeploymentRepository>
      <altSnapshotDeploymentRepository>
        rdc-snapshots::default::https://packages.aliyun.com/maven/repository/autuan/
      </altSnapshotDeploymentRepository>
    </properties>
  </profile>
</profiles>
````

设置完 profiles 之后， 别忘记启用 ： 
```` xml
<activeProfiles>
  <activeProfile>rdc</activeProfile>
</activeProfiles>
````

## 下载
配置完 server 之后 ， 想要下载的话，加上镜像地址即可：
```` xml
<mirrors>
  <mirror>
    <id>mirror</id>
    <mirrorOf>central,jcenter,!rdc-releases,!rdc-snapshots</mirrorOf>
    <name>mirror</name>
    <url>https://maven.aliyun.com/nexus/content/groups/public</url>
  </mirror>
</mirrors>
````

over~


# PS 
## 收费方式
截止到发文时，云效仓库还是免费开放的。
