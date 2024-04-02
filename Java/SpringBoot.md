## SpringBoot自动装配原理

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401150720223.png" alt="image-20240401150720223" style="zoom:33%;" />

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401150753144.png" alt="image-20240401150753144" style="zoom:33%;" />



1. 在 SpringBoot 项目中的引导类上有一个注解@SpringBootApplication，这个注解是对三个注解进行了封装，分别是：

* @SpringBootConfiguration
* @EnableAutoConfiguration
* @ComponentScan

2. 其中@EnableAutoConfiguration 是实现自动化配置的核心注解。该注解通过@import 注解导入对应的配置选择器。

   内部就是读取了该项目和该项目引起的 jar 包的 classpath 路径下 META-INF/spring.factories 文件中的所配置的类的全类名。在这些配置类中所定义的 Bean 会根据条件注解所指定的条件来决定是否需要将其导入到 Spring 容器中。

3. 条件判断会有像@ConditionalOnClass 这样的注解，判断是否有对应的 class 文件，如果有则加载该类，把这个配置类的所有 Bean 放入 spring 容器中使用。



## SpringBoot常见注解？

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401152421423.png" alt="image-20240401152421423" style="zoom:50%;" />



<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401152525277.png" alt="image-20240401152525277" style="zoom:50%;" />



<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401152605931.png" alt="image-20240401152605931" style="zoom:50%;" />
