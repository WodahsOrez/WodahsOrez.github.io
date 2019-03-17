# 输入输出流

## 字节流

特点：最小操作单位为字节(byte)

### InputStream抽象类

方法：     

```java
int read()从输入流读取下一个字节，返回0~255的值
int read(byte[] b)从输入流读取一堆字节，存储在b中,返回值为数组的长度。
int read(byte[] b, int off, int len)off为开始存储字节的数组下标，len为读到的字节个数。
void close()关闭流
int available() 可以从输入流中读取的剩余字节长度。while(dis.available()>0)
```



#### BufferedInputStream类

构造方法：     `BufferedInputStream(InputStream in)`

#### FileInputStream类

构造方法：`FileInputStream(File file)     FileInputStream(String pathname)`

#### SequenceInputStream类

特点：按顺序合并若干个流。
构造函数：

```java
SequenceInputStream( Enumeration<? extends InputStream> e)
SequenceInputStream(InputStream s1,InputSteam s2)
```

#### ObjectInputStream类

构造方法：`ObjectInputStream(InputStream in)`

#### PipedInputStream类

特点：配合PipedOutputStream使用，避免两者在同一线程，会死锁。
构造方法：`PipedInputStream(PipedOutputStream src)`
方法：     `void connect(PipedOutputStream src)`  联系两者

#### DataInputStream类

特点：写入基本数据类型
构造方法： `DataInputStream(InputStream in)`
方法：`String readUTF()`     修改版UTF-8格式读取

### OutputStream抽象类

方法：     

```java
void write(int c)
void write(byte[] buf)
void write(byte[] b, int of, int len)
void close()
void flush()强制把缓冲区的数据写到输出流中,写入得时候只是把数据写入到当前流类的缓冲区内,而没有写入到流所指向的文件或者其他流中,所以需要flush以写入到目标源
```



#### FileOutputStream类

构造方法：     

```java
FileOutputStream(File file)
FileOutputStream(String name)
FileOutputStream(String name, boolean append)
```

前两种构造方法在向文件写数据的时将覆盖文件中原有的内容，FileOutputStream实例时，
如果文件不存在，则自动生成一个空的文件。如果路径不存在则报异常

#### PrintStream类

特点：永远不会抛出IOException
构造函数：

```java
PrintStream(File file)
PrintStream(OutputStream out)
PrintStream(String fileName)
```

方法：

```java
void print(基础数据类型)     
// 包括：boolean,char,char[],double,float,int,long,String,Object,保持数据的表现形式。
void write(int b)     //写入字节码为b的值，只保留int的后8位。
```



#### BufferedOutputStream类

构造方法：    `BufferedOutputStream(OutputStream out) `
方法：    ` void flush()`

#### ObjectOutputStream类

构造：`ObjectOutputStream(OutputStream)`
方法：`void writeObject(Object obj) `    对象必须实现Serializable接口

#### PipedOutputStraem类

构造方法：`PipedOutputStream(PipedInputStream snk)`
方法：     `void connect(PipedInputStream snk)`

#### DataOutputStream类

构造方法：`DataOutputStream(OutputStream out)`
方法：`void writeUTF(String str) `    修改版UTF-8格式写入

## 字符流

特点：最小处理单元为2个字节的Unicode字符。有缓冲区。处理文字意外的数据会出错。

### Writer抽象类

方法：     

```java
abstract void flush()     刷新流，把缓冲区的内容写入到目标文件
abstract void close()     关闭流，会先执行flush后关闭流。
void writer(char[] cbuf)
void write(char[] cbuf, int off, int len)      off为起始位置，len为长度。
void write(int c)     写入编号为c的字符。
void write(String str)
void write(String str, int off, int len)     写入字符串，off为起始下标，len为长度。
```



#### BufferedWriter类

构造函数：`BufferedWriter(Write out)  `   创建一个缓冲输入流，效率高
方法：     

```java
void newLine()     新建一个依据系统的换行
void close()     Writer也会被关闭
```

#### OutputStreamWriter类

构造方法：     

```java
OutputStreamWriter(OutputStream out)
OutputStreamReader(OutputStream in, Charset cs)     Charset为编码表,如"GBK","gbk","utf-8"
```

#### FileWriter类

构造方法：     

```java
FileWriter(File file)
FileWriter(String fileName)     如果不存在则自动创建，如果存在则会被覆盖。
FileWriter(String fileName, boolean append)     append为true时，可以续写。
```

#### PrintWriter类

构造方法：     

```java
PrintWriter(File file)
PrintWriter(OutputStream out)     可选项boolean autoFlush.printf,println,format方法将自动刷新
PrintWriter(String fileName)
PrintWriter(Writer out)     可选项boolean autoFlush.printf,println,format方法将自动刷新
```



### reader抽象类

方法：     

```java
int read() 返回一个字符的号码0~65535，当到达末尾时返回-1
int read(char[] cbuf)     返回读取的个数，没有读取到或末尾则返回-1
// 例：条件:文件为"abcde",buf为char[3].
int num = fr.read(buf);
System.out.println(num + ":"+new String(buf)):      //输出3:abc   读取了三个字符，分别写入buf数组
int num1 = fr.read(buf);
System.out.println(num1 + ":"+new String(buf)):  //输出2:dec 读取了两个字符，覆盖了buf[0]和buf[1]
int num2 = fr.read(buf);
System.out.println(num2 + ":"+new String(buf)):     //输出-1:dec  没读取到字符，返回-1
```



#### BufferedReader类

构造函数：     `BufferedReader(Reader in)`
方法：     `String readLine() `读取一行文本，不包括换行符。没有下一行返回null,若没有换行符则会阻塞

#### InputStreamReader类

构造函数：     

```java
InputStreamReader(InputStream in)
InputStreamReader(InputStream in, Charset cs)     Charset为编码表,如"GBK","gbk","utf-8"
```

#### FileReader类

构造方法：     

```java
FileReader(File file)     如果不存在，抛出异常
FileReader(String fileName) 
```

#### LineNumberReader类

构造方法： `LineNumberReader(Reader in)`
方法：     

```java
int getLineNumber()     获取行号++
void setLineNumber(int lineNumber)     设置行号
String readLine()     读一行
```



## Serializable接口

默认会计算出一个序号用以判断序列化反序列化时对象是否有改变。
建议显式声明一个Static final long的序列号，以避免默认生成高敏感性带来的问题。

### 序列化和反序列化

把对象转换为字节序列的过程称为对象的序列化。
把字节序列恢复为对象的过程称为对象的反序列化。

## RandomAccessFile类

特点：只能对文件进行操作。综合了输入和输出流。如果文件不存在，则创建，如果存在，不创建，使用原文件。用于多线程输出。
构造方法：

```java
RandomAccessFile(File file,String mode) mode为r只读，rw读写
RandomAccessFile(String name ,String mode) 以name为路径的文件名
```

方法：     

```java
long getFilePointer()     返回当前指针的位置
void seek(long pos)     设置指针的位置
```



## 装饰设计思想

BufferedWriter就是一种装饰类，运用的是装饰设计，与继承的差别在于，对于同一个功能的拓展，继承需要对每个子类都进行继承，会使体系越来越臃肿。而装饰只需要对父类或接口进行关联，就可以应用于所有子类，更灵活。
总结：装饰类比继承灵活。装饰类和被装饰类都必须所属同一个接口或父类。
阻塞式方法：必须等待输入数据可用或者检测到输入结束或者抛出异常，否则程序会一直停留在该语句上，不会执行下面的语句。

### 将Iterator和Enumeration转换

```java
ArrayList<FileInputStream> a1 = new ArrayList<FileInputStream>();
Iterator it<FileInputStream> = a1.iterator();
//匿名类重写方法。用Iterator的方法来实现。
Enumeration<FileInputStream> = new Enumeration<FileInputStream>(){
    public boolean hasMoreElements(){
    	return it.hasNext();
    }
    public FileInputStream nextElement(){
        return it.next(); 
    }
} ;
```

Java 8更加智能：如果局部变量被匿名内部类访问，那么该局部变量相当于自动使用了final修饰。
流的read方法都是阻塞的:即流内没有内容时会一直等待.