# String类

## String类

字符串对象一旦被初始化就不会被改变。

```java
String s = "abc";//创建一个字符串对象在常量池中。
String s1 = new String("abc");//创建两个对象一个new一个字符串对象在堆内存中。
System.out.println(s.equals(s1));
//string类中的equals复写Object中的equals建立了string类自己的判断字符串对象是否相同的依据。
//其实就是比较字符串内容。
```



### 获取

```java
// 获取字符串中字符的个数(长度).
int length();
// 根据位置获取字符。
char charAt(int index);
// 根据字符获取在字符串中的第一次出现的位置
int indexOf(int ch)
int indexOf(int ch,int fromIndex) // 从指定位置进行ch的查找第一次出现位置
int indexOf(String str);
int indexOf(String str,int fromIndex);
// 根据字符串获取在字符串中的第一次出现的位置.
int lastIndexOf(int ch)
int lastIndexOf(int ch,int fromIndex) // 从指定位置进行ch的反向查找第一次出现位置
int lastIndexOf(String str);
int lastIndexOf(String str,int fromIndex);
// 获取字符串中一部分字符串。也叫子串.
String substring(int beginIndex, int endIndex) //包含begin 不包含end 。
String substring(int beginIndex);
```



### 转换

```java
// 将字符串变成字符串数组(字符串的切割)
String[]  split(String regex):涉及到正则表达式. "\\s+ "可以切割多个空格 
// 将字符串变成字符数组。
char[] toCharArray();
// 将字符串变成字节数组。
byte[] getBytes();
// 将字符串中的字母转成大小写。
String toUpperCase():大写
String toLowerCase():小写
// 将字符串中的内容进行替换
String replace(char oldch,char newch);
String replace(String s1,String s2);
// 将字符串两端的空格去除。
String trim();
// 将字符串进行连接 。
String concat(string);
```



### 判断

```java
// 两个字符串内容是否相同啊？
boolean equals(Object obj);
boolean equalsIgnoreCase(string str); // 忽略大写比较字符串内容。
// 字符串中是否包含指定字符串？
boolean contains(string str);
// 字符串是否以指定字符串开头。是否以指定字符串结尾。
boolean startsWith(string);
boolean endsWith(string);
```



### 比较

```java
int compareTo(string) // 逐个比较字符的Unicode值，短的比长的小。小返回负数，大返回正数，相同返回0
String s2 = s1.intern(); // 返回一个和s1相同的池中的字符串，如果没有则在池中创建并返回
```



### 连接

```java
String concat(String str) // 返回一个连接的字符串。
// String 虽然是引用类型，但在参数传递时，都是创建新的String对象，对原对象的引用不会造成改变。
public String replaceAll(String regex,String replacement) 
// $引用已捕获的序列例子引用已捕获的序列,例如:
str.replaceAll("(.)\\1+", "$1");  
// "\\1" 是对于(.)的引用
```



## StringBuffer

就是字符串缓冲区。用于存储数据的容器。

### 特点

1. 长度的可变的。
2. 可以存储不同类型数据。
3. 最终要转成字符串进行使用。
4. 可以对字符串进行修改。

### 方法

```java
// 添加：
StringBuffer append(data);
StringBuffer insert(index,data); //（插入，不会覆盖该位置的原有数据）
// 删除：
StringBuffer delete(start,end) // 包含头，不包含尾。
StringBuffer deleteCharAt(int index) // 删除指定位置的元素
// 查找：
char charAt(index);
int indexOf(string);
int lastIndexOf(string);
// 修改：
StringBuffer replace(start,end,string);
void setCharAt(index,char);
```

jdk1.5以后出现了功能和StringBuffer一模一样的对象。就是**StringBuilder**

**不同的是**：
StringBuffer是线程同步的。通常用于多线程。
StringBuilder是线程不同步的。通常用于单线程。 它的出现是为了提高效率。

**unicode编码范围**：
汉字：[0x4e00,0x9fa5]（或十进制[19968,40869]）
数字：[0x30,0x39]（或十进制[48, 57]）
小写字母：[0x61,0x7a]（或十进制[97, 122]）
大写字母：[0x41,0x5a]（或十进制[65, 90]）