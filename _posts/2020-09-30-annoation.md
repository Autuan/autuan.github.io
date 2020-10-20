---
layout:     post  
title:      使用注解来简单实现aop功能  
subtitle:   初识注解  
date:       2020-09-30 
author:     Autuan.Yu 
header-img: 
catalog: true
tags:
    - Java
	- Spring
---

> 欲穷千里目，更上一层楼。

## 前言
如果看这篇文章的小伙伴有过找Java方面工作的经历，一定会对两个知识点印象深刻： Spring 的 `aop` / `ioc` 。  

我也相信有很多人能够立刻说出他们是什么： aop -> 切面编程嘛，ioc -> 控制反转嘛， 很好记。  

可是，当深入一些，问到它们的应用场景时，很多人就会露出一脸茫然的表情来。  

这次，我就简单的说一下aop的应用场景。  

如有错误，还烦请各位大佬指教。  

![一脸茫然](https://i.loli.net/2020/09/30/TuEj2nPgN1bOyBA.png)

## 理论知识回顾
不论是新学，还是复习，我们对待知识的时候，如果有可能，我们尽量要知其所以然，而不仅仅是知其然。  

那么，我们先简单的复习一下 spring 以及 aop 相关的理论，放心，不会很长。  



众所周知，spring 可以简单的理解为一个容器，管理我们的对象。 

而对象中又有各自的方法函数去执行。  

![容器](https://i.loli.net/2020/09/30/O3uMicYjw2kWdln.png)  


那么，在容器里的方法执行时，自然就有个执行前，执行后的概念。  

通过这个概念，我们就可以在方法的执行前，执行后再去做一些别的操作。  

而这种概念，就叫做切面。  

通过切面，我们可以将原本臃肿地在同一个方法里的代码，优雅地改写。 

before:   

```` java
void exampleFun(){
	// do some check 
	...
	// do main
	... 
	// do log
	...
}

````  

after :
```` java
void before(){
	// do some check
	...
}
void example(){
	// do main
	...
}
void after(){
	// do log
	...
}

````

如果是一个刚接触的同学，可能会有困惑，之前的写法不是也挺好吗？ 换了一种方法更复杂了不是吗？  

首先，我打了个比较常见的比方，在 before 中的 exampleFun() 这个方法中，一共做了3件事：  

* 进行一些校验，在实际项目中，我们经常要做一些权限验证，毕竟，有些操作是不能让随便什么人都可以做的  
* 做这个方法自己的事  
* 保存一下操作记录，也就是log， log 也是我们项目中常用到的功能，可以方便程序出问题之后进行数据恢复。

在这三件事中，首先呢，开始的校验和结束的日志，是个很多处都需要用到的地方，如果采用 before 中的写法，那么在其他方法中，我们还是要再cv一次，使代码极度膨胀，也不符合现在的代码规范：  

```` java
void exampleFun(){
	// do some check 
	...
	// do main
	... 
	// do log
	...
}

void otherFun(){
	// check 和 log 的重复代码需要cv
	// do some check 
	...
	// do main
	... 
	// do log
	...
}

````  

其次，即使 `exampleFun` 这个方法只执行一次，该方法也违反了函数方法的准则： 一个方法只做一件事。  

所以，我们是必须要让 before 中的代码臭味道散掉的。  

而在spring中， aop 就可以很优雅地完成这事。   


## 注解
使用aop 的最常见形式就是使用注解， 通过自定义注解, 我们可以大大提高代码的可读性并避免令人厌恶的重复代码。
假设我们已经写好了两个自定义注解`Log` 及 `SecurityCheck`，我们的方法只需做自己需要做的事就行了：  

```` java
@Log
@SecurityCheck(permission = "admin")
void exampleFun(){
	// do it
	... 
}

@Log
@SecurityCheck(permission = "user")
void otherFun(){
	// do it
	... 
}

````

代码是不是清爽了许多？  

那么，自定义注解，也就是切面，要如何实现呢？  

### 实现自己的注解
使用自定义注解，我们需要先新建一个类。

当然类型不能再定义为普通的`class`对象了，要定义为注解使用的接口。  

```` java
public @interface MyAnnotation {
}
````

#### 应用场景  
我相信即使没写过自定义注解，大部人也都以各种方式使用过注解，比如 `@Autowired`,`@Service`, `@Override` 等，相信大家也都知道这些注解的应用场景：  

* @Autowired 应用在 变量属性上
* @Service 应用在 类上
* @Override 应用在 方法上

大家会发现，不同的注解可以用于在不同的位置，我们的自定义注解当然也需要如此。

我们需要在我们的自定义注解上打上 `@Target` 注解，用来声明注解的应用范围。

所有的可选范围都在一个枚举对象中：

>   /** Class, interface (including annotation type), enum, or record
>     * declaration */
>    TYPE,
>
>    /** Field declaration (includes enum constants) */
>    FIELD,
>
>    /** Method declaration */
>    METHOD,
>
>   /** Formal parameter declaration */
>    PARAMETER,
>
>    /** Constructor declaration */
>    CONSTRUCTOR,
>
>    /** Local variable declaration */
>    LOCAL_VARIABLE,
>
>    /** Annotation type declaration */
>    ANNOTATION_TYPE,
>
>    /** Package declaration */
>    PACKAGE,
>
>    /**
>     * Type parameter declaration
>     *
>     * @since 1.8
>     */
>    TYPE_PARAMETER,
>
>    /**
>     * Use of a type
>     *
>     * @since 1.8
>     */
>    TYPE_USE,
>
>    /**
>     * Module declaration.
>     *
>     * @since 9
>     */
>    MODULE,
>
>    /**
>     * {@preview Associated with records, a preview feature of the Java language.
>     *
>     *           This constant is associated with <i>records</i>, a preview
>     *           feature of the Java language. Programs can only use this
>     *           constant when preview features are enabled. Preview features
>     *           may be removed in a future release, or upgraded to permanent
>     *           features of the Java language.}
>     *
>     * Record component
>     *
>     * @jls 8.10.3 Record Members
>     * @jls 9.7.4 Where Annotations May Appear
>     *
>     * @since 14
>     */
>    @jdk.internal.PreviewFeature(feature=jdk.internal.PreviewFeature.Feature.RECORDS,
>                                 essentialAPI=true)
>    RECORD_COMPONENT;

我们今天只要开始做一个简易的demo就行，标记方法即可：  

```` java
@Target({ElementType.METHOD})
public @interface MyAnnotation {
}
````

#### 生命周期  

有一些小伙伴可能使用过lombok，用过`@Data`之类的注解，使用该注解后，在已编译后的class文件中，是没有 @Data 这个注解的，取而代之的，则是 Getter 和 Setter 方法：  
![class文件中](https://i.loli.net/2020/09/30/qefUOKxPB6GuLVj.png)

这个就涉及到注解的生命周期了。  

注解的生命周期一般可以划为三类:  

- RetentionPolicy.SOURCE
- RetentionPolicy.CLASS
- RetentionPolicy.RUNTIME  

分别对应了 源码、class文件、机器码，我们把我们的自定义注解声明为等级最高的机器码，使用另一个元注解 `@Retention` 来注明生命周期：  

```` java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
}
````
#### 注解属性 
优秀的各位想必已经发现了，和 `@Autowired`  `@Data` 这些注解相比，`@Target` 和 `@Retention` 是有参数的。  

事实上，在注解中使用参数是一件很常见的事，比如说 `@RequestMapping` 中，就需要配置请求路径，那么让我们来为自己的注解添加参数：  

```` java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String msg() default "";
    String title() default "";
}

````

定义的属性，实际上是方法， `defalut` 是指默认值。

### 使用这个注解
至此，我们已经定义了一个简单的注解，但是仅仅定义了一个注解是不行的。就像我们在实际开发中，只定义了一个对象，但是却没有用到，那么这个对象实际上就是个无用功。  

首先，我们定义一个新类，类名就叫做 `MyAnnotationAspect`   

```` java
@Aspect
@Component
public class MyAnnotationAspect {
}

````

其中，`@Component`是声明一个组件，交由spring管理，其次，`@Aspect`则是切面声明，在文章开头，我也说了本文主旨是切面相关的。  

有了这个类，我们需要做什么呢？  

很简单，两件事便可：  

第一件事，是声明切入点，这就好像切西瓜，你说你想切成两半，那么首先得知道在哪个位置切吧？ 在空气中乱砍可不行。   

声明切入点的方法很简单：编写一个方法，通过注解的方式声明即可：  

```` java
	@Pointcut("@annotation(com.autuan.webdemo.project.aop.MyAnnotation)")
    public void aspectLocation(){

    }

````

第二件事：就是我们要做的事，我们写了一个切面，总得是想实现什么功能吧？ 比如说日志，权限验证什么的。  

本文因为是个 demo ， 就只做了打印操作：  
 
```` java
	@Around("aspectLocation()")
    public void doMyWant(ProceedingJoinPoint pjp){
        Signature signature = pjp.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();
        MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
        String msg = annotation.msg();
        String title = annotation.title();

        System.out.println("自定义注解执行---》");
        System.out.println("自定义注解 ---》 msg ---> " + msg);
        System.out.println("自定义注解 ---》 title ---> " + title);
    }
````

现在，我们已经做好注解的切面逻辑了，那么，一切都准备就绪了吗？  No.  

在之前，已经说过，我们对这个注解的应用场景定义的是 方法级，那么我们还得需要在方法中标记上这个注解才行。  

在我们的 `Controller` 中定义一个方法：  

```` java
	@PostMapping("/update")
    @MyAnnotation(msg = "this is msg",title = "hello,autuan")
    public Object update() {
        return ResDTO.builder()
                .responseId("hello,Autuan")
                .build();
    }

````

然后通过浏览器访问，会发现控制台打印：  

![print](https://i.loli.net/2020/09/30/LlIFPWQj3ZkEpAD.png)

nice! 你已经初步的掌握了切面和注解。  

可喜可贺！

