# EL(表达式语言)

## EL是JSP内置的表达式语言

- jsp2.0开始，不让再使用java脚本，而是使用el表达式和动态标签来替代java脚本！

- EL替代的是<%= ... %>，也就是说，EL只能做输出！

- 如果希望忽略某个EL表达式，可以在EL表达式之前添加“\”，例如：\${1 + 2}。

- **下标写法**：`${initParam['a']}`等效于`${initParam.a}`,且当属性名包含"_"时必须用下标写法



## EL内置对象

EL一共11个内置对象，无需创建即可以使用。这11个内置对象中有10个是Map类型的，最后一个是pageContext对象不是map。

四大域相关内置对象

- `${xxx}`，全域查找名为xxx的属性(顺序为page,request,session,application)，如果不存在，输出空字符串，而不是null。
- `${pageScope.xxx}、${requestScope.xxx}、${sessionScope.xxx}、${applicationScope.xxx}`，指定域获取属性

### 请求参数和头相关内置对象

- param：Map<String,String>类型,适用于单值的参数。

- paramValues：Map<String, String[]>类型,适用于多值的参数。

- header： Map<String,String>类型，适用于单值请求头

- headerValues：Map<String,String[]>类型，适用于多值请求头

- initParam：Map<String,String>类型,获取`<context-param>`内的参数！

```xml
<context-param>
    <param-name>xxx</param-name>
    <param-value>XXX</param-value>
</context-param>
<context-param>
    <param-name>yyy</param-name>
    <param-value>YYY</param-value>
</context-param>
```

例如：`${initParam.xxx}`

### 其他内置对象

- cookie：Map<String,Cookie>类型，其中key是cookie的name，value是cookie对象。`${cookie.username.value}`
- pageContext：它是PageContext类型！`${pageContext.request.contextPath}`

### javaBean导航

```jsp
<%
    Address address = new Address();
    address.setCity("北京");
    address.setStreet("西三旗");

    Employee emp = new Employee();
    emp.setName("李小四");
    emp.setSalary(123456);
    emp.setAddress(address);

    request.setAttribute("emp", emp);
%>
```

使用el获取request域的emp

`${requestScope.emp.address.street }`等效于request.getAttribute("emp").getAddress().getStreet()

request的emp(对象)属性的address(对象)属性的street(对象)属性,这里指的是javaBean里的属性,用get方法实现

## EL函数库（由JSTL提供的）

导入标签库：`<%@ tablib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>`

- String toUpperCase(String input)：把参数转换成大写
- String toLowerCase(String input)：把参数转换成小写
- int indexOf(String input, String substring)：从大串，输出小串的位置！
- boolean contains(String input, String substring)：查看大串中是否包含小串
- boolean containsIgnoreCase(String input, String substring)：忽略大小写的，是否包含
- boolean startsWith(String input, String substring)：是否以小串为前缀
- boolean endsWith(String input, String substring)：是否以小串为后缀
- String substring(String input, int beginIndex, int endIndex)：截取子串
- String substringAfter(String input, String substring)：获取大串中，小串所在位置后面的字符串
- substringBefore(String input, String substring)：获取大串中，小串所在位置前面的字符串
- String escapeXml(String input)：把input中“<”、">"、"&"、"'"、"""，进行转义
- String trim(String input)：去除前后空格
- String replace(String input, String substringBefore, String substringAfter)：替换
- String[] split(String input, String delimiters)：分割字符串，得到字符串数组
- int length(Object obj)：可以获取字符串、数组、各种集合的长度！
- String join(String array[], String separator)：联合字符串数组！



#### 自定义函数库

写一个java类，类中可以定义0~N个方法，但必须是static，而且有返回值的

在WEB-INF目录下创建一个tld文件

```xml
<function>
    <name>fun</name> //方法名
    <function-class>cn.itcast.fn.MyFunction</function-class> //方法所在类
    <function-signature>java.lang.String fun()</function-signature> //方法签名
</function>
```

在jsp页面中导入标签库

```jsp
<%@ taglib prefix="it" uri="/WEB-INF/tlds/itcast.tld" %>
```

在jsp页面中使用自定义的函数：`${it:fun() }`

## EL中的基础操作符

EL表达式支持大部分Java所提供的算术和逻辑操作符：

| **操作符**   | **描述**                         |
| ------------ | -------------------------------- |
| .            | 访问一个Bean属性或者一个映射条目 |
| []           | 访问一个数组或者链表的元素       |
| ( )          | 组织一个子表达式以改变优先级     |
| +            | 加                               |
| -            | 减或负                           |
| *            | 乘                               |
| / or   div   | 除                               |
| % or   mod   | 取模                             |
| == or   eq   | 测试是否相等                     |
| != or   ne   | 测试是否不等                     |
| < or   lt    | 测试是否小于                     |
| > or   gt    | 测试是否大于                     |
| <=   or le   | 测试是否小于等于                 |
| >=   or ge   | 测试是否大于等于                 |
| &&   or and  | 测试逻辑与                       |
| \|\| or   or | 测试逻辑或                       |
| ! or   not   | 测试取反                         |
| empty        | 测试是否空值                     |