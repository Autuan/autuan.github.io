---
layout:     post
title:      通过中央门户发布MavenJar包到中央仓库
subtitle:   Invalid signature file 问题处理
date:       2024-04-11
author:     Autuan.Yu
header-img: 
catalog: true
tags:
    - SpringBoot
    - Maven
---



通过 github 开源的项目，其制品 jar 虽然可以在 github packages 上直接下载，但是却不支持maven匿名方式直接引入，这时，我们就需要发布到maven中央仓库


# 1. 生成 GPG 密钥
要发布到中央仓库，GPG  是必须的

## 1.1 配置 GPG
如果电脑上没有 GPG ， 可以下载安装一下  

如果电脑上有安装 git , 也可以直接使用 git 目录下的 GPG

以 windows 系统为例，将 git 的 bin 目录配置到系统环境目录下的 `path` 中 ：
```
C:\Program Files\Git\usr\bin
```

配置好后打开cmd (有些环境可能需要重启Windows) ，并在cmd 窗口输入如下命令：
```
gpg --version
```

输出如下：
```
C:\Windows\System32>gpg --version
gpg (GnuPG) 2.2.41-unknown
libgcrypt 1.9.4-unknown
Copyright (C) 2022 g10 Code GmbH
License GNU GPL-3.0-or-later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: /c/Users/rot/.gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```

> [!TIP]
> 不要使用 GitBash /  GitCmd 验证

## 生成密钥
使用如下命令生成一个新密钥
```
gpg --gen-key
```

会提示选择密钥有效时间 / 邮箱信息 等内容， 按实际需要填入便可。  

最后还会要求输入密钥的密码，请妥善保管，如果遗失只能重新生成一个新密钥了。  

## 查看密钥
生成后使用如下命令查看生成的密钥信息：
```
gpg --list-keys
```

响应的内容类似这样 根据实际填入的资料而不同：
```
C:\Windows\System32>gpg --list-keys
/c/Users/rot/.gnupg/pubring.kbx
-------------------------------
pub   rsa2048 2024-04-10 [SC]
      280A9B9761C171FA21B5150609BE9909A384DE7D
uid           [ultimate] autuan (autuan-framework-srping-boot-starter) <yu@autuan.top>
sub   rsa2048 2024-04-10 [E]
```

在 `pub` 栏下的 `280A9B9761C171FA21B5150609BE9909A384DE7D` 为本次生成的 keyId ， 复制这个keyId ，后续我们需要填入其他配置里


## 公开公钥
密钥生成后我们需要公开我们的公钥以方便其他人来验证我们的文件一致性。

使用如下命令公开我们的密钥：
```
gpg --keyserver keyserver.ubuntu.com --send-keys 280A9B9761C171FA21B5150609BE9909A384DE7D
```
* 280A9B9761C171FA21B5150609BE9909A384DE7D 是我们实际生成的密钥 keyId ， 执行时记得替换

推送后我们可以使用如下命令检查是否已经推送成功：
```
gpg --keyserver keyserver.ubuntu.com --recv-keys 280A9B9761C171FA21B5150609BE9909A384DE7D
```

如果响应内容如下，便是正常的：
```
C:\Windows\System32>gpg --keyserver keyserver.ubuntu.com --recv-keys 280A9B9761C171FA21B5150609BE9909A384DE7D
gpg: key 09BE9909A384DE7D: "autuan (autuan-framework-srping-boot-starter) <yu@autuan.top>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
```
主要是 `processed` 和 `unchanged` 的值

# sonatype 账号注册及配置
1 https://central.sonatype.com/
注册 https://central.sonatype.com/ 账号
我们可以通过 github 账号直接授权注册登录 
sign in -> sign up -> github 登录

2 ## Namespace 创建
如果使用 github 授权登录，默认就已经有一个 namespace 可用，不过如果你是自行注册或是拥有域名，还是需要添加一个新的 Namespace , 此处以域名为例：

右上角选择 个人信息 ，进入 Namespaces ， 点击 `Add Namespaces` 添加
[添加Namespaces](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-central/mvn-add-namespace.png?x-oss-process=style/toWebp)

域名的话我们要按着反向输入，比如我的域名是 autuan.top , 那么我要验证的就是 `top.autuan` 包名

2.2 verify
添加域名后需要验证，在网站上先获取 verifyKey 
[key](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-central/mvn-verify-namespace.png?x-oss-process=style/toWebp)

然后去云服务商添加一个 `TXT` 的域名解析，解析值为 `Verification Key` 便可

配置好解析后，在 sonatype.com 验证，因为网络延迟的问题可能需要等待几分钟，博主很快就验证通过了


# Maven配置
## 生成UserToken
3 publish 
https://central.sonatype.org/publish-ea/publish-ea-guide/

右上角个人信息 -> View Account 

进入账号页面后点击生成 一个 UserToken
[生成Token](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/maven-central/mvn-verify-generate-token.png?x-oss-process=style/toWebp)

生成后记得保存你的token 信息

## 配置UserToken
- Publish components to Maven Central using a Maven build plugin
编辑电脑maven的`config.xml`文件，新增一个server节点信息，username / password 在刚刚生成时有提示：

```
<settings>
  <servers>
    <server>
      <id>central</id>
      <username><!-- your token username --></username>
      <password><!-- your token password --></password>
    </server>
  </servers>
</settings>
```

# 配置项目 pom.xml
## 配置项目基本信息
推送到公开仓库的内容，推荐配置以下信息：

```
<!-- groupId / artifactId / version 是一定要有的 -->  
<groupId>top.autuan</groupId>  
<artifactId>autuan-framework-spring-boot-starter</artifactId>  
<version>1.0.0-Java8</version>  

<!-- 名称，可以使用 groupId + ':' + artifactId 的形式 -->  
<name>top.autuan:autuan-framework-spring-boot-starter</name>  
<description>集成了大多数系统常用功能</description>  
<url>https://github.com/Autuan/autuan-framework-spring-boot-starter</url>

<!-- 许可证信息，这里是Apache 2.0的许可证，大家根据实际情况修改 -->  
<licenses>  
    <license>        
    <name>The Apache Software License, Version2.0</name>  
        <url>https://www.apache.org/licenses/</url>  
        <distribution>repo</distribution>  
    </license>
</licenses>  
  
  
<!--   开发人员信息         -->  
<developers>  
    <developer>        <name>Autuan</name>  
        <email>yu@autuan.top</email>  
    </developer></developers>  
  
<!--   项目仓库信息         -->  
<scm>  
    <connection>scm:git:https://github.com/Autuan/autuan-framework-spring-boot-starter.git</connection>  
    <developerConnection>https://github.com/Autuan/autuan-framework-spring-boot-starter</developerConnection>  
    <url>https://github.com/Autuan/autuan-framework-spring-boot-starter.git</url>  
    <tag>1.0.0-Java8</tag>  
</scm>
```


## 配置打包插件
```

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-gpg-plugin</artifactId>
                <version>1.6</version>
                <executions>
                    <execution>
                        <id>sign-artifacts</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>sign</goal>
                        </goals>
                        <configuration>
                        <!--这里是密钥的 key Id  -->
                            <keyname>280A9B9761C171FA21B5150609BE9909A384DE7D</keyname>                                      <passphraseServerId>280A9B9761C171FA21B5150609BE9909A384DE7D</passphraseServerId>
                            <gpgArguments>
                                <arg>--pinentry-mode</arg>
                                <arg>loopback</arg>
                            </gpgArguments>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

            <!-- Maven Source 插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.2.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- Maven Javadoc 插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>3.3.0</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.sonatype.central</groupId>
                <artifactId>central-publishing-maven-plugin</artifactId>
                <version>0.4.0</version>
                <extensions>true</extensions>
                <configuration>
                    <publishingServerId>central</publishingServerId>
                    <tokenAuth>true</tokenAuth>
                </configuration>
            </plugin>

        </plugins>
    </build>


```

你可以先直接将上面的 build 内容复制到 pom.xml 中再做修改
- spring-boot-maven-plugin ： SpringBoot 的 maven 编译插件 ， 无需修改
- maven-gpg-plugin ： GPG 插件，需要将 `configruation`中的 `keyname` 和 `passphraseServerId` 修改为我们开始保存的 keyId
- maven-source-plugin : 生成 source-code , 为发布到公共仓库所必须的内容，无需修改或更换为更新版本
- maven-javadoc-plugin : 生成 java-doc, 为发布到公共仓库所必须的内容，无需修改或更换为更新版本
- central-publishing-maven-plugin : 推送到中央仓库的插件，无需修改或更换为更新版本

# 推送

## 检查
在推送前可以先执行一次 `mvn clean install -X` 打包到本地先检查一下，当然也可以跳过此步骤，博主建议首次推送前进行一次检查 

在生成 java-doc 时，可能会因为不按正式的 javadoc写法报错.  

如果看到了类似下面的提示：
```
2 errors
94 warnings
```

errors 是必须要修正的，可以在详情内搜索 `error:` 来一一定位解决



## 推送
使用如下命令进行发布：
```
mvn deploy
```

执行后此命令便可以将我们的项目推到中央仓库

常见问题：
```
Invalid signature file
```

如果你是在短时间内（若干小时）便从开始执行到最后一步，你可能会在推送时发现如上的错误

这个是因为密钥公开的同步问题，需要等待 central.sonatype.com 同步我们的密钥后才能检查通过。  

博主是在等待了`一天`之后才推送成功的，给大家做参考。  


## 发布
如果你是按着本文一步步做的，那么在推送成功之后还需要登录 central.sonatype.com 

在 Deployments 页面点击 `Publish` 按钮手动发布。  


我们也可以在 maven 中配置自动发布，不过考虑到篇幅和主题的关系，就不在此赘述了。 
