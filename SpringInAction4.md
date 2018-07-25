#第1章 Spring之旅

Spring的bean容器
介绍Spring的核心模块
更为强大的Spring生态系统
Spring新功能

##1.1简化Java开发

###1.1.1激发POJO的潜能

Spring赋予POJO魔力的方式之一就是通过DI来装配它们。

###1.1.2依赖注入（DI）

依赖注入会将所依赖的关系自动交给目标对象，而不是让对象自己去获取依赖。松散耦合。

传统的做法使得对象之间高度耦合以及难以测试。高度耦合是因为对象之间的关系是通过代码硬编码编死的，如果要使用其他的实现逻辑（实现类），则需要修改代码。难以测试是因为在测试的时候，还需要再次手动创建对象，如果使用Spring，读一下配置即可。如果修改代码，可能测试代码也要改动。所以说，紧密耦合的代码，导致难以测试，难以复用，难以理解。

创建应用组件之间协作的行为通常称为装配（wiring）。Spring有多种装配bean的方式，：采用XML、基于Java的配置。可以在不改变所依赖的类的情况下，修改依赖关系。

DI可以带来松耦合，通过接口来表明依赖关系。

这里其实就是依赖反转、里氏替换设计原则的体现。

###1.1.3应用切面（AOP）

系统由许多不同的组件组成，每一个组件各负责一块特定功能。某些组件，比如日志，事务管理，权限等功能通常会分散到多个组件中去，这样代码将会带来双重的复杂性：
	1.这些功能会重复出现在多个组件中，当需要修改这些功能的逻辑的时候，必须修改各个模块中的实现。即使这些功能被抽象成一个统一的独立的模块，在其他模块中也需要重复的方法调用。
	2.组件会因为那些与自身核心业务无关的代码而变得混乱。
	
AOP能够使这些服务模块化，并以声明的方式将它们应用到它们需要影响的组件中去。那么这些组件只需要关注自身的业务。AOP能够确保POJO的简单性。
	
###1.1.4使用模版消除样板式代码

Spring旨在通过模版封装来消除样板式代码。比如Spring的JdbcTemplate。

##1.2容纳你的Bean

容器是Spring框架的核心。Spring自带了多个容器实现，可以归为两种不同的类型。bean工厂（BeanFactory接口）和应用上下文（ApplicationContext接口）。

###1.2.1使用应用上下文

上下文准备就绪之后，我们就可以调用上下文的getBean()方法从Spring容器中获取bean。

###1.2.2bean的生命周期

实例化->
填充属性->
调用BeanNameAware的setBeanName()方法->
调用BeanFactoryAware的setBeanFactory()方法->
调用ApplicationContextAware的setApplicationContext()方法->
调用BeanPostProcessor的预初始化方法->
调用InitializingBean的afterPropertiesSet()方法->
调用自定义的初始化方法->
调用BeanPostProcessor的初始化后方法->bean可以使用了

容器关闭->
调用DisposableBean的destory()方法->
调用自定义的销毁方法

##1.3俯瞰Spring风景线

 Spring框架
 
#第2章
 
 声明bean
 构造器注入和Setter注入
 装配bean
 控制bean的创建和销毁
 
##2.1 Spring配置的可选方案

- 在XML中进行显式的配置
- 在Java中进行显式的配置
- 隐式的bean发现机制和自动装配

##2.2 自动化装配bean

Spring从两个角度来实现自动化装配：
1.组件扫描（component scanning）：Spring自发现应用上下文中所创建的bean
2.自动装配（autowiring）：Spring自动满足bean之间的依赖

###2.2.1创建可被发现的bean

###2.2.2为组件扫描的bean命名

如果没有明确地为bean设置ID，默认会将类名的第一个字母变成小写作为ID。
自定义 @Component("beanID")

###2.2.3设置组件扫描的基础包

按照默认规则，以配置类所在的包作为基础包来扫描组件。
明确的设置基础包：@ComponentScan("packageName")
或者@ComponentScan(basePackages="packageName")
如果要设置多个基础包 @ComponentScan(basePackeages={"packageName1","packageName2"})

还提供一种方式，就是指定包中所包含的类或者接口
@ComponentScan(basePackageClasses={CDPlayer.class, DVDPlayer.class})

###2.2.4通过为bean添加注解实现自动装配

使用@Autowired注解，可以使用在构造器上，还能用在属性的setter方法上。

###2.2.5验证自动装配
 