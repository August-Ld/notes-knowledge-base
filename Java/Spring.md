## 面试题

### Spring框架中的bean是单例的吗？

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401091143709.png" alt="image-20240401091143709" style="zoom:50%;" />

* Singleton： bean 在每个 Spring IOC 容器中只有一个实例。
* prototype： 一个 bean 的定义可以有多个实例。



### Spring框架中的单例bean是线程安全的吗？

不是线程安全的

Spring 中有一个**@Scope** 注解，默认的值是Singleton，单例的

因为一般在 Spring 中的 bean 的注入都是**无状态**的对象，没有线程安全问题，如果在 bean 中定义了可修改的成员变量，是要考虑线程安全问题的，可以使用多例或者加锁来解决。

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401094606502.png" alt="image-20240401094606502" style="zoom:30%;" />

答：

不是线程安全的，

当多用户同时请求一个服务时，容器会给每一个请求分配一个线程，这是多个线程会并发执行该请求对应的业务逻辑（成员方法），如果该处理逻辑汇总有对该单例状态的修改（体现为该单例的成员属性），则必须要考虑线程同步问题。

Spring 框架并没有对单例 bean 进行任何多线程的封装处理。关于单例 bean 的线程安全和并发问题需要开发者自行去搞定。

比如：我们通常在项目中使用的 Spring bean 都是不可变的状态（比如 Service 类和 DAO 类），所以在某种程度上说 Spring 的单例 bean 是线程安全的。

如果你的 bean 的多种状态的话（比如 ViewModel 对象），就需要自行保证线程安全。最浅显的解决办法就是将多态bean 的作用域由 Singleton 变更为 prototype。





### 什么是AOP，你们项目中有没有使用到AOP？

#### 什么是AOP

AOP 成为面向切面编程，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，减低模块的耦合度，同时提升系统可维护性。

常见的 AOP 使用场景：

* 记录操作日志
* 缓存记录
* Spring 中内置的事务处理

#### 项目中使用

记录操作日志：获取请求的用户名、请求方式、访问地址、模块名称、登录 IP、操作时间，记录到数据库的日志表中。

**核心**：使用 aop 中的环绕通知+切点表达式（找到要记录日志的方法），通过环绕通知的参数获取请求方法的参数（类、方法、注解、请求方式等），获取到这些参数以后，保存到数据库。

环绕通知

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401095939031.png" alt="image-20240401095939031" style="zoom:50%;" />





#### Spring 中的事务是如何实现的？

Spring 支持编程式事务管理和声明式事务管理两种方式。

* 编程式事务控制：需要使用 TransactionTemplate 来进行实现，对业务代码有侵入性，项目中少使用。
* 声明式事务管理：声明式事务管理建立在 AOP 之上。其本质是通过 AOP 功能，对方法前后进行拦截，将事务处理的功能编织到拦截的方法中，也就是在目标方法开始之前加入一个事物，在执行完目标方法之后根据执行情况提交或者回滚事务。
  * 本质是通过 AOP 功能，对方法前后进行拦截，在执行方法之前开启事务，在执行完目标方法之后根据执行情况提交或者回滚事务。

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401100346911.png" alt="image-20240401100346911" style="zoom:40%;" />





### Spring 中事务失效的场景？

#### 场景一：异常捕获处理

**原因**：事务通知只有捕捉到目标抛出的异常，才能进行后续的会滚处理，如果目标自己处理掉异常，事务通知无法知悉。

**解决**：在 catch 块中添加 throw new RuntimeException(e) 抛出

#### 场景二：抛出检查异常

**原因**：Spring 默认只会回滚非检查异常。

**解决**：配置 rollbackFor 属性

​			@Transactional(rollbackFor=Exception.class)

#### 场景三：非 public 方法导致的事务失效

**原因**：Spring 为方法创建代理、添加事务通知、前提条件都是该方法是 public 的

**解决**：改为 public 方法



### Spring中bean的生命周期

1. 通过 BeanDefinition 获取 bean 的定义信息
2. 调用构造函数实例化 bean
3. bean 的依赖注入
4. 处理 Aware 接口（BeanNameAware、BeanFactoryAware、ApplicationContextAware）
5. Bean 的后置处理器 BeanPostProcessor-前置
6. 初始化方法（InitializingBean、init-method)
7. Bean 的后置处理器 BeanPostProcessor-后置（aop、动态代理：JDK 动态代理、CGLIB 动态代理）
8. 销毁 bean

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401101841101.png" alt="image-20240401101841101" style="zoom:50%;" />

### Spring中的循环引用

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401103405867.png" alt="image-20240401103405867" style="zoom:50%;" />



### 三级缓存解决循环依赖

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401103451950.png" alt="image-20240401103451950" style="zoom:50%;" />

* 一级缓存作用：限制 bean 在 beanFactory 中只存一份，即实现Singleton scope，解决不了循环依赖





<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401135929012.png" alt="image-20240401135929012" style="zoom:33%;" />





#### 构造方法出现循环依赖怎么解决？

懒加载 @Lazy

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401140045516.png" alt="image-20240401140045516" style="zoom: 33%;" />





### Spring的常见注解

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401152219267.png" alt="image-20240401152219267" style="zoom:50%;" />

