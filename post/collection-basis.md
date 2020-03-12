---
title: "集合相关知识点整理"
tags: ["Java", "Interview", "Collection"]
date: 2019-11-10
---

Java的集合主要由Collection和Map两个接口派生，这篇文章就来整理一下集合相关的知识点。

<!--more-->

## 数组和集合的区别

- 数组长度固定；集合长度可变
- 数组存储的是同一种类型的元素；集合可以存储不同类型的元素（一般都放同种）
- 数组可以存储基本数据类型,也可以存储引用类型；集合只能存储引用类型（基本数据类型会被自动装箱）

## Collection 整体介绍

![image](/media/posts/collection-basis/1.jpg)

Set和List是Collection派生的两个子接口，Set代表无序不可重复的集合，List代表有序可重复的集合，Queue代表一种队列的集合实现

## 常用 List 介绍

- Vector
    -- 底层数据结构是数组
    -- Vector所有方法都是同步的，线程安全，但同步也会造成性能损失
    -- Vector初始length是10 超过length时 以100%比率增长，相比于ArrayList更多消耗内存
- ArrayList
    -- 底层数据结构是数组
    -- 线程不安全
    -- ArrayList的默认初始化容量是10，每次扩容时候增加原先容量的一半，也就是变为原来的1.5倍
    -- 查询效率更高
- LinkedList
    -- 底层数据结构是双向链表（双向链表方便实现往前遍历）
    -- 线程不安全
    -- 增加删改效率更高

## 常用 Set 介绍

- HashSet
    -- 底层数据结构是HashMap（散列表+红黑树）
    -- 不可重复是指equals和hashCode值都不相等才可将新值存入
    -- 适用于多插入少遍历，无序，允许为null
- LinkedHashSet
    -- 底层数据结构是LinkedHashMap（HashMap+双向链表）
    -- 根据hashCode值来决定元素的存储位置，同时用链表来维护其次序，故惠安插入顺序做为遍历顺序
    -- 适用于多遍历少插入，迭代有序，允许为null
- TreeSet
    -- 底层数据结构是TreeMap（红黑树)
    -- 支持自然排序和定制排序两种排序方法，默认是自然排序（Comparator接口协助实现定制排序）
    -- TreeSet只能添加同种类型的对象，且对象的类必须实现Comparable接口，重写equals()方法（保证该方法与compareTo()方法结果一致，从而实现Set的不可重复）
    -- 适用于有排序要求，有序，不允许为null

## Map 整体介绍

![image](/media/posts/collection-basis/2.jpg)

Map保存的每项数据都是key-value对，key是唯一的value可重复，用于保存具有映射关系的数据，Map集合的数据结构只对键有效，跟值无关。

## 常用 Map 介绍

- HashMap
    -- 底层数据结构是散列表(是一个元素为链表的数组) + 元素数大于8是红黑树
    -- 初始容量的默认值是16，当装载因子*初始容量小于散列表元素时，该散列表会再散列，扩容2倍，装载因子的默认值是0.75
    -- 允许为null，不同步，插入的顺序是有序的(底层链表致使有序)
- LinkedHashMap
    -- 底层数据结构是HashMap+双向链表
- TreeMap
    -- 底层数据结构是红黑树
    -- 允许为null，不同步，默认是根据key自然排序，也可在构造方法中传入Comparator对象定制排序


## HashMap和HashTable的区别

- 同步性：
    -- HashMap是非同步的
    -- Hashtable是同步的
    -- 需要同步的时候，我们往往不使用，而使用ConcurrentHashMapConcurrentHashMap基于JDK1.8源码剖析
- 是否允许为null：
    -- HashMap允许为null
    -- Hashtable不允许为null
- contains方法
    -- Hashtable有contains方法
    -- HashMap把Hashtable的contains方法去掉了，改成了containsValue和containsKey
- 继承不同：
    -- HashMap extends AbstractMap
    -- Hashtable extends Dictionary

![image](/media/posts/collection-basis/3.jpg)

图片来源 Java3y微信公众号 侵权请联系我