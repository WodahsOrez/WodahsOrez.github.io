# Oracle

## Oracle与MySQL的差异

### 排序问题

针对null值排序的问题Oracle和MySQL有不同的写法

#### Oracle写法

null值排在最前

```sql
select * from A order by a desc null first
```

null值排在最后

```sql
select * from A order by a desc null last
```

#### MySQL写法

null值排在最后，用Mysql的IF和ISNULL函数。如果为空返回1，否返回0

```sql
select * from A order by IF(ISNULL(a),1,0),a desc
```

null值排在最前，用Mysql的IF和ISNULL函数。如果为空返回0，否返回1

```sql
select * from A order by IF(ISNULL(a),0,1),a desc
```

### 模糊匹配

#### Oracle写法：`||`用来连接字符串

```sql
where a like '%'|| #{参数} ||'%'
```

#### MySQL写法：使用concat函数

```sql
where a like concat('%', #{参数} ,'%')
```

### 基础类型

Oracle是没有int类型，只有number类型

### 分页问题

#### Oracle写法：

注意：如下方法使用rownum查询无结果

```sql
select count(*) from gcfr_t_vch a where rownum>1;
```

因为是伪列，每次从1开始计数，如果1不能rownum的条件那么下一次还会从1开始，导致所有数据都不符合。

通过把rownum变成子表变量来查询可以规避该风险。pageNo要显示第几页，PageSize每页显示的条数。

```sql
select * from (select rownum r,t.* from gcfr_t_vch t) tt where tt.r >((pageNo - 1) * pageSize) and tt.r <= (pageNo * pageSize);
```

#### MySQL写法：

关键词limit m,n 其中m表示起始坐标(从0开始)，n表示要想显示的条数(包含其实坐标)。

```sql
select * from A limit 1,4
```

### 自增主键的实现

#### Oracle写法：

```sql
create sequence student_id_seq;  --创建序列
insert into student values (student_id_seq.nextval,'张三');
--触发器自动增加
create or replace trigger test_id
before insert on student  --before:执行DML等操作之前触发
for each row  --行级触发器
begin 
	select student_id_seq.nextval into :new.id from dual;
end;
```

#### MySQL写法：

```sql
id INT UNSIGNED NOT NULL AUTO_INCREMENT --建表时约束自增
```

