---
title: "面向对象知识点整理"
tags: ["Java", "Interview"]
date: 2019-10-01
---

之前整理了Java基础的知识点，这篇文章就来整理一下面向对象相关的知识点。

<!--more-->

## 成员变量 局部变量

- 成员变量是指在类中定义的变量
- 局部变量是指在方法中定义的变量

具体变量分类如下图所示：

![image](/media/posts/object-oriented-basis/1.png)

注意：局部变量名和成员变量名相同时 局部变量覆盖成员变量

## 访问控制符

![image](/media/posts/object-oriented-basis/2.png)

关于访问控制符的使用，存在如下几条基本原则：

- 大部分成员变量都应该用private修饰，只有一些需要全局使用的静态变量会用public和static修饰
- 只有父类中的方法仅希望被子类重写且不被外界调用，才使用protected
- 可以被其他类调用的方法用public修饰，仅在该类中使用的方法用private修饰

## 面向对象三大特征

- 封装：将对象的状态信息隐藏在对象内部，可通过该类提供的方法访问或操作内部信息，不允许外界直接访问
- 继承：用extends关键字实现继承，继承的是子类，被继承的是父类
    - 子类可以获得父类的全部成员变量和方法（不能获得父类的构造器，可以通过supper()调用父类构造器）
    - 子类重写父类方法需要两同两小一大（方法名、形参列表相同，返回值类型及抛出异常比父类小或相等，访问权限比父类大或相等）
- 多态
    - 编译类型为父类运行类型为子类的对象，调用子类重写父类的方法后，会表现出子类方法的行为特征
    - 如上就会出现：相同类型的变量调用同一个方法时会呈现出不同的行为特征，这就是多态
    
```java
// 父类
public class Parent {
    public String write(){
        return "Parent";
    }
}
// 子类
public class Children extends Parent {
    public String write(){
        return "Children";
    }
}
// 测试
public void test() {
    Parent parent = new Parent();
    System.out.println("父类编译对象 父类运行对象 " + parent.write());
    Children children = new Children();
    System.out.println("子类编译对象 子类运行对象 " + children.write());
    Parent polymorphism = new Children();
    System.out.println("父类编译对象 子类运行对象 " + polymorphism.write());
}
```

输出结果：

![image](/media/posts/object-oriented-basis/3.png)

其中第一、三行打出的信息不同，即为多态。

也就是说引用变量在编译时并不确定会指向哪一个实例对象，只有在运行时才能确定具体的类，这样该引用变量可以绑定到不同的对象上，导致该引用调用的方法也随之改变，程序可以选择多个运行状态，这就是多态。

## final相关知识点

- final修饰的成员变量必须显示赋值，系统不会对其进行隐形的初始化
- final修饰的成员变量为基本数据类型时，不能对其重新赋值，即该变量不能被改变
- final修饰的成员变量为引用变量时，仅限制其所引用地址不变，即一直指向同一个对象，但这个对象完全可以改变
- final修饰的方法不可以被重写，通常用于修饰父类中不想被子类重写的方法
- final修饰的类不可以被继承

## 