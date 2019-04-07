# 存储过程

## 语法

### 定义存储过程

```mysql
DELIMITER //
CREATE PROCEDURE sp_name ([proc_parameter[,...]])
[characteristic ...] routine_body
//
DELIMITER ;

-- proc_parameter(存储过程参数):
[ IN | OUT | INOUT ] param_name type

-- type:
任何合法的MySQL数据类型
-- routine_body 存储过程体
```

#### proc_parameter(存储过程参数)

- IN：传入参数(默认选项)。可以是字面量或变量，在存储过程中改变该变量的值不会影响该变量在存储过程外的值，即只是传值不传址。
- OUT：输出参数。只能是变量。在存储过程中该变量的值为null，存储过程结束时，把存储过程外该变量的值变成存储过程内该变量的值。即只传了一个值为null的址。
- INOUT：输入输出参数。只能是变量。外部的值可以在存储过程内使用，存储过程内改变的值也会影响外部。即传了一个带本身值得址。

#### characteristic(特征值)

-  **LANGUAGE SQL**：说明下面过程的BODY 是使用SQL 语言编写，这条是系统默认的，为今后MySQL 会支持的除SQL 外的其他语言支持的存储过程而准备。
-  **[NOT] DETERMINISTIC**：如果程序或线程总是对同样的输入参数产生同样的结果，则被认为它是“确定的”（DETERMINISTIC），否则就是“非确定”的。默认的就是NOT DETERMINISTIC。
-  **{ CONTAINS SQL | NO SQL | READS SQL DATA      | MODIFIES SQL DATA }**：这些特征值提供子程序使用数据的内在信息，这些特征值目前只是提供给服务器，并没有根据这些特征值来约束过程实际使用数据的情况。默认使用的值是CONTAINS SQL。
  - **CONTAINS SQL** 表示子程序不包含读或写数据的语句。
  - **NO SQL** 表示子程序不包含SQL 语句。
  - **READS SQL DATA** 表示子程序包含读数据的语句，但不包含写数据的语句。
  - **MODIFIES SQL DATA**      表示子程序包含写数据的语句。如果这些特征没有明确给定。
-  **SQL SECURITY { DEFINER | INVOKER}**：可以用来指定子程序该用创建子程序者的许可来执行，还是使用调用者的许可来执行。默认值是DEFINER(创建者)。
-  **COMMENT 'string'**：存储过程或者函数的注释信息。

#### 分隔符

MySQL默认以";"为分隔符，如果没有声明分割符，则编译器会把存储过程当成SQL语句进行处理，因此编译过程会报错，所以要事先用“DELIMITER //”声明当前段分隔符，让编译器把两个"//"之间的内容当做存储过程的代码，不会执行这些代码；“DELIMITER ;”的意为把分隔符还原。

#### 创建函数

```mysql
CREATE FUNCTION sp_name ([func_parameter[,...]])
RETURNS type [characteristic ...] routine_body
```

函数的参数都是类似in型的，且只能有一个返回值用return定义。

#### 修改存储过程/函数特性

```mysql
ALTER {PROCEDURE | FUNCTION} sp_name [characteristic ...]
```

#### 删除存储过程/函数

```mysql
DROP {PROCEDURE | FUNCTION} [IF EXISTS] sp_name
```

#### 查看存储过程/函数

```mysql
SHOW {PROCEDURE | FUNCTION} STATUS [LIKE 'pattern'] -- 查看状态
SHOW CREATE {PROCEDURE | FUNCTION} sp_name -- 查看定义
select * from information_schema.Routines where ROUTINE_NAME = 'sp_name '
```

#### 存储过程/函数的调用

```mysql
CALL sp_name([parameter[,...]])
```



### 过程体语法

#### 变量操作

变量的定义必须写在begin/end的开头，其作用域即begin/end的范围。多层嵌套begin/end时，对于同名变量采取就近原则，优先使用本作用域内的变量。

```mysql
-- 声明变量，必须复合语句的开头其他语句前，可以规定默认值。
DECLARE var_name[,...] type [DEFAULT value]
-- 变量赋值，一个SET语句可以赋值多个变量。
SET var_name = expr [, var_name = expr] ...
-- 把查询语句的列名域内的值赋值给变量。
SELECT col_name[,...] INTO var_name[,...] table_expr
@是用户自定义变量，@@是系统定义变量
```

#### 流程语句

##### BEGIN END语句(复合语句)

```mysql
[begin_label:] BEGIN
　　[statement_list]
END [end_label];
```

##### IF 语句

```mysql
IF search_condition THEN statement_list
[ELSEIF search_condition THEN statement_list] ...
[ELSE statement_list]
END IF;
-- search_condition 条件表达式,即where后跟的那种
-- statement_list 执行的代码语句
```

##### CASE语句

```mysql
CASE case_value
WHEN when_value THEN statement_list
[WHEN when_value THEN statement_list] ...
[ELSE statement_list]
END CASE；
-- case_value 条件判断的变量
-- when_value 变量的取值
-- statement_list 满足特定when_value时执行的语句

CASE
WHEN search_condition THEN statement_list
[WHEN search_condition THEN statement_list] ...
[ELSE statement_list]
END CASE；
-- search_condition 条件判断语句
-- statement_list 满足特定search_condition时执行的语句
```

##### LOOP语句

```mysql
[begin_label:] LOOP 
-- 循环执行的语句
END LOOP [end_label] ;
```

begin_label参数和end_label参数分别表示循环开始和结束的标志，这两个标志必须相同，而且都可以省略

##### LEAVE语句

```mysql
LEAVE label ;
```

LEAVE语句主要用于跳出循环控制，label参数表示循环的标志。

##### ITERATE语句

```mysql
ITERATE label ;
```

ITERATE语句是跳出本次循环，然后直接进入下一次循环。只可以出现在LOOP、REPEAT、WHILE语句内

##### REPEAT语句

```mysql
[begin_label:] REPEAT 
-- 循环执行的语句 
UNTIL search_condition 
END REPEAT [end_label] ;
```

search_condition参数表示结束循环的条件，满足该条件时循环结束。

search_condition为0就是死循环。

##### WHILE语句

```mysql
[begin_label:] WHILE search_condition DO 
-- 循环执行的语句  
END WHILE [end_label] ;
```

search_condition参数表示循环执行的条件，满足该条件时循环执行。

## 条件和处理

### 条件的定义

```mysql
DECLARE condition_name CONDITION FOR condition_value
```

**condition_value**(条件类型):

- SQLSTATE [VALUE] sqlstate_value：如ERROR 1142（42000）的42000
- mysql_error_code ：如ERROR 1142（42000）的1142

### 条件的处理

```mysql
DECLARE handler_type HANDLER FOR condition_value[,...] sp_statement
```

#### handler_type ：指定错误处理方式

- CONTINUE 表示继续执行下面的语句。
- EXIT 则表示执行终止。
- UNDO 现在还不支持。

**sp_statement**是处理语句，都在continue和exit之前执行。

#### condition_value ：表示错误类型

- SQLSTATE [VALUE] sqlstate_value：包含5个字符的字符串错误值
- mysql_error_code：数值类型的错误代码
- cond_name：定义条件的名称

- SQLWARNING 是对所有以01 开头的SQLSTATE 代码的速记。
- NOT FOUND 是对所有以02 开头的SQLSTATE 代码的速记。
- SQLEXCEPTION 是对所有没有被SQLWARNING 或NOT FOUND 捕获的SQLSTATE 代码的速记。

#### 参考：

[MySql sqlstate代码大全](http://blog.csdn.net/u013847120/article/details/52887813)

## 游标的使用

### 声明游标

```mysql
DECLARE cursor_name CURSOR FOR select_statement
```

声明一个指向select_statement结果集的游标

### OPEN 游标

```mysql
OPEN cursor_name
```

### FETCH 游标

```mysql
FETCH cursor_name INTO var_name [, var_name] ...
```

读取游标下一行数据，并存入var_name内，最后游标下移一行。

### CLOSE 游标

```mysql
CLOSE cursor_name
```

### 注意：

1. 变量、条件、处理程序、游标都是通过DECLARE 定义的，它们之间是有先后顺序的要求的。变量和条件必须在最前面声明，然后才能是游标的声明，最后才可以是处理程序的声明。

2. 表的列名一旦改变就有可能使存储过程失效

## 触发器

它是个特殊的存储过程，它的执行不是由程序调用，也不是手工启动，而是由事件来触发，比如当对一个表进行操作（insert，delete，update）时就会激活它执行。触发器经常用于加强数据的完整性约束和业务规则等。 触发器可以从DBA_TRIGGERS，USER_TRIGGERS 数据字典中查到。

### 创建触发器

```mysql
CREATE [DEFINER = { user | CURRENT_USER }]
TRIGGER <触发器名称>
{ BEFORE | AFTER }
{ INSERT | UPDATE | DELETE }
ON <表名称>
FOR EACH ROW
<触发的SQL语句>
```

**说明**：

1. DEFINER：触发器触发时检查权限，一般格式为'user_name'@'host_name'。

2. 触发器名称：触发器必须有名字，最多64个字符，可能后面会附有分隔符。它和MySQL中其他对象的命名方式基本相象

3. 触发程序的动作时间：BEFORE  AFTER. 可以设置为事件发生前或后.

4. 事件：指明了激活触发程序的语句的类型。可以是下述值之一：

    - INSERT：将新行插入表时激活触发程序，例如，通过INSERT、LOADDATA和REPLACE语句。
    - UPDATE：更改某一行时激活触发程序，例如，通过UPDATE语句。
    - DELETE：从表中删除某一行时激活触发程序，例如，通过DELETE和REPLACE语句。

5. 表名称：触发器是属于某一个表的：当在这个表上执行插入、更新或删除操作的时候就导致触发器的激活。我们不能给同一张表的同一个事件安排两个触发器，而且必须引用永久性表，不能将触发程序与TEMPORARY表或视图关联起来。(MySQL 5.7支持多触发器)

6. 触发间隔：FOR EACH ROW通知触发器每隔一行执行一次动作，而不是对整个表执行一次。

7. 关于OLD的和NEW的列名称标识

    - 在触发器的SQL语句中，对于表中的任意列，你会需要用到该事件发生后该列的数据NEW . column_name和事件发生前该列的数据OLD. column_name，用OLD和NEW来区分事件前后数据的变化。
    - 对于INSERT语句,只有NEW是合法的；对于DELETE语句，只有OLD才合法；而UPDATE语句可以同时使用NEW和OLD。

8. 触发的SQL语句：是当触发程序激活时执行的语句。如果你打算执行多个语句，可使用BEGIN
   ... END复合语句结构。这样，就能使用存储子程序中允许的相同语句。

### 删除触发器

```mysql
DROP TRIGGER [schema_name.]trigger_name
```

#### 说明：

方案名称（schema_name）是可选的。如果省略了schema（方案），将从当前方案中舍弃触发程序。DROP TRIGGER语句需要SUPER权限。

### 查询触发器

#### 模糊查询

```mysql
SHOW TRIGGERS [{FROM | IN} db_name]
[LIKE 'pattern' | WHERE expr]
```

#### 查询所有触发器:

```mysql
SELECT * FROM information_schema.`TRIGGERS`
```

#### 查询触发器定义语句:

```mysql
select * from information_schema.triggers where TRIGGER_NAME='触发器名';
```

### 触发器执行的语句有以下两个限制

1. 触发程序不能调用将数据返回客户端的存储程序，也不能使用采用CALL 语句的动态SQL语句，但是允许存储程序通过参数将数据返回触发程序。也就是存储过程或者函数通过OUT或者INOUT 类型的参数将数据返回触发器是可以的，但是不能调用直接返回数据的过程。
2. 不能在触发器中使用以显式或隐式方式开始或结束事务的语句，如START TRANSACTION、COMMIT 或ROLLBACK。

### 注意:

MySQL 的触发器是按照BEFORE 触发器、行操作、AFTER 触发器的顺序执行的，其中任何一
步操作发生错误都不会继续执行剩下的操作。如果是对事务表进行的操作，那么会整个作为
一个事务被回滚（Rollback），但是如果是对非事务表进行的操作，那么已经更新的记录将无
法回滚，这也是设计触发器的时候需要注意的问题。