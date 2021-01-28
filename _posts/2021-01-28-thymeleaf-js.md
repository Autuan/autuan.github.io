---
layout:     post
title:      Thymeleaf中将对象赋值到js对象中
subtitle:   Thymeleaf中将对象赋值到js对象中
date:       2021-01-28
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Thymeleaf
---

> 有些烦恼是我们凭空虚构的，而我们却把它当成真实去承受.

## 简介
使用 Thymeleaf 开发时，我们可能需要将控制器中的参数传递到 JS 变量中，应该怎么做呢？ 

## example

### java Controller
```` java
@RequestMapping("/hello")
ModelAndView hello(ModelAndView mav){
  // 自定义对象
  AutuanBean bean = new AutuanBean();
  bean.setDescription("hello,autuan");
  mav.addObject("bean",bean);
  mav.setViewName("hello");
  return mav;
}
````
### JS 赋值
```` html
<script th:inline="javascript">
      //<![CDATA[
        let bean = [[${bean}]];
        //]]>
        console.info(bean.description);
</script>
````
## PS
如果有更好的解决方法,欢迎留言.  

感激不尽
