---
layout:     post
title:      swagger SyntaxError Unexpected token ' in JSON at position 150750
subtitle:   swagger常见问题了
date:       2021-07-08
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2020/03/12/22/23/greens-4926370_960_720.jpg
catalog: true
tags:
    - Java
---
> 他的衬衫上不知打了多少次补丁，弄得像他那张帆一样，这些补丁被阳光晒得褪成了许多深浅不同的颜色。

### 描述
使用swagger时，发现文档显示不出来，控制台报错：
````
SyntaxError: Unexpected token ' in JSON at position 150750
    at JSON.parse (<anonymous>)
    at Object.json5parse (app.0f2f48b5.js:1)
    at r.transformResponse (app.0f2f48b5.js:1)
    at chunk-vendors.9a4add2b.js:2
    at Object.l [as forEach] (chunk-vendors.9a4add2b.js:2)
    at e.exports (chunk-vendors.9a4add2b.js:2)
    at chunk-vendors.9a4add2b.js:2
````

### 解决
有对 List<String> 数据结构的example声明没有加引号.  
 
Before :  
```` Java
   @ApiModelProperty(name = "demoList", value = "博客示例", example = "[zhangsan,lisi]")
   private List<String> demoList;
````
  
After :
````
  @ApiModelProperty(name = "demoList", value = "博客示例", example = "['zhangsan','lisi']")
  private List<String> demoList;
````

