## 约束,索引

### 字段属性

| NOT   NULL/NULL         | 能否为空    |
| ----------------------- | ----------- |
| UNSIGNED                | 无符号的    |
| DEFAULT   '非数值'/数值 | 默认值      |
| ZEROFILL                | 不足位0填充 |
| AUTO_INCREMENT          | 自增列      |

### 索引

| PRIMARY KEY              | 主键(非空,唯一) | 自然主键(自增数)荐/业务主键(字段)                    |
| ------------------------ | --------------- | ---------------------------------------------------- |
| FOREIGN   KEY/references | 外键约束        | 可以为null,只有表级描述/   关联的列必须唯一/推荐主键 |
| UNIQUE                   | 唯一,null除外   |                                                      |
| INDEX/KEY                | 常规索引        |                                                      |
| FULLTEXT                 | 全文索引        |                                                      |

#### 创建索引

CREATE 时 跟在字段后面,或者在下方 索引名(字段名) 使用

CREATE [unique] index 索引名 on 表名(列名)

ALTER table 表名 add [unique] index 索引名(列名)

#### 删除索引

DROP index 索引名 on 表名

ALTER table 表名 drop 索引名

#### 查看索引

SHOW INDEX/KEYS FROM 表名

什么时候加索引：字段数据庞大,字段很少用DML操作,字段经常出现在where条件中

外键约束定义语句  on update cascade  级联更新

 

### 注释

#### 创建表的时候写注释

```sql
create table test1
(
	field_name int comment '字段的注释'
)comment='表的注释';
```

#### 修改表的注释

```sql
alter table test1 comment '修改后的表的注释';
```

#### 修改字段的注释

```sql
alter table test1 modify column field_name int comment '修改后的字段注释';
--注意：字段名和字段类型照写就行
```

