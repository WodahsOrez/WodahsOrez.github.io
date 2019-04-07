# 约束，索引，注释

## 数据完整性(data integrity)

数据完整性(complete)、一致性(consistent)[^1]和准确无误(accurate)的程度。

存储在数据库中的所有数据值均正确的状态。如果数据库中存储有不正确的数据值，则该数据库称为已丧失数据完整性。

### 实体完整性

表中每一行记录代表一个实体，指这些实体的完整性。

方法：主键约束和唯一值约束。

### 域完整性

指列的值域的完整性。

方法：数据类型、CHECK约束、UNIQUE约束、default默认值、自增、not null/null

### 参照完整性

指表与表之间数据参照关系的完整性。

方法：外键约束FOREIGN KEY 

[^1]: 数据的自洽和逻辑关系

## 约束

| 约束名                  | 作用                               |
| ----------------------- | ---------------------------------- |
| NOT   NULL/NULL         | 能否为空                           |
| UNIQUE                  | 保证某列的每行都唯一，允许多个null |
| PRIMARY KEY             | 主键约束，非空唯一                 |
| FOREIGN KEY             | 外键约束，列值参照另一表           |
| CHECK                   | 限制列值范围                       |
| DEFAULT   '非数值'/数值 | 默认值                             |
| AUTO_INCREMENT          | 自增列                             |
| ZEROFILL                | 不足位0填充，自带UNSIGNED          |
| UNSIGNED                | 无符号的                           |

### 约束的通用写法


```sql
-- 在创建列时声明
CREATE TABLE student(
id int PRIMARY KEY auto_increment,
tid int ZEROFILL CHECK (tid > 0), 
name varchar(50) UNIQUE NOT NULL,
Sex varchar(10) default '男',
-- 在列创建完后声明约束
[CONSTRAINT fk_student_tid] FOREIGN KEY (tid) REFERENCES teacher(id)，
-- FOREIGN KEY (tid,name) REFERENCES teacher(id，name) 组合外键
-- PRIMARY KEY(id) 可以放到下面写
-- PRIMARY KEY(id，tid) 复合主键
-- UNIQUE(id,tid)
-- [CONSTRAINT chk_student] CHECK (tid > 0)
);
-- 用ALTER添加
ALTER TABLE student ADD PRIMARY KEY (id);
ALTER TABLE student ADD UNIQUE (id);
ALTER TABLE student ADD CHECK (tid > 0);
ALTER TABLE student ALTER Sex SET DEFAULT '女'
ALTER TABLE student ADD [CONSTRAINT fk_student_tid] FOREIGN KEY(tid) REFERENCES teacher(id);
-- 用ALTER修改
ALTER TABLE student MODIFY id int primary key；
ALTER TABLE student MODIFY id int primary key；
ALTER TABLE student MODIFY Sex VARCHAR(50) DEFAULT '女';
ALTER TABLE student MODIFY name VARCHAR(50) unique not null;
-- 删除约束
ALTER TABLE student DROP PRIMARY KEY;
ALTER TABLE student DROP index name;
ALTER TABLE student DROP CHECK chk_student;
ALTER TABLE student DROP FOREIGN KEY fk_student_tid;
```

## 索引

索引是一个单独的、物理的数据库结构，它是某个表中一列或若干列值的集合和相应的指向表中物理标识这些值的数据页的逻辑指针清单。

优缺点：提高查询速度,降低更新,删除,插入的速度

| 索引名称    | 中文名        | 作用                                                         |
| ----------- | ------------- | ------------------------------------------------------------ |
| PRIMARY KEY | 主键索引      | 自然主键(自增数)荐/业务主键(字段)                            |
| UNIQUE      | 唯一,null除外 | 加速查询 + 列值唯一（可以有null）                            |
| INDEX       | 普通索引      | 最基本的索引，没有任何限制                                   |
| FULLTEXT    | 全文索引      | 仅可以适用于MyISAM引擎的数据表；作用于CHAR、VARCHAR、TEXT数据类型的列。 |

#### 创建索引

```mysql
CREATE TABLE table_name(
-- CREATE 时 跟在字段后面
INDEX index_name (colname,...)
}
CREATE [UNIQUE|FULLTEXT] INDEX index_name ON table(colname,...)
ALTER TABLE table_name ADD [UNIQUE|INDEX|FULLTEXT]  index_name ON (colname,...)
-- 对于字符类型的列，可以编制“前缀索引”，length表示按照列的指定长度的字符串索引
索引列名 [(length)] [ASC|DESC]
```

#### 删除索引

DROP index 索引名 on 表名

ALTER table 表名 drop 索引名

#### 查看索引

SHOW INDEX/KEYS FROM 表名

什么时候加索引：字段数据庞大,字段很少用DML操作,字段经常出现在where条件中

外键约束定义语句  on update cascade  级联更新

## 注释

### 创建表的时候写注释

```sql
create table test1
(
	field_name int comment '字段的注释'
)comment='表的注释';
```

### 修改表的注释

```sql
alter table test1 comment '修改后的表的注释';
```

### 修改字段的注释

```sql
alter table test1 modify column field_name int comment '修改后的字段注释';
--注意：字段名和字段类型照写就行
```

