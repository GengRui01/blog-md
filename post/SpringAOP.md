---
title: "SpringAOP"
tags: ["Spring"]
date: 2018-09-11
---

之前整理了SpringIOC的笔记，Spring的核心特性就是IOC和AOP，这篇文章就来写一下SpringAOP（Aspect Oriented Programming），即：面向切面编程，是可以通过预编译方式和运行期动态代理实现在不修改源代码的情况下给程序动态统一添加功能的一种技术。

<!--more-->

SpringAOP的作用就是把程序中重复的代码抽取出来，在需要执行的时候，使用动态代理技术，在不修改源码的基础上，对已有方法进行增强。优势就是减少了重复代码，提高代码复用性，提高开发效率，使得代码的维护更加方便。

这里提到了动态代理的概念，首先解释一下代理模式就是给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用，通俗的来讲代理模式就是生活中常见的中介。代理模式的存在有如下两个意义：

- 中间隔离作用：在一些情况下，一个客户类不想或者不能直接引用一个委托对象，而代理类对象可以在客户类和委托对象之间起到中介的作用，其特征是代理类和委托类实现相同的接口
- 开闭原则，增加功能：给代理类增加额外的功能可以用来扩展委托类的功能，这样做只需要修改代理类而不需要再修改委托类，符合代码设计的开闭原则。这里再说下代理类和委托类分别实现什么功能：
    - 代理类主要负责为委托类预处理消息、过滤消息、把消息转发给委托类，以及事后对返回结果的处理等。代理类本身并不真正实现服务，而是同过调用委托类的相关方法，来提供特定的服务
    - 委托类实现真正的业务功能，也可以在业务功能执行的前后加入一些公共的服务。例如将项目加入缓存、日志这些功能就可以使用代理类来完成，没必要打开已经封装好的委托类。

代理模式按照其创建时间可以分为静态代理和动态代理：

- 静态代理是由程序员创建或特定工具自动生成源代码，在程序运行之前，代理类就已经编译生成了.class文件。
  - 静态代理的优点是可以在符合开闭原则的情况下对目标对象进行功能扩展，缺点则是开发人员需要为每一个服务都得创建代理类，工作量太大，不易管理。同时接口一旦发生改变，代理类也得相应修改
- 动态代理是在程序运行时通过反射机制动态创建的，随用随加载。动态代理常用的有基于接口和基于子类两种方式
  - 基于接口的动态代理指的是由JDK官方提供的Proxy类，要求被代理类最少实现一个接口，这种方式大大减少了开发人员的开发任务，减少了对业务接口的依赖，降低了耦合度，缺点就是注定有一个共同的父类叫Proxy，Java的继承机制注定了这些动态代理类们无法实现对class的动态代理，原因是多继承在Java中本质上就行不通
  - 基于子类的动态代理指的是由第三方提供的CGLib，CGLib采用了非常底层的字节码技术，其原理是通过字节码技术为一个类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，顺势织入横切逻辑。但因为采用的是继承，所以要求被代理类不能用final修饰，即不能是最终类。

以上的JDK动态代理与CGLib动态代理两种动态代理的方式都是实现SpringAOP的基础。在spring中，虽然引入了AspectJ的语法，但是他本质上使用的是动态代理的方式，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。如果目标对象有接口，优先使用JDK 动态代理，如果目标对象没有接口，则使用CGLib动态代理。

在开发时通常将日志记录，数据库连接池的管理，系统统一的认证、权限管理等用面向切面的方式开发。

## 基于XML的AOP配置

基于XML的AOP的配置常用的标签如下：

- <aop:config>
  - 作用：用于声明开始aop的配置
- <aop:aspect>
  - 作用：用于配置切面
  - 属性：
    - id：给切面提供一个唯一标识
    - ref：引用配置好的通知类bean的id
- <aop:pointcut>
  - 作用：用于配置切入点表达式
  - 属性：
	- expression：用于定义切入点表达式
	- id：用于给切入点表达式提供一个唯一标识
- <aop:before>
  - 作用：用于配置前置通知
  - 执行时间点：切入点方法执行之前执行
  - 属性：
	- method：指定通知中方法的名称
	- pointct：定义切入点表达式
	- pointcut-ref：指定切入点表达式的引用
- <aop:after-returning>
  - 作用：用于配置后置通知
  - 执行时间点：切入点方法执行之后执行
  - 属性：
	- method：指定通知中方法的名称
	- pointct：定义切入点表达式
	- pointcut-ref：指定切入点表达式的引用
- <aop:after-throwing>
  - 作用：用于配置异常通知
  - 执行时间点：切入点方法执行产生异常后执行
  - 属性：
	- method：指定通知中方法的名称
	- pointct：定义切入点表达式
	- pointcut-ref：指定切入点表达式的引用
- <aop:after>
  - 作用：用于配置最终通知
  - 执行时间点：无论切入点方法执行时是否有异常，它都会在其后面执行
  - 属性：
	- method：指定通知中方法的名称
	- pointct：定义切入点表达式
	- pointcut-ref：指定切入点表达式的引用
- <aop:around>
  - 作用：用于配置环绕通知
  - 他和前面四个不一样，他不是用于指定通知方法何时执行的。它是spring框架为我们提供的一种可以在代码中手动控制增强部分什么时候执行的方式
  - 属性：
	- method：指定通知中方法的名称
	- pointct：定义切入点表达式
	- pointcut-ref：指定切入点表达式的引用

## 基于注解的AOP配置

基于注解的AOP配置常用的注解如下：

- @Aspect 对应标签：<aop:aspect>
- @Before 对应标签：<aop:before>
- @AfterReturning 对应标签：<aop:after-returning>
- @AfterThrowing 对应标签：<aop:after-throwing>
- @After 对应标签：<aop:after>
- @Around 对应标签：<aop:around>
- @Pointcut 对应标签：<aop:pointcut>

## JdbcTemplate对象

传统的JDBC虽然执行速度很快，但开发效率很低。随着面向对象开发的设计思想，在编程中将对象进行持久化，但存入数据库时由于关系数据库是数学思维，在持久化时必须将对象拆分为一个一个的属性值才可存入数据库。为了让调节面向对象设计的开发者思想与落后数据库之间持久化时的繁琐问题，产生了ORM设计规范，即对象关系映射设计规范。该设计模式的优点是开发效率高，可维护性强；缺点是高效率开发以及处理复杂关系时性能会变差

JdbcTemple是Spring提供的一个对象，是对传统JDBC API对象的简单封装，以达到简化开发的目的。

在使用Spring的JDBC时开发者可以不用管连接，不管事务，不管异常，不管关闭。使用不知第一步是获取Spring容器；第二步根据id获取bean对象；之后执行增删改查操作。JdbcTemple主要提供了如下五类操作方法：

1. execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
2. update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；
3. batchUpdate方法用于执行批处理相关语句；
4. query方法及queryForXXX方法：用于执行查询相关语句；
5. call方法：用于执行存储过程、函数相关语句。

## Spring中的事务控制

在分层开发中，事务处理一般位于业务层，Spring在JAR包中提供了一组事务控制的接口，可作为分层设计业务层的事务处理解决方案。Spring中事务控制的API主要有如下三种：
- PlatformTransactionManager是Spring的事务管理器，提供了操作事务的方法，如下三个方法经常使用：
  - TransactionStatus getTransaction(TransactionDefinition transactionDefinition)传入事务的定义信息对象  返回事务状态信息
  - void commit(TransactionStatus transactionStatus)提交事务
  - void rollback(TransactionStatus transactionStatus)回滚事务
- TransactionDefinition是事务的定义信息对象，里面包含如下信息：
  - int getIsolationLevel()事务的隔离级别：反应事务提交并发访问时的处理态度，包含如下几个级别：
    - ISOLATION_DEFAULT 默认级别
    - ISOLATION_READ_UNCOMMITTED 可以读取未提交数据
    - ISOLATION_READ_COMMITTED 只能读取已提交数据 解决脏读问题（Oracle默认级别）
    - ISOLATION_REPEATABLE_READ 是否读取其他事务提交修改后的数据，解决不可重复读问题（MySQL默认级别）
    - ISOLATION_SERIALIZABLE 是否读取其他事务提交添加后的数据，解决幻影读问题
  - int getPropagationBehavior()事务的传播行为
    - REQUIRED：如果当前没有事务，就新建一个事务；如果已经存在事务，加入到这个事务中。（默认值）
    - SUPPORTS：支持当前事务，如果没有当前事务，就以非事务方式执行
    - MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
    - REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起
    - NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
    - NEVER：以非事务方式运行，如果当前存在事务，抛出异常
    - NESTED：如果当前存在事务，则在嵌套事务内执行；如果当前没有事务，则执行REQUIRED类似的操作
  - int getTimeout()事务超时时间
    - 默认值是-1，没有超时限制；如果有，以秒为单位进行设置
  - boolean isReadOnly()标记是否为只读事务
    - 一般在查询时会将事务设置为只读
- TransactionStatus是事务具体的运行状态，描述了某个时间点上事务的状态信息，包含6个具体的操作：
  - void flush() 刷新事务
  - boolean hasSavepoint() 获取是否存在存储点
  - boolean isCompleted() 获取是否完成
  - boolean isNewTransaction() 获取是否为新的事务
  - boolean isRollbackOnly() 获取事务是否回滚
  - void setRollLBackOnly() 设置事务回滚

## 基于XML的声明式事务控制

- <tx:advice> 配置一个事务
- <tx:attributes> 位于tx:advice标签内部，用于配置事务的属性，其内部使用<tx:method>配置每一条方法，tx:method有如下属性：
  - name：定方法名称，是业务核心方法
  - read-only：是否是只读事务，默认是false，不只读
  - isolation：指定事务的隔离级别，默认值是使用数据库的默认隔离级别
  - propagation：指定事务的传播行为
  - timeout：指定超时时间，默认值为-1，永不超时
  - rollback-for：用于指定一个异常，当执行产生该异常时，事务回滚；产生其他异常，事务不回滚；没有默认值，任何异常都回滚
  - no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚；产生其他异常时，事务回滚；没有默认值，任何异常都回滚
- <aop:pointcut> 在aop:config标签内部，用于配置AOP-切入点表达式
- <aop:advisor>在aop:config标签内部，用于配置切入点表达式和事务通知的对应关系