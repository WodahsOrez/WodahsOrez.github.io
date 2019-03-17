# 实用类

## 枚举类

方法：

```java
boolean hasMoreElements()
E nextElement()
```



### 枚举

枚举是定义的类，{}里的是它的实例，所以比较值时应该用==，不要用equals 

```java
public enum Size {SMALL,MEDIUM,LARGE,EXTRA_LARGE};
```

枚举里也可以有构造函数和方法和域。当有这些时，枚举常量列表用豆号隔开，最后一定要有分号。  

```java
public enum Size{
     SMALL("S"),MEDIUM("M"),LARGE("L"),EXTRA_LARGE("XL");
     private String abbreviation;
     private Size(String abbrevation) {this.abbrevation = abbrevation;} 构造必为私有
     publc String getAbbrecvation() {return abbrevation; }
}
```

枚举值可以看成是 `public static final Size SMALL = new Size("S");`
	

## System类

### 成员变量：

```java
static InputStream in
static PrintStream out
```

### 方法：     

```java
static long currentTimeMillis()     获取当前时间的毫秒值
static Properties getProperties()     获取当前系统的属性信息
static String getProperty(String key)  返回键值对应的属性信息。
```

## Runtime类

特点：没有构造方法，只能通过getRuntime()获得实例，且只有一个，是一个单例设计。

### 方法：          

```java
static Runtime getRuntime()     返回与当前运行java程序相关的Runtime对象。
Process exec(String command) 执行指定的字符串命令，并返回进程。
```

###  举例：

```java
Runtime r = Runtime.getRuntime();
Process p = r.exec("notepad.exe c:\\RuntimeDemo.java");
Thread.sleep(5000);
p.destory();     关闭进程
```

## Math类

### 方法：

```java
static int abs(int a) 绝对值，输入什么类型返回什么类型，同样的还有double,float,long
static double ceil(double a) 返回大于或等于参数的最小整数(进1)
static double floor(double a) 返回小于或等于参数的最小整数(去尾)
static long round(double a)     static int round(float a) 四舍五入
max和min为比较两个数的大小，支持double,float,in,long
static double pow(doube a, double b)a的b次幂
static double random()返回一个0.0~1.0的数
```

## Random类

特点：用同一个种子值来初始化两个Random 对象，然后用每个对象调用相同的方法，得到的随机数也是相同的。
构造方法：`Random()`   或  `Random(long seed)`      用种子生成随机对象。

### 方法：

```java
int nextInt(int bound)     返回一个伪随机数，它是取自此随机数生成器序列的、在 0（包括）和指定值bound（不包括）之间均匀分布的 int 值。
int nextInt()     返回一个伪随机数，-2^31~2^31
Math.abs(new Random().nextInt()) % 52 表示取0~51的随机数。
```

## date类

### 构造方法：

```java
Date date = new Date();//将当前日期和时间封装成Date对象。空参构造
Date date2 = new Date(1335664696656l);//将指定毫秒值封装成Date对象 long型带参构造
```

### 方法:

```java
long    getTime() 返回毫秒值
void    setTime(long time) 设置Date对象的时间
String toString() 格式：dow mon dd hh:mm:ss zzz yyyy     dow星期 zzz时区
```

## DateFormat类

### 方法： 

```java
static DateFormat getDateInstance(int style)返回日期格式器
static DateFormat getDateTimmeInstance(int style)返回日期时间格式器，风格FULL,LONG,MEDIUM,SHORT.
String format(Date date)格式化并返回字符串
Date parse(String source)按DateFormat对象格式，解析成date对象。
```

## SimpleDateFormat类

特点：可以自定义风格
构造方法：`SimpleDateFormat(String pattern)`

### 自定义格式规则 

年y月M日d 时H分m秒s

| Letter | Date or Time Component                             | Presentation             | Examples                               |
| ------ | -------------------------------------------------- | ------------------------ | -------------------------------------- |
| G      | Era designator                                     | [Text]()                 | AD                                     |
| y      | Year                                               | [Year]()                 | 1996; 96                               |
| Y      | Week year                                          | [Year]()                 | 2009; 09                               |
| M      | Month in year (context   sensitive)                | [Month]()                | July; Jul; 07                          |
| L      | Month in year (standalone form)                    | [Month]()                | July; Jul; 07                          |
| w      | Week in year                                       | [Number]()               | 27                                     |
| W      | Week in month                                      | [Number]()               | 2                                      |
| D      | Day in year                                        | [Number]()               | 189                                    |
| d      | Day in month                                       | [Number]()               | 10                                     |
| F      | Day of week in month                               | [Number]()               | 2                                      |
| E      | Day name in week                                   | [Text]()                 | Tuesday; Tue                           |
| u      | Day number of week (1 = Monday,   ..., 7 = Sunday) | [Number]()               | 1                                      |
| a      | Am/pm marker                                       | [Text]()                 | PM                                     |
| H      | Hour in day (0-23)                                 | [Number]()               | 0                                      |
| k      | Hour in day (1-24)                                 | [Number]()               | 24                                     |
| K      | Hour in am/pm (0-11)                               | [Number]()               | 0                                      |
| h      | Hour in am/pm (1-12)                               | [Number]()               | 12                                     |
| m      | Minute in hour                                     | [Number]()               | 30                                     |
| s      | Second in minute                                   | [Number]()               | 55                                     |
| S      | Millisecond                                        | [Number]()               | 978                                    |
| z      | Time zone                                          | [General   time zone]()  | Pacific Standard   Time; PST;GMT-08:00 |
| Z      | Time zone                                          | [RFC   822 time zone]()  | -800                                   |
| X      | Time zone                                          | [ISO   8601 time zone]() | -08; -0800; -08:00                     |

## Calendar类

### 方法：

```java
int get(int field)返回对应成员变量的值。
static Calendar getInstance()无法通过构造函数创建对象，只能用此方法。
void set(int field, int value)
void set(int year, int month, int date)
void set(int year, int month, int date, int hourOfDay, int minute)
void set(int year, int month, int date, int hourOfDay, int minute, int second)
abstract void	add(int field, int amount) 偏移相应field的时间,用+-表示前后
```

Calendar 的 month 从 0 开始，也就是全年 12 个月由 0 ~ 11 进行表示。
而 Calendar.DAY_OF_WEEK 定义和值如下：

```java
Calendar.SUNDAY = 1
Calendar.MONDAY = 2
Calendar.TUESDAY = 3
Calendar.WEDNESDAY = 4
Calendar.THURSDAY = 5
Calendar.FRIDAY = 6
Calendar.SATURDAY = 7
```

## 包装类

装箱：基本类型转换为包装类的对象
拆箱：包装类对象转换为基本类型的值
Type：Byte，Boolean，Short，Character，Integer，Long，Float，Double

### 构造方法：

```java
Type(type value)
Type(String value)
```

1. Boolean类时，若该字符串内容为true(不考虑每个字母的大小写)，则该Boolean对象表示true。否则都表示false.
2. 当Number包装类构造方法参数为String 类型时，字符串不能为null，且该字符串必须可解析为相应的基本数据类型的数据，否则编译通过，运行时NumberFormatException异常。

### 方法：

```java
type typeValue()     type换成对应的基本数据类型
String toString()     返回对象的字符串形式
public static String toString(type i) 返回i的字符串形式
public static type parseType(String type) 
把字符串转换成对应的基本数据类型，(Character除外)不匹配会抛NumberFormatException异常
public static Type valueOf(type value)     所有类都有，基本类型转换为包装类
public static Type valueOf(String s)     除Character外都有，匹配的字符串转换为包装类
```

基本类型和包装类的自动转换

```java
//装箱
Integer intObject = 5;
//拆箱
int intValue3 = intObject;
```

## File类
### 成员变量：

```java
static String pathSeparator     多个路径之间分隔符，UNIX为：，windows为；
static char pathSeparatorChar
static String separator     路径分隔符,UNIX为'/', windows为'\\'
static char separatorChar
```

### 构造方法： 

```java
// File(File parent, String child)     
File f = new File("c:\\");    
File f1 = new File(f,"a.txt");
// File(String pathname)               
File f2 = new File("c:\\a.txt"); // 相对路径也可以,相对于class所在的文件夹(jvm产生的路径)
File (String parent, String child)     
File f3 = new File("c:\\", "a.txt");
// File(URI uri)
```

### 方法：

#### 获取

```java
String getPath()     相对路径,相对于当前路径 
String getAbsolutePath()     绝对路径
String getName()
long lastModified()     返回最后一次修改时间的毫秒数
long length()      返回文件长度，单位为字节，不存在则返回0L
```

#### 创建与删除

```java
boolean createNewFile()     创建名称得的空文件，不能创建文件夹，即无目录时报异常。文件存在时返回false
boolean mkdir()     一级目录
boolean mkdirs()     多级目录
boolean delete()     删除文件或目录，目录内有文件时无法删除。
```

#### 判断

```java
boolean exists()    文件或目录是否存在
boolean isFile()    是否是文件，存在且是文件返回true
boolean isDirectory()     是否是目录，存在且是目录返回true
```

#### 重命名

```java
boolean renameTo(File dest)     
// 重命名此抽象路径名表示的文件。可以重命名路径实现剪切。dest为指定文件的新抽象路径名
```

#### 其他

```java
static File[] listRoots()     获取系统根目录的File数组
long getFreeSpace()     获取此抽象路径指定的分区的剩余可用空间
long getTotalSpace()     获取此抽象路径指定的分区的总容量
long getUsableSpace()     获取此抽象路径指定的分区的可用于虚拟机的字节数
```

#### 获取文件和目录

```java
String[] list()     
// 获取当前目录下的文件以及文件夹名称的数组，包含隐藏文件。
// 如果抽象路径为文件或者为系统隐藏目录时，返回null。
// 如果目录内没内容，则会返回一个长度为0的数组。
String[] list(FilenameFilter filter)     
// 返回符合过滤器的boolean accept(File dir，String name)的文件名或目录名。
File[] listFiles()
File[] listFiles(FileFilter filter)
File[] listFiles(FilenameFilter filter)
```

