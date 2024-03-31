

## JDBC缺点

1. 硬编码

   a. 注册驱动，获取链接

   b. SQL 语句

2. 操作繁琐

   a. 手动设置参数

   b. 手动封装结果集

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240328134802103.png" alt="image-20240328134802103" style="zoom:40%;" />

Mybatis 免除了几乎所有的 jdbc 代码以及设置参数和获取数据集的工作。

## 快速入门

1. 创建数据库表，添加数据

2. 创建模块，导入 maven 坐标：Mybatis、mySQL、junit、slf4j等

3. 编写 Mybatis 核心配置文件：mybatis-config.xml。（SqlSessionFactory)

   1. 填写 jdbc数据库连接信息：driver、url、username、password。
   2. 加载 sql 映射文件：添加<mappers>: <mapper resource = "xxxMapper.xml" />等

4. 编写SQL 映射文件：xxxMapper.xml文件。统一管理 sql 语句。

   ```xml
   <mapper namespace="test">
     <select id="selectBlog" resultType="Blog">
     		select * from Blog where id = #{id}
     </select>
   </mapper>
   ```

5. 编码

   1. 定义 pojo 实体类
   2. 加载核心配置文件，获取 SqlSessionFactory 对象
   3. 获取 SqlSession 对象，执行 SQL 语句。
   4. 释放资源。

   



## Mapper代理开发

步骤：

1. 定义与 SQL 映射文件（xxxMapper.xml）同名的 **Mapper 接口**(xxxMapper.java)，并且将 Mapper 接口与 SQL 映射文件放置在同一目录下。
2. 设置 SQL 映射文件的 namespace 属性为 Mapper 接口权限定名。
3. 在 Mapper 接口中定义方法，方法名就是 SQL 映射文件中的 sql 语句的 id，并保持参数类型和返回值类型一致
4. 编码：
   1. 通过 SqlSession 的 getMapper 方法获取 Mapper 接口的代理对象。
   2. 调用对应方法完成 sql 的执行。





## Mapper核心配置文件

mapper-config.xml

可配置多个数据连接

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329102435115.png" alt="image-20240329102435115" style="zoom: 35%;" />



## 配置文件完成增删改查

安装 MybatisX 插件：XML 和接口方法相互跳转、根据接口方法生成 statement。



### 查



数据库字段名称 和 实体类属性名称 不一致，不能自动封装数据。

1. 起别名，sql 语句起别名，每次查询都要定义别名，可抽离 sql 片段，但仍有局限性。
2. 定义**resultMap**，完成字段映射。
   * resultMap
     1. ﻿﻿﻿定义<resultMap>标签
     2. ﻿﻿﻿在<select>标签中，使用resultMap属性替换 resultType属性

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329111416802.png" alt="image-20240329111416802" style="zoom:45%;" />



参数占位符

* #{} : 会将其替换为  ？，为了防止SQL 注入
* ${}：拼接 SQL，会存在 SQL 注入问题。
* 使用时机：
  * 参数传递的时候：#{}；
  * 表名、列名不固定的情况下：${}。会存在 SQL 注入问题。



sql 语句中 特殊字符（如"<"）处理

* 转义字符
* ﻿<I［CDATA［ 内容11> 区：输入“CD”提示。



#### 条件查询



SQL 语句设置多个参数有几种方式？

  		1. 散装参数：需要使用@Param（“SQL 中的参数占位符名称”）
  		2. 实体类封装参数
       * 只需要保证SQL 中的参数名和实体类属性名对应上，即可设置成功。
  		3. map 集合
       * 只需要保证 SQL 中的参数明 和 map 集合的键的名称对应上，即可。

![image-20240329114245001](/Users/ding/Library/Application Support/typora-user-images/image-20240329114245001.png)



#### 多条件-动态条件查询

* SQL语句会随着用户的输入或外部条件的变化而变化，称为**动态SQL**

标签：if、choose

* if：多条件判断，用户判断参数是否有值，使用 test 属性进行条件判断：test：逻辑表达式

  * 问题：SQL 拼接会报错，第一个条件不需要逻辑运算符。

    * 解决：1、恒等式，条件前加 “1=1‘恒等式；

      ​			2、Mybatis 提供<where> 标签替换 where 关键字。解决上述问题。

* choose（when、otherwise）单条件动态查询。

  <img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329142450020.png" alt="image-20240329142450020" style="zoom:33%;" />

### 增

注意 MyBatis 事务：

* openSession（）：默认开启事务，不会自动提交，进行增删改操作后需要使用 sqlSession.commit(); 手动提交食物。
* openSession（true）：可以设置为自动提交事务（关闭事务）；



#####  主键返回

添加成功后，需要获取插入数据库数据的主键的值，比如添加订单和订单项。

 添加如下属性值：<insert useGeneratedKeys="true" keyProperty="id">

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329143936608.png" alt="image-20240329143936608" style="zoom:50%;" />

 

### 改

* 修改全部字段

* 修改**动态**字段：标签<set>

   <img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329144847495.png" alt="image-20240329144847495" style="zoom:50%;" />

    

### 删

* 删一个

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329145906292.png" alt="image-20240329145906292" style="zoom:50%;" />

* 批量删除 

  <img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329145535860.png" alt="image-20240329145535860" style="zoom:50%;" />

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329145933684.png" alt="image-20240329145933684" style="zoom:50%;" />



## MyBatis参数传递

MyBatis 接口方法中可以接受各式各样的参数，Mybatis 底层对于这些参数进行了不同的封装处理方式。

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329152006807.png" alt="image-20240329152006807" style="zoom:25%;" />

* 单个参数：

  * POJO 参数：直接使用，保证属性名 与参数占位符名称一致。

  * Map 集合：直接使用，保证键名 与参数占位符名称一致。

  * Collection：底层封装为 Map 集合

      			  map.put("arg0", collection集合)

    ​				map.put("collection", collection集合)

    * List：底层封装为 Map 集合

    ​			 	map.put("arg0", list集合)

    ​				map.put("collection", list集合)

    ​				map.put("list", list集合)

    * Array：底层封装为 Map 集合

    ​			    map.put("arg0", 数组）

    ​				map.put("array", 数组)

  * 其他类型：直接使用，比如 int

* 多个参数：底层封装为 Map 集合（ map.put("arg0",参数值 1)、map.put("arg1",参数值 2)、map.put("param1",参数值 1)、map.put("param1",参数值 2)、)

  * 多个利用**@Param**（“username”），映射参数，替换底层 Map 中默认的 arg 键名

Mybatis 提供了**ParamNameResolver**类来进行参数封装。

**建议**：将来都使用@Param注解来修改Map集合中默认的键名，并使用修改后的名称来获取值，这样。可读性更高！





## 注解开发 完成 CRUD

提示：

* 简单功能用注解

* 复杂功能用配置文件

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240329152144231.png" alt="image-20240329152144231" style="zoom:50%;" />



