---
layout: post
title: 2020 java面试整理
category: java
tags: [java,myBatis]
excerpt: java面试整理
---

## 一. java基础
### Java中8大基本类型占用的字节数?
>- 1字节： byte , boolean
>- 2字节： short , char
>- 4字节： int , float
>- 8字节： long , double
>注：1字节(byte)=8位(bits)

### ==和equals的区别是什么?
>==对于基本类型来说是值比较,对于引用类型来说是比较的引用;而equals默认情况下是引用比较,只是很多类重写了equals方法,比如String,Integer等把它变成了值比较,所以一般情况下equals比较的是值是否相等

### final在java中有什么作用?
>final修饰的类叫最终类,该类不能被继承.
final修饰的方法不能被重写
final修饰的变量是常量,常量必须初始化,初始化之后不能被修改

### java容器有哪些?
>java容器分为Collection和Map两大类.其下又有很多子类.如下所示:

> Collection
	>- **List** ----------	 **ArrayList**, **LinkedList**, **Vector**, **Stack**
	>- **Set** ----------	**HashSet**, **LinkedHashSet**, **TreeSet**
	
> Map
	>- **HashMap**	**LinkedHashMap**
	>- **TreeMap**
	>- **ConcurrentHashMap**
	>- **HashTable**

### HashMap和Hashtable有什么区别?
>- 存储: HashMap运行key和value为null,而Hashtable不允许.
>- 线程安全: Hashtable是线程安全的,而HashMap是非线程安全的.
>- 推荐使用: 在Hashtable的类注释可以看到,Hashtbale是保留类不建议使用,推荐在单线程环境下使用HashMap替代,如果需要多线程则用ConcurrentHashMap替代

### HashMap的实现原理?
>HashMap是使用拉链法.HashMap基于Hash算法实现,通过put(key,value)存储,get(key)来获取.当传入key时,HashMap会根据
key.hashCode()计算hash值,根据hash值将value保存在bucket里.当计算出的hash值相同时,我们称之为hash冲突
HashMap的做法是用链表和红黑树存储相同的hash值得value.当hash冲突的个数比较少时,使用链表,当大于8个时使用红黑树.

### ArrayList和LinkedList的区别是什么?
>- 数据结构实现: ArrayList是动态数组的数据结构实现,而LinkedList是双向链表的数据结构实现.
>- 随机访问效率: ArrayList在随机访问的时候效率要高,因为LinkedList是线性的数据存储方式,所以需要移动指针从前往后一次查找.
>- 增加和删除效率: 在非首尾的增加和删除操作,LinkedList要比ArrayList效率要高,因为ArrayList增删操作要影响数组内的其他数据的下标.
综合来说,在需要频繁读取集合中的元素时,更推荐使用ArrayList,而在插入和删除操作比较多时,更推荐使用LinkedList.

### sleep()和wait()有什么区别?
>- 类的不同: sleep()来自Thread,wati()来自object.
>- 释放锁: sleep()不释放锁;wait()释放锁.
>- 用法不同: sleep()时间到会自动恢复;wait()可以使用notify()/notifyAll()直接唤醒.

### 在java程序中怎么保证多线程的运行安全?
>- 方法一: 使用安全类,比如Java.util.concurrent下的类.
>- 方法二: 使用自动锁synchronized
>- 方法三: 使用手动锁Lock

### 什么是反射?
>反射是在运行状态中,对于任意一个类,都能够知道这个类的所有属性和方法;对于任意一个对象,都能够调用他的任意一个方法和属性;
这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制.

### tcp和udp的区别?
>tcp和udp是OSI模型中的运输层中的协议.tcp提供可靠的通信传输.而udp则常被用于让广播和细节控制交给应用的通信传输.
两者的区别大致如下:
>- tcp面向连接,udp面向非连接即发送数据前不需要建立连接;
>- tcp提供可靠的服务(数据传输),udp无法保证;
>- tcp面向字节流,udp面向报文;
>- tcp数据传输慢,udp数据传输快;

### 如何实现跨域?
>- 服务器端运行跨域 甚至CORS等于*;
>- 在单个接口使用注解@CorssOrigin运行跨域;
>- 使用jsonpk跨域;

### 说一下你熟悉的设计模式?
>- 单例模式: 保证被创建一次,节省系统开销.
>- 工厂模式: 解耦代码.
>- 观察者模式: 定义了对象之间的一对多的依赖,这样一来,当一个对象改变时,它的所有的依赖都会受到通知并自动更新.
>- 外观模式: 提供一个统一的接口,用来访问子系统的一群接口,外观定义了一个高层的接口,让子系统更容易使用.
>- 模板方法模式: 定义了一个算法的骨架,而将一些不走延迟到子类中,模板方法使得子类可以在不改变算法结构的情况下,重新定义算法的步骤.
>- 状态模式: 允许对象在内部状态改变时改变它的行为,对象看起来好像修改了它的类.
### mybatis 的 dao 接口跟 xml 文件里面的sql 是如何建立关系的？
>是通过JDK动态代理，返回了一个Dao接口的代理对象，这个代理对象的处理器是MapperProxy对象。所以，我们通过@Autowired注入Dao接口的时候，注入的就是这个代理对象，我们调用到Dao接口的方法时，则会调用到MapperProxy对象的invoke方法。
>只要保证namespace+id唯一即可

## 二. 多线程

### 线程池的几种常见的创建的方式?

>一：创建大小不固定的线程池  
二：创建固定数量线程的线程池   
三：创建单线程的线程池     
四：创建定时线程    

>java类库提供一个灵活的线程池以及一些有用的默认配置；我们可以通过Executors的静态方法来创建线程池。    
>一: newCachedThreadPool()   
> 二: newFixedThreadPool(int nThreads)   
> 三: newSingleThreadExecutor()  
> 四: newScheduledThreadPool(int corePoolSize)   


## 三. jvm
### JVM模型?
>- 程序计数器
>- JVM栈
>- 本地方法栈
>- 方法区
>- 堆

## 四. Spring


## 五. mysql

### 事务的四大特性?
>- 原子性
>- 一致性
>- 隔离性
>- 持久性

### 四种隔离级别?
>- ① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。
>- ② Repeatable read (可重复读)：可避免脏读、不可重复读的发生。
>- ③ Read committed (读已提交)：可避免脏读的发生。
>- ④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。
### 不考虑事务的隔离性,会发生什么问题?
>- 脏读
>- 不可重复读
>- 幻读
### 数据库的索引有哪些?
>- 普通索引(INDEX)：最基本的索引，没有任何限制
>- 唯一索引(UNIQUE)：与"普通索引"类似，不同的就是：索引列的值必须唯一，但允许有空值。
>- 主键索引(PRIMARY)：它 是一种特殊的唯一索引，不允许有空值。
>- 全文索引(FULLTEXT )：仅可用于 MyISAM 表， 用于在一篇文章中，检索文本信息的, 针对较大的数据，生成全文索引很耗时好空间。
>- 组合索引：为了更多的提高mysql效率可建立组合索引，遵循”最左前缀“原则。
### 什么操作会破坏索引?
>- 1.如果条件中有or，即使其中有条件带索引也不会使用;要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引.
>- 2.like查询是以%开头
>- 3.如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
>- 4.对于多列索引，不是使用的第一部分，则不会使用索引
>- 5.如果mysql估计使用全表扫描要比使用索引快,则不使用索引
### Mysql 性能优化
>- 为搜索字段创建索引。
>- 避免使用 select *，列出需要查询的字段。
>- 垂直分割分表。
>- 选择正确的存储引擎。
>- 使用join时,小表驱大表
>- 避免字段为null

## 六. MyBatis

## 七. Spring boot

### 为什么要使用 spring？
>spring 提供 ioc 技术，容器会帮你管理依赖的对象，从而不需要自己创建和管理依赖对象了，更轻松的实现了程序的解耦。
spring 提供了事务支持，使得事务操作变的更加方便。
spring 提供了面向切片编程，这样可以更方便的处理某一类的问题。
更方便的框架集成，spring 可以很方便的集成其他框架，比如 MyBatis、hibernate 等。

### 解释一下什么是 aop？
>aop 是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

### 解释一下什么是 ioc？
>ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。
简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

### spring 常用的注入方式有哪些？
>setter 属性注入
构造方法注入
注解方式注入

### spring 中的 bean 是线程安全的吗？
>spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

>实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

>有状态就是有数据存储功能。
无状态就是不会保存数据。

>spring 事务实现方式有哪些？
声明式事务：声明式事务也有两种实现方式，基于 xml 配置文件的方式和注解方式（在类上添加 @Transaction 注解）。
编码方式：提供编码的形式管理和维护事务。

### spring boot优势
（1）独立运行的Spring项目    
Spring Boot可以以jar包的形式进行独立的运行，使用：java -jar xx.jar 就可以成功的运行项目，或者在应用项目的主程序中运行main函数即可；     
（2）内嵌的Servlet容器   
内嵌容器，使得我们可以执行运行项目的主程序main函数，并让项目的快速运行；  
（3）提供starter简化Maven配置   
Spring Boot提供了一系列的starter pom用来简化我们的Maven依赖     
（4）自动配置Spring   
Spring Boot会根据我们项目中类路径的jar包/类，为jar包的类进行自动配置Bean，这样一来就大大的简化了我们的配置。当然，这只是Spring考虑到的大多数的使用场景，在一些特殊情况，我们还需要自定义自动配置； 
（5）应用监控     
Spring Boot提供了基于http、ssh、telnet对运行时的项目进行监控； 

## spring boot 常用注解
@SpringBootApplication
@Configuration
@Bean

### 自动配置原理
Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。


## 八. Spring cloud
### 什么是spring cloud
spring cloud 是一系列框架的有序集合。它利用 spring boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 spring boot 的开发风格做到一键启动和部署。

## 九. redis

### redis持久化方式?
>- RDB(redis database)：在指定的时间间隔能对你的数据进行快照存储。
>- AOF(append only file)：记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据。
### redis有哪些数据类型?
>- String
>- Hash
>- List
>- Set
>- zset
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190306101116895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VxcXRr,size_16,color_FFFFFF,t_70)
### redis常见应用场景?
>-  全页面缓存
>- 排行榜
>-  Session 存储
>-  队列
>-  发布/订阅

## 十. RabbitMQ

