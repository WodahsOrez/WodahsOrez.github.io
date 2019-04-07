## 视图

视图的定义保存在数据字典内。创建视图所基于的表为“基表”

### 创建视图

```mysql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```

#### 说明

1. **OR REPLACE**：给定了OR REPLACE子句，语句能够替换已有的同名视图。

2. **ALGORITHM**：可选的mysql算法扩展，算法会影响MySQL处理视图的方式。有以下三个值：
    - **UNDEFINED**--将由MySQL选择所要使用的算法。如果可能，它倾向于MERGE而不是TEMPTABLE，这是因为MERGE通常更有效，而且如果使用了临时表，视图是不可更新的。
    - **MERGE**--语句上的合并,视图的查询语句与使用视图的语句组合。
    - **TEMPTABLE**--视图的结果将被置于临时表中，然后使用它执行语句。

3. **column_list**：要想为视图的列定义明确的名称，列出由逗号隔开的列名。column_list中的名称数目必须等于SELECT语句检索的列数。若使用与源表或视图中相同的列名时可以省略column_list。

4. **select_statement**：用来创建视图的SELECT语句，可在SELECT语句中查询多个表或视图。但对SELECT语句有以下的限制：
    - 定义视图的用户必须对所参照的表或视图有查询（即可执行SELECT语句）权限；
    - 在定义中引用的表或视图必须存在；

5. WITH[cascaded|local] CHECK OPTION：不写默认为cascaded

    - **cascaded**：所有针对该视图执行的语句都必须满足视图和所有底层视图的where条件—— 即使那些视图不是带检查选项定义的，也是如此
    - **local**：针对这个视图执行的语句只需要满足指定了检查选项的视图的where条件就可以了

6. 视图定义的限制条件

    -  SELECT语句不能包含FROM子句中的子查询。

    -  SELECT语句不能引用系统或用户变量。

    -  SELECT语句不能引用预处理语句参数。

    -  在存储子程序内，定义不能引用子程序参数或局部变量。

    -  在定义中引用的表或视图必须存在。但是，创建了视图后，能够舍弃定义引用的表或视图。要想检查视图定义是否存在这类问题，可使用CHECK TABLE语句。

    -  在定义中不能引用TEMPORARY表，不能创建TEMPORARY视图。

    -  在视图定义中命名的表必须已存在。

    -  不能将触发程序与视图关联在一起。

 

### 修改视图结构

```mysql
ALTER [ALGORITHM = {UNDEFINED | MERGE |TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL]CHECK OPTION]
```

#### 说明：

该语句用于更改已有视图的定义。其语法与CREATE VIEW类似。该语句需要具有针对视图的CREATE VIEW和DROP权限，也需要针对SELECT语句中引用的每一列的某些权限。

 

### 查看视图结构

SHOW CREATE VIEW view_name

#### 说明：

该语句给出了1个创建给定视图的CREATE VIEW语句。

 

### 删除视图

DROP VIEW [IF EXISTS]

view_name [, view_name]... [RESTRICT | CASCADE]

#### 说明：

- DROP VIEW能够删除1个或多个视图。必须在每个视图上拥有DROP权限。

- 可以使用关键字IF EXISTS来防止因不存在的视图而出错。

- 如果给定了RESTRICT和CASCADE，将解析并忽略它们。



### 更新视图数据

#### 概述：

1. 视图的使用与表一样，有增删改查四种操作，且语法也与表相同。

2. 在视图上也可以使用修改数据的DML语句，如INSERT、UPDATE和DELETE可以统称为“通过视图更新数据”。

3. 通过视图更新数据有如下限制：

    - 一次只能修改一个底层的基表

    - 如果修改违反了基表的约束条件，则无法更新视图

    - 如果视图中的列不是表中的原始列（如创建视图时使用了连接操作符、聚合函数等），则不能通过视图更新。



#### 视图更新操作：

可更新的视图：要通过视图更新基本表数据，必须保证视图是可更新视图，即可以在INSET、UPDATE或DELETE等语句当中使用它们。对于可更新的视图，在视图中的行和基表中的行之间必须具有一对一的关系。还有一些特定的其他结构，这类结构会使得视图不可更新。如果视图包含下述结构中的任何一种，那么它就是不可更新的：

- 聚合函数；
- DISTINCT关键字；
- GROUP BY子句；
- ORDER BY子句；
- HAVING子句；
- UNION运算符；
- 位于选择列表中的子查询；
- FROM子句中包含多个表；
- SELECT语句中引用了不可更新视图；



**插入数据**：使用INSERT语句通过视图向基本表插入数据

**注意**：

-  当视图所依赖的基本表有多个时，不能向该视图插入数据，因为这将会影响多个基本表。

-  对INSERT语句还有一个限制：SELECT语句中必须包含FROM子句中指定表的所有不能为空的列。

**修改数据**：使用UPDATE语句可以通过视图修改基本表的数据

**注意**：若视图是基于多个表使用联接操作而导出的，那么对这个视图执行更新操作时，每次只能影响其中的一个表。

**删除数据**：使用DELETE语句可以通过视图删除基本表的数据

**注意**：对依赖于多个基本表的视图，不能使用DELETE语句。