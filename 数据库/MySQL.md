## SQL 基本语法、连接查询、分页查询

### 基础

* DDL：数据定义，定义数据库对象（库、表、字段）
* DML：数据操作，对表中数据进行增删改
* DQL：数据查询，查表记录
* DCL：数据控制，用来创建 库用户、控制权限等。

#### DDL

1、数据库操作：

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

2、表操作



## 数据库特性

### 索引



### 锁



### 事务



### Master/Slave





### 容灾









## 数据切分

### 读分离库切分



### 主库切分





