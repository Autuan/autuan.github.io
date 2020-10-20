---
layout:     post
title:      Postman 的动态变量 Dynamic variables 介绍与使用
subtitle:   高效率使用Postman的必备知识
date:       2019-8-30
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2016/12/14/04/13/wave-1905610__340.jpg
catalog: true
tags:
    - Postman
---

> 工欲善其事,必先利其器

## 前言
如果写完了代码不进行测试,那么,这个代码是不能说已经**完成** 了,只有当测试也通过了,才能说这个功能块完成了,本文主要简单介绍一下Postman的一些常见小知识/技巧 以及 API 相关的内容

## 动态变量  
### 使用动态变量
我们在实际接口开发时,有时要发送一些随机的值(如果说新用户接口的会员昵称),Postman 支持Dynamic variables 生成一些随机值来给我们调用  

做一个简单的例子吧! 还是之前的那个接口,请求参数有一个String类型的name字段,在接收后会把信息返还给我们:
![返回name](https://i.loli.net/2019/08/29/R72OeNB3tnSshVU.png)

第一次发送请求  
写一个固定值发送请求:
![固定](https://i.loli.net/2019/08/29/zkBoh3I97ZXrERP.png)  

使用随机的一个名字生成请求:
![随机](https://i.loli.net/2019/08/29/ZHeWt5QDFow2Gpv.png)
每次请求,名称都会变化----合理使用动态变量,有很多方面的事我们可以很简单的通过

Good job!! 动态变量的用法就是这么简单,下面是Postman可以使用的动态变量
### 基本值  

|变量名|描述|示例值|
|----|----|----|
|$guid|一串uuid-v4格式的guid|"0091b389-0096-4345-bfa7-9a6725cf58b1"|
|$timestamp|当前时间戳|1567068615|
|$randomUUID|一串随机的36位长度的UUID|"4f94ddd0-3819-40c7-93e3-f60a523aa439"|

### 文本/数字/颜色  

|变量名|描述|示例值|
|----|----|----|
|$randomAlphaNumeric|随机字母数字字符|"a","s",0|
|$randomBoolean|随机布尔值|true/false|
|$randomColor|随机颜色值|"blue"|
|$randomHexColor|随机颜色十六进制值|"#106f21"|
|$randomInt|随机int值|99|
|$randomAbbreviation|随机缩写|"SQL"|

### 网络IP地址

|变量名|描述|示例值|
|----|----|----|
|$randomIP|一个随机的IPv4地址|"104.27.76.148"|
|$randomIPV6|一个随机的IPv6地址|"f8e8:a46c:311b:bbc0:2fd4:8262:02f6:624c"|
|$randomMACAddress|一个随机的Mac地址|"f2:94:9f:71:d8:e28"|
|$randomPassword|一个随机的包含大小写及数字的15位长度字符串|"Nyrr9phyat8ddTV"|
|$randomLocale|一个随机基于ISO 639-1规范的双字母语言代码|"en"|
|$randomUserAgent|一个随机的user agent|"Mozilla/5.0 (Windows; U; Windows NT 6.1) AppleWebKit/537.2.2 (KHTML, like Gecko) Chrome/13.0.838.0 Safari/537.2.2"|
|$$randomProtocol|一个随机的互联网协议|"http:"/"https"|
|$randomSemver|一个随机的版本号|"8.3.2"|

### 名称
|变量名|描述|示例值|
|----|----|----|
|$randomFirstName|一个随机的英文名|"Esteban"|
|$randomLastName|一个随机的英文姓|"Beatty"|
|$randomFullName|一个随机的英文姓名|"Ellsworth Murphy"|
|$randomNamePrefix|一个随机的名称前缀|"Dr."|
|$randomNameSuffix|一个随机的名称后缀|"MD"|

### 职业
|变量名|描述|示例值|
|----|----|----|
|$randomJobArea|一个随机的工作范围|"Intranet"|
|$randomJobDescriptor|一个随机的工作描述|"Product"|
|$randomJobTitle|一个随机的岗位名称|"Dynamic Implementation Administrator"|
|$randomJobType|一个随机的工作类型|"Planner"|

### 电话/地址

|变量名|描述|示例值|
|----|----|----|
|$randomPhoneNumber|一个随机的电话号码|"1-190-992-9427 x39501"|
|$randomCity|一个随机的城市名称|"Rosendomouth"|
|$randomStreetName|一个随机的街道名|"Nikki Shoal"|
|$randomStreetAddress|一个随机的街道地址|"6052 Windler Bypass"|
|$randomCountry|一个随机的国家/地区|"Niger"|
|$randomCountryCode|一个随机基于ISO 3166-1 alpha-2的国家/地区简码|"HM"|
|$randomLatitude|一个随机的纬度|"47.0251"|
|$randomLongitude|一个随机的经度|"-72.7924"|

### 图片

|变量名|描述|示例值|
|----|----|----|
|$randomImage|一张随机图片|"http://lorempixel.com/640/480/sports"|
|$randomAvatarImage|一张随机头像图片|"https://s3.amazonaws.com/uifaces/faces/twitter/stefanotirloni/128.jpg"|
|$randomImageUrl|一个随机的图片路径|"randomImageUrl"|
|$randomAbstractImage|一张随机抽象图片|"http://lorempixel.com/640/480/abstract"|
|$randomAnimalsImage|一张随机动物图片|"http://lorempixel.com/640/480/animals"|
|$randomBusinessImage|一张随机商务图片|"http://lorempixel.com/640/480/business"|
|$randomCatsImage|一张随机猫科图片|"http://lorempixel.com/640/480/cats"|
|$randomCityImage|一张随机城市图片|"http://lorempixel.com/640/480/city"|
|$randomFoodImage|一张随机食物图片|"http://lorempixel.com/640/480/food"|
|$randomNightlifeImage|一张随机夜景图片|"http://lorempixel.com/640/480/nightlife"|
|$randomFashionImage|一张随机时尚图片|"http://lorempixel.com/640/480/fashion"|
|$randomPeopleImage|一张随机人物图片|"http://lorempixel.com/640/480/people"|
|$randomNatureImage|一张随机自然图片|"http://lorempixel.com/640/480/nature"|
|$randomSportsImage|一张随机运动图片|"http://lorempixel.com/640/480/sports"|
|$randomTechnicsImage|一张随机教学图片|"http://lorempixel.com/640/480/technics"|
|$randomTransportImage|一张随机交通图片|"http://lorempixel.com/640/480/transport"|
|$randomImageDataUri|一张随机图片数据(Base64数据)|内容过长,略|

### 金融

|变量名|描述|示例值|
|----|----|----|
|$randomBankAccount|一个随机的8位数账户|"64794792" |
|$randomBankAccountName|一个随机的账户名称|"Money Market Account" |
|$randomCreditCardMask|随机屏蔽的信用卡号码|"2069" |
|$randomBankAccountBic|随机BIC（银行识别码）|"UVHUMTU1" |
|$randomBankAccountIban|一个随机的国际银行帐号|"IE09169600810901009629" |
|$randomTransactionType|一个随机的交易类型|"payment" |
|$randomCurrencyCode|一个随机的基于ISO-4217的货币代码|"USD" |
|$randomCurrencyName|一个随机的货币名称|"Boliviano Mvdol" |
|$randomCurrencySymbol|一个随机的货币符号|"$" |
|$randomBitcoin|一个随机的比特币地址|"3XGGZANI2SOJ9ONA3G7NO42IJXEH6" |

### 商务

|变量名|描述|示例值|
|----|----|----|
|$randomCompanyName|一个随机的公司名称|"Leffler, Franecki and Brekke" |
|$randomCompanySuffix|一个随机的公司后缀|"Inc" |
|$randomBs|一句随机的商业用语|"lug-and-play syndicate functionalities" |
|$randomBsAdjective|一句随机的商业用语形容词|"cutting-edge" |
|$randomBsBuzz|一句随机的商业用语流行词|"transition" |
|$randomBsNoun|一句随机的商业名词|"markets " |

### 短语

|变量名|描述|示例值|
|----|----|----|
|$randomCatchPhrase|一句随机短语|"Streamlined didactic software" |
|$randomCatchPhraseAdjective|随机形容词|"Synergized" |
|$randomCatchPhraseDescriptor|随机描述|"human-resource" |
|$randomCatchPhraseNoun|随机名词|"monitoring" |

### 数据库

|变量名|描述|示例值|
|----|----|----|
|$randomDatabaseColumn|一个随机的数据库列名|"updatedAt" |
|$randomDatabaseType|一个随机的数据库类型格式|"text" |
|$randomDatabaseCollation|一个随机的数据库排序比较方式|"utf8_general_ci" |
|$randomDatabaseEngine|一个随机的数据库引擎|"InnoDB" |

### 日期

|变量名|描述|示例值|
|----|----|----|
|$randomDateFuture|一个随机的未来日期时间|"Mon Dec 02 2019 08:13:49 GMT 0800 (中国标准时间)" |
|$randomDatePast|一个随机的过去日期时间|"Sat Jun 22 2019 21:31:11 GMT 0800 (中国标准时间)" |
|$randomDateRecent|一个随机的最近日期|"Fri Aug 30 2019 09:36:48 GMT 0800 (中国标准时间)" |
|$randomWeekday|一个随机的星期平日(周一~周日)|"Wednesday" |
|$randomMonth|一个随机的月份|"May" |

### 域名/电子邮箱及用户名

|变量名|描述|示例值|
|----|----|----|
|$randomDomainName|一个随机的域名|"autuan.top" |
|$randomDomainSuffix|一个随机的域名后缀|"com" |
|$randomDomainWord|一个随机的域名词汇|"marion" |
|$randomEmail|一个随机的电子邮件地址|"autuan.yu@gmail.com" |
|$randomExampleEmail|一个随机的来自example域的电子邮件地址|"autuan.yu@example.com" |
|$randomUserName|一个随机的用户名|"Autuan.Yu" |
|$randomUrl|一个随机的URL|"http://autuan.top" |

### 文件及目录

|变量名|描述|示例值|
|----|----|----|
|$randomFileName|一个随机的文件名|"autuan_is_cool.ggb" |
|$randomFileType|一个随机的文件类型|"video" |
|$randomFileExt|一个随机的文件后缀扩展名|"zip" |
|$randomCommonFileName|一个随机的文件名|"autuan_is_cool.ggb" |
|$randomCommonFileType|一个随机的常见文件类型|"application" |
|$randomCommonFileExt|一个随机的文件常见后缀扩展名|"zip" |
|$randomMimeType|一个随机MIME类型|"application/vnd.sss-cod" |

### 电商

|变量名|描述|示例值|
|----|----|----|
|$randomPrice|一个随机的价格|"29.00" |
|$randomProduct|一个随机的产品|"Gloves" |
|$randomProductAdjective|一个随机的产品形容|"Tasty" |
|$randomProductMaterial|一个随机的产品材质|"Soft" |
|$randomProductName|一个随机的产品名称|"Incredible Fresh Towels" |
|$randomDepartment|一个随机的产品分类|"Shoes" |

### 语法

|变量名|描述|示例值|
|----|----|----|
|$randomNoun|一个随机名词|"bus" |
|$randomVerb|一个随机动词|"parse" |
|$randomIngverb|一个随机的现在进行时|"synthesizing" |
|$randomAdjective|一个随机形容词|"multi-byte" |
|$randomWord|一个随机单词|"word" |
|$randomWords|一串随机单词|"Saint Barthelemy connecting Frozen" |
|$randomPhrase|一句随机短语|"You can't override the feed without calculating the back-end SMS sensor!" |
|$randomFileType|一个随机的文件类型|"Inc" |
|$randomFileType|一个随机的文件类型|"Inc" |

### 占位文字

|变量名|描述|示例值|
|----|----|----|
|$randomLoremWord|一个占位词|"autem" |
|$randomLoremWords|一串占位词|"vero aut placeat" |
|$randomLoremSentence|一句占位语|"Eum ducimus et et inventore." |
|$randomLoremSentences|一串占位语|"Accusantium qui et consectetur adipisci. Voluptatum consequuntur non voluptatibus. Aut quia laborum consequatur." |
|$randomLoremParagraph|一段占位语|"Iste voluptatibus corporis quam nobis amet. Culpa quo et voluptas dolor saepe optio. Maxime laudantium cupiditate incidunt. Suscipit accusamus molestiae quia qui nisi et." |
|$randomLoremParagraphs|几段占位语|内容过长,略 |
|$randomLoremText|随机数量的占位文本|"Maiores harum sed ut ut omnis aut quibusdam est." |
|$randomLoremSlug|一个随机值|"sunt-cupiditate-et" |
|$randomLoremLines|随机行数的占位内容|"Accusamus ab aut reprehenderit cupiditate inventore vitae." |
