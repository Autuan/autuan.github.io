---
layout:     post
title:      Required request part 'file' is not present
subtitle:   总有时会忽略这些
date:       2021-01-29
author:     Autuan.Yu
header-img: https://i.loli.net/2020/08/27/jFXCrMgxzHWGpcP.jpg
catalog: true
tags:
    - SpringBoot
---

> 所谓完美，不是指不能再添加别的东西了，而是指没有东西可以从其中拿掉了。

## 简介
使用SpringBoot 图片上传时出现了一个错误：
````
 Required request part 'file' is not present
````

提示是 file没有值，但实际上，在 postman 中是有这个的。


## 解决
需要在SpringBoot 中自定 multipartResolver ：

### 新建 config 对象
```` java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.multipart.commons.CommonsMultipartResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * 多媒体解析
 */
@Configuration
public class CommonMultipartResolver implements WebMvcConfigurer {
    @Bean(name = "multipartResolver")
    public CommonsMultipartResolver multipartResolver() {
        CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();
        return multipartResolver;
    }
}
````
### 启动类排除默认解析
```` java
@SpringBootApplication(exclude={MultipartAutoConfiguration.class})
````

### 问题解决！
## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
