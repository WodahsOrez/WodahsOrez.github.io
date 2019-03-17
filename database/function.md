# MySQL数据库函数

## 多行函数/聚合函数

对一组值执行计算并返回单一的值的函数,不能直接在where使用

| 聚合函数               | 支持的数据类型       | 描述                                                         |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| sum(name)              | 数字                 | 对指定列中的所有非空值求和                                   |
| avg(name)              | 数字                 | 对指定列中的所有非空值求平均值                               |
| min(name)              | 数字,字符,日期       | 返回指定列的最小值                                           |
| max(name)              | 数字,字符,日期       | 返回指定列的最大值                                           |
| count([DISTINCT] name) | 任意基于行的数据类型 | 返回匹配指定列的非空行数，*包含null，DISTINCT字段统计不重复记录。 |

 count(expression or null) 可以统计匹配expression的行数，因为count只当参数为null时不计数，其他包括false都会计数，同理可以用count(if(expression,1,null))   

## 字符串函数

| lower(name)/lcase()       | 小写                                                         |
| ------------------------- | ------------------------------------------------------------ |
| upper(name)/ucase()       | 大写                                                         |
| substr(name,off,len)/mid  | 截取字符串,off是第几位,len为截几个                           |
| trim(name)/LTRIM/RTRIM    | 去除前后的空格,删除左/右的空格                               |
| trim(s1   from s)         | 删除s左右两边的s1                                            |
| str_to_date(str,pattern)  | 字符转日期pattern:%Y-%m-%d %H:%i:%s   默认日期格式为:%Y-%m-%d,会自动转型 |
| date_format(date,pattern) | 日期转字符                                                   |
| format(name,num)          | 转成千位表示,num为小数位,四舍五入                            |
| ifnull(name,value)        | 将null转换为指定值                                           |
| char_length(name)         | 获取字符个数                                                 |
| length(name)              | 获取占用字节数                                               |
| concat(s1,s2...)          | 连接多个字符串, mysql的concat函数拼接规则是   当多个拼接的字段的字段值中存在null时，返回的一定是 null。 |
| concat_ws(x,s1,s2)        | 连接多个字符串以x为分隔符                                    |
| insert(s1,x,len,s2)       | 在s1里从第x个字符开始,len个字符替换成s2                      |
| replace(s,s1,s2)          | 替换s中所有的s1为s2                                          |
| left(s,len)               | 从左开始截取len个字符                                        |
| right(s,len)              | 从右开始截取len个字符                                        |
| repeat(s,len)             | 返回s重复len遍                                               |
| LPAD(S,len,s2)            | 用s2填充s的左边知道总字符为len                               |
| RPAD(s,len,s2)            | 用s2填充s的右边知道总字符为len                               |
| elt(n,s1,s2...)           | 从后面的多个字段中返回第N个                                  |
| field(s,s1,s2..)          | 返回s在s1,s2...中处于第几个,无返回0                          |

 

## 日期与时间函数

| now()/current_timestamp()   localtime(),sysdate() | 获取当前日期和时间                      |
| ------------------------------------------------- | --------------------------------------- |
| curdate()/current_date()                          | 获取当前的日期,+0获得20170616类似的数值 |
| curtime()/current_time()                          | 获取当前的时间,+0获得时分秒拼接的数值   |
| month(date)/monthname(date)                       | 获取指定date的月份或月份名              |
| dayname(date)                                     | 获取指定date的星期名                    |
| dayofweek(date)                                   | 获取指定date处于一周的第几天(周日为1)   |
| weekday(date)                                     | 获取指定date对应的星期几(一~日对应0~6)  |
| week(date)                                        | 获取指定date处于一年的第几个星期(0~)    |
| weekofyear(date)                                  | 获取指定date处于一年的第几个星期(1~)    |
| dayofyear(date)                                   | 获取指定date是一年的第几天(1~)          |
| dayofmonth(date)                                  | 获取指定date是一月的第几天(1~)          |
| year(date)                                        | 获取指定date的年份                      |
| quarter(date)                                     | 获取指定date的季度                      |
| hour(date)                                        | 获取指定date的小时                      |
| minute(date)                                      | 获取指定date的分钟                      |
| second(date)                                      | 获取指定date的秒钟                      |

## 数学函数

| ABS(x)             | 绝对值                                     |
| ------------------ | ------------------------------------------ |
| SIGN(X)            | 符号函数,返回-1,0,1表示负数,零,正数        |
| rand()             | 生成0~1的随机数,开区间                     |
| cell(x)/celling(x) | 进一                                       |
| floor(x)           | 去尾                                       |
| round(name,num)    | 四舍五入,num为保留几位小数,可为负,不写取整 |
| truncate(name,num) | 保留num位小数,直接截取                     |
| MOD(x,y)           | 取模x%y                                    |
| POW(X,Y)           | X的Y次幂                                   |
| EXP(x)             | e的x次幂                                   |
| SQRT(X)            | 求x的平方根                                |
| Radians(x)         | x换成弧度                                  |
| degrees(x)         | x换成角度                                  |
| pi()               | 返回π的值                                  |
| 三角函数(x)        | 三角函数:sin,cos,tan,asin,acos,atan,cot    |

 

## 系统函数

| version()                                            | 系统版本             |
| ---------------------------------------------------- | -------------------- |
| connection_id()                                      | 当前用户连接数       |
| datebase()/schema()                                  | 返回当前使用的数据库 |
| user()/current_user()   system_user()/session_user() | 返回当前用户名       |

 

## 其他

### LAST_INSERT_ID()

自动返回最后一个INSERT或 UPDATE 操作为 AUTO_INCREMENT列设置的第一个发生的值。

即一次操作产生了多行数据时,只返回第一个值

### IFNULL()当为null时返回给定值

IFNULL(expression_1,expression_2);

如果expression_1不为NULL，则IFNULL函数返回expression_1; 否则返回expression_2的结果。