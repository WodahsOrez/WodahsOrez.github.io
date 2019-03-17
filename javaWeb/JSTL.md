# JSTL(JSP标准标签库)

导包：JSTL.jar，standard.jar(1.2后不需要这个包了)

## core标签库(c标签)

在需要使用的JSP页面引用标签库

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

### out和set标签

`<c:out>`：输出

- value：可以是字符串常量，也可以是EL表达式

- default：当要输出的内容为null时，会输出default指定的值

- escapeXml：默认值为true，当为false时解析XML

`<c:set>`：设置(创建域的属性)

- var：变量名

- value：变量值，可以是EL表达式

- scope：域，默认为page，可选值：page、request、session、application

### remove标签

`<c:remove>`：删除域变量

- var：变量名

- scope：如果不给出scope，表示删除所有域中的该名称的变量；如果指定了域，那么只删除该域的变量。

### url标签

`<c:url>`：输出url

- value：指定一个路径！它会在路径前面自动添加项目名。

  - <c:url value="/index.jsp"/>，它会输出/day13_1/index.jsp

- var：指定变量名，一旦添加了这个属性，那么url标签就不会再输出到页面，而是把生成url保存到域中。

- scope：它与var一起使用，用来保存url。

子标签：`<c:param>`，用来给url后面添加参数，例如：
```jsp
<c:url value="/index.jsp">
	<c:param name="username" value="张三"/>  <!--可以对参数进行url编码！！-->
</c:url>
```
结果为：/day13_1/index.jsp?username=%ED%2C%3F%ED%2C%3F

### if标签

对应java中的if语句

`<c:if test="布尔类型">...</c:if>`，当test为true时，执行标签体内容

var用于存储条件结果的变量，scope为var所在域

### choose标签
它对应java中的if/else if/ ... /else,更接近switch

例如：

```jsp
<c:choose>
    <c:when test="">...</c:when>
    <c:when test="">...</c:when>
    <c:when test="">...</c:when>
    ... 
    <c:otherwise> ...</c:otherwise>
</c:choose>
```

### forEach标签

#### 属性：

- var：循环变量

- begin：设置循环变量从几开始。

- end：设置循环变量到几结束。

- step：设置步长！等同与java中的i++，或i+=2。step默认为1

- status：代表循环状态的变量名称,有属性

  - `${status.current}`当前这次迭代的（集合中的）项  
  - `${status.index}`当前这次迭代从 0 开始的迭代索引
  - `${status.count}` 当前这次迭代从 1 开始的迭代计数
  - `${status.first}` 用来表明当前这轮迭代是否为第一次迭代的标志
  - `${status.last}` 用来表明当前这轮迭代是否为最后一次迭代的标志

- items：指定要循环谁，它可以是一个数组或一个集合

#### a)循环变量方式：

```jsp
<c:forEach var="i" begin="1" end="10">
	${i}
</c:forEach>
```


#### b)遍历数组、集合

```jsp
<c:forEach var="str"  items="${strs}">
 ${str }<br/>
</c:forEach>
```

- 
  items：指定要循环谁，它可以是一个数组或一个集合

- var：把数组或集合中的每个元素赋值给var指定的变量。

- 当遍历map时,用`${str.key}`和`${str.value}`输出

### forTokens标签

和foreach相似，指定分隔符，分隔字符串为数组，然后循环

独有属性：delims分隔符

```jsp
<c:forTokens items="google,runoob,taobao" delims="," var="name">
```

## fmt库,格式化库

### 格式化时间

`<fmt:formatDate>`

- value：指定一个Date类型的变量
- pattern：用来指定输出的模板！例如：yyyy-MM-dd HH:mm:ss
- type：time，date，both
- dateStyle：short，medium，long，full，default
- timeStyle：short，medium，long，full，default

### 格式化数字

```jsp
<fmt:formatNumber value="${num1}" pattern="0.00">
```

保留小数点后2位，它会四舍五入！如果不足2位，以0补位！

```jsp
<fmt:formatNumber value="${num1}" pattern="#.##">
```

保留小数点后2位，它会四舍五入！如果不足2位，不补位！

| value             | 要显示的数字                       | 是   | 无              |
| ----------------- | ---------------------------------- | ---- | --------------- |
| type              | NUMBER，CURRENCY，或   PERCENT类型 | 否   | Number          |
| pattern           | 指定一个自定义的格式化模式用与输出 | 否   | 无              |
| currencyCode      | 货币码（当type="currency"时）      | 否   | 取决于默认区域  |
| currencySymbol    | 货币符号 (当   type="currency"时)  | 否   | 取决于默认区域  |
| groupingUsed      | 是否对数字分组   (TRUE 或 FALSE)   | 否   | true            |
| maxIntegerDigits  | 整型数最大的位数                   | 否   | 无              |
| minIntegerDigits  | 整型数最小的位数                   | 否   | 无              |
| maxFractionDigits | 小数点后最大的位数                 | 否   | 无              |
| minFractionDigits | 小数点后最小的位数                 | 否   | 无              |
| var               | 存储格式化数字的变量               | 否   | Print   to page |
| scope             | var属性的作用域                    | 否   | page            |

## SQL标签

JSTL SQL标签库提供了与关系型数据库（Oracle，MySQL，SQL Server等等）进行交互的标签。引用SQL标签库的语法如下：

```jsp
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
```

| 标签                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| `<sql:setDataSource>` | 指定数据源                                                   |
| `<sql:query>`         | 运行SQL查询语句                                              |
| `<sql:update>`        | 运行SQL更新语句                                              |
| `<sql:param>`         | 将SQL语句中的参数设为指定值                                  |
| `<sql:dateParam>`     | 将SQL语句中的日期参数设为指定的java.util.Date   对象值       |
| `<sql:transaction>`   | 在共享数据库连接中提供嵌套的数据库行为元素，将所有语句以一个事务的形式来运行 |

## 自定义标签

### 步骤

1. 定义标签处理类（必须是Tag或SimpleTag的实现类,继承SimpleTagSupport更方便）

2. 编写tld文件，它是一个xml

3. 页面中使用`<%@taglib%>`来指定tld文件的位置

 

### 标签处理类

#### SimpleTag接口：

- void doTag()：每次执行标签时都会调用这个方法；

- JspTag getParent()：返回父标签（非生命周期方法）

- void setParent(JspTag)：设置父标签

- void setJspBody(JspFragment)：设置标签体对象

- void seetJspContext(JspContext)：设置jsp上下文对象，其实传入的是子类PageContext

在执行doTag()之前，其他三个set方法会被tomcat调用，所以可以在doTag中使用三个成员变量

可以在标签处理类中的doTag()中抛出SkipPageException，当Tomcat调用doTag()方法，并捕获到SkipPageException时，它会跳过本页面其他内容，结束处理。

```java
package my.tag;

import java.io.IOException;
import java.io.StringWriter;
import javax.servlet.jsp.JspContext;
import javax.servlet.jsp.JspException;
import javax.servlet.jsp.tagext.SimpleTagSupport;

public class HelloTag extends SimpleTagSupport{
	// 标签属性设置为同名的bean属性，并配上get/set方法
    private String who = "";
	
	StringWriter sw = new StringWriter();
	
	@Override
	public void doTag() throws JspException, IOException {
		// 获取标签体内容传给sw
		getJspBody().invoke(sw);
		
		JspContext jctx = this.getJspContext();
		jctx.getOut().write("hello "+this.who+"!"+sw.toString());
		
	}

	public String getWho() {
		return who;
	}

	public void setWho(String who) {
		this.who = who;
	}
}

```



### 配置tld文件

tld文件一般都放到WEB-INF之下，这样保证客户端访问不到
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<taglib xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd" version="2.1">
  <!-- 标签库的描述 -->
  <description>This is my tag library</description>
  <!-- 标签库的显示名称 -->
  <display-name>MY Tag</display-name>
  <!-- 标签库的版本信息 -->
  <tlib-version>1.0</tlib-version>
  <!-- 建议使用的标签库的前缀 -->
  <short-name>my</short-name>
  <!-- 标签库的uri,导入标签库时用到-->
  <uri>my.tag</uri>

  <tag>
    <!-- 标签的描述 -->
    <description>print "hello my tag"</description>
    <!-- 标签的名称 -->
    <name>hello</name>
    <!-- 自定义的标签处理类 -->
    <tag-class>my.tag.HelloTag</tag-class>
    <!-- 标签的内容，empty就表示是一个单标签 -->
    <body-content>scriptless</body-content>
      <attribute>
          <!-- 标签属性的描述信息 -->
          <description>the name of a person</description>
          <!-- 每个标签的属性的名称必须唯一 -->
          <name>who</name>
          <!-- 指明这个属性是必须(true)还是可选(false) -->
          <required>false</required>
          <!-- 该属性是否可以接受运行时表达式的值(EL表达式) -->
          <rtexprvalue>true</rtexprvalue>
          <!-- 标签的属性的类型 -->
          <type>java.lang.String</type>
          <!-- 如果为true,属性值将被视为一个JspFragment -->
          <fragment>false</fragment>
      </attribute>
  </tag>

</taglib>
```

`<body-content>`标签体取值：

- empty：无标签体,即忽略标签体。

- JSP：可以包含所有类型的JSP语法

- scriptless：接受文本，EL表达式和JSP动作标签，如JSP脚本元素(`<%%>`)就不行

  - getjspbody().invoke(Writer out) 处理标签体内容,并输出到指定out。参数为null是输出到pageContext.getOut()得到的JspWriter对象中。

- tagdependent：标签体原封不动转交由标签处理类处理

 

### JSP页面中指定tld文件位置

```jsp
<%@ taglib prefix="my" uri="my.tag" %>
```

然后使用标签

```xml
<my:hello who="John">How are you?</my:hello>
```

运行结果

```
hello john!How are you?
```

 
