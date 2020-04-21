---
layout: post
title: 2020 java面试整理
category: java
tags: [java,myBatis]
excerpt: java面试整理
---

## 一. Java基础
#### Java中8大基本类型占用的字节数?
- 1字节： byte , boolean
- 2字节： short , char
- 4字节： int , float
- 8字节： long , double
>注：1字节(byte)=8位(bits)

#### ==和equals的区别是什么?
==对于基本类型来说是值比较,对于引用类型来说是比较的引用;而equals默认情况下是引用比较,只是很多类重写了equals方法,比如String,Integer等把它变成了值比较,所以一般情况下equals比较的是值是否相等

#### final在java中有什么作用?
final修饰的类叫最终类,该类不能被继承.
final修饰的方法不能被重写
final修饰的变量是常量,常量必须初始化,初始化之后不能被修改

#### java容器有哪些?
java容器分为Collection和Map两大类.其下又有很多子类.如下所示:

- Collection  
	- **List**    
	    - **ArrayList**
	    - **LinkedList**
	    - **Vector**
	    - **Stack**
	- **Set**   
		- **HashSet**
		- **LinkedHashSet**
		- **TreeSet**
	
- Map
	- **HashMap**	
	- **LinkedHashMap**
	- **TreeMap**
	- **ConcurrentHashMap**
	- **HashTable**

#### HashMap和Hashtable有什么区别?
- 存储: HashMap运行key和value为null,而Hashtable不允许.
- 线程安全: Hashtable是线程安全的,而HashMap是非线程安全的.
- 推荐使用: 在Hashtable的类注释可以看到,Hashtbale是保留类不建议使用,推荐在单线程环境下使用HashMap替代,如果需要多线程则用ConcurrentHashMap替代

#### HashMap的实现原理?
HashMap是使用拉链法.HashMap基于Hash算法实现,通过put(key,value)存储,get(key)来获取.当传入key时,HashMap会根据
key.hashCode()计算hash值,根据hash值将value保存在bucket里.当计算出的hash值相同时,我们称之为hash冲突
HashMap的做法是用链表和红黑树存储相同的hash值得value.当hash冲突的个数比较少时,使用链表,当大于8个时使用红黑树.

#### ArrayList和LinkedList的区别是什么?
- 数据结构实现: ArrayList是动态数组的数据结构实现,而LinkedList是双向链表的数据结构实现.
- 随机访问效率: ArrayList在随机访问的时候效率要高,因为LinkedList是线性的数据存储方式,所以需要移动指针从前往后一次查找.
- 增加和删除效率: 在非首尾的增加和删除操作,LinkedList要比ArrayList效率要高,因为ArrayList增删操作要影响数组内的其他数据的下标.    
综合来说,在需要频繁读取集合中的元素时,更推荐使用ArrayList,而在插入和删除操作比较多时,更推荐使用LinkedList.

#### sleep()和wait()有什么区别?
- 类的不同: sleep()来自Thread,wait()来自object.
- 释放锁: sleep()不释放锁;wait()释放锁.
- 用法不同: sleep()时间到会自动恢复;wait()可以使用notify()/notifyAll()直接唤醒.

#### 在java程序中怎么保证多线程的运行安全?
- 方法一: 使用安全类,比如Java.util.concurrent下的类.
- 方法二: 使用自动锁synchronized
- 方法三: 使用手动锁Lock

#### 什么是反射
反射是在运行状态中,对于任意一个类,都能够知道这个类的所有属性和方法;对于任意一个对象,都能够调用他的任意一个方法和属性;
这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制.

#### tcp和 udp的区别
tcp和udp是OSI模型中的运输层中的协议.tcp提供可靠的通信传输.而udp则常被用于让广播和细节控制交给应用的通信传输.
两者的区别大致如下:
- tcp面向连接,udp面向非连接即发送数据前不需要建立连接;
- tcp提供可靠的服务(数据传输),udp无法保证;
- tcp面向字节流,udp面向报文;
- tcp数据传输慢,udp数据传输快;

#### 如何实现跨域?
- 服务器端运行跨域 甚至CORS等于*;
- 在单个接口使用注解@CorssOrigin运行跨域;
- 使用jsonpk跨域;

#### 说一下你熟悉的设计模式?
- 单例模式: 保证被创建一次,节省系统开销.
- 工厂模式: 解耦代码.
- 观察者模式: 定义了对象之间的一对多的依赖,这样一来,当一个对象改变时,它的所有的依赖都会受到通知并自动更新.
- 外观模式: 提供一个统一的接口,用来访问子系统的一群接口,外观定义了一个高层的接口,让子系统更容易使用.
- 模板方法模式: 定义了一个算法的骨架,而将一些不走延迟到子类中,模板方法使得子类可以在不改变算法结构的情况下,重新定义算法的步骤.
- 状态模式: 允许对象在内部状态改变时改变它的行为,对象看起来好像修改了它的类.
#### mybatis 的 dao 接口跟 xml 文件里面的sql 是如何建立关系的？
是通过JDK动态代理，返回了一个Dao接口的代理对象，这个代理对象的处理器是MapperProxy对象。所以，我们通过@Autowired注入Dao接口的时候，注入的就是这个代理对象，我们调用到Dao接口的方法时，则会调用到MapperProxy对象的invoke方法。
只要保证namespace+id唯一即可

## 二. 多线程
#### 创建线程的方式
- 继承 Thread 类，重写 run 方法
- 实现 Runnable 接口，实现 run 方法
- 实现 Callable 接口，实现 call 方法

#### 如何保证一个线程执行完再执行第二个线程？
- 使用join
- countDownLatch

#### 如何预防死锁？
- 尽量使用 tryLock(long timeout, TimeUnit unit) 的方法 (ReentrantLock、ReentrantReadWriteLock)，设置超时时间，超时可以退出防止死锁；
- 尽量使用 Java. util. concurrent 并发类代替自己手写锁；
- 尽量降低锁的使用粒度，尽量不要几个功能用同一把锁；
- 尽量减少同步的代码块。

#### Executors 可以创建以下六种线程池?

- FixedThreadPool(n)：创建一个数量固定的线程池，超出的任务会在队列中等待空闲的线程，可用于控制程序的最大并发数。
- CachedThreadPool()：短时间内处理大量工作的线程池，会根据任务数量产生对应的线程，并试图缓存线程以便重复使用，如果限制 60 秒没被使用，则会被移除缓存。
- SingleThreadExecutor()：创建一个单线程线程池。
- ScheduledThreadPool(n)：创建一个数量固定的线程池，支持执行定时性或周期性任务。
- SingleThreadScheduledExecutor()：此线程池就是单线程的 newScheduledThreadPool。
- WorkStealingPool(n)：Java 8 新增创建线程池的方法，创建时如果不设置任何参数，则以当前机器处理器个数作为线程个数，此线程池会并行处理任务，不能保证执行顺序。 

#### 线程池7个参数
- corePoolSize 线程池核心线程大小
- maximumPoolSize 线程池最大线程数量
- keepAliveTime 空闲线程存活时间
- unit 空间线程存活时间单位
- workQueue 工作队列
- threadFactory 线程工厂
- handler 拒绝策略

#### 线程的工作原理
当线程池中有任务需要执行时，线程池会判断如果线程数量没有超过核心数量就会新建线程池进行任务执行，如果线程池中的线程数量已经超过核心线程数，这时候任务就会被放入任务队列中排队等待执行；如果任务队列超过最大队列数，并且线程池没有达到最大线程数，就会新建线程来执行任务；如果超过了最大线程数，就会执行拒绝执行策略。

#### ThreadLocal 为什么是线程安全的？
ThreadLocal 为每一个线程维护变量的副本，把共享数据的可见范围限制在同一个线程之内，因此 ThreadLocal 是线程安全的，每个线程都有属于自己的变量。

#### ThreadLocal 和 Synchronized 有什么区别？
Synchronized 用于实现同步机制，是利用锁的机制使变量或代码块在某一时刻只能被一个线程访问，是一种 “以时间换空间” 的方式；而 ThreadLocal 为每一个线程提供了独立的变量副本，这样每个线程的（变量）操作都是相互隔离的，这是一种 “以空间换时间” 的方式。

#### CAS 与 ABA
CAS（Compare and Swap）比较并交换，是一种乐观锁的实现，是用非阻塞算法来代替锁定，其中 java.util.concurrent 包下的 AtomicInteger 就是借助 CAS 来实现的。
但 CAS 也不是没有任何副作用，比如著名的 ABA 问题就是 CAS 引起的。       
常见解决 ABA 问题的方案加版本号，来区分值是否有变动。

#### volatile 的作用是什么？
volatile 是 Java 虚拟机提供的最轻量级的同步机制。
当变量被定义成 volatile 之后，具备两种特性：

保证此变量对所有线程的可见性，当一条线程修改了这个变量的值，修改的新值对于其他线程是可见的（可以立即得知的）            
禁止指令重排序优化，普通变量仅仅能保证在该方法执行过程中，得到正确结果，但是不保证程序代码的执行顺序。 

#### volatile 对比 synchronized 有什么区别？
synchronized 既能保证可见性，又能保证原子性，而 volatile 只能保证可见性，无法保证原子性。比如，i++ 如果使用 synchronized 修饰是线程安全的，而 volatile 会有线程安全的问题。

## 三. Jvm
#### JVM 主要组成部分有哪些？
- 类加载器（ClassLoader）
- 运行时数据区（Runtime Data Area）
- 执行引擎（Execution Engine）
- 本地库接口（Native Interface）

#### JVM 是如何工作的？
首先程序在执行之前先要把 Java 代码（.java）转换成字节码（.class），JVM 通过类加载器（ClassLoader）把字节码加载到内存中，但字节码文件是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine） 将字节码翻译成底层机器码，再交由 CPU 去执行，CPU 执行的过程中需要调用本地库接口（Native Interface）来完成整个程序的运行。

#### 类加载器
- 启动类加载器：Bootstrap ClassLoader，负责加载存放在JDK\jre\lib(JDK代表JDK的安装目录，下同)下，或被-Xbootclasspath参数指定的路径中的，并且能被虚拟机识别的类库（如rt.jar，所有的java.开头的类均被Bootstrap ClassLoader加载）。启动类加载器是无法被Java程序直接引用的。
- 扩展类加载器：Extension ClassLoader，该加载器由sun.misc.Launcher$ExtClassLoader实现，它负责加载JDK\jre\lib\ext目录中，或者由java.ext.dirs系统变量指定的路径中的所有类库（如javax.开头的类），开发者可以直接使用扩展类加载器。
- 应用程序类加载器：Application ClassLoader，该类加载器由sun.misc.Launcher$AppClassLoader来实现，它负责加载用户类路径（ClassPath）所指定的类，开发者可以直接使用该类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

#### 类加载执行流程
- 加载：根据查找路径找到相应的 class 文件然后导入；
- 检查：检查加载的 class 文件的正确性；
- 准备：给类中的静态变量分配内存空间；
- 解析：虚拟机将常量池中的符号引用替换成直接引用的过程。符号引用就理解为一个标示，而在直接引用直接指向内存中的地址；
- 初始化：对静态变量和静态代码块执行初始化工作。

#### 类加载机制
- 全盘负责，当一个类加载器负责加载某个Class时，该Class所依赖的和引用的其他Class也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入
- 父类委托，先让父类加载器试图加载该类，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类
- 缓存机制，缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个Class时，类加载器先从缓存区寻找该Class，只有缓存区不存在，系统才会读取该类对应的二进制数据，并将其转换成Class对象，存入缓存区。这就是为什么修改了Class后，必须重启JVM，程序的修改才会生效

#### JVM 内存布局是怎样的？
- 程序计数器（Program Counter Register）
- Java 虚拟机栈（Java Virtual Machine Stacks）
- 本地方法栈（Native Method Stack）
- Java 堆（Java Heap）
- 方法区（Method Area）

#### 垃圾收集算法
- 标记-清除算法    
- 复制算法
- 标记-压缩算法
- 分代收集算法

>分代收集算法把java堆分为新生代和老年代, 新生代选用复制算法,老年代采用标记-清理算法或标记-整理算法;     
默认情况下新生代和老生代的内存比例是 1:2  
新生代是由：Eden、Form Survivor、To Survivor 三个区域组成的，它们内存默认占比是 8:1:1

#### 新生代垃圾回收是怎么执行的？
- Eden 区 + From Survivor 区存活着的对象复制到 To Survivor 区；
- 清空 Eden 和 From Survivor 分区；
- From Survivor 和 To Survivor 分区交换（From 变 To，To 变 From）。

#### 垃圾回收器
- CMS（Concurrent Mark Sweep）一种以获得最短停顿时间为目标的收集器，非常适用 B/S 系统。
- G1 垃圾回收器是一种兼顾吞吐量和停顿时间的 GC 实现，是 JDK 9 以后的默认 GC 选项。G1 可以直观的设定停顿时间的目标，相比于 CMS CG，G1 未必能做到 CMS 在最好情况下的延时停顿，但是最差情况要好很多

#### 垃圾回收的调优参数有哪些？
- -Xmx:512 设置最大堆内存为 512 M；
- -Xms:215 初始堆内存为 215 M；
- -XX:MaxNewSize 设置最大年轻区内存；
- -XX:MaxTenuringThreshold=5 设置新生代对象经过 5 次 GC 晋升到老年代；
- -XX:PretrnureSizeThreshold 设置大对象的值，超过这个值的大对象直接进入老生代；
- -XX:NewRatio 设置分代垃圾回收器新生代和老生代内存占比；
- -XX:SurvivorRatio 设置新生代 Eden、Form Survivor、To Survivor 占比。

## 四. Mysql

#### 事务的四大特性?
- 原子性
- 一致性
- 隔离性
- 持久性

#### 四种隔离级别?
- Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。
- Repeatable read (可重复读)：可避免脏读、不可重复读的发生。
- Read committed (读已提交)：可避免脏读的发生。
- Read uncommitted (读未提交)：最低级别，任何情况都无法保证。

#### 不考虑事务的隔离性,会发生什么问题?
- 脏读
- 不可重复读
- 幻读

#### 数据库的索引有哪些?
- 普通索引(INDEX)：最基本的索引，没有任何限制
- 唯一索引(UNIQUE)：与"普通索引"类似，不同的就是：索引列的值必须唯一，但允许有空值。
- 主键索引(PRIMARY)：它 是一种特殊的唯一索引，不允许有空值。
- 全文索引(FULLTEXT )：仅可用于 MyISAM 表， 用于在一篇文章中，检索文本信息的, 针对较大的数据，生成全文索引很耗时好空间。
- 组合索引：为了更多的提高mysql效率可建立组合索引，遵循”最左前缀“原则。

#### 什么操作会破坏索引?
- 1.如果条件中有or，即使其中有条件带索引也不会使用;要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引.
- 2.like查询是以%开头
- 3.如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
- 4.对于多列索引，不是使用的第一部分，则不会使用索引
- 5.如果mysql估计使用全表扫描要比使用索引快,则不使用索引

#### MySQL 问题排查都有哪些手段？
- 使用 show processlist 命令查看当前所有连接信息。
- 使用 explain 命令查询 SQL 语句执行计划。
- 开启慢查询日志，查看慢查询的 SQL。

#### Mysql 性能优化
- 为搜索字段创建索引。
- 避免使用 select *，列出需要查询的字段。
- 垂直分割分表。
- 选择正确的存储引擎。
- 使用join时,小表驱大表
- 避免字段为null

## 五. Spring
#### 为什么要使用 spring？
spring 提供 ioc 技术，容器会帮你管理依赖的对象，从而不需要自己创建和管理依赖对象了，更轻松的实现了程序的解耦。
spring 提供了事务支持，使得事务操作变的更加方便。
spring 提供了面向切片编程，这样可以更方便的处理某一类的问题。
更方便的框架集成，spring 可以很方便的集成其他框架，比如 MyBatis、hibernate 等。

#### 解释一下什么是 aop？
aop 是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

#### 解释一下什么是 ioc？
ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。
简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

#### spring 常用的注入方式有哪些？
- setter 属性注入 
- 构造方法注入  
- 注解方式注入  

#### spring 中的 bean 是线程安全的吗？
spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。

实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

有状态就是有数据存储功能。   
无状态就是不会保存数据。    

spring 事务实现方式有哪些？   
声明式事务：声明式事务也有两种实现方式，基于 xml 配置文件的方式和注解方式（在类上添加 @Transaction 注解）。     
编程式事务：提供编码的形式管理和维护事务。 

#### Spring 中都是用了哪些设计模式？
- 工厂模式：通过 BeanFactory、ApplicationContext 来创建 bean 都是属于工厂模式；
- 单例、原型模式：创建 bean 对象设置作用域时，就可以声明 Singleton（单例模式）、Prototype（原型模式）；
- 观察者模式：Spring 可以定义一下监听，如 ApplicationListener 当某个动作触发时就会发出通知；
- 责任链模式：AOP 拦截器的执行；
- 策略模式：在创建代理类时，如果代理的是接口使用的是 JDK 自身的动态代理，如果不是接口使用的是 CGLIB 实现动态代理。

#### Spring MVC 的执行流程?
前端控制器（DispatcherServlet） 接收请求，通过映射从 IoC 容器中获取对应的 Controller 对象和 Method 方法，在方法中进行业务逻辑处理组装数据，组装完数据把数据发给视图解析器，视图解析器根据数据和页面信息生成最终的页面，然后再返回给客户端。

## 六. MyBatis

## 七. Spring boot

#### spring boot优势
- （1）独立运行的Spring项目    
Spring Boot可以以jar包的形式进行独立的运行，使用：java -jar xx.jar 就可以成功的运行项目，或者在应用项目的主程序中运行main函数即可；     
- （2）内嵌的Servlet容器   
内嵌容器，使得我们可以执行运行项目的主程序main函数，并让项目的快速运行；  
- （3）提供starter简化Maven配置   
Spring Boot提供了一系列的starter pom用来简化我们的Maven依赖     
- （4）自动配置Spring   
Spring Boot会根据我们项目中类路径的jar包/类，为jar包的类进行自动配置Bean，这样一来就大大的简化了我们的配置。当然，这只是Spring考虑到的大多数的使用场景，在一些特殊情况，我们还需要自定义自动配置； 
- （5）应用监控     
Spring Boot提供了基于http、ssh、telnet对运行时的项目进行监控； 

#### spring boot 常用注解
@SpringBootApplication  
@Configuration  
@Bean   

#### 常见的 starter 有哪些?
- spring-boot-starter-web：Web 开发支持
- spring-boot-starter-data-jpa：JPA 操作数据库支持
- spring-boot-starter-data-redis：Redis 操作支持
- spring-boot-starter-data-solr：Solr 权限支持
- mybatis-spring-boot-starter：MyBatis 框架支持

#### 自动配置原理
Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。


## 八. Spring cloud
#### 什么是spring cloud
spring cloud 是一系列框架的有序集合。它利用 spring boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 spring boot 的开发风格做到一键启动和部署。

## 九. Redis

#### redis持久化方式?
- RDB(redis database)：在指定的时间间隔能对你的数据进行快照存储。
- AOF(append only file)：记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据。

#### redis有哪些数据类型?
- String
- Hash
- List
- Set
- zset
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190306101116895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2VxcXRr,size_16,color_FFFFFF,t_70)

#### redis常见应用场景?
- 全页面缓存
- 排行榜
- Session 存储
- 队列
- 发布/订阅

## 十. RabbitMQ
#### 消息队列主要功能
- 解耦
- 削峰
- 异步

#### 消息队列的应用场景有哪些？

- 应用解耦，比如，用户下单后，订单系统需要通知库存系统，假如库存系统无法访问，则订单减库存将失败，从而导致订单失败。订单系统与库存系统耦合，这个时候如果使用消息队列，可以返回给用户成功，先把消息持久化，等库存系统恢复后，就可以正常消费减去库存了。
- 削峰填谷，比如，秒杀活动，一般会因为流量过大，从而导致流量暴增，应用挂掉，这个时候加上消息队列，服务器接收到用户的请求后，首先写入消息队列，假如消息队列长度超过最大数量，则直接抛弃用户请求或跳转到错误页面。
- 日志系统，比如，客户端负责将日志采集，然后定时写入消息队列，消息队列再统一将日志数据存储和转发。

#### RabbitMQ 有哪些优点？
- 可靠性，RabbitMQ 的持久化支持，保证了消息的稳定性；
- 高并发，RabbitMQ 使用了 Erlang 开发语言，Erlang 是为电话交换机开发的语言，天生自带高并发光环和高可用特性；
- 集群部署简单，正是因为 Erlang 使得 RabbitMQ 集群部署变的非常简单；
- 社区活跃度高，因为 RabbitMQ 应用比较广泛，所以社区的活跃度也很高；
- 解决问题成本低，因为资料比较多，所以解决问题的成本也很低；
- 支持多种语言，主流的编程语言都支持，如 Java、.NET、PHP、Python、JavaScript、Ruby、Go 等；
- 插件多方便使用，如网页控制台消息管理插件、消息延迟插件等。

#### RabbitMQ 有哪些重要的角色？
- 生产者：消息的创建者，负责创建和推送数据到消息服务器；
- 消费者：消息的接收方，用于处理数据和确认消息；
- 代理：就是 RabbitMQ 本身，用于扮演“快递”的角色，本身不生产消息，只是扮演“快递”的角色。

#### RabbitMQ 有哪些重要的组件？它们有什么作用？
- ConnectionFactory（连接管理器）：应用程序与 RabbitMQ 之间建立连接的管理器，程序代码中使用；
- Channel（信道）：消息推送使用的通道；
- Exchange（交换器）：用于接受、分配消息；
- Queue（队列）：用于存储生产者的消息；
- RoutingKey（路由键）：用于把生成者的数据分配到交换器上；
- BindingKey（绑定键）：用于把交换器的消息绑定到队列上。

#### RabbitMQ 交换器类型有哪些？
- direct：轮询方式
- headers：轮询方式，允许使用 header 而非路由键匹配消息，性能差，几乎不用
- fanout：广播方式，发送给所有订阅者
- topic：匹配模式，允许使用正则表达式匹配消息

> RabbitMQ 默认的是 direct 方式。

#### RabbitMQ 如何确保每个消息能被消费？
RabbitMQ 使用 ack 消息确认的方式保证每个消息都能被消费，开发者可根据自己的实际业务，选择 channel.basicAck() 方法手动确认消息被消费。

#### RabbitMQ 接收到消息之后必须消费吗？
RabbitMQ 接收到消息之后可以不消费，在消息确认消费之前，可以做以下两件事：

- 拒绝消息消费，使用 channel.basicReject(消息编号, true) 方法，消息会被分配给其他订阅者；
- 设置为死信队列，死信队列是用于专门存放被拒绝的消息队列。

#### RabbitMQ 包含事务功能吗？如何使用？
RabbitMQ 包含事务功能，主要是对信道（Channel）的设置，主要方法有以下三个：

- channel.txSelect() 声明启动事务模式；
- channel.txComment() 提交事务；
- channel.txRollback() 回滚事务。

#### RabbitMQ 的事务在什么情况下是无效的？
RabbitMQ 的事务在 autoAck=true 也就是自动消费确认的时候，事务是无效的。因为如果是自动消费确认，RabbitMQ 会直接把消息从队列中移除，即使后面事务回滚也不能起到任何作用。