---
layout:     post
title:      重复ID导致取值失败
subtitle:   时间越紧急,出错的概率越大
date:       2020-09-01
author:     Autuan.Yu
header-img: https://i.loli.net/2020/09/01/GZpmtYyf5WCBKV2.png
catalog: true
tags:
    - Thymeleaf
---
> 细节决定成败

## 简介
前两日,在赶一个小项目,被一个小细节给折腾了不少时间.   

发现了问题之后,令我哭笑不得.  

## 复现   
### 代码  
为了帮助大家理解,代码已经简化:   

控制器层代码:  

```` Java
@RequestMapping("/example")
public ModelAndView example(ModelAndView mav) {
  mav.addObject("title","Hello,Autuan");
  mav.setViewName("/example");
  return mav;
}

````
页面:  
```` HTML
// ... 页面标题及CSS 样式引用略过
<input type="hidden" id="title" th:value="${title}" />
// 页面通用代码块
<th:block th:include="include :: base" />
// JQuery等JS 引用略过
<script>
  $(function(){
    let title = $('#title').val();
    console.info(title);
  });
</script>
````
### 描述
代码是不是很简单?  

正常情况下,JS的变量`title`打印值应该是'Hello,Autuan',没人有异议吧?  

但实际上,`console.info(title)`的打印值是空.  

### 原因
至于为什么会为空呢?  

其实很简单,因为这个页面有两个 id为`title` 的标签.  

另外一个就隐藏在这里:  
````HTML
<th:block th:include="include :: base" />
````
一般情况下,如果有ID重复,idea 为下标红色波浪线提示开发人员.  

但因为这里使用了代码块引入,所以idea也不会做提示.  

如果页面引用的代码块很多,而id又是一种比较通用的名称,比如`name`,`title`,`no`这些,则很有可能会因为重复ID的问题导致我们的取值赋值和预期不同.  

## 解决
* 修改ID: 比如说input的id可以修改为 `pageTitle` 云云  
* JS直接赋值: 如果只是在JS里面取值,不涉及页面特效修改的话,可以直接使用Thymeleaf的语法直接将变量值赋给JS变量:   
```` JavaScript
 let title = '#[[${name}]]';
````


## PS
这是一个非常简单的问题.  

我相信任何一名学习了HTML的朋友,都会注意id同名的问题.  

但正如俗语所言: 智者千虑必有一失.  

随着项目工程的庞大及复杂,我们或主动,或被动,必定会引用越来越多的不是由自己所负责的代码.  

而国内的开发环境又往往是对于需求的时效性要求很高.  

我们也不能对其他人所负责的代码进行详细的理解.  

碰到问题时先不要着急,越着急可能就越找不到真正的问题所在.  

却还以为是API出问题了(笑);

希望博主的这次低级错误能给大家带来帮助.
