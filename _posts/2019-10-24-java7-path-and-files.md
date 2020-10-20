---
layout:     post
title:      Files_文件操作的简化版本,你知道吗?
subtitle:   从Java7开始的,可以大大方便我们的文件操作方法
date:       2019-10-24
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - Java
---

> 软件供应商在努力尝试让他们的软件更'易于操作'…迄今为止，他们最好的办法就是翻出所有的老手册，然后在封面盖上'易于操作'这几个字

## 前言
每个从事Java方面的程序员都知道使用输入流输出流来进行文件操作,但事实上,自Java 7 以来,对于一些简单的操作,有着更方便的手法来处理.

## Path
Path 表示一个文件目录.我们可以使用静态方法 Paths.get() 来取得文件目录.
````Java
  // 获取 Windows 系统下 D盘 java 文件夹下的  file.txt
  Path path1 = Paths.get("d://java//file.txt");
  // 获取相对路径下的file.txt文件
  Path path2 = Paths.get("file.txt");
  // 获取 Linux 系统下 usr文件夹中doc文件夹中的file.txt文件
  Path path3 = Paths.get("/usr","doc","file.txt");
````
相信各位小伙伴通过短短的几行代码就可以看出两个Paths.get的特点:   
* get()方法中的参数是String类型的可变长参数.  
  其中,每一个String参数都表示一个目录或者文件,参数之间使用当前系统的文件分割符分割. 比如说,win下会使用 `\`,而linux下会使用`/`.  

* get()方法会根据会传回绝对路径文件或相对路径文件.  
  如果传入的参数,第一个位置是根路径的话,就会作为一个绝对路径去处理.  win下的根路径是 : `c:\`, 而 Linux 下的根路径则是 / . 如果传入的参数,第一个不是根路径,那么就会作为相对路径处理

需要大家注意的是,在Paths中声明的文件,如我示例的`text.txt`,在该文件目录下是不存在的,是允许的.  

不过,你如果输入的路径不符合当前系统的表达式,则会抛出 InvalidPathException 异常.

比方说,你在Linux下执行了下列代码:  

````Java
/*
 * 在 windows 下正常
 * 在 linux 下会抛出 InvalidPathException
 * 因为在linux下,d://java//file.txt 不是linux文件系统中的合法路径
*/
Path path1 = Paths.get("d://java//file.txt");
````

当你有了路径之后,就可以对文件进行操作了!  

## Files
为了和之前的文件处理区分开来,Java 7 中新加的文件处理类使用了File的复数形式`Files`.  

Files 中最常用的操作自然是读取了:  
````Java
byte[] bytes = Files.readAllBytes(path);
````

当然,既然是Java7出现的新事物,自然有很多可以大大方便我们的方法:   
````Java
Path path1 = Paths.get("d://java//file.txt");
// 将文件内容作为字符读入
String content = new String(Files.readAllBytes(path), StandardCharsets.UTF_8);
// 将文件内容以行为分割元素,以字符串形式读入
List<String> lines = Files.readAllLines(path);
````

有这些方法,在很多情况下可以极大地方便我们操作.  

写方法自然也简便了操作:  
````Java
Path path1 = Paths.get("d://java//file.txt");
String content = "要写入的内容";
List<String> lines = Files.readAllLines(path);

// 直接写入
Files.write(path,content.getBytes());
// 追加
Files.write(path,content.getBytes(), StandardOpenOption.APPEND);
// 将列表写入
Files.write(path,lines);
````

## 其他方法  
读和写方法是我们最常用到的方法.  
但是他们当然不是说只有这些方法.  
其他的还有很多:  
````Java
// 文件复制
Files.copy(source,target);
// 文件移动
Files.move(source,target);
// 文件删除
Files.deleteIfExists(target);
// 创建文件目录
Files.createDirectories(path);
// 创建文件
Files.createFile(path);
````
Files 中有许多方法,展示的仅仅是其中一部分,如果对这些感兴趣的话,推荐阅读[Files的相关API](https://www.w3schools.com/java/java_files.asp).本文不做赘述.

## 注意事项
虽然Path和Files极大方便了我们的操作,但是如果处理的文件较大或者说要处理二进制的文件,推荐小伙伴们还是使用输入流和输出流.  
