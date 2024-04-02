## SQL 基本语法

### 通用语法

* DDL：数据定义，定义数据库对象（库、表、字段）
* DML：数据操作，对表中数据进行增删改
* DQL：数据查询，查表记录
* DCL：数据控制，用来创建 库用户、控制权限等。

#### DDL

##### 数据库操作

* 查询所有数据库

```sql
SHOW DATABASES
```

* 查询当前数据库

```sql
SELECT DATABASE()
```

* 创建数据库

```sql
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]；

-- 例如：
CREATE DATABASE mydatabase
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```

* 删除数据库

```sql
DROP DATABASE [IF EXISTS] 数据库名;
```

* 使用数据库

```sql
USE 数据库名
```

注意⚠️：UTF8字符集长度为3字节，有些符号占4字节，所以推荐用utf8mb4字符集



##### 表操作

* 查询当前数据库所有表

```sql
 SHOW TABLES;
```

*  查询表结构

```sql
DESC 表名;
```

*  查询指定表的建表语句：

```sql
 SHOW CREATE TABLE 表名;
```

* 创建表

```sql
CREATE TABLE 表名
				( 字段1 字段1类型 [COMMENT 字段1注释], 
          字段2 字段2类型 [COMMENT 字段2注释], 
          字段3 字段3类型 [COMMENT 字段3注释], 
          ... 
          字段n 字段n类型 [COMMENT 字段n注释])
        [ COMMENT 表注释 ];
```

注：**最后一个字段后面没有逗号**



* 添加字段：

```sql
 ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
 -- 例：
 ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';
```

* 修改数据类型：

```sql
 ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
```

* 修改字段名和字段类型：

```sql
 ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
 -- 例：将emp表的nickname字段修改为username，类型为varchar(30)
 ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';
```

* 删除字段：

```sql
 ALTER TABLE 表名 DROP 字段名;
```

* 修改表名：

```sql
ALTER TABLE 表名 RENAME TO 新表名
```

* 删除表

```sql
 DROP TABLE [IF EXISTS] 表名;

```

*  删除表，并重新创建该表。（快速清空表中的所有数据）

```sql
 TRUNCATE TABLE 表名;
```



#### DML

##### 添加数据

* 指定字段：

```sql
 INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
```

* 全部字段：

```sql
 INSERT INTO 表名 VALUES (值1, 值2, ...);
```

* 批量添加数据：

```sql
 INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
 INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...);
```

**注意事项**

- 字符串和日期类型数据应该包含在引号中
- 插入的数据大小应该在字段的规定范围内



##### 更新和删除数据

* 修改数据：

```sql
 UPDATE 表名 SET 字段名1 = 值1, 字段名2 = 值2, ... [ WHERE 条件 ];
 -- 例：
 UPDATE emp SET name = 'Jack' WHERE id = 1;
```

* 删除数据：

```sql
 DELETE FROM 表名 [ WHERE 条件 ];
```



#### DQL

语法：

```sql
SELECT 字段列表
FROM 表名字段
WHERE 条件列表
	GROUP BY 分组字段列表
	HAVING 分组后的条件列表
	ORDER BY 排序字段列表
		LIMIT 分页参数
```

* 基础查询

  * 查询多个字段

  ```sql
   SELECT 字段1, 字段2, 字段3, ... 
   FROM 表名;
   SELECT * FROM 表名;
  ```

  * 设置别名

  ```sql
   SELECT 字段1 [ AS 别名1 ], 字段2 [ AS 别名2 ], 字段3 [ AS 别名3 ], ... 
   FROM 表名;
   
   SELECT 字段1 [ 别名1 ], 字段2 [ 别名2 ], 字段3 [ 别名3 ], ... 
   FROM 表名;
  ```

  * 去除重复记录

  ```sql
  SELECT DISTINCT 字段列表 
  FROM 表名;
  ```

  * 转义

  ```sql
  SELECT * FROM 表名 
  WHERE name LIKE '/_张三' ESCAPE '/'
   / 之后的_不作为通配符
  ```

  

##### 条件查询

语法：

```sql
SELECT 字段列表 
FROM 表名 
WHERE 条件列表;
```

条件：


| 比较运算符      | 功能 |
| ----------- | ----------- |
|    >     | 大于       |
| >=   | 大于等于        |
| <   | 小于        |
| <=   | 小于等于        |
| =   | 等于        |
| <>或！=   | 不等于        |
| between ... and ...   | 范围内（含最大、最小）  |
| in(...)   | 在 in 之后的列表中的值，多选一 |
| like占位符   | 模糊匹配（_匹配单个字符，% 匹配任意个字符     |
| is null | 是 null   |

| 逻辑运算符    | 功能         |
|----------|------------|
| and 或 && | 并（多条件同时成立） |
| or 或\|\| | 或（多条件任一成立） |
| NOT 或 !  | 非，不是       |

例：
```sql
-- 年龄等于30
select * from employee where age = 30;
-- 年龄小于30
select * from employee where age < 30;
-- 小于等于
select * from employee where age <= 30;
-- 没有身份证
select * from employee where idcard is null or idcard = '';
-- 有身份证
select * from employee where idcard;
select * from employee where idcard is not null;
-- 不等于
select * from employee where age != 30;
-- 年龄在20到30之间
select * from employee where age between 20 and 30;
select * from employee where age >= 20 and age <= 30;
-- 下面语句不报错，但查不到任何信息
select * from employee where age between 30 and 20;
-- 性别为女且年龄小于30
select * from employee where age < 30 and gender = '女';
-- 年龄等于25或30或35
select * from employee where age = 25 or age = 30 or age = 35;
select * from employee where age in (25, 30, 35);
-- 姓名为两个字
select * from employee where name like '__';
-- 身份证最后为X
select * from employee where idcard like '%X';
```

###### 聚合查询

常见聚合函数

| 函数    | 功能   |
|-------|------|
| count | 统计数量 |
| max   | 最大值  |
| min   | 最小值  |
| avg   | 平均值  |
| sum   | 求和   |
语法：
```sql
SELECT 聚合函数(字段列表) FROM 表名;
--例如：
SELECT count(id) from employee where workaddress = "广东省";
```

##### 分组查询

语法：
```sql
 SELECT 字段列表 
 FROM 表名 
   [ WHERE 条件 ] 
     GROUP BY 分组字段名 
   [ HAVING 分组后的过滤条件 ];
```
where 和 having 的区别：
* 执行时机不同：where 在数据分组前进行过滤，having 在数据分组后进行过滤
* 判断条件不同：where 不能对聚合函数进行判断，而 having 可以。

例：
```sql
-- 根据性别分组，统计男性和女性数量（只显示分组数量，不显示哪个是男哪个是女）
select count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性数量
select gender, count(*) from employee group by gender;
-- 根据性别分组，统计男性和女性的平均年龄
select gender, avg(age) from employee group by gender;
-- 年龄小于45，并根据工作地址分组
select workaddress, count(*) 
from employee 
where age < 45 
group by workaddress;
-- 年龄小于45，并根据工作地址分组，获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count 
from employee 
where age < 45 
group by workaddress 
having address_count >= 3;
```
  注意事项：
```text
 1. 执行顺序： where > 聚合函数 > having
 2. 分组之后，查询的字段一般为聚合函数和分组函数，查询其他字段无任何意义
```

##### 排序查询

语法
```sql
SELECT 字段列表 
FROM 表名 
ORDER BY 字段1 排序方式1, 字段2 排序方式2;
```

排序方式：
* ASC：升序，默认
* DESC：降序

例：
```sql
-- 根据年龄升序排序
SELECT * FROM employee ORDER BY age ASC;
SELECT * FROM employee ORDER BY age;
-- 两字段排序，根据年龄升序排序，入职时间降序排序
SELECT * FROM employee ORDER BY age ASC, entrydate DESC;
```

注意事项
```text
如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序
```

##### 分页查询

语法：
```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数;
```
例：
```sql
-- 查询第一页数据，展示10条
SELECT * 
FROM employee 
    LIMIT 0, 10;
-- 查询第二页
SELECT * FROM employee LIMIT 10, 10;
```

注意事项
```text
1、起始索引从0开始，起始索引 = （查询页码 - 1） * 每页显示记录数
2、分页查询是数据库的方言，不同数据库有不同实现，MySQL是LIMIT
3、如果查询的是第一页数据，起始索引可以省略，直接简写 LIMIT 10
```

* DQL 执行顺序
FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT



#### DCL

* 管理用户

查询用户
```sql
USER mysql;
SELECT * FROM user;
```
创建用户
```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```
修改用户密码：
```sql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```
删除用户
```sql
DROP USER '用户名'@'主机名';
```
例：
```sql
-- 创建用户test，只能在当前主机localhost访问
create user 'test'@'localhost' identified by '123456';
-- 创建用户test，能在任意主机访问
create user 'test'@'%' identified by '123456';
create user 'test' identified by '123456';
-- 修改密码
alter user 'test'@'localhost' identified with mysql_native_password by '1234';
-- 删除用户
drop user 'test'@'localhost';
```
**注意事项**
```text
主机名可以使用 % 通配
```
* 权限控制

常用权限

| 权限                  | 说明         |
|---------------------|------------|
| ALL, ALL PRIVILEGES | 所有权限       |
| SELECT              | 查询数据       |
| INSERT              | 插入数据       |
| UPDATE              | 修改数据       |
| DELETE              | 删除数据       |
| ALTER               | 修改表        |
| DROP                | 删除数据库/表/视图 |
| CREATE              | 创建数据库/表    |

查询数据
```sql
 SHOW GRANTS FOR '用户名'@'主机名';
```
授予权限
```sql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```
撤销权限
```sql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```
**注意事项**
```text
1. 多个权限用逗号分隔
2. 授权时，数据库名和表名可以用 * 进行通配，代表所有
```

### 函数
#### 字符串函数

常用函数

| 函数                  | 功能        |
|---------------------|------------|
| CONCAT(s1, s2, …, sn)      | 字符串拼接，将s1, s2, …, sn拼接成一个字符串        |
| LOWER(str)      | 将字符串全部转为小写        |
| UPPER(str)      | 将字符串全部转为大写        |
| LPAD(str, n, pad)      | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度        |
| RPAD(str, n, pad)      | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度       |
| TRIM(str)      | 去掉字符串头部和尾部的空格        |
| SUBSTRING(str, start, len)      | 返回从字符串str从start位置起的len个长度的字符串        |

使用示例：
```sql
-- 拼接
SELECT CONCAT('Hello', 'World');
-- 小写
SELECT LOWER('Hello');
-- 大写
SELECT UPPER('Hello');
-- 左填充
SELECT LPAD('01', 5, '-');
-- 右填充
SELECT RPAD('01', 5, '-');
-- 去除空格
SELECT TRIM(' Hello World ');
-- 切片（起始索引为1）
SELECT SUBSTRING('Hello World', 1, 5);
```

#### 数值函数

| 函数                  | 功能        |
|---------------------|------------|
| CEIL(x)      | 向上取整       |
| FLOOR(x)      | 向下取整       |
| MOD(x, y)      | 返回x/y的模       |
| RAND()      | 返回0~1内的随机数       |
| ROUND(x, y)      | 求参数x的四舍五入值，保留y位小数       |

#### 日期函数

| 函数                  | 功能                        |
|---------------------|---------------------------|
| CURDATE()                  | 返回当前日期                        |
| CURTIME()                  | 返回当前时间                        |
| NOW()                  | 返回当前日期和时间                          |
| YEAR(date)                  | 获取指定date的年份               |
| MONTH(date)                  | 获取指定date的月份               |
| DAY(date)                  | 获取指定date的日期               |
| DATE_ADD(date, INTERVAL expr type)                  | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1, date2)                  | 返回起始时间date1和结束时间date2之间的天数 |

例子：
```sql
-- DATE_ADD
SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR);
```

#### 流程函数

| 函数                | 功能                        |
|-------------------|---------------------------|
| IF(value, t, f)                | 如果value为true，则返回t，否则返回f                        |
| CASE WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END                  | 如果val1为true，返回res1，… 否则返回default默认值                        |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END                | 如果expr的值等于val1，返回res1，… 否则返回default默认值                        |

例：
```sql
select name, (case when age > 30 then '中年' else '青年' end)
from employee;

select name, (case workaddress when '北京市' then '一线城市' when '上海市' then '一线城市' else '二线城市' end) as '工作地址'
from employee;
```

### 约束

分类

| 约束             | 描述                           | 关键字        |
|----------------|------------------------------|------------|
| 非空约束           | 限制该字段的数据不能为null              | NOT NULL   |
| 唯一约束           | 保证该字段的所有数据都是唯一、不重复的          | UNIQUE     |
| 主键约束           | 主键是一行数据的唯一标识，要求非空且唯一         | PRIMARY KEY  |
| 默认约束           | 保存数据时，如果未指定该字段的值，则采用默认值      | DEFAULT    |
| 检查约束（8.0.1版本后） | 保证字段值满足某一个条件                 | CHECK      |
| 外键约束           | 用来让两张图的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY   |

约束是作用于表中字段上的，可以再创建表/修改表的时候添加约束。

#### 常用约束

| 约束   | 关键字            |
|------|----------------|
| 主键   | PRIMARY KEY    |
| 自动增长 | AUTO_INCREMENT |
| 非空   | NOT NULL       |
| 唯一   | UNIQUE         |
| 逻辑条件 | CHECK          |
| 默认值  | DEFAULT        |

例如
```sql
create table user( 
    id int primary key auto_increment, 
    name varchar(10) not null unique, 
    age int check(age > 0 and age < 120), 
    status char(1) default '1', gender char(1)
                 );
```

#### 外检约束

添加外键
```sql
CREATE TABLE 表名(
    字段名 字段类型, ... [CONSTRAINT] [外键名称] 
        FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
    );

ALTER TABLE 表名 
    ADD CONSTRAINT 外键名称 
        FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
-- 例子
alter table emp 
    add constraint fk_emp_dept_id 
        foreign key(dept_id) references dept(id);
```

删除外键：
```sql
 ALTER TABLE 表名 DROP FOREIGN KEY 外键名;
```

删除/更新行为

| 行为 | 说明 |
|----|----|
| NO ACTION | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新（与RESTRICT一致） |
| RESTRICT | 默认值，拒绝删除或更新主表数据 |
| CASCADE | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录 |
| SET NULL | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（要求该外键允许为null） |
| SET DEFAULT | 父表有变更时，子表将外键设为一个默认值（Innodb不支持） |

更改删除/更新行为：
```sql
ALTER TABLE 表名 
    ADD CONSTRAINT 外键名称 
        FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) 
        ON UPDATE 行为 ON DELETE 行为;
```

### 多表查询

#### 多表关系
* 一对多
* 多对多
* 一对一

#### 查询

合并查询（笛卡尔积，会展示所有组合结果）：

`select * from employee, dept;`

_笛卡尔积：两个集合A集合和B集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）_

消除无效笛卡尔积：

`select * from employee, dept where employee.dept = dept.id;`

#### 内连接查询

内连接查询的是两张表交集的部分

隐式内连接：

`SELECT 字段列表 FROM 表1, 表2 WHERE 条件 ...;`

显式内连接：

`SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ...;`

显式性能比隐式高

例子：
```sql
-- 查询员工姓名，及关联的部门的名称
-- 隐式
select e.name, d.name from employee as e, dept as d where e.dept = d.id;
-- 显式
select e.name, d.name from employee as e inner join dept as d on e.dept = d.id;
```

#### 外链接查询

* 左外连接：
查询左表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ...;`
相当于查询表1的所有数据，包含表1和表2交集部分数据

* 右外连接：
查询右表所有数据，以及两张表交集部分数据
`SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ...;`

例子：
```sql
-- 左
select e.*, d.name 
from employee as e 
    left outer join dept as d on e.dept = d.id;

select d.name, e.* 
from dept d 
    left outer join emp e on e.dept = d.id; 
-- 这条语句与下面的语句效果一样
-- 右
select d.name, e.* 
from employee as e 
    right outer join dept as d on e.dept = d.id;
```
左连接可以查询到没有dept的employee，右连接可以查询到没有employee的dept

#### 自连接查询

当前表与自身的连接查询，自连接必须使用表别名

语法：
`SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;`

自连接查询，可以是内连接查询，也可以是外连接查询

例子：
```sql
-- 查询员工及其所属领导的名字
select a.name, b.name 
from employee a, employee b 
where a.manager = b.id;
-- 没有领导的也查询出来
select a.name, b.name 
from employee a left join employee b on a.manager = b.id;
```

#### 联合查询 union，union all

把多次查询的结果合并，形成一个新的查询集

语法：
```sql
SELECT 字段列表 FROM 表A ...
    UNION [ALL]
SELECT 字段列表 FROM 表B ...
```
注意事项

* UNION ALL 会有重复结果，UNION 不会
* 联合查询比使用or效率高，不会使索引失效

#### 子查询

SQL语句中嵌套SELECT语句，称谓嵌套查询，又称子查询。
```sql
SELECT * FROM t1 
         WHERE column1 = ( SELECT column1 FROM t2);
```

**子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个**

根据子查询结果可以分为：

* 标量子查询（子查询结果为单个值）
* 列子查询（子查询结果为一列）
* 行子查询（子查询结果为一行）
* 表子查询（子查询结果为多行多列）

根据子查询位置可分为：

* WHERE 之后
* FROM 之后
* SELECT 之后

##### 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等）。
常用操作符：- < > > >= < <=

例子：
```sql
-- 查询销售部所有员工
select id from dept where name = '销售部';
-- 根据销售部部门ID，查询员工信息
select * from employee where dept = 4;
-- 合并（子查询）
select * from employee where dept = (select id from dept where name = '销售部');
-- 查询xxx入职之后的员工信息
select * from employee where entrydate > (select entrydate from employee where name = 'xxx');
```

##### 列子查询

返回的结果是一列（可以是多行）。

常用操作符：

| 操作符    | 描述 |
|--------|----|
| IN     | 在指定的集合范围内，多选一 |
| NOT IN | 不在指定的集合范围内 |
| ANY    | 子查询返回列表中，有任意一个满足即可 |
| SOME   | 与ANY等同，使用SOME的地方都可以使用ANY |
| ALL    | 子查询返回列表的所有值都必须满足 |








### 事务

#### 四大特性 ACID

#### 并发事务



# 其他

## 数据库特性

### 索引



### 锁



### 事务HAVING



### Master/Slave





### 容灾









## 数据切分

### 读分离库切分



### 主库切分





