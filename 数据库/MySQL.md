## SQL 基本语法



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











## 数据库特性

### 索引



### 锁



### 事务



### Master/Slave





### 容灾









## 数据切分

### 读分离库切分



### 主库切分





