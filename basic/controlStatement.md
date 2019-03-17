# 流程控制语句

## 选择判断语句

### if…else语句

```java
if (布尔表达式) {
    执行语句;
} else if (布尔表达式) {
    执行语句;
……
} else {
    执行语句;
}
```



### switch语句

```java
switch(expression){
    case value1 :
       //语句
       break; //可选
    case value2 :
       //语句
       break; //可选
    ……
    default : //可选
       //语句
       break; //可选
}
```

- expression支持类型为int，String和枚举.(byte,short,char是通过自动转成int实现的 )
- case 后的值必须与expression的数据类型相同，而且只能是常量或者字面常量。
- 当expression的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句。
- 如果没有 break 语句出现，程序会继续执行下一条 case 语句，直到出现 break 语句。
- default 在没有 case 语句的值和变量值相等的时候执行。推荐写在末尾



## 循环结构

### while语句

```java
while( 布尔表达式 ) {
  //循环内容
}
```

当满足布尔表达式时，进入循环，执行语句，再次判断，直到不满足布尔表达式，退出循环。



### do while语句

```java
do {
   //代码语句
}while(布尔表达式);
```

先执行循环语句，然后判断布尔表达式，满足后就再次执行，反之，退出循环。



### for循环语句

```java
for(初始化; 布尔表达式; 更新) {
    //代码语句
}
```

执行顺序：初始化→布尔表达式→代码语句→更新→布尔表达式……

初始化可以声明一个或多个循环控制变量，也可以为空



### for each语句

```java
for(声明语句 : 表达式){
   //代码句子
}
```

**声明语句：**声明新的局部变量，该变量的类型必须和数组元素的类型匹配。其作用域限定在循环语句块，其值与当前循环到的数组元素的值相等。

**表达式：**要访问的数组名，或者是返回值为数组的方法。



### continue 关键字

continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。

在 for 循环中，continue 语句使程序立即跳转到更新语句。

在 while 或者 do…while 循环中，程序立即跳转到布尔表达式的判断语句。



### break 关键字

break 主要用在循环语句或者 switch 语句中，用来跳出整个语句块。

嵌套循环结构中，break 跳出最里层的循环结构，并且继续执行外层循环接下来的语句。



### break和continue带label的用法

标签（label）：用标识符和`：`给代码块或是条件控制语句命名。

```java
a:{
    b: while(true) {
        c: for(;;) {
            d: if (true) {
                break c;
            }
        }
        continue b;
    }
}
```

break label：跳出label命名的代码块，继续执行外部的代码。

continue label：跳过label命名的**循环**代码块的本次循环，进入下一次循环。

注意：以上两种语句的label命名的代码块只能是包含该语句的代码块。

```java
a: do {
    b: switch(1){
        default:
            break c;  //不被c包含，无法跳出
    }
    continue c;  //不被c包含，无法跳过
}while(true);

c: for(;;){}
```

