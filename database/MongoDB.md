# MongoDB数据库

## 理论概念

### SQL和NoSQL数据库

- SQL数据库即关系型数据库。
  1. 基于表。
  2. 特点安全，但是结构的变化困难且对系统造成破坏。

- NoSQL数据库即非关系型数据库或分布式数据库。
  1. 基于文档，列，图形，键值
  2. 特点灵活，方便扩展改变结构。

### MongoDB

基于文档型的NoSQL数据库。

##### 数据库结构

- 数据库(database)，存放集合。
  - 集合(collection)，类似于数组，存放文档。
    - 文档(document)，最小存储单位。

## 基础指令

### 数据库操作


```java
//显示所有数据库
show dbs / show databases 
//进入数据库,如没有则新建一个并进入
use 数据库名 
//显示当前所处数据库
db 
//删除数据库
db.dropDatabase()
//运行MongoDB服务
mongod --dbpath c:\data\db --port 123
  dbpath：指定服务器的位置
  port：指定访问服务器的端口
//连接MongoDB服务
mongo
```

### 集合操作

```java
//显示数据库中所有的集合
show collections / show tables
//删除集合
db.<collection_name>.drop()
//创建集合,name是集合名，options是JSON串的可选参数
db.createCollection(name, options)
{capped:true(固定集合，须指定size，超过则覆盖),
 autoIndexId:true(自动在 _id 字段创建索引默认为 false),
 size:数值(单位KB),
 max:数值(集合内最多能包含的文档数)}
//返回count()文档数量
db.<collection_name>.count()
db.<collection_name>.find().count()
```

### 文档操作

#### 插入文档

```java
//插入文档，使用insert()或save(),document为JSON格式,如果集合不存在，新建之并插入document
db.collection.insert(
   <document or array of documents>,
   {
     writeConcern: <document>,
     ordered: <boolean>
   }
)
//插入一个文档
db.<collection_name>.insertOne(
   <document>,
   {
      writeConcern: <document>
   }
)
//插入多个文档
db.<collection_name>.insertMany(
   { [ <document 1> , <document 2>, ... ] },
   {
      writeConcern: <document>,
      ordered: <boolean>
   }
)
//writeConcern[可选]指定抛出异常的级别，orderd[可选]默认为true，即有序操作，当插入过程中出现异常时，剩下的文档将不会被插入，值为false时，出现异常不影响其他文档的插入
```

#### 查询文档

```java
//查看所有集合，query filter[可选]查询条件，projection用来指定需要返回的字段
db.<collection_name>.find( <query filter>, <projection> )
//projection中除了_id字段外(默认显示)，其他字段要么都是0，指定的字段不显示，要么都不是0，指定的字段显示。
db.students.find({sex:1},{_id:0,name:1,age:1,school:-1}) //只显示name，age,school,不显示_id
db.students.find({sex:1},{name:0,age:0}) //显示除name，age之外的字段，_id显示
```

#### 查询操作符

```
//等于
{ <field>: { $eq: <value> } }

```



#### 更新文档

```java
//更新文档，query查询条件，update修改内容，upsert默认false，true时如果没有匹配的文档则新建一个，multi默认false，只修改第一条记录，true时，修改匹配的所有记录
db.<collection_name>.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
//update如果只是一个文档的话，会把除_id之外的内容整个替换掉
db.students.update({sex:0},{age:23})   //更新后的文档变为{_id:XXXX,age:23}
//替换文档,替换的document，须带有_id属性
db.<collection_name>.save(<document>,{writeConcern:<document>(抛出异常的级别)})
```

#### 更新操作符

```java
//set操作符，修改指定字段的值，不影响其他字段
{ $set: { <field1>: <value1>, ... } }
//unset操作符，删除指定字段，值任意
{ $unset: { <field1>: "", ... } }
```

#### 删除文档

```
//删除文档，query查询条件，justOne默认false删除所有匹配的文档，true时只删除第一个匹配的文档
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>,
     collation: <document>
   }
)

```

其余操作

```java


```

