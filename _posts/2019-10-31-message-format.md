---
layout:     post
title:      Java中的消息格式化:MessageFormat
subtitle:   可能有点儿班门弄斧了
date:       2019-10-31
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Java
---

> 计算机并不解决问题，它们只是执行解决方案。

## 前言  
相信每位Java开发都在学习或工作中使用过消息格式化.今天主要就粗略地讲一讲这个.  

## 初识  
最开始,大家对于消息格式化的使用大概停留在这种印象上:  

````Java
  String stringSource = "定时器将于%s执行,有%d个任务正在等待.";
  String format = String.format(stringSource,new Date(), 5);
  System.out.println(format);
````

它的输出结果是:  
```` Console
定时器将于Tue Oct 31 09:35:10 CST 2019执行,有5个任务正在等待.
````
对于一些人来说,我们使用消息的格式化就止步于此了.  

## MessageFormat
而实际上,Java中还有一个类库`MessageFormat`
MessageFormat中的format有2个参数,第一个参数是带占位符变量的文本,还有一个是实际值的可变长度参数.   
比如说,让上面的格式化信息以MessageFormat重写的话,就是:  
````Java
String source = "定时器将于{0}执行,有{1}个任务正在等待.";
String message = MessageFormat.format(source, new Date(), 5);
System.out.println(message);
````
控制台打印是这个样子:  
````
定时器将于19-10-31 上午9:35执行,有5个任务正在等待.
````

相信有多小伙伴看出了其中的区别:  

1. MessageFormat使用了`{index}`占位符类型取代了String中的`%s`点位符类型.也许很多小伙伴会觉得String中的占位符用着也很不错. 不过呢.对于我而言,它显得比较复杂.整数,浮点数,字符串都要使用不同的占位符号标记,我是真的不想记啊 ~~~~~~~~

2. 对于日期的打印,String的格式化打印出的结果是`Tue Oct 31 09:35:10 CST 2019`,而MessageFormat的打印结果则是`19-10-31 上午9:35`.很明显,String的format方法调用了Date的toString方法,而MessageFormat的format则对日期进行了一个格式化.这就是MessageFormat比String要人性化的一点了.  

## MessageFormat中的占位符  
MessageFormat中的占位符简写的话是 `{0}`这个样子,但实际上,占位符后面和可以写上其他的参数.  
````Java
       String source = "定时器将于{0}执行,有{1}个任务正在等待.当前系统资源占用{2,number,percent}";
       String message = MessageFormat.format(source, new Date(), 5,0.68);
       System.out.println(message);
````
它的打印结果是:
````
      定时器将于19-10-31 上午9:55执行,有5个任务正在等待.当前系统资源占用68%
````
其中,占位符的写法是`{2,number,percent}`,花括号里的三个参数分别指的是`占位符索引`,`类型`,`风格`.   

占位符索引相信大家都明白,不多介绍了.  

跟在占位符后面的,分别是类型(type)和风格(style).   

类型分别是 `number`,`time`,`date`,`choice`. 分别指的是`数字`,`时间`,`日期`,`选择`.   

风格根据类型的不同而不同:    

|风格|所属类型|描述|
|---|---|---|
|integer|number|整数|
|currency|number|货币|
|percent|number|百分比|
|short|time|上午10:50|
|medium|time|DateFormat.DEFAULT(HH:mm:ss)|
|long|time|上午HH时mm分ss秒|
|full|time|上午HH时mm分ss秒 CST|
|short|date|yyyy-MM-dd|
|medium|date|DateFormat.DEFAULT(yyyy-MM-dd)|
|long|date|yyyy年MM月dd日|
|full|date|yyyy年MM月dd日 星期*|

是不是简单明了呢?  

## MessageFormat中的选择格式  
回到我们的示例中来:
````Java
        String source = "定时器将于 {0,time,short} 执行,有{1}个任务正在等待." +
                "当前系统资源占用{2,number,percent}";
        String message = MessageFormat.format(source, new Date(), 5,0.68);
        System.out.println(message);

````
````Console
        定时器将于 上午10:50 执行,有5个任务正在等待.当前系统资源占用68%
````
有X个任务等待.   

这句话并不十分好. 因为如果没有任务的话,它就会打印出`有0个任务正在等待`,这样子不太符合我们的阅读习惯,如果改成`没有任务正在等待`,就好很多.  

而 `choice` 就是应用此场景的.

将代码改造成:  
````Java
      String source = "定时器将于 {0,time,short} 执行,{1,choice,0#没有|1#有{1}个}任务正在等待.当前系统资源占用{2,number,percent}";
      String message1 = MessageFormat.format(source, new Date(), 0,0.68);
      String message2 = MessageFormat.format(source, new Date(), 1,0.68);
      System.out.println(message1);
      System.out.println(message2);
````

索引值为1的风格内容是`0#没有|1#有{1}个`.
这两面有两对内容:
  * 0#没有
  * 1#有{1}个
其中每对之间使用符号`|`进行分割.  

而每对内容,又由2个部分组成:
* 下限  
* 格式字符串  
下限和格式字符串之间由符号`#`进行分割.   

下限指的是实际值.格式字符串则是指该值时,使用的内容.  

在本例中,它的含义是:  

实际值小于等于0,使用`没有`字符串.  

实际值为1或大于1,使用`有{1}个`字符串.  

打印结果则是:  
````Console
      定时器将于 上午11:01 执行,没有任务正在等待.当前系统资源占用68%
      定时器将于 上午11:01 执行,有5个任务正在等待.当前系统资源占用68%
````

完美符合我们的需求!不是吗?  

## 后记
MessageFormat还可以使用更高级别的格式化,因为本人觉得更高级别的格式化在大家的工作学习时使用的频率不多,所以就没有写下来.  

如果有兴趣,可以稳步到[这里](https://docs.oracle.com/javase/7/docs/api/java/text/MessageFormat.html)进行进一步的学习和研究.  

如果本文有任何内容出现错误,欢迎留言.  

## 参考文档
[1] [美]Cay S. Horstmann Gary Cornell. Core Java Volume II----Advanced Features[M].陈昊鹏,王浩,姚建平 译.北京：机械工业出版社，2016：270-273.
[2] Oracle.MessageFormat (Java Platform SE 7 ). https://docs.oracle.com/javase/7/docs/api/java/text/MessageFormat.html .
