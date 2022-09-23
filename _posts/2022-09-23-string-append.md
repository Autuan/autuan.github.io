---
layout:     post
title:      String\StringBuilder\StringBuffer
subtitle:   算是一个常见的八股题了
date:       2022-09-23
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Java
---
> 伟大的成绩和辛勤的劳动是成正比例的，有一分劳动就有一分收获，日积月累，从少到多，奇迹就可以创造出来。  


在 Java 中，有一个很常见的面试八股题：  

String 中 ， `+` 、 StringBuilder 、 StringBuffer 的区别是什么？  

相信只要是参加过几次面试的同学，都能信手拈来：  

String 是不可变字符串  

StringBuffer 是线程安全的可变字符串  

StringBuilder 是线程不安全的可变字符串  

完事之后 ， 往往还会补充一句 ： 不推荐使用 `+` 号拼接,应当使用 StringBuilder。  


那么,"+"号是怎么做到的呢？   

事实上， "+" 号就是使用的 StringBuilder.  

我们先写一个demo出来 ：  

```` java

 		String strA = "Hello,";
        String strB = "Autuan";
        String strC = " !";
        String result = strA + strB + strC;

        System.out.println(result);
````

接着，我们看一下编译后的文件在 jvm 中的执行顺序 ：  

![JVM创建了StringBuilder对象](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/string-builder/stringbuilder.png)

我们会发现：在使用 "+"号拼接的时候，它就是调用了 `StringBuilder` .   

惊不惊喜.jpg  

那么，我们是不是就可以理直气壮的说，不需要再声明 SpringBuilder 了呢？    

嗯哼，让我们再写一个示例代码并反编译看一下：  
````java
		String str = "Autuan: ";
        for (int i = 0; i < 10; i++) {
            str += i;
        }
        System.out.println(str);

````  

![循环下的字节码](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/string-builder/stringbuilder-for.png)  

划红线的为进入迭代语句 ( int < 10 ).  

划蓝线的为StringBuilder 创建语句 .   

也就是说，StringBuilder 是在循环体内复用的。  

使用过 StringBuilder 的小伙伴们都知道，StringBuilder 是可变字符串，每次都新建一个新的StringBuilder , 对于性能影响还是很大的。    


总结：  

普通情况下，我们拼接字符串只要使用 `+` `+=` ， 就可以了，因为其底层使用的就是 StringBuilder。  

如果涉及到循环遍历，我们还是要手动声明 StringBuilder . 因为JVM并不会复用同一个 StringBuilder 对象。  

当然，如果有线程安全问题，就不需要想这么多了，请直接使用 StringBuffer 。  

你，学废了吗？   

![nice](https://autuan-blog.oss-cn-shanghai.aliyuncs.com/string-builder/nice.gif)

PS.
JDK 9 之后（包括），将是 invokedynamic ，而不再是 invokevirtual .  