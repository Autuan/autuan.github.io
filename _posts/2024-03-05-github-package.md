---
layout:     post
title:      Maven项目引入Github Packages的包
subtitle:   一个关于SpringBoot使用GithubPackages的依赖简单而实用的教程
date:       2024-03-05
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - SpringBoot
---


Github Packages 是 Github 提供的一种服务，可以让你在 Github 上存储和管理你的项目包。它可以和 Github Action 配合，实现自动化的编译、打包、上传等操作。  

但是如果我们的开源项目想要让其他人或者其他项目通过 Maven 匿名的形式引用 Github Packages 中的资源，却十分麻烦。     

即使仓库是公开的，也不能直接提供依赖下载，而需要提供一个拥有 Package 读取权限的 Github Token。   

对此，Github 的官方建议是：将包上传到其他可以匿名访问的 Maven 仓库，比如 Maven Central 或者 JitPack。  

但是因为麻烦、烦琐、时间等成本抑或是由于仓库的私密性等原因，不想这样做。     

此文则是讲解一种方法：通过配置 Github Token 在 Maven 项目中直接引入 Github Packages 中的包。     

# Step 1 准备
在开始前，我们需要有一个拥有Package 读取权限的Github Token 。  

如果你已有token, 可以直接到下一步。 

如果你没有token但有对应仓库的权限或者需要生成一个新的，可以按下列步骤生成一个：  

- 进入 Github 的 Setting -> Developer Settings -> Personal access Token -> Tokens (classic)
- 点击 Generate new token，输入一个描述，勾选 read:packages 权限，点击 Generate token
- 复制生成的 Token，保存好，Token 在网站上只会展示一次

请注意！
read:packages 权限是拥有所有 packages 的权限，不能只限于一个仓库。  所以如果你是要开放出去的话，一定要注意是否有私有包，以及权限是否开放了太多。

或者，你也可以使用细粒度的 Token ，即 `Fine-grained tokens` ， 此文章不再赘述


# Step 2 配置 maven setting.xml

添加 server
```
<servers>
    <server>
        <id>github</id>
        <username>github name</username>
        <password>your_token</password>
    </server>
</servers>
```

其中，id 属性是一个标识，可以随便取，但要和后面的 pom.xml 中的一致。username 和 password 属性分别是你的 Github 用户名和 Token。


另外有一个细节，很多其他博客都没有提及的事，如果你的 mirror 中配置了 `*` 的镜像，需要在 mirrorOf 属性中添加排除 `github` 的依赖，`github` 是 server 中的 id 属性。

注意是所有配置了 `*` 的镜像都要排除。  

例如：  

```
<mirror>
    <id>mvnrepo-central</id>
    <mirrorOf>*,!github</mirrorOf>
    <name>mvnrepo central.</name>
    <url>http://central.maven.org/maven2/</url>
</mirror>
```

# Step 3 配置 project pom.xml  

在项目的 pom.xml 文件中，我们需要添加一个 repository，用来指定我们要引用的 Github Packages 中的包的地址。repository 的配置如下：

```
<repositories>  
    <repository>
        <id>github</id>  
        <name>GitHub Autuan Packages</name>  
        <url>https://maven.pkg.github.com/Autuan/autuan-framework-spring-boot-starter</url>  
        <releases><enabled>true</enabled></releases>  
        <snapshots><enabled>true</enabled></snapshots>  
    </repository></repositories>
```

`id`  属性要和 setting.xml 中的 server id 一致
`name` 属性是一个描述，可以随便取
`url` 属性是 Github Packages 中的包的地址，格式是 `https://maven.pkg.github.com/用户名/仓库名`。



然后，我们就可以在 pom.xml 中声明 Github Packages 中的包作为依赖，和其他 Maven 仓库中的包一样。例如：

```
<dependency>
    <groupId>top.autuan</groupId>
    <artifactId>autuan-framework-spring-boot-starter</artifactId>
    <version>1.1.4</version>
</dependency>
```

这样，我们就可以在 Maven 项目中直接引入 Github Packages 中的包了。我们可以在 IDEA 中点击刷新依赖，就可以加载对应的 jar 文件。我们也可以在代码中使用这些包中的类和方法，就像使用其他 Maven 依赖一样。


