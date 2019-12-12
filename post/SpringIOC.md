---
title: "SpringIOC"
tags: ["Spring"]
date: 2018-08-10
---

Spring的核心特性就是IOC和AOP，这篇文章就来写一下SpringIOC（Inversion of Control），即：控制反转

<!--more-->

在我之前的文章[JDBC连接MySQL数据库](https://gengrui01.github.io/post/jdbc-connection-mysql/)中写到过耦合，提过的解耦办法有如下两个：

- 通过反射来注册驱动：保证了在没有驱动包的情况下编译不会失败，但会因为驱动包的不存在而无法运行

- 使用配置文件配置信息：保证部分信息的修改不需要改动源码，提高了代码复用性以及可维护性

其实在实际开发中可以把所有的对象（类似于dao、service、controller和mapper等）使用配置文件配置起来，当启动服务器应用加载的时候，通过读取配置文件，把这些对象创建并存起来。在接下来的使用的时候，直接拿过来用就好了，不需要再去创建。

上述解耦方式称为工厂模式解耦，这种解耦思路还有两个问题需要理解：

**1、启动服务器时创建的对象要存到哪里去？**

因为有多个对象要存，所以需要在Map和List里选一个来存储这些对象路径，由于这些对象在创建后经常需要使用，所以选择更容易进行查找操作的Map集合。也就是说，在我们的应用加载时就会创建一个Map，用于存放dao、service、controller等对象，这个Map也就是容器。

**2、“工厂”的含义**

工厂存在的意义就是从容器中获取指定对象的类，“工厂”的出现导致获取对象的方式发生了改变。

在使用工厂模式之前程序需要获取对象都是用new语句，在虚拟机初始化Java类的时候遇到“new”就会创建对象，当该对象不再使用，垃圾回收机制会自动进行内存管理，将其销毁。

现在加入工厂之后，工厂会读取配置信息，当应用加载创建容器时部分对象就被创建并放到了容器中，其他对象也在第一次使用时会创建好存放在容器中，程序需要获取对象就和工厂要，工厂会在容器中找到对象然后提供给程序，程序只能被动的获取工厂注入的对象，不能主动的new对象。

![image](/media/posts/SpringIOC/1.png)

这种通过被动接收来获取对象的思想就是Spring框架的核心思想之一：控制反转（Inversion Of Control），其作用就是将对象放进容器里，减少程序耦合关系。

## Spring基于XML的IOC配置

Spring中工厂的类结构图如下图所示：

![image](/media/posts/SpringIOC/2.png)

其中BeanFactory是Spring容器中的顶层接口，ApplicationContext是它的子接口，两者的主要区别在于创建时间的不同：

- ApplicationContext：只要一读取配置文件，默认就会创建对象

- BeanFactory：什么使用什么时候创建对象

ApplicationContext接口有ClassPathXmlApplicationContext和FileSystemXmlApplicationContext两个实现类，两者的主要去辨别在于所加载的配置文件路径不同：

- ClassPathXmlApplicationContext：从类的根路径下加载配置文件（推荐使用这种）

- FileSystemXmlApplicationContext：从磁盘路径上加载配置文件 配置文件可以在磁盘的任意位置

bean标签的作用是用于配置对象让spring来创建，在默认情况下它会根据默认无参构造函数来创建类对象，如果bean中没有默认无参构造函数将会创建失败。每个bean标签包含如下属性：

- id：给对象在容器中提供一个唯一标识。用于获取对象。

- class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。

- scope：指定对象的作用范围，有如下几种作用范围的选择

    - singleton：单例对象，一个应用只有一个对象的实例。它的作用范围就是整个引用。当应用加载创建容器时，对象就被创建了；只要容器在，对象一直活着；当应用卸载，销毁容器时，对象被销毁。
    - prototype：多例对象，每次使用对象时，都会创建对象实例；只要对象在使用中，就一直活着；当对象长时间不用时，会被java的垃圾回收器所回收。
    - request：WEB项目中，Spring创建一个Bean的对象，将对象存入到request域中。
    - session：WEB项目中，Spring创建一个Bean的对象，将对象存入到session域中。
    - globalSession：WEB项目中，应用在Portlet环境，如果没有Portlet环境那么globalSession相当于session。
    
- init-method：指定类中的初始化方法名称。

- destroy-method：指定类中销毁方法名称。

刚才有提到“工厂会在容器中找到对象然后提供给程序”，这种“提供”称为“依赖注入”，是SpringIOC的具体实现方式，简单来说就是坐等工厂把对象传入，而不用我们自己去获取。有如下四种注入方式：

- 构造函数注入：使用类中的构造函数，给成员变量赋值。其中赋值的操作不是我们自己做的，而是通过配置的方式，让spring框架来为我们注入。

- set方法注入：在类中提供需要注入成员的set方法。
- 使用p名称空间注入数据：通过在xml中导入p名称空间，使用p:propertyName来注入数据，它的本质仍然是调用类中的set方法实现注入功能。
- 注入集合属性：给类中的集合成员传值，它用的也是set方法注入的方式，只不过变量的数据类型都是集合。

## Spring基于注解的IOC配置

常用的IOC配置的Spring注解分为创建对象、注入数据、改变作用范围以及生命周期相关的注解，以下为对这四种注解的详细介绍：

- 用于创建对象的（相当于<bean id="" class="">）

    - @Component：用来把资源让spring来管理，相当于在xml中配置一个bean。value属性用来指定bean的id，如果不指定value属性则默认bean的id是当前类的类名（首字母小写）
    - @Controller @Service @Repository：这三个是@Component的衍生注解，他们的作用和属性都是一样的，注意如果注解中有且只有一个属性要赋值且名称是value时，value在赋值时可以不写。其中@Controller一般用于表现层的注解，@Service一般用于业务层的注解，@Repository一般用于持久层的注解。
    
- 用于注入数据的（相当于<property name="" ref="">和<property name="" value="">）

    - @Autowired：自动按照类型注入。当使用此注解注入属性时，set方法可以省略。它只能注入其他bean类型，当有多个类型匹配时，使用要注入的对象变量名称作为bean的id，在spring容器查找，找到了也可以注入成功，找不到就报错。
    - @Qualifier：在自动按照类型注入的基础之上，再按照Bean的id注入。它在给字段注入时不能独立使用，必须和@Autowire一起使用，但是给方法参数注入时可以独立使用。
    - @Resource：直接按照Bean的id注入，只能注入其他bean类型。其中name属性用于指定bean的id。
    - @Value：用于注入基本数据类型和String类型数据，其中value属性用于指定值。

- 用于改变作用范围的（相当于<bean id="" class="" scope="">）

    - @Scope：指定bean的作用范围，其中value属性的值可选择“singleton”、“prototype”、“request”、“session”、“globalsession”五种用于指定范围的值。

- （**不常用**）和生命周期相关的（相当于<bean id="" class="" init-method="" destroy-method="" />）

    - @PostConstruct：用于指定初始化方法
    - @PreDestroy：用于指定销毁方法

上面写了两种配置方式，其实注解配置和xml配置要实现的功能都是一样的，目的都是要降低程序间的耦合，只是配置的形式不一样而已。其中XML的优势是在修改时不用改源码，不涉及重新编译和部署；注解的优势是配置简单，维护方便（我们找到类，就相当于找到了对应的配置）。接下来比较一下两种配置管理Bean方式的不同：

|  /          | 基于XML的配置                                 | 基于注解的配置|
| :---------- |:--------------------------------------------:| ---------------------------------:|
| 定义Bean     | <bean id="" class="">                       | @Component 及其衍生类 @Controller @Service @Repository|
| Bean名称     | 通过id或者name指定                            | @Component(“name”)|
| Bean注入     | <property>或者通过p命名空间                    | @Autowired 按照类型注入 @Qualifier 按名称注入|
| Bean作用范围 | <bean>标签“scope”属性                         | @Scope 指定bean的作用范围|
| Bean生命周期 | <bean>标签“init-method”和“destroy-method”属性  | @PostConstruct 用于指定初始化方法 @PreDestroy 用于指定销毁方法|

写到此处基于注解的IOC配置已经完成，但是会发现仍然离不开spring的xml配置文件，其实可以不写这个bean.xml文件，将所有的配置用注释来实现，只不过**很少这样使用**，接下来介绍如果想要将所有配置用注释来实现需要用到的注释：

- @Configuration：用于说明当前类是一个spring配置类，当创建容器时会从该类上加载注解。获取容器时需要使用AnnotationApplicationContext(有@Configuration注解的类.class)。value属性用于指定配置类的字节码。

- @ComponentScan：用于指定spring在初始化容器时要扫描的包，作用和在spring的xml配置文件中的
<context:component-scan base-package="com.gengrui"/>是一样的。value属性用于指定要扫描的包，和该标签中的base-package属性作用一样。

- @PropertySource：用于加载.properties文件中的配置。例如配置数据源时，可以把连接数据库的信息写到properties配置文件中，就可以使用此注解指定properties配置文件的位置。value[]属性用于指定properties文件位置，如果是在类路径下，需要写上classpath。

- @Import：用于导入其他配置类，在引入其他配置类时，可以不用再写@Configuration注解（写上也没问题）。value[]属性用于指定其他配置类的字节码。

- @Bean：该注解只能写在方法上，表明使用此方法创建一个对象，并且放入spring容器。就相当于xml配置中的factory-bean和factory-method。name属性给当前@Bean注解方法创建的对象指定一个名称（即bean的id）。

至于实际的开发中到底使用xml还是注解，每个开发者、每家公司有着不同的使用习惯，这里就涉及到了团队合作，不多加描述了。

最后来总结一下本篇文章的内容（下面这段书上抄来的啊哈哈哈哈哈）

IOC是一个容器，在Spring中会认为一切Java资源都是Java资源都是Java Bean，Spring IOC中装载的不同的Bean也可以理解为Java的各种资源，容器的目标就是管理这些Bean以及他们之间的依赖关系。

只是Spring IOC管理对象和其依赖关系，采用的不是人为的主动创建，而是由Spring IOC自己通过描述创建的，也就是说Spring是依靠描述来对象及其依赖关系创建的。

在使用这些资源时，程序不需要自己去找资源，只要向Spring IOC容器描述所需资源，Spring IOC会自己找到程序所需要的资源，这就是Spring IOC的理念。这样就把Bean之间的依赖关系解耦了，更容易写出结构清晰的程序。

除此之外，Spring IOC还提供对Java Bean生命周期的管理，可以延迟加载，可以在其生命周期内定义一些行为等，更加方便有效的使用和管理Java资源，这就是Spring IOC的魅力。