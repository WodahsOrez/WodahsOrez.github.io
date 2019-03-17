# servlet

## javax.servlet.Servlet 接口

servlet都是单例

### 方法: 
```java
public void init(ServletConfig config) throws ServletException;  初始化操作,只在创建Servlet对象之后执行一次
public void destroy();  在Servlet销毁之前执行一次,本身不执行销毁
public void service(ServletRequest req, ServletResponse res)  throws ServletException, IOException;  每次处理请求时调用
public ServletConfig getServletConfig();   获取Servlet配置信息
public String getServletInfo();  获取Servlet信息
```
### 1.在web.xml里配置Servlet

```xml
<servlet>
	<servlet-name>hello</servlet-name> 
	<servlet-class>cn.itcast.servlet.HelloServlet</servlet-class>
	//随服务器启动,值为大于等于0的整数,小的先执行
	<load-on-startup>0</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>hello</servlet-name> 
	<url-pattern>/helloworld</url-pattern>
</servlet-mapping>  
```
通过相同的servlet-name把servlet的实现类与url路径关联起来.

### 2.ServletConfig接口

  ServletConfig对象封装了Servlet在web.xml中的配置信息，它对应<servlet>元素。ServletConfig类的功能有：
```java
String getServletName()：获取Servlet配置名，即<servlet-name>的值；
ServletContext getServletContext()：获取ServletContext对象，这个对象稍后介绍
String getInitParameter(String name)：获取Servlet自己的初始化参数
Enumeration getInitParameterNames()：获取Servlet自己的所有初始化参数的名称
```
  在web.xml文件中，配置`<servlet>`时可以为`<servlet>`配置0~N个初始化参数，例如：
```xml
<servlet>
  <servlet-name>xxx</servlet-name>
  <servlet-class>cn.itcast.servlet.MyServlet</servlet-class>
  <init-param>
    <param-name>p1</param-name>
    <param-value>v1</param-value>
  </init-param>
  <init-param>
    <param-name>p2</param-name>
    <param-value>v2</param-value>
  </init-param>
</servlet>
```
## GenericServlet抽象类

GenericServlet是Servlet接口的实现类，但它是一个抽象类，它唯一的抽象方法就是service()方法

### GenericServlet实现了Servlet接口：

- 实现了String getServletInfo()方法
- 实现了void destory()方法，空实现
- 实现了void init(ServletConfig)方法，用来保存ServletConfig参数
- 实现了ServletConfig getServletConfig()方法

### GenericServlet实现了ServletConfig接口：

- 实现了ServletContext getServletContext()方法
- 实现了String getInitParameter()方法
- 实现了String getServletName()方法
- 实现了Enumeration getInitParameterNames()方法

### GenericServlet添加了init()方法：

- 该方法会被init(ServletConfig)方法调用
- 如果希望对Servlet进行初始化，那么应该覆盖init()方法，而不是init(ServletConfig)方法

## HttpServlet类

HttpServlet是GenericServlet的子类，它专注HTTP请求

### HttpServlet类的方法：

1. 实现了void service(ServletRequest,ServletResponse)方法，实现内容是：
    * 把ServletRequest强转成HttpServletRequest
    * 把ServletResponse强转成HttpServletResponse
    * 调用本类添加的void service(HttpServletRequest,HttpServletResponse)方法
2. 添加了void service(HttpServletRequest,HttpServletResponse)方法，内容是：
    * 调用request的getMethod()获取请求方式
    * 如果请求方式为GET，那么调用本类添加的doGet(HttpServletRequest,HttpServletResponse)方法
    * 如果请求方式为POST，那么调用本类添加的doPost(HttpServletRequest,HttpServletResponse)方法
3. 添加了doGet(HttpServletRequest,HttpServletResponse)方法，内容是响应405，表示错误，所以我们应该去覆盖这个方法
4. 添加了doPost(HttpServletRequest,HttpServletResponse)方法，内容是响应405，表示错误，所以我们应用去覆盖这个方法

### 如果是通过继承HttpServlet类来自定义Sevlet的话，那么：

  * 不要去覆盖void service(ServletRequest,ServletResponse)
  * 不要去覆盖void service(HttpServletRequest, HttpServletResponse)
  * 而应该去覆盖doGet()或doPost()方法。

### `<url-pattern>`配置

url-pattern是一种匹配,匹配的是项目路径后的url部分,匹配到则跳转相应的Servlet

`<url-pattern>`是`<servlet-mapping>`的子元素，用来绑定Servlet的访问路径

可以在一个`<servlet-mapping>`中给出多个`<url-pattern>`，也就是说一个Servlet可以有

多个访问路径：

```xml
  <servlet-mapping>
    <servlet-name>xxx</servlet-name>
    <url-pattern>/helo1<url-pattern>
    <url-pattern>/hello2<url-pattern>
  </servlet-mapping>
```
还可以在`<url-pattern>`中使用通配符，即“*”。
*  `<url-pattern>/*<url-pattern>`：表示匹配任何路径(全匹配)
* ` <url-pattern>/do/*<url-pattern>`：表示匹配以/do开头的任何路径(路径匹配)
* `<url-pattern>*.do<url-pattern>`：表示匹配任何以“.do”结尾的路径(拓展名匹配)
* 同时匹配多个Servlet时,匹配越多优先级越低

**注意**：

* 通配符要么在开头，要么在结尾，不能在中间，例如：/*.do就是错误的使用。
* 如果不使用通配符，那么<url-pattern>必须以“/”开头，例如：<url-pattern>abc</url-pattern>就是错误的



## Servlet域对象

### HttpServletContext接口

ServletContext是Servlet三大域对象之一.ServletContext在服务器启动时创建，在服务器关闭时销毁，一个JavaWeb应用只创建一个ServletContext对象

#### 获取ServletContext对象
```java
ServletConfig  getServletContext()；
GenericServlet  getServletContext();
HttpSession  getServletContext()
ServletContextEvent  getServletContext()
// 在HttpServlet中可以通过以下方法来获取ServletContext对象
ServletContext sc = this.getServletContext()
ServletContext sc = this.getServletConfig().getServletContext()
```

#### 存取数据

因为在一个JavaWeb应用中，只有一个ServletContext对象，所以在ServletContext中保存的数据可以共整个JavaWeb应用中的动态资源共享.ServletContext是Servlet三大域对象之一，域对象内部有一个Map，用来保存数据
```java
void setAttribute(String name, Object value)：用来添加或替换ServletContext域数据
Object getAttribute(String name)：通过名称来获取域数据
void removeAttribute(String name)：通过名称移除域数据
Enumeration<String> getAttributeNames()：获取所有ServletContext域数据的名称
```

#### 读取web.xml中配置的应用初始化参数

```xml
<context-param>
    <param-name>p1</param-name>
    <param-value>v1</param-value>  	
</context-param>
<context-param> 
    <param-name>p2</param-name>
    <param-value>v2</param-value>  	
</context-param>
```
```java
servletContext.getInitParameter("p1")，返回v1
servletContext.getInitParameter("p2")，返回v2
 servletContext.getInitParameterNames()，返回Enumeration<String>，包含p1和p2
```

#### 获取项目资源

相对路径和/路径都是项目目录 
```java
// String getRealPath(String path)：获取资源的真实路径
String path = servletContext.getRealPath("/WEB-INF/a.jpg");
返回值为/WEB-INF/a.jpg真实路径，即磁盘路径：
C:/tomcat6/wabapps/hello/WEB-INF/a.jpg

// InputStream getResourceAsStream(String path)：获取资源的输入流
InputStream in = servletContext.getResourceAsStream("/WEB-INF/a.jpg");
返回的是a.jpg的输入流对象，可以从流中得到a.jpg的数据

// Set<String> getResourcePaths(String path)：获取指定目录下的所有资源路径
Set<String> paths = servletContext.getResourcePaths("/WEB-INF");
返回的Set中包含如下字符串：
/WEB-INF/lib/
/WEB-INF/classes/
/WEB-INF/web.xml
/WEB-INF/a.jpg
```

#### 获取类路径资源

可以通过Class类的对象来获取类路径下的资源，对应JavaWeb应用的类路径就是classes目录下的资源，

例如：

```java
InputStream in = cn.itcast.servlet.MyServlet.class.getResourceAsStream("a.jpg");
获取的是：/WEB-INF/classes/cn/itcast/servlet/a.jpg，即与MyServlet.class同目录下的资源
```

例如：

```java
InputStream in = cn.itcast.servlet.MyServlet.class.getResourceAsStream("/a.jpg");
获取的是：/WEB-INF/classes/a.jpg，即类路径的根目录下的资源，类路径的根目录就是/classes目录
```



### HttpServletRequest接口

**服务器处理请求的流程**：

1. 服务器每次收到请求时，都会为这个请求开辟一个新的线程。
2. 服务器会把客户端的请求数据封装到request对象中，request就是请求数据的载体！
3. 服务器还会创建response对象，这个对象与客户端连接在一起，它可以用来向客户端发送响应。

获取请求头信息

请求协议中的数据都可以通过request对象来获取
```java
* 获取常用信息
> 获取客户端IP，案例：封IP。request.getRemoteAddr()
> 请求方式，request.getMethod()，可能是POST也可能是GET
* 获取HTTP请求头
> String getHeader(String name)，适用于单值头
> int getIntHeader(String name)，适用于单值int类型的请求头
> long getDateHeader(String name)，适用于单值毫秒类型的请求头
> Enumeration<String> getHeaders(String name)，适用于多值请求头
```
**案例**：

1. 通过User-Agent识别用户浏览器类型
2. 防盗链：如果请求不是通过本站的超链接发出的，发送错误状态码404。Referer这个请求头，表示请求的来源。

#### 获取请求URL

```java
http://localhost:8080/day10_2/AServlet?username=xxx&password=yyy
> String getScheme()：获取协议，http
> String getServerName()：获取服务器名，localhost
> String getServerPort()：获取服务器端口，8080
> String getContextPath()：获取项目名，/day10_2
> String getServletPath()：获取Servlet路径，/AServlet
> String getQueryString()：获取参数部分，即问号后面的部分。username=xxx&password=yyy
> String getRequestURI()：获取请求URI，等于项目名+Servlet路径。/day10_2/AServlet
> String getRequestURL()：获取请求URL，等于不包含参数的整个请求路径。http://localhost:8080/day10_2/AServlet
> String getProtocol()  返回请求用的协议类型及版本号 
```

#### 获取请求参数

请求参数是由客户端发送给服务器的！有可能是在请求体中（POST），也可能是在URL之后（GET）
请求参数：有一个参数一个值的，还有一个参数多个值

```java
String getParameter(String name)：获取指定名称的请求参数值，适用于单值请求参数
String[] getParameterValues(String name)：获取指定名称的请求参数值，适用于多值请求参数
Enumeration<String> getParameterNames()：获取所有请求参数名称
Map<String,String[]> getParameterMap()：获取所有请求参数，其中key为参数名，value为参数值。
```

#### 请求转发和请求包含

```java
RequestDispatcher rd = request.getRequestDispatcher("/MyServlet");
使用request获取RequestDispatcher对象，方法的参数是被转发或包含的Servlet的Servlet路径(url-pattern)
请求转发：rd.forward(request,response);
请求包含：rd.include(request,response);
```

有时一个请求需要多个Servlet协作才能完成，所以需要在一个Servlet跳到另一个Servlet
1. 一个请求跨多个Servlet，需要使用转发和包含。
2. 请求转发：由下一个Servlet完成响应体！当前Servlet可以设置响应头！（留头不留体,响应体会被后一个覆盖）
3. 请求包含：由两个Servlet共同来完成响应体！（都留）
4. 无论是请求转发还是请求包含，都在一个请求范围内！使用同一个request和response！

#### request域

Servlet中三大域对象：request、session、application(ServletContext)，都有如下三个方法：
```java
void setAttribute(String name, Object value)
Object getAttribute(String name)
void removeAttribute(String name);
同一请求范围内使用request.setAttribute()、request.getAttribute()来传值！前一个Servlet调用setAttribute()保存值，后一个Servlet调用getAttribute()获取值。
```

##### 请求转发和重定向的区别

1. 请求转发是一个请求一次响应，而重定向是两次请求两次响应
2. 请求转发地址栏不变化，而重定向会显示后一个请求的地址
3. 请求转发只能转发到本项目其他Servlet，而重定向不只能重定向到本项目的其他Servlet，还能定向到其他项目
4. 请求转发是服务器端行为，只需给出转发的Servlet路径，而重定向需要给出requestURI，即包含项目名！
5. 请求转发和重定向效率是转发高！因为是一个请求！

- 需要地址栏发生变化，那么必须使用重定向！
- 需要在下一个Servlet中获取request域中的数据，必须要使用转发！



### HttpServletResponse接口

继承自ServletResponse接口,response使用的就是其对象.

#### 方法
```java
sendError(int sc) --> 发送错误状态码，例如404、500
sendError(int sc, String msg) --> 也是发送错误状态码，还可以带一个错误信息！
setStatus(int sc) --> 发送成功的状态码，可以用来发送302
```
#### 响应头方法(头名不区分大小写)

```java
setHeader(String name, String value)：适用于单值的响应头
addHeader(String name, String value)：适用于多值的响应头
setIntHeader(String name, int value)：适用于单值的int类型的响应头
addIntHeader(String name, int value)：适用于多值的int类型的响应头
setDateHeader(String name, long value)：适用于单值的毫秒类型的响应头
addDateHeader(String name, long value)：适用于多值的毫秒类型的响应头
```
#### 案例：

- 发送302，设置Location头，完成重定向,值为包含项目名的/路径
- 定时刷新：设置Refresh头，你可以把它理解成，定时重定向！
  `response.setHeader("Refresh", "5;URL=/firstWeb/servlet/NewAServlet")`
- 禁用浏览器缓存：Cache-Control、pragma、expires
- `<meta>`标签可以代替响应头：`<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">`

#### response的两个流：

- ServletOutputStream getOutputStream()用来向客户端发送字节数据。
- PrintWriter getWriter()，用来向客户端发送字符数据！需要设置编码。
- 两个流不能同时使用！否则会抛出IllegalStateException异常
- sendRedirect(String location)重定向方法,设置302，设置Location




## 编码

常见字符编码：iso-8859-1(不支持中文)、gb2312、gbk、gb18030(系统默认编码，中国的国标码)、utf-8(万国码，支持全世界的编码，所以我们使用这个)

### 响应编码
```java
服务端字符流编码设置
response.setCharaceterEncoding()
客户端解读编码设置,且会自动执行setCharacterEncding()方法
response.setHeader("Content-type","text/html;charset=utf-8")
setContentType("text/html;charset=utf-8)。
```
### 请求编码

地址栏直接给出参数编码为GBK,表单或链接则与页面编码格式相同
服务器端默认使用ISO-8859-1来解码！
```java
// GET请求编码处理：
String username = new String(request.getParameter("name").getBytes("iso-8859-1"), "utf-8");
在server.xml中配置URIEncoding=utf-8(不推荐,不通用)
// POST请求编码处理：
String username = new String(request.getParameter("name").getBytes("iso-8859-1"), "utf-8");
在获取参数之前调用request.setCharacterEncoding("utf-8");
```

### URL编码

**表单的类型**：`Content-Type: application/x-www-form-urlencoded`，就是把中文转换成%后面跟随两位的16进制。
**作用**：在客户端和服务器之间传递中文时需要把它转换成网络适合的方式,防止传输过程中数据丢失

* URL编码需要先指定一种字符编码，把字符串解码后，得到byte[]，然后把小于0的字节+256，再转换成16进制。前面再添加一个%。
* POST请求默认就使用URL编码！tomcat会自动使用URL解码！
* URL编码：String username = URLEncoder.encode(username, "utf-8");
* URL解码：String username = URLDecoder.decode(username, "utf-8");

## 路径

**web.xml中`<url-pattern>`路径，（叫它Servlet路径！）**
要么以“*”开头，要么为“/”开头

**转发和包含路径**
以“/”开头：相对当前项目路径，即http://localhost:8080/项目名/
不以“/”开头：相对当前Servlet路径。http://localhost:8080/项目名/servlet/

**重定向路径（客户端路径）**
以“/”开头：相对当前主机，例如：http://localhost:8080/，　所以需要自己手动添加项目名，例如；response.sendRedirect("/day10_1/Bservlet");

**页面中超链接和表单路径**
与重定向相同，都是客户端路径！需要添加项目名
`<form action="/day10_1/AServlet">`
`<a href="/day10_/AServlet">`
`<a href="AServlet">`
如果不以“/”开头，那么相对当前页面所在路径。如果是http://localhost:8080/day10_1/html/form.html。　即：http://localhost:8080/day10_1/html/ASevlet
建立使用以“/”开头的路径，即绝对路径！

**ServletContext获取资源路径**
以不以"/"开头都是相对当前项目目录，即当然index.jsp所在目录

**ClassLoader获取资源路径**
相对classes目录(不能以"/"开头)

**Class获取资源路径**
以“/”开头相对classes目录
不以“/”开头相对当前.class文件所在目录。

#### servlet细节

- 不要在Servlet中创建成员！创建局部变量即可！
- 可以创建无状态成员！(无论谁执行都不会变化)
- 可以创建有状态的成员，但状态必须为只读(无法修改)的！