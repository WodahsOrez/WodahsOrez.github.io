# JSP(Java Server Pages)

运行在服务器端的Java页面.使用HTML嵌套Java代码实现。其本质是把JSP页面解析为对应的Servlet，然后通过Servlet返回静态页面。

## 语法

### 注释和写法

`<%...%>`：相当于写在方法内

`<%=...%>`：相当于写在response.getWriter().print( ... )里

`<%!...%>`：相当于写在类里,与成员变量或方法同级

`<%-- jsp注释 --%>`,jsp内可以使用Java注释

 

### 声明和使用

#### 变量

`<%! %>`类成员声明和`<% %>`方法内声明

`<%=变量名%>`等效于print("变量名") 

#### 方法

只能在`<%!  %>`中声明



## JSP隐式对象

### request对象

客户端的请求信息被封装在request对象

#### 方法

- String getParameter(String      name)  返回name指定参数的参数值 
- String[]      getparameterValues(String name)       返回包含参数name的所有值的数组 
- void      setCharacterEncoding("gb2312")       设置接受参数的字符集 
- RequestDispatcher   getRequestDispatcher(String path)   返回一个RequestDispatcher对象，该对象的forward(request,response)方法用于转发请求
- object getAttribute(String   name)  返回指定属性的属性值 
- void setAttribute(String key  , Object obj)  设置属性的属性值 
- Cookie[] getCookies()      获取Cookie数组
- String getProtocol()  返回请求用的协议类型及版本号 

 

### response对象

服务端的响应信息被封装在response对象

#### 方法

- sendRedirect(String location) 重新定向客户端的请求,跳转到location的页面 

- addCookie(Cookie cookie) 添加Cookie数据




### application对象

即servletContext，整个项目共享的使用数据,服务器开启时创建,服务器关闭消失.

#### 方法

- Object getAttribute(String name)  返回给定名的属性值  

-  void setAttribute(String name,Object obj)  设定属性的属性值  




### session对象

#### 范围:

指的是客户端与服务器的一次会话,从首次访问服务器开始，到该用户关闭浏览器结束(存储在服务器)

#### 生命周期:

- 当不在被需要时,从服务器中销毁,分为于主动调用销毁和过期自动销毁
- 其实服务器是不会知道浏览器关闭了没有，所以关闭浏览器时服务器是不会删除Session的，也正是这个原因服务器才会设置一个Session失效时间的

- session靠的是浏览器cookie里的sessionId区分的,不同sessionId对应不同session,相互之间数据不共享


#### 方法

##### 获取session对象

- Jsp中得到session对象：session是jsp内置对象之下，不用创建就可以直接使用！

- Servlet中得到session对象：HttpSession session = request.getSession();
  - 该方法还有个可选boolean参数,默认为true 
  - 为false时:如果session缓存中(如果cookie不存在)，不存在session，那么返回null，而不会创建session对象。

 

###### 实现原理

request.getSession()方法：不调用则不创建Session,jsp本身调用这个方法所以一定有Session

- 获取Cookie中的JSESSIONID：

- 如果sessionId不存在，创建session，把session保存起来，把新创建的sessionId保存到Cookie中

  - 如果sessionId存在，通过sessionId查找session对象，如果没有查找到，创建session，把session保存起来，把新创建的sessionId保存到Cookie中

  - 如果sessionId存在，通过sessionId查找到了session对象，那么就不会再创建session对象了。

  - 返回session

- 如果创建了新的session，浏览器会得到一个包含了sessionId的Cookie，这个Cookie的生命为-1，即只在浏览器内存中存在，如果不关闭浏览器，那么Cookie就一直存在。

- 下次请求时，再次执行request.getSession()方法时，因为可以通过Cookie中的sessionId找到session对象，所以与上一次请求使用的是同一session对象

  - 服务器不会马上给你创建session，在第一次获取session时，才会创建，即调用request.getSession()时

  - request.getSession(false)、request.getSession(true)、request.getSession()，后两个方法效果相同，

    - 第一个方法：如果session缓存中(如果cookie不存在)，不存在session，那么返回null，而不会创建session对象。

 

##### session域相关方法

- void setAttribute(String key,Object obj)  设置Session的属性 

- Object getAttribute(String name)  返回session的属性，不存在则返回null

- void removeAttribute(String name)  移除session中指定名称的对象

- String getId()   返回session对象的ID

- void invalidate()  将session无效化，解绑任何与该session绑定的对象,再次请求会新建一个替代它

- void setMaxInactiveInterval(int interval)   最大活动时间，以秒为单位，默认30分钟

- boolean isNew()：查看session是否为新。当客户端第一次请求时，服务器为客户端创建session，但这时服务器还没有响应客户端，也就是还没有把sessionId响应给客户端时，这时session的状态为新




##### 设置session自动清除时间(单位分钟)web.xml中
```xml
<session-config>
  <session-timeout>30</session-timeout>
</session-config>
```



### Cookie对象

存储在客户机的文本文件,保存了大量轨迹信息

#### 方法

##### 该方法设置 cookie 过期的时间（以秒为单位）。

- public void setMaxAge(int expiry)  如果不设置，cookie 只会在当前 session 会话中持续有效。
  - expiry>0：浏览器会把Cookie保存到客户机硬盘上，有效时长为maxAge的值决定。
  - expiry<0：Cookie只在浏览器内存中存在，当用户关闭浏览器时，浏览器进程结束，同时Cookie也就死亡了。
  - expiry=0：浏览器会马上删除这个Cookie！
- public String getName()  返回 cookie的名称，名称创建后将不能被修改

- public String getValue()  获取cookie的值



##### JavaWeb中使用Cookie

原始方式：

- 使用response发送Set-Cookie响应头:response.addHeader("Set-Cookie", "bbb=BBB")

- 使用request获取Cookie请求头:request.getHeader("Set-Cookie")

便捷方式：

- 使用repsonse.addCookie()方法向浏览器保存Cookie

- 使用request.getCookies()方法获取浏览器归还的Cookie

 

##### Cookie中存中文需要编码和解码

- URLEncoder.encode(String s,String enc)  以enc编码字符串s

- URLDecoder.decode(String s,String enc)  以enc解码字符串s




##### Cookie的path：

-  Cookie的path由服务器创建Cookie时设置,并不是设置这个Cookie在客户端的保存路径

- 浏览器访问服务器的路径，如果包含某个Cookie的路径，那么就会归还这个Cookie。

- 例如：

  - aCookie.path=/day11_1/; bCookie.path=/day11_1/jsps/; cCookie.path=/day11_1/jsps/cookie/;

  - 访问：/day11_1/index.jsp时，归还：aCookie

  - 访问：/day11_1/jsps/a.jsp时，归还：aCookie、bCookie

  - 访问：/day11_1/jsps/cookie/b.jsp时，归还：aCookie、bCookie、cCookie

- Cookie的path默认值：当前访问路径的父路径。例如在访问/day11_1/jsps/a.jsp时，响应的cookie，那么这个cookie的默认path为/day11_1/jsps/

 

##### Cookie的domain

- domain用来指定Cookie的域名！当多个二级域中共享Cookie时才有用。

- 例如；`www.baidu.com、zhidao.baidu.com、news.baidu.com、tieba.baidu.com`之间共享Cookie时可以使用domain

- 设置domain为：cookie.setDomain(".baidu.com"); 匹配域名的就会共享cookie.

- 设置path为：cookie.setPath("/"); 为了可以共享,必须为"/"

 

Http协议规定（保证不给浏览器太大压力）：

- 1个Cookie最大4KB

- 1个服务器最多向1个浏览器保存20个Cookie

- 1个浏览器最多可以保存300个Cookie

 

 

### pageContext对象

JSP的第四大域对象,主要作用是获取并代理其他域对象.

**作用范围**：为当前页面,这个域是在当前jsp页面和当前jsp页面中使用的标签之间共享数据,

**生命周期**：是当对JSP的请求时开始，当响应结束时销毁。即response不存在时结束.

 

#### 主要方法

##### 代理其他域对象

- void setAttribute(String name, Object value,  int scope)：在指定范围中添加数据；
- Object getAttribute(String name, int scope)：获取指定范围的数据；
- void removeAttribute(String  name, int scope)：移除指定范围的数据；
- scope为SESSION_SCOPE/REQUEST_SCOPE/APPLICATION_SCOPE

 

##### 全域查找

Object findAttribute(String name)：依次在pageContext、request、session、application范围查找名称为name的数据，如果找到就停止查找。这说明在这个范围内有相同名称的数据，那么page范围的优先级最高！

 

##### 获取其他8个内置对象：

- JspWriter getOut()：获取out内置对象；
- ServletConfig      getServletConfig()：获取config内置对象；
- Object getPage()：获取page内置对象；
- ServletRequest      getRequest()：获取request内置对象；
- ServletResponse      getResponse()：获取response内置对象；
- HttpSession      getSession()：获取session内置对象；
- ServletContext      getServletContext()：获取application内置对象；
- Exception      getException()：获取exception内置对象；



### page对象

final java.lang.Object page = this; 

page（当前JSP的真身类型）：当前JSP页面的“this”，即当前对象

 

### config对象

config（ServletConfig）：对应“真身”中的ServletConfig

 

### exception对象



### out对象

out（JspWriter）：等同与response.getWriter()，用来向客户端发送文本数据

public abstract void print() 显示各种数据类型的内容。



## 三大指令标签

  一个jsp页面中，可以有0~N个指令的定义。

###  

### page指令标签

#### 用来设置jsp页面的属性

`<%@ page language="java" contentType="text/html; charset=UTF-8"pageEncoding="UTF-8"%  import="java.util.Date,java.text.*">`

- pageEncoding是设置的JSP页面源代码的字符编码格式
- charset是请求服务器以后返回过来的内容的字符编码(当浏览器得到此文件时以什么方式解码)

  - contentType：它表示添加一个响应头：Content-Type！等效于response.setContentType("text/html;charset=utf-8");

  - 如果两个属性只提供一个，那么另一个的默认值为设置那一个。

  - 如果两个属性都没有设置，那么默认为iso

- import后更需要导入的包,可多个

 

#### errorPage和isErrorPage

- errorPage：当前页面如果抛出异常，那么要转发到哪一个页面，由errorPage来指定

- isErrorPage：它指定当前页面是否为处理错误的页面！当该属性为true时，这个页面会设置状态码为500！(不写状态码为200)而且只有这个页面可以使用9大内置对象中的exception!

##### web.xml里设置错误页面 

```xml
<error-page>
    <error-code>404</error-code>
    <location>/error/errorPage.jsp</location>
</error-page>
<error-page>
    <error-code>500</error-code>
    <location>/error/errorPage.jsp</location>
</error-page>
<error-page>
    <exception-type>java.lang.RuntimeException</exception-type>
    <location>/index.jsp</location>
</error-page>

```

 

#### autoFlush和buffer

- autoFlush：指定jsp的输出流缓冲区满时，是否自动刷新！默认为true，如果为false，那么在缓冲区满时抛出异常！

- buffer：指定缓冲区大小，默认为8kb，通常不需要修改！指定out对象的缓存区

 

#### 其他属性：

- isELIgnored：是否忽略el表达式，默认值为false，不忽略，即支持！

- language：指定当前jsp编译后的语言类型，默认值为java。

- info：信息！

- isThreadSafe：当前的jsp是否支持并发访问！

- session：当前页面是否支持session，如果为false，那么当前页面就没有session这个内置对象！

- extends：让jsp生成的servlet去继承该属性指定的类！

 

### include静态包含

与RequestDispatcher的include()方法的功能相似。

- `<%@include file=”b.jsp”flush="true"%>` 它是在jsp编译成java文件时完成的！他们共同生成一个java(就是一个servlet)文件，然后再生成一个class！
- RequestDispatcher的include()是一个方法，包含和被包含的是两个servlet，即两个.class！他们只是把响应的内容在运行时合并了！


**作用**：把页面分解了，使用包含的方式组合在一起，这样一个页面中不变的部分，就是一个独立jsp，而我们只需要处理变化的页面。

 

### taglib导入标签库

两个属性：

- prefix：指定标签库在本页面中的前缀！由我们自己来起名称！

- uri: 相对于项目目录的路径,指向tld文件

- `<%@taglib prefix="s" uri="/struts-tags"%>` 前缀的用法`<s:text>`



## JSP的动作标签

这些jsp的动作标签，与html提供的标签有本质的区别，是由tomcat(服务器)来解释执行。都有id和scope属性。

- `<jsp:forward page="a.jsp">`：转发！它与RequestDispatcher的forward方法是一样的
- `<jsp:include page="{要包含的文件路径|<%=表达式%>}" flush="true">`：包含：它与RequestDispatcher的include方法是一样的,本质上只是调用另一个Servlet

- `<%@include>`编译级别(同一个Servlet)`<jsp:include>`运行级别(不同Servlet)

- `<jsp:param name="name" value="<%=username%>">`：它用来作为forward和include的子标签！用来给转发或包含的页面传递参数！

- `<jsp:plugin>`嵌入Applet



## JavaBean

### javaBean的规范

1. 必须要有一个无参构造器

2. 提供get/set方法，如果只有get方法，那么这个属性是只读属性,如果只有set方法,为只写属性.

3. 属性：有get/set方法的成员，还可以没有成员，只有get/set方法。属性名称由get/set方法来决定！而不是成员名称！

4. 方法名称满足一定的规范，那么它就是属性！boolean类型的属性，它的读方法可以是is开头，也可以是get开头！

5. JavaBean属性名要求：一般小写开头,特殊时需前两个字母要么都大写，要么都小写

 

### 内省

```java
// 通过java.beans.Introspector的getBeanInfo()方法来获取java.beans.BeanInfo实例。
BeanInfo beanInfo = Introspector.getBeanInfo(User.class);

// 通过BeanInfo可以得到这个类的所有JavaBean属性的PropertyDescriptor(属性描述符)对象。
PropertyDescriptor[] pds = beanInfo.getPropertyDescriptors();

// 每个PropertyDescriptor对象对应一个JavaBean属性：
String getName()：获取JavaBean属性名称；
Method getReadMethod：获取属性的读方法；
Method getWriteMethod：获取属性的写方法。
```

 

#### commons-beanutils，它是依赖内省完成

**导包**：commons-beanutils.jar，commons-logging.jar

**方法**

- BeanUtils.getProperty(Object bean, String propertyName) 获取JavaBean属性

- BeanUtils.setProperty(Object bean, String propertyName, String propertyValue) 设置JavaBean属性

- BeanUtils.populate(Map map, Object bean) 封装Map数据到JavaBean对象中

- CommontUtils.toBean(Map map, Class class) 返回封装了Map数据的JavaBean对象




### jsp中与javaBean相关的标签

`<jsp:useBean>`  创建或查询bean
```
<jsp:useBean id="user1" class="cn.itcast.domain.User" scope="session"/> 
在session域中查找名为user1的bean，如果不存在，创建之
```
`<jsp:setProperty>`设置属性
```
<jsp:setProperty property="username" name="user1" value="admin"/> 
设置名为user1的这个javabean的username属性值为admin
```
`<jsp:getProperty>`获取属性
```
<jsp:getProperty property="username" name="user1"/> 
获取名为user1的javabean的名为username属性值
```

