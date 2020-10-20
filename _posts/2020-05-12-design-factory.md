---
layout:     post
title:      更快更好的使用工厂模式
subtitle:   通过枚举使用工厂模式
date:       2020-05-12
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2018/10/08/05/17/workshop-3731975_960_720.jpg
catalog: true
tags:
    - 设计模式
    - Java
---

> 计算机会不会思考这个问题就像问潜水艇会不会游泳一样.

## 前言
工厂模式是面试常见的设计模式之一,也是工作中常用的设计模式之一.  

但是工厂模式究竟有如何使用呢?  

下面先定义两个产品类,作为工厂要生产的对象:

```` Java
/**
* 抽象产品类
*/
public interface Car {}

/**
* 产品1
*/
public class GeelyCar implements Car{}

  /**
  * 产品2
  */
public class CheryCar implements Car {}

````  

然后是普通的普通工厂 (´・ω・`)  , 如下:


```` Java
public class GeneralFactory {
    /**
    * 请注意!
    * 在实际实用中如果使用这种方式,一般是需要定义常量类的,本次略过
    */
    public static Car produce(String brand){
        switch (brand) {
            case "Geely" : {
                return new GeelyCar();
            }
            case "Chery" : {
                return new CheryCar();
            }
            default:{
                return null;
            }
        }
    }

    /**
    * 请注意!
    * 在实际实用中如果使用这种方式,一般是需要定义常量类的,本次略过
    */
    public static Car produce(int type){
      switch (type) {
          case 0 : {
              return new GeelyCar();
          }
          case 1 : {
              return new CheryCar();
          }
          default:{
              return null;
          }
      }
  }
}

````

这两种方式在学习中十分常见,如果需要通过工厂方法生产实际,如下 :  

```` Java
// 使用字符串生产
Car car1 = GeneralFactory.produce("Chery");
// 使用int参数生产
Car car2 = GeneralFactory.produce(1);
````

在工作中常见的工厂方式则是通过类型参数生产,如下:

```` Java
public class GeneralFactory {
// ... other methods

public Car produce(Class<? extends Car> c) {
      try {
          return c.newInstance();
      } catch (Exception e) {
          e.printStackTrace();
          return null;
      }
  }
}
````  

生产时则通过类型参数生产:  
```` Java
Car car3 = GeneralFactory.produce(CheryCar.class);
````

以上三种是常见的工厂方法的定义,使用方式.  

但,是不是说只有这三种呢?  

当然不是,事实上我们还可以使用枚举的方式实现工厂:
```` Java
public enum EnumFactory {
    /**
     * 吉利汽车
     */
    GeelyCar {
      @Override
      public Car produce(){
          return new GeelyCar();
      }  
    },
    /**
     * 奇瑞汽车
     */
    CheryCar {
        @Override
        public Car produce(){
            return new CheryCar();
        }
    };

    public abstract Car produce();
}
````

使用时也是非常的简单:  
```` Java
Car car4 = EnumFactory.CheryCar.produce();
````

通过枚举创建工厂有几个优点:
* 降低类之间耦合
* 避免错误
* 性能好

工厂模式在工作中的应用场景还是很多的,比如说Spring就是使用工厂模式通过`BeanFactory`和`ApplicationContext`来创建Bean对象的  

所以博主推荐大家,如果无特别需要,一般的工厂模式只使用枚举方式就可以了. (ゝ∀･)  

## 更新记录
2020.05.12 发布  

## 参考文档
[1] 秦小波. 编写高质量代码:改善Java程序的151个建议[M].北京：机械工业出版社，2011：155-157.
[1] [美]Craig Walls. Spring IN ACTION[M].张卫滨 译.北京：人民邮电出版社，2016：33-65.  
