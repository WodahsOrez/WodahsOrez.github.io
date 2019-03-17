# 过滤器和监视器

## 过滤器

javax.servlet.Filter接口

它会在一组资源（jsp、servlet、.css、.html等等）的前面执行，它可以让请求得到目标资源，也可以不让请求达到，过滤器有拦截请求的能力

### 过滤器如何编写

#### 实现javax.servlet.Filter接口(单例)

方法:

```java
void init(FilterConfig)
// 创建之后，马上执行；Filter会在服务器启动时就创建！
void destory()
// 销毁之前执行！在服务器关闭时销毁
void doFilter(ServletRequest,ServletResponse,FilterChain)
// 每次过滤时都会执行
```

#### 配置web.xml

```xml
<filter>
  <filter-name>xxx</filter-name>
  <filter-class>cn.itcast.web.filter.AFitler</fitler-class>
</filter>
<fitler-mapping>
  <filter-name>xxx</filter-name>
  <url-pattern>/*</url-pattern>
  <!-- <servlet-name>myservlet</servlet-name> 配置servlet为目标资源 -->
</filter-mapping>
```

##### FilterConfig-->与ServletConfig相似

- 获取初始化参数：getInitParameter()
- 获取过滤器名称：getFilterName()
- 获取appliction：getServletContext()
- 获取所有初始化参数的名称: Enumeration getInitParameterNames()

##### FilterChain(链条)

doFilter(ServletRequest, ServletResponse)：放行，执行目标资源，或是执行下一个过滤器

 

### 多过滤器

#### 过滤器的四种拦截方式

```xml
<dispatcher>REQUEST</dispatcher>默认的,请求(直接访问)
<dispatcher>FORWARD</dispatcher>转发
<dispatcher>INCLUDE</dispatcher>包含(动态包含时才触发即<JSP:include/>)
<dispatcher>ERROR</dispatcher>通过<%@ pgge errorPage="error.jsp">这种方式指定的页面
```

在`<filter-mapping>`中的最后进行配置。

#### 多个过滤器的执行顺序

`<filter-mapping>`的配置先后顺序决定了过滤器的执行顺序！

## 监视器

### 事件源：三大域

#### ServletContext

生命周期监听：ServletContextListener

- void      contextInitialized(ServletContextEvent sce)：创建Servletcontext(后)时
- void      contextDestroyed(ServletContextEvent sce)：销毁Servletcontext(前)时

属性监听：ServletContextAttributeListener

- void      attributeAdded(ServletContextAttributeEvent event)：添加属性时；
- void      attributeReplaced(ServletContextAttributeEvent event)：替换属性时；
- void      attributeRemoved(ServletContextAttributeEvent event)：移除属性时；

#### HttpSession

生命周期监听：HttpSessionListener

void sessionCreated(HttpSessionEvent se)：创建session(后)时

- void      sessionDestroyed(HttpSessionEvent se)：销毁session(前)时

属性监听：HttpSessioniAttributeListener

- void      attributeAdded(HttpSessionBindingEvent event)：添加属性时；
- void      attributeReplaced(HttpSessionBindingEvent event)：替换属性时
- void      attributeRemoved(HttpSessionBindingEvent event)：移除属性时

#### ServletRequest

生命周期监听：ServletRequestListener

- void      requestInitialized(ServletRequestEvent sre)：创建request(后)时
- void      requestDestroyed(ServletRequestEvent sre)：销毁request(前)时

属性监听：ServletRequestAttributeListener

- void      attributeAdded(ServletRequestAttributeEvent srae)：添加属性时
- void      attributeReplaced(ServletRequestAttributeEvent srae)：替换属性时
- void      attributeRemoved(ServletRequestAttributeEvent srae)：移除属性时

 

### 事件对象

- ServletContextEvent：ServletContext getServletContext()

- HttpSessionEvent：HttpSession getSession()

- ServletRequest：

  - ServletContext      getServletContext()；
  - ServletReques      getServletRequest()；

- ServletContextAttributeEvent：

  - ServletContext      getServletContext()；
  - String getName()：获取当前操作的属性名
  - Object getValue()：获取当前操作的属性值

- HttpSessionBindingEvent：

  - String getName()：获取当前操作的属性名；
  - Object getValue()：获取当前操作的属性值；
  - HttpSession      getSession()：获取当前操作的session对象。

- ServletRequestAttributeEvent ：

  - String getName()：获取当前操作的属性名；
  - Object getValue()：获取当前操作的属性值；
  - ServletContext      getServletContext()：获取ServletContext对象；
  - ServletRequest      getServletRequest()：获取当前操作的ServletRequest对象。

 

#### 关于属性监听中event的getValue的结果的分析:

- 添加:getValue获取这次添加的值                     

- 替换:getValue获取替换前的值,两次值相同也当成是替换

- 移除:getValue获取移除的值


 

### 感知监听（都与HttpSession相关）

它用来添加到JavaBean上，而不是添加到三大域上，这两个监听器都不需要在web.xml中注册。

#### HttpSessionBindingListener接口

javabean实现该接口，javabean就知道自己是否添加到session中了。

- public void valueBound(HttpSessionBindingEvent event)：当把监听器对象添加到session中会调用监听器对象的本方法；

- public void valueUnbound(HttpSessionBindingEvent event)：当把监听器对象从session中移除时会调用监听器对象的本方法；


#### HttpSessionActivationListener接口

javabean实现该接口，javabean就知道自己是否随session钝化活化了.且要实现serializable接口,否则无法序列化,将会被从session中移除

- public void sessionWillPassivate(HttpSessionEvent se)：当对象感知被活化时调用本方法；

- public void sessionDidActivate(HttpSessionEvent se)：当对象感知被钝化时调用本方法；


**注意**：`<listener>`标签一般配置在`<servlet>`标签前面,多个时按配置顺序执行