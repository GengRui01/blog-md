---
title: "多线程知识点整理"
tags: ["Java", "Interview", "Thread"]
date: 2019-11-30
---

多线程相关知识点整理

<!--more-->

## 线程池

概念：简单理解就是一个管理线程的池子

- 线程池协助管理线程，避免增加创建线程和销毁线程的资源损耗
- 从线程池拿线程比重新去创建一条线程更快，所以线程池可以提高响应速度
- 线程用完，再放回池子，可以达到重复利用的效果，节省资源

## 线程池的创建

线程池可以通过ThreadPoolExecutor来创建，创建需要的核心参数的作用如下：

- corePoolSize： 线程池核心线程数最大值
- maximumPoolSize： 线程池最大线程数大小
- keepAliveTime： 线程池中非核心线程空闲的存活时间大小
- unit： 线程空闲存活时间单位
- workQueue： 存放任务的阻塞队列
- threadFactory： 用于设置创建线程的工厂，可以给创建的线程设置有意义的名字，可方便排查问题
- handler： 线城池的饱和策略事件，主要有四种类型

## 线程池任务执行

![image](/media/posts/thread-basis/1.jpg)

- 提交一个任务，线程池里存活的核心线程数小于线程数corePoolSize时，线程池会创建一个核心线程去处理提交的任务。
- 如果线程池核心线程数已满，即线程数已经等于corePoolSize，一个新提交的任务，会被放进任务队列workQueue排队等待执行。
- 当线程池里面存活的线程数已经等于corePoolSize了,并且任务队列workQueue也满，判断线程数是否达到maximumPoolSize，即最大线程数是否已满，如果没到达，创建一个非核心线程执行提交的任务。
- 如果当前的线程数达到了maximumPoolSize，还有新的任务过来的话，直接采用拒绝策略处理。

## Synchronized和lock的区别

![image](/media/posts/thread-basis/2.png)

图片来源 https://blog.csdn.net/hefenglian 侵权请联系我