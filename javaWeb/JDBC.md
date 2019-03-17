# JDBC(Java数据库连接）

## JDBC 编程步骤

### 加载驱动程序：

```java
Class.forName(driverClass)
//加载MySql驱动
Class.forName("com.mysql.jdbc.Driver")
//加载Oracle驱动
Class.forName("oracle.jdbc.driver.OracleDriver")
```

**原理**：

- 利用加载class文件执行static代码
- 所有的java.sql.Driver实现类，都提供了static块，块内的代码就是把自己的对象传给DriverManager.registerDriver(对象)方法来注册.
- jdbc4.0后每个驱动jar包中，在META-INF/services目录下提供了一个名为java.sql.Driver的文件,内容就是该接口的实现类名称.会自动获取该实现类并加载,所以可以省略第一步.

### 获得数据库连接：

```java
//方法Connection   getConnection(URL,username,password)
Connection con=DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb3", "root", "root");
//MySQL的URL格式
jdbc:mysql://主机名或IP地址：端口号/数据库名?useUnicode=true&characterEncoding =UTF-8
```

### 创建Statement/PreparedStatement对象：

```java
//创建Statement
Statement stat = con.createStatement();
//发送DML或者DDL的sql语句
String sql ="INSERT INTO stu VALUES('ITCAST_003','zhangsan',88,'male')"  //不加;
int r = stat.executeUpdate(sql)  //r为影响的行数
//发送DQL语句
ResultSet rs = stmt.executeQuery("SELECT * FROM stu");  
//解析ResultSet
while(rs.next()) {//把光标向下移动一行，并判断下一行是否存在(初始在beforeFirst)
	int empno = rs.getInt(1);//通过列编号来获取该列的值！
	String ename = rs.getString("ename");//通过列名称来获取该列的值
	double sal = rs.getDouble("sal");
	System.out.println(empno +  ", " + ename + ", " + sal);
}
```

### 关闭资源：

```java
rs.close();
stmt.close();
con.close();
```

### 例子

```java
public void fun3() throws Exception {
	Connection con = null;// 定义引用
	Statement stmt = null;
	ResultSet rs = null;
	try {
		/*
		 * 一、得到连接
		 */
		String driverClassName = "com.mysql.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/exam";
		String username = "root";
		String password = "123";

		Class.forName(driverClassName);
		con = DriverManager.getConnection(url, username, password);// 实例化

		/*
		 * 二、创建Statement
		 */
		stmt = con.createStatement();
		String sql = "select * from emp";
		rs = stmt.executeQuery(sql);

		rs.last();// 把光标移动到最后一行
		System.out.println(rs.getRow());
		rs.beforeFirst();

		/*
		 * 三、循环遍历rs，打印其中数据
		 * 
		 * getString()和getObject()是通用的！
		 */
		// while(rs.next()) {
		// System.out.println(rs.getObject(1) + ", "
		// + rs.getString("ename") + ", " + rs.getDouble("sal"));
		// }

		int count = rs.getMetaData().getColumnCount();
		while (rs.next()) {// 遍历行
			for (int i = 1; i <= count; i++) {// 遍历列
				System.out.print(rs.getString(i));
				if (i < count) {
					System.out.print(", ");
				}
			}
			System.out.println();
		}
	} catch (Exception e) {
		throw new RuntimeException(e);
	} finally {
		// 关闭
		if (rs != null)
			rs.close();
		if (stmt != null)
			stmt.close();
		if (con != null)
			con.close();
	}
}
```

## JDBC对象

### DriverManager类

#### 方法：

```java
Class.forName(“com.mysql.jdbc.Driver”);//注册驱动
String url = “jdbc:mysql://localhost:3306/mydb1”;
String username = “root”;
String password = “123”;
Connection con = DriverManager.getConnection(url, username, password);
```

#### 注意，上面代码可能出现的两种异常：

- ClassNotFoundException：在第1句上出现，原因可能有两个：
  - 你没有给出mysql的jar包；
  - 你把类名称打错了，查看类名是不是com.mysql.jdbc.Driver。
- SQLException：出现在第5句，出现这个异常就是三个参数的问题，往往username和password一般不是出错，所以需要认真查看url是否打错。

对于DriverManager.registerDriver()方法了解即可，因为我们今后注册驱动只会Class.forName()，而不会使用这个方法。

### Connection接口

#### 方法:

```java
Statement stmt = con.createStatement();  获取Statement
Statement stmt = con.createStatement(int,int);
```

### Statement接口

#### 方法：

- int executeUpdate(String sql)：执行更新操作，即执行insert、update、delete语句，其实这个方法也可以执行create table、alter table，以及drop table等语句，但我们很少会使用JDBC来执行这些语句；
- ResultSet executeQuery(String sql)：执行查询操作，执行查询操作会返回ResultSet，即结果集。
- boolean execute()  这个方法可以用来执行增、删、改、查所有SQL语句。该方法返回的是boolean类型，表示SQL语句是否有结果集。
  - 如果使用execute()方法执行的是更新语句，那么还要调用int getUpdateCount()来获取insert、update、delete语句所影响的行数。
  - 如果使用execute()方法执行的是查询语句，那么还要调用ResultSet getResultSet()来获取select语句的查询结果。

### ResultSet接口

ResultSet表示结果集，它是一个二维的表格。ResultSet内部维护一个行光标（游标）

#### 游标控制判断方法

```java
void beforeFirst()：把光标放到第一行的前面，这也是光标默认的位置；
void afterLast()：把光标放到最后一行的后面；
boolean first()：把光标放到第一行的位置上，返回值表示调控光标是否成功；
boolean last()：把光标放到最后一行的位置上；
	
boolean isBeforeFirst()：当前光标位置是否在第一行前面；
boolean isAfterLast()：当前光标位置是否在最后一行的后面；
boolean isFirst()：当前光标位置是否在第一行上；
boolean isLast()：当前光标位置是否在最后一行上；
	
boolean previous()：把光标向上挪一行；
boolean next()：把光标向下挪一行；
boolean relative(int row)：相对位移，当row为正数时，表示向下移动row行，为负数时表示向上移动row行；
boolean absolute(int row)：绝对位移，把光标移动到指定的行上,正数第一行(1)开始,负数最后一行(-1)开始,超出范围分别为afterLast和beforeFirst,这两者返回false
int getRow()：返回当前光标所在行。beforeFirst为0
```

#### 获取结果集元数据

- 得到元数据：rs.getMetaData()，返回值为ResultSetMetaData,其方法有:
  - 获取结果集列数：int getColumnCount()
  - 获取指定列的列名：String getColumnName(int colIndex) 第一列为1

#### 结果集特性

con.createStatement()：生成的结果集：不滚动、不敏感、不可更新！(**mysql默认可滚动**)
`Statement createStatement(int resultSetType, int resultSetConcurrency)`
第一个参数：

- ResultSet.TYPE_FORWARD_ONLY：不滚动结果集；
- ResultSet.TYPE_SCROLL_INSENSITIVE：滚动结果集，但结果集数据不会再跟随数据库而变化；
- ResultSet.TYPE_SCROLL_SENSITIVE：滚动结果集，但结果集数据不会再跟随数据库而变化；

第二个参数：

- CONCUR_READ_ONLY：结果集是只读的，不能通过修改结果集而反向影响数据库；
- CONCUR_UPDATABLE：结果集是可更新的，对结果集的更新可以反向影响数据库。

上面方法分为两类，一类用来判断游标位置的，另一类是用来移动游标的。如果结果集是不可滚动的，那么只能使用next()方法来移动游标，而beforeFirst()、afterLast()、first()、last()、previous()、relative()方法都不能使用。

#### 获取列数据方法

可以通过next()方法使ResultSet的游标向下移动，当游标移动到你需要的行时，就需要来获取该行的数据了，ResultSet提供了一系列的获取列数据的方法：

```java
String getString(int columnIndex)：获取指定列的String类型数据；
int getInt(int columnIndex)：获取指定列的int类型数据；
double getDouble(int columnIndex)：获取指定列的double类型数据；
boolean getBoolean(int columnIndex)：获取指定列的boolean类型数据；
Object getObject(int columnIndex)：获取指定列的Object类型的数据,不确定列类型时
```

上面方法中，参数columnIndex表示列的索引，列索引从1开始，而不是0
ResultSet还提供了一套通过列名称来获取列数据的方法：

```java
String getString(String columnName)：获取指定名称的列的String数据；
int getInt(String columnName)：获取指定名称的列的int数据；
double getDouble(String columnName)：获取指定名称的列的double数据；
boolean getBoolean(String columnName)：获取指定名称的列的boolean数据；
Object getObject(String columnName)：获取指定名称的列的Object数据；
```

### PreparedStatement预编译声明

它是Statement接口的子接口。

**优点**：1,防SQL攻击  2,提高代码的可读性、可维护性  3,提高效率

#### 获得PreparedStatement对象：

- 给出SQL模板,即有“?”的SQL语句

- 调用Connection的PreparedStatement prepareStatement(String sql模板)；

- 调用pstmt的setXxx(int index,值)给sql模板中的第index个?赋值

- 调用pstmt的executeUpdate()或executeQuery()，但它们的方法都没有参数。

#### 预处理的原理

##### 服务器的工作：

- 校验sql语句的语法
- 编译：一个与函数相似的东西
- 执行：调用函数

##### PreparedStatement：

- 前提：连接的数据库必须支持预处理，几乎没有不支持的。
- 每个pstmt都与一个sql模板绑定在一起，先把sql模板给数据库，数据库先进行校验，再进行编译。执行时只是把参数传递过去而已。
- 若二次执行时，就不用再次校验语法，也不用再次编译，直接执行。

#### 防止SQL攻击

- 过滤用户输入的数据中是否包含非法字符。
- 分步交验！先使用用户名来查询用户，如果查找到了，再比较密码。
- 使用PreparedStatement。

#### MySQL默认预处理关闭,需在URL里添加下列参数?属性1&属性2

- useServerPrepStmts=true 开启预编译功能
- cachePrepStmts=true 缓存编译后函数的key,避免不同pstmt执行相同sql语句产生的二次编译
- rewriteBatchedStatements=true  MySQL的批处理也需要通过参数来打开

## 批处理

### Statement批处理

批处理就是把几条sql存在一批里,一次性执行,批处理只针对更新（增、删、改）语句，批处理没有查询

#### 方法:

```java
void addBatch(String sql)：添加一条语句到“批”中；
int[] executeBatch()：执行“批”中所有语句。返回值表示每条语句所影响的行数据；
void clearBatch()：清空“批”中的所有语句。
```

**注意**：当执行了executeBatch()之后，“批”中的SQL语句就会被清空。还可以在执行“批”之前，调用Statement的clearBatch()方法来清空“批”。

 

### PreparedStatement批处理

PreparedStatement的批处理有所不同，因为每个PreparedStatement对象都绑定一条SQL模板。所以向PreparedStatement中添加的不是SQL语句，而是给“?”赋值。

#### 方法:

```java
void addBatch()：添加一组?值到“批”中；
int[] executeBatch()：执行“批”中所有语句。返回值表示每条语句所影响的行数据；
```

## JdbcUtils工具类

### JdbcUtils的作用

连接数据库的四大参数是：驱动类、url、用户名，以及密码。这些参数都与特定数据库关联，如果将来想更改数据库，那么就要去修改这四大参数，那么为了不去修改代码，写一个JdbcUtils类，让它从配置文件中读取配置参数，然后创建连接对象。

#### JdbcUtils.java

```java
public class JdbcUtils {
	private static final String dbconfig = "dbconfig.properties";
	private static Properties prop = new Properties();
	static {
		try {
			InputStream in = Thread.currentThread().getContextClassLoader().getResourceAsStream(dbconfig);
			prop.load(in);
			Class.forName(prop.getProperty("driverClassName"));
		} catch (IOException e) {
			throw new RuntimeException(e);
		}
	}

	public static Connection getConnection() {
		try {
			return DriverManager.getConnection(prop.getProperty("url"), prop.getProperty("username"),
					prop.getProperty("password"));
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```

#### dbconfig.properties文件

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mydb1?useUnicode=true&characterEncoding=UTF8
username=root
password=123
```

## 连接池

### tomcat的conf文件夹下context.xml文件`<context>`里

```xml
<Resource name="jdbc/suo"   //数据源的JNDI名称
          auth="Container"     //连接池管理权属性,容器管理:tomcat
          type="javax.sql.DataSource"  
          driverClassName="com.mysql.jdbc.Driver"  
          url="jdbc:mysql://localhost:3306/suo"  
          username="root"  
          password="123456"  
          maxActive="50"  //最大活动连接数
          maxIdle="30"      //即使没有数据库连接时依然可以保持30空闲的连接
          maxWait="10000" />   //最大等待时间(毫秒)

```

### 数据源(DataSource)

#### javax.sql.DataSource接口

```java
Context ct = new InitialContext();//构造一个初始上下文。 得到一个上下文对象
DataSource d = (DataSource)ct.lookup("java:comp/env/jdbc/suo");
//检索指定的对象。 得到一个数据源对象,参数为JNDI地址:java:comp/env/JNDI名称
conn=d.getConnection();
```

