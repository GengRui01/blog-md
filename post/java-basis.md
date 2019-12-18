---
title: "java基础知识点整理"
tags: ["Java", "Interview"]
date: 2019-10-11
---

在公司已经呆久了，经常写业务代码学新东西，基础都忘得差不多了，深拷贝浅拷贝导致的一个bug居然缠了我两个小时，还是在同事协助下找到的问题，是时候花点时间回顾一下基础了，顺带整理成文档，以后换工作面试前复习估计也会用到

<!--more-->

## JDK JRE JVM

JDK 全称 Java SE Development Kit，即java的标准版开发包，它提供了编译、运行Java程序所需的各种工具和资源，包括常用Java类库、Java编译器及Java运行环境等

这里提到的“Java运行环境”就是 Java Runtime Environment，即 JRE，也就是说 JDK 中是包含 JRE 的，JRE中包含Java虚拟机、类加载器、字节码校验器 等

这里提到的“Java虚拟机”就是 Java Virtual Machine，即 JVM，也就是说 JRE 中是包含 JVM 的

故三者的关系 如下图所示：

![image](/media/posts/java-basis/1.png)

## 数据类型 装箱 拆箱

**1. 数据类型**

![image](/media/posts/java-basis/2.png)

**注：** 将 表数范围小的数据类型 赋值给 表数范围大的数据类型 会进行自动类型转换 反之则需要强制类型转换

**2. 装箱 & 拆箱**

- 装箱：将基本数据类型转换为包装类型
- 拆箱：将包装类型转换为基本数据类型

```java
// 自动拆箱
Integer I = 1;
int i = I; // 相当于下面一行
// int i = I.intValue();

// 自动装箱
double d = 0.1;
Double D = d; // 相当于下面一行
// Double D = new Double(d);
```

## 二维数组堆栈图

```java
// 1. 定义一个二维数据
int[][] intArrays;
// 2. 将 intArrays 初始化为长度为 length 的数组 其数组元素为引用类型
intArrays = new int[3][];
// 3. 将 intArrays 的第一个元素初始化为长度为 length 的数组
intArrays[0] = new int[2];
// 4. 将 intArrays 的第一个元素所指数组的第二个元素设置为 1
intArrays[0][1] = 1;
```

上述代码画出的堆栈图如下：

![image](/media/posts/java-basis/3.png)

由上图可知：从底层机制而言不存在多维数组，所谓二维数组也就是一维数组中元素为引用类型，指向另一个数组而已

## for for:loop foreach

- for:loop 不可以新增或删除集合中的元素
- foreach 不可以新增或删除集合中的元素 & 不可以修改循环外的变量值

## == 和 equals 的区别

- “==”比较的是两个引用在内存中的地址是否相同，也就是说在内存中的地址是否一样。
- equals方法是由Object类提供的，可以由子类来进行重写。默认的实现只有当对象和自身进行比较时才会返回true， 这个时候和 “==”是等价的。

Java中很多类（String类 Date类 File类）等都对equals方法进行了重写，多数重写后只要内部所存值一样就会返回true，与地址无关。

```java
String str1 = "hello";
String str2 = "hello";
System.out.println(str1 == str2);//true

String str3 = new String("world");
String str4 = new String("world");
System.out.println(str3 == str4);//false
System.out.println(str3.equals(str4));//true
```
对于上面这段代码，运行结果及理由如下：
1. 第一行代码会在常量池中找“hello”常量，没有找到就创建“hello”常量并将其放到常量池中，然后创建一个String对象str1指向该常量；
2. 第二行仍然会去常量池中找“hello”常量，因为刚才已经创建过了，所以可以找到“hello”常量，然后创建一个String对象str2指向该常量；
3. “==”比较的是两个引用在内存中的地址是否相同，由于str1和str2指向的是同一个常量，所以第三行打印true；
4. 第五行第六行分别创建了两个新的String对象，故两者物理地址不同，所以第七行打印false；
5. String类中重写了equals方法，只要内部所存值一样就会返回true，与地址无关，str3和str4内部存值都是“world”，所以第八行打印true。

## string stringBuilder stringBuffer

- String
    - final修饰，String类的方法都是返回新的对象（包括修改操作）
    - 对String对象的任何改变都不影响到原对象
- StringBuffer
    - 对字符串的操作的方法都加了synchronized，保证线程安全
- StringBuilder
    - 不保证线程安全
    - 调用StringBuilder对象的append、replace、delete等方法修改字符串不会生成新的对象
