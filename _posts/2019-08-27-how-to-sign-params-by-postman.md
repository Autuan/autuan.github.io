---
layout:     post
title:      如何使用Postman对接口参数进行签名
subtitle:   没有签名的接口就好像上大街没有装衣服一样(逃
date:       2019-8-28
author:     Autuan.Yu
header-img: https://i0.hippopx.com/photos/476/548/951/pen-leave-paper-terminal-board-preview.jpg
catalog: true
tags:
    - Postman
---

> 安全重于泰山

## 前言
做接口一方面我们要给前端使用,另一方面我们又要避免接口被人恶心使用.所以,签名成为了每个接口开发人员都要考虑的事情.  
如果我们在进行接口测试的时候,无法通过我们的签名,就测不了了, 本文,主要说一下如何使用Postman自动为我们的请求添加签名
## 导航
关于Postman的基本使用,可以看[这里](http://autuan.top/2019/08/23/how-to-use-postman/)  
## Pre-request Script
关于Postman中 Pre-request Script 的官方介绍,在[这里](https://learning.getpostman.com/docs/postman/scripts/intro_to_scripts/)  
官方是这么说的:  
> This allows you to write test suites, build requests that can contain dynamic parameters, pass data between requests, and a lot more. You can add JavaScript code to execute during 2 events in the flow: Before a request is sent to the server, as a pre-request script under the Pre-request Script tab.  

简单的来说,我们可以在请求服务器之前,对请求参数添加动态参,就是合签名的辙啊

## 如何使用Postman对接口参数进行签名
### 添加必填参数
正常来说,我们的接口都会有一些必填参数的,比如版本,设备信息 等,如下:  
![](https://i.loli.net/2019/08/27/V7NzKCtLmRId8yZ.png)  

** 请注意!真正开发时,关于必填字段的信息理应在拦截器中使用,本文为了讲解方便,直接写在了接口及请求类中,好孩子千万不要学哦 **  
请求参数有了,我们再做一个非空验证,如果必填参数都为空的话,那么也就不需要再对签名进行校验了  
![](https://i.loli.net/2019/08/27/TCnx21KQS5E8wrG.png)  
* ReturnResult 是一个基本的结果响应包装类,包含了错误码,错误信息以及响应数据 *

没有必填参数的话,就会返回错
![响应为错](https://i.loli.net/2019/08/27/9ygesUBPAWh2jdQ.png)

在Postman的环境设置中添加必填参数
![](https://i.loli.net/2019/08/27/Prumhejd7lUiCK6.png)

打开Pre-request Script 选项卡,输入下列代码  
``` JavaScript
// 获取当前的请求路径
var url = pm.request.url;

// 获取环境变量
const client=pm.environment.get("client");
const reqVersion=pm.environment.get("reqVersion");
const sign=pm.environment.get("reqVersion");

// 将必填参数拼接到路径上
url = url + '&client='+client+'&reqVersion='+reqVersion+'&sign='+sign;

// 重写url
pm.request.url = url
```
![输入Pre-request](https://i.loli.net/2019/08/28/Vjhgv1u3CUHbARS.png)

重新点击发送,会发现我们的接口又可以正常响应了
![正常响应](https://i.loli.net/2019/08/27/4aPNxXftUvesAyH.png)

审视这些代码,其中,有些地方是这么写的
```JavaScript
var url = pm.request.url;
const reqVersion=pm.environment.get("reqVersion");
```
``pm``是什么,相信很多同学都不知道,实际上,它是Postman提供的一个可以操作关于本次请求及环境的一个对象,在postman的老版本,称为*postman*对象,详细内容可以看[这里](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/)  
pm下属的子对象如下:  
* environment  
操作当前环境变量的值
* request  
操作关于本次请求的相关值
* global  
操作全局变量的值
* variables  
从当前环境变量和全局环境变量中搜索值
* info
包含与正在执行的脚本有关的信息
* iterationData  
运行期间集合中数据相关
* cookies
cookie 相关
* test & expect  & response  
用于Test选项卡的对象,未来有机会,博主会详细地讲解此对象

pm下包含了一些方法:
* sendRequest 发送一个异步请求

用上述方式可以很快捷地将必填参数写上,但因为对值的操作很麻烦,所以博主不推荐使用这种方式,博主推荐使用键-值对的形式,所以,我们还需要对它进行一次改造:
```JavaScript
// 根据?值获得请求字符串和请求参数
const urlSplit = request.url.split('?'); // 通过 ? 分割请求参数和请求路径
// 通过&符号 将请求参数改为数组
const paramsString = urlSplit[1];
const eachParamArray = paramsString.split('&');
// 获取环境变量
const client=pm.environment.get("client");
const reqVersion=pm.environment.get("reqVersion");
const sign=pm.environment.get("reqVersion");

// 定义一个对象,将目前的所有请求参以键值对的形式保存
let params = {};
// 将之前的参数放入
eachParamArray.forEach((param) => {
    const key = param.split('=')[0];
    const value = param.split('=')[1];
    Object.assign(params, {[key]: value});
});
// 放入必填参数
params.client=pm.environment.get("client");
params.reqVersion=pm.environment.get("reqVersion");
params.sign=pm.environment.get("reqVersion");

// 迭代param对象,生成?&的http参数字符串
let newQueryString = Object.keys(params).map(key => key + '=' + encodeURIComponent(params[key])).join('&');
// 重写url
pm.request.url = urlSplit[0]+'?'+newQueryString;
```  
![正常响应](https://i.loli.net/2019/08/27/4aPNxXftUvesAyH.png)  

Excellent! 同学,你已经掌握了最基本的Pre-request Script的写法了!

### 字典表排序
为了保证服务器端的验证通过,请求的所有属性都需要进行一次有序排序,毕竟,如果参与签名的值不排序的话,那么基本上是不打算正常使用接口了
常见的排序为ASCII排序  
我们直接调用 sort()方法便可,因为此方法的默认实现便是ASCII码排序
``` JavaScript
keys = Object.keys(params).sort()
```
### MD5签名加密
排序之后我们就要对参数进行签名了
一般来说,将所有非空值组合为一个字符串,进行MD5加密便可  
MD5的加密方式,Postman也有提供,真是方便啊:  
```JavaScript
//拼接待签名字符串
let signStr = '';
for(var i=0;i<keys.length;i++) {
    var val = params[keys[i]];
    if(val) {
        signStr +=val;
    }
}

```
如果对之前的代码有研究的话,会发现我们之前必填参数`sign`是直接使用的环境变量:
>```JavaScript
>params.sign=pm.environment.get("reqVersion");
>```

正式使用的时候,我们当然不能这么搞,我们需要把我们算出的签名字符串追加到我们的请求参数里:
```JavaScript
// MD5加密 并放入请求参数中
params.sign=CryptoJS.MD5(signStr).toString();
```

完整的 Script 如下:
``` JavaScript
// 根据?值获得请求字符串和请求参数
const urlSplit = request.url.split('?'); // 通过 ? 分割请求参数和请求路径
// 通过&符号 将请求参数改为数组
const paramsString = urlSplit[1];
const eachParamArray = paramsString.split('&');
// 获取环境变量
const client=pm.environment.get("client");
const reqVersion=pm.environment.get("reqVersion");
const sign=pm.environment.get("reqVersion");

// 定义一个对象,将目前的所有请求参以键值对的形式保存
let params = {};
// 将之前的参数放入
eachParamArray.forEach((param) => {
    const key = param.split('=')[0];
    const value = param.split('=')[1];
    Object.assign(params, {[key]: value});
});
// 放入必填参数
params.client=pm.environment.get("client");
params.reqVersion=pm.environment.get("reqVersion");
params.sign=pm.environment.get("reqVersion");
// ASCII排序
keys = Object.keys(params).sort();
//拼接待签名字符串
let signStr = '';
for(var i=0;i<keys.length;i++) {
    var val = params[keys[i]];
    if(val) {
        signStr +=val;
    }
}
// MD5加密 并放入请求参数中
params.sign=CryptoJS.MD5(signStr).toString();
// 迭代param对象,生成?&的http参数字符串
let newQueryString = Object.keys(params).map(key => key + '=' + encodeURIComponent(params[key])).join('&');
// 重写url
pm.request.url = urlSplit[0]+'?'+newQueryString;

```
你已经成功地签名了请求!  

![正常响应](https://i.loli.net/2019/08/27/4aPNxXftUvesAyH.png)    

### 其他
本文只是简单地对Postman签名请求作出了基础的讲解,事实上,在项目中的签名不会这么简单,一般来说,还会有:
* 随机字符串
* 时间戳
* 签名密钥
而对安全性要求更高的接口,可能还会有多次加密,非对称加密等

本文主要起一个抛砖引玉的作用,具体使用请参考各位同学实际开发时,各项目的签名要求  
如果对本文有问题或者有沟通需求的,欢迎留言或者电邮!
