---
layout:     post
title:      取消浏览器的自动填充
subtitle:   总有的时候,不想让它填充
date:       2019-12-12
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - HTML
---

> 让流程说话，流程是将说转化为做的惟一出路

在目前的网站会员管理时,因为业务逻辑可能同一会员会有多个密码,比如`登录密码`,`二级密码`,`支付密码`等.  

在浏览器上输入密码时,如果用户选择了浏览器保存密码,可能在二级密码等输入框会自动填充登录密码.  

使用`autocomplete`可以取消填充,如下:  

```HTML
<input type="password" autocomplete="new-password" name="secondLevelPassword" />
```

希望能帮助到各位小伙伴!
