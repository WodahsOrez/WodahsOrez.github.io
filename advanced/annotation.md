# 注解(annotation)

## 内建注解

```java
// java.lang
@Override  在重写的子类方法前加上即可
@Deprecated  在已过时的程序元素前标识
@SuppressWarnings  关闭编译器对该程序元素和其所有子元素的警告
// 例:  
@SuppressWarnings(value ="unchecked");
@SuppressWarnings(value ={"unchecked","fallthrough"});其中value =可以省略
```

### value的常用参数有:

- deprecation:使用了过时程序元素
- unchecked:执行了未检查的转换
- unused:有程序元素未被使用
- fallthrough:switch程序块直接通往下一种情况而没有break
- path:在类路径,源文件路径等中有不存在的路径
- serial:在可序列化的类上缺少serialVersionUID定义
- finally:任何finally子句不能正常完成
- all:所有情况

## 自定义注解

使用@interface自定义注解时，自动继承了java.lang.annotation.Annotation接口，由编译程序自动完成其他细节。在定义注解时，不能继承其他的注解或接口。@interface用来声明一个注解，其中的每一个方法实际上是声明了一个配置参数。方法的名称就是参数的名称，返回值类型就是参数的类型。可以通过default来声明参数的默认值。

### 定义注解格式：

```java
public @interface 注解名 {
	成员类型 成员名() [default 默认值];
}
```

### 成员的可支持数据类型：

1. 所有基本数据类型（int,float,boolean,byte,double,char,long,short)及其封装类
2. String类型
3. Class类型
4. enum类型
5. Annotation类型
6. 以上所有类型的数组(赋值时用{})

### Annotation类型里面的参数该怎么设定: 

1. 只能用public或默认(default)这两个访问权修饰.例如,String value();这里把方法设为defaul默认类型；　 　
2. 如果只有一个参数成员,必须把参数名称设为"value",后加小括号.只对value成员赋值时可以省略成员名和赋值号=,同时对多个成员赋值时,必须使用赋值号.

### 注解成员的默认值：

总结：成员必须有值(默认指定，使用指定)，用负数或空字符串表示成员缺失。　　
注解成员必须有确定的值，要么在定义注解的默认值中指定，要么在使用注解时指定，非基本类型的注解成员的值不可为null。因此, 使用空字符串或0作为默认值是一种常用的做法。这个约束使得处理器很难表现一个成员的存在或缺失的状态，因为每个注解的声明中，所有成员都存在，并且都具有相应的值，为了绕开这个约束，我们只能定义一些特殊的值，例如空字符串或者负数，以此表示某个成员不存在，在定义注解时，这已经成为一个习惯用法。

## 元注解

java.lang.annotation
元注解,是给注解的注解.

### @Target　　

作用：用于描述注解的使用范围（即：被描述的注解可以用在什么地方）
取值(ElementType)有：
1. CONSTRUCTOR:用于描述构造器
2. FIELD:用于描述域,类成员变量或常量
3. LOCAL_VARIABLE:用于描述局部变量
4. METHOD:用于描述方法
5. PACKAGE:用于描述包
6. PARAMETER:用于描述参数
7. TYPE:用于描述类、接口(包括注解类型) 或enum声明

### @Retention

作用：表示需要在什么级别保存该注释信息，用于描述注解的生命周期（即：被描述的注解在什么范围内有效）
取值（RetentionPoicy）有：
1. SOURCE:在源文件中有效（即源文件保留）,字节码中不会有
2. CLASS:在class文件中有效（即class保留）,JVM中不会有
3. RUNTIME:在运行时有效（即运行时保留）,可以通过反射机制读取注解信息

### @Inherited 

作用：@Inherited 元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

注意：@Inherited annotation类型是被标注过的class的子类所继承。类并不从它所实现的接口继承annotation，方法并不从它所重载的方法继承annotation。

### @Documented：此为标识符，标识该类是否出现在Javadoc中

附录：[注解的自定义和使用](https://www.cnblogs.com/linjiqin/archive/2011/02/16/1956426.html)