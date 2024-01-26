
---
layout:     post                    
title:      阿里云云效流水线部署SpringBoot 包         
subtitle:   阿里云DevOpsCIDI
date:       2024-01-26              
author:     Autuan.Yu
header-img: 
catalog: true                      
tags:                               
    - SpringBoot
---


阿里云 云效流水线 是 DevOps 其中CIDI 的一种实现，本文以 ruoyi 管理后台为例，讲解一下使用云效流水线自动部署若依管理后台的流程。  

# 准备事项
- 已有云效账号，且拥有部署资源 （免费账号拥有 1800分钟/每月 的资源 ）。
- 已有 若依 Java 端 git 仓库地址，且已验证构建通过。
- 拥有阿里云ECS 或 已有云服务器，且已纳入云效工作台的`主机组管理`中。


# 步骤
## 新建流水线
1. 进入云效工作台 流水线 Flow ，并点击新建流水线
2. Java · 构建、部署到阿里云ECS/自有主机
        
    ![新建](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/aliyun-flow/new_flow.png?x-oss-process=style/toWebp)
    
        
        
    我们不需要测试模块，直接删除测试相关的任务。
        
         
    ![Desc](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/aliyun-flow/desc.png?x-oss-process=style/toWebp)
## 流水线配置：添加流水线源
点击 `添加流水线源`  

在这里你可以添加 gitee \ github 或者 其他 git 源，当然，如果只是体验一下的话，也可以直接选中`示例代码源`  

![git源](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/aliyun-flow/git.png?x-oss-process=style/toWebp)
## 流水线配置：构建

Java 构建中我们可以选择构建的 jdk 版本等，如果你使用的是 Java 1.8 , 是不需要特别做出修改，直接使用默认的便可。

## 流水线配置：部署

在流水线中的部署配置很少：  
先选择制品 和 要部署的主机组 ， 默认的制品就是前一步构建出来的包（压缩后）

随后在`部署脚本`中输入执行需要执行命令就可。  

虽然也可以在`部署脚本` 模块中直接写入 jar 包的启动命令，不过还是推荐大家使用 bash 脚本。 

下载路径和执行用户在实际使用中则根据实际需要修改，如果只是demo尝试，则不必特意修改


![Deploy](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/aliyun-flow/deploy.png?x-oss-process=style/toWebp)

这里假设脚本存放在服务器的 /projects/autuan 目录中

在本文章中，就是  /projects/autuan/deploy.sh 。


此处，在云效的部署脚本中只要简单的写一句便可以：

```
sh /projects/autuan/deploy.sh restart
```

### 编辑启动脚本 

在云效官方文档里，它推荐将部署脚本写在git源中并执行，为了方便学习，此博客里直接上传一份固定脚本到服务器上。 如果你有这个需求，可以参考官方文档或自行实现。  

编辑脚本，创建`deplpy.sh` 脚本，脚本内容可以参考[官方示例](https://atomgit.com/flow-example/spring-boot/blob/master/deploy.sh)  .

如果 你是通过 windows 系统编辑好脚本后上传至 linux 的， 记得使用 `dos2unix` 或其他方式转换文本格式。


### 运行
编辑好脚本后，只要点击右上方的保存并运行就会立即执行这次的流水线构建。  

至此，一个简单的示例就全部完成了。  


## 运行流水线：


对比：
Github Actions
Azure Pipeline
Tencent Cloud


yum install dos2unix
