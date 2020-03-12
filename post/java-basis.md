---
title: "java基础知识点整理"
tags: ["Java", "Interview"]
date: 2019-09-11
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

## == equals

- “==”是运算符，在两个变量都为基本数据类型，且为数值类型时，只要两个变量值相等就返回true；当为引用类型变量时比较的是两个引用在内存中的地址是否相同。
- equals方法是由Object类提供的方法，可以由子类来进行重写。默认的实现只有当两者内存中的地址时才会返回true， 这个时候和 “==”是等价的。
- 但是Java中很多类（String类 Date类 File类）等都对equals方法进行了重写，多数重写后只要内部所存值一样就会返回true，与地址无关。

```java
int it = 65;
float fl = 65.0f;
System.out.println(it == fl);//true
char ch = 'A';
System.out.println(it == ch);//true

String str1 = "hello";
String str2 = "hello";
System.out.println(str1 == str2);//true

String str3 = new String("world");
String str4 = new String("world");
System.out.println(str3 == str4);//false
System.out.println(str3.equals(str4));//true
```
对于上面这段代码，运行结果及理由如下：
1. 第一行第二行都为数值类型，所以第三行打印true
2. 第四行字母'A'的ASC码为65，故第五行打印true
3. 第七行代码会在常量池中找“hello”常量，没有找到就创建"hello"常量并将其放到常量池中，然后创建一个String对象str1指向该常量；
4. 第八行仍然会去常量池中找“hello”常量，因为刚才已经创建过了，所以可以找到"hello"常量，然后创建一个String对象str2指向该常量；
5. “==”比较的是两个引用在内存中的地址是否相同，由于str1和str2指向的是同一个常量，所以第九行打印true；
6. 第十一行第十二行分别创建了两个新的String对象，故两者物理地址不同，所以第十三行打印false；
7. String类中重写了equals方法，只要内部所存值一样就会返回true，与地址无关，str3和str4内部存值都是“world”，所以第十四行打印true。

## equals hashcode

hashCode()是Object类的一个方法，返回一个哈希值，它与equals()方法关系特别紧密，常用于基于hash的集合类。

将对象放入到集合中的步骤如下：

![image](/media/posts/java-basis/4.png)

所以equals()相等hashCode()一定相等，hashCode()相等equals()不一定相等

## string stringBuilder stringBuffer

- String
    - final修饰，String类的方法都是返回新的对象（包括修改操作）
    - 对String对象的任何改变都不影响到原对象
- StringBuffer
    - 对字符串的操作的方法都加了synchronized，保证线程安全
- StringBuilder
    - 不保证线程安全
    - 调用StringBuilder对象的append、replace、delete等方法修改字符串不会生成新的对象

## final finally finalize

- final
    - final 修饰的类叫做最终类 该类不可被继承
    - final 修饰的方法不能被重写
    - final 修饰的变量叫常量 常量必须初始化 初始化之后的值不可被修改
- finally
    finally是对 Java 异常处理模型的最佳补充。它只能用在try/catch语句中并且附带着一个语句块，表示不管有无异常发生，finally 代码块总会被执行。
    使用 finally 可以维护对象的内部状态，并可以清理非内存资源。特别是在关闭流这方面，如果程序员把io流的close()方法放到finally中，就会大大降低程序出错的几率。
- finalize
    finalize()是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，供垃圾收集时的其他资源回收，例如关闭文件等。
    finalize()方法是在GC清理它所从属的对象时被调用的，如果执行它的过程中抛出了无法捕获的异常（uncaught exception），GC将终止对改对象的清理，并且该异常会被忽略；直到下一次GC开始清理这个对象时，它的finalize()会被再次调用。
