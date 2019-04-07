# SpringMVC

## 基本概念

### 执行过程

**执行步骤**

1. 发起请求到前端控制器(DispatcherServlet)
2. 前端控制器请求HandlerMapping查找 Handler
   - 可以根据xml配置、注解进行查找
3. 处理器映射器HandlerMapping向前端控制器返回Handler
4. 前端控制器调用处理器适配器HandlerAdapter去执行Handler
5. 处理器适配器去执行Handler
6. Handler执行完成给适配器返回ModelAndView
7. 处理器适配器向前端控制器返回ModelAndView
   - ModelAndView是springmvc框架的一个底层对象，包括 Model和view
8. 前端控制器请求视图解析器ViewReslover去进行视图解析
   - 根据逻辑视图名解析成真正的视图(jsp)
9. 视图解析器向前端控制器返回View
10. 前端控制器进行视图渲染
    - 视图渲染将模型数据(在ModelAndView对象中)填充到request域
11. 前端控制器向用户响应结果 


## 配置使用

### 配置DispatcherServlet（前端控制器），web.xml内添加

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
     <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!-- 配置映射，支持RESTful，需额外配置静态文件 -->
    <url-pattern>/</url-pattern> 
</servlet-mapping>
```

### 导入MVC命名空间约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">
```

### 非注解配置处理器适配器和映射器。（一般不用）

```xml
<!-- 配置处理器适配器 -->
<!-- 简单的控制器处理器适配器，它支持所有实现了Controller接口的Handler控制器 -->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />

<!--  http 请求处理器适配器，它要求编写的Handler需要实现HttpRequestHandler接口 -->
<bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter" />

<!-- 配置处理器映射器，多个映射器可以并存 -->
<!-- BeanNameUrlHandlerMapping,根据对象的名字来进行处理器映射，此时该对象要继承AbstractController 实现handlerRequestInternal方法；对请求的处理主要在该方法中完成 -->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />
<!-- 对应Handler的配置，对象配置时的name属性必须为 "/xxx.action"的形式，即为访问的URL名-->
<bean name="/index.action" class="com.lh.controller.testController"></bean>

<!-- SimpleUrlHandlerMapping，简单url映射,可以统一配 -->
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="mappings">
        <props>
            <prop key="/items1.action">controller的bean id</prop>
            <prop key="/items2.action">controller的bean id</prop>
        </props>
    </property>
</bean>

<!-- ControllerClassNameHandlerMapping，对应Handler的类名必须为xxxController，访问地址为输入/xxxController或 /xxxcontroller或/xxx -->
<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping"></bean>
```

### 使用注解配置（常用方式）

```xml
<!-- 默认加载注解适配器和映射器 -->
<mvc:annotation-driven /> 
<!-- 指定需要扫描的包,省去配置用注解的bean类 -->
<context:component-scan base-package="spring.controller"></context:component-scan>
```

### 配置视图解析器

```xml
<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- JstlView表示JSP模板页面需要使用JSTL标签库，所以classpath中必须包含jstl -->
	<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
	<!-- 配置前后缀，在写jsp路径时可以省略前后缀部分 -->
    <property name="prefix" value="/WEB-INF/jsp/"/>
	<property name="suffix" value=".jsp"/>
</bean>
```

### 文件上传

```xml
<!-- 文件上传配置,上传拦截，如最大上传值及最小上传值 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >   
	<property name="maxUploadSize" value="104857600"></property>   
	<property name="maxInMemorySize" value="4096"></property>   
	<property name="defaultEncoding" value="utf-8"></property> 
</bean>  
```

### 静态资源

```xml
<!-- 静态文件地址配置,所有mapping都到location里进行查找 -->
<mvc:resources location="upload/img/" mapping="/upload/img/**" />
<mvc:resources location="static_resources/css/" mapping="/styles/**" />   
```

## 注解

**@Controller**：标识Handler类，修饰类

**@RestController** ：Spring4之后新加入的注解，原来返回json需要@ResponseBody和@Controller配合。即@RestController是@ResponseBody和@Controller的组合注解。

@RequestBody：将页面的json字符串转成pojo对象。

**@ResponseBody** ：注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据，需要注意的呢，在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。

```java
@RequestMapping("/login")
@ResponseBody
public User login(User user){
	return user;
}
// 效果等同于如下代码：
@RequestMapping("/login")
public void login(User user, HttpServletResponse response){
	response.getWriter.write(JSONObject.fromObject(user).toString());
}
```

**@RequestMapping**(value="/item")或@RequestMapping("/item）作用于类和方法，标识映射URL，带/为绝对路径(推荐)

- value的值是数组，可以将多个url映射到同一个方法或类。

- method的值为RequestMethod.GET/POST，多值用{,}


**@PathVariable()**：为支持REST，用于匹配RequestMapping中的{xxx}占位符参数

**@RequestParam**(value,required) 把request的参数绑定到方法的形参上

- value是匹配传递的参数名，等效于Request.getParamrter()

- required默认为true,表示必须包含对应的参数


**@ModelAttribute(value)** 

**作用于方法：**

- 该方法会在此controller每个方法执行前被执行。
- 如果方法有返回值，则使用Model的addAttribute添加该返回值为属性。
- 属性名为value值，如果没有value值则为首字母小写的类型名。

如下所示和**@RequestMapping**一起注释一个方法时，方法返回值不在表示视图名称，视图名称由RequestToViewNameTranslator根据请求"/helloWorld.do"转换为逻辑视图helloWorld。并且执行了model.addAttribute("attributeName","hi")把返回值存进了model里。

```java
1 @Controller 
2 public class HelloWorldController { 
3     @RequestMapping(value = "/helloWorld.do") 
4     @ModelAttribute("attributeName") 
5     public String helloWorld() { 
6          return "hi"; 
7     } 
8 }
```

**作用于形参：**

会将客户端传递过来的参数按名称注入到指定对象中，并且会将这个对象自动加入ModelMap中，便于View层使用。

**@SessionAttributes**("userSession")只能修饰类，可以将Model中的属性同步到session当中，通过匹配参数名或者类型

- String[] value：要保存到session中的参数名称
- Class[] typtes：要保存的参数的类型，和value是并集关系，即互相独立起作用。

**@RequestParam()**自定义绑定，把请求里的参数绑定到形参里

- value：传入key/value的key，即参数名称
- required：true必须传此参数，否则报400 Required Integer parameter 'XXXX' is not present
- defaultValue：默认值，没同名参数时的默认值

## 参数绑定

### 默认支持参数类型(直接写在方法形参就可使用)

- HttpServletRequest获取请求信息
- HttpServletResponse处理响应信息
- HttpSession得到session中存放的对象
- Model/ModelMap向页面传递数据（底层就是写入request域）
- RedirectAttributes用于重定向存储数据

### 简单类型绑定

- 基本数据类型的值，请求的参数名和形参名一致可传值
- pojo绑定(和简单类型绑定同时生效,即同名两个都获得值)
  - 形参pojo的属性名和请求参数名相同即可传值
  - 请求参数名为item.name则匹配，某个pojo形参的item属性的name属性
  - pojo绑定的同时会注入pojo到request域，基础类型则不会。
- 数组绑定：页面多个参数name相同，形参同名数组类型即可
- list绑定：页面参数名写成：items[0].name的形式
- map绑定：页面参数名写成：items['name']的形式

### 自定义Converter

配置方式

```xml
<!-- conversion-service属性填写自定义的转换器服务的bean的id -->
<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
<!-- conversionService转换器服务，多个转换器配置在list里 -->
<bean id="conversionService"	class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
	<!-- 转换器 -->
	<property name="converters">
		<list>
			<bean class="cn.itcast.ssm.controller.converter.CustomDateConverter"/>
		</list>
	</property>
</bean>
```

编写自定义Converter类

```java
// 接口的泛型就是需要转换的类型，一般是String转成目标类型
public class CustomDateConverter implements Converter<String, Date> {
    @Override
    public Date convert(String source) {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            return simpleDateFormat.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

原理分析：页面的参数名用来匹配形参的参数名，然后会根据形参的具体类型去自动匹配对应的转换器（如果有的话），把转换后的结果注入到变量中。

## 异常处理

### 局部异常处理

@ExceptionHandler：定义在Controller内部，作用于异常处理方法上.

例:

```java
@ExceptionHandler(value={IOException.class,SQLException.class})  
public String exp(Exception ex,HttpServletRequest request) {  }
```

 

### 全局异常处理

1. 实现HandlerExceptionResolver接口，并配bean或者注解@Component就可以生效。

2. 配置SimpleMappingExceptionResolver

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">  
    <!-- 定义默认的异常处理页面，默认异常会跳转到error.jsp -->  
    <property name="defaultErrorView" value="error"></property>  
    <!-- 定义异常处理页面用来获取异常信息的变量名，默认名为exception -->  
    <property name="exceptionAttribute" value="ex"></property>  
    <!-- 定义需要特殊处理的异常，用类名或完全路径名作为key，异常也页名作为值 -->  
    <property name="exceptionMappings">  
        <props>  
            <prop key="IOException">error/ioexp</prop>  
            <prop key="java.sql.SQLException">error/sqlexp</prop>  
        </props>  
    </property>  
</bean>
```

## 上传文件

需要jar包：commons-fileupload/io

前台form属性enctype="multipart/form-data"

### 配置CommonsMultipartResolver

```xml
<!-- 文件上传配置,上传拦截，如最大上传值及最小上传值 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" >   
	<property name="maxUploadSize" value="104857600"></property>   
	<property name="maxInMemorySize" value="4096"></property>   
	 <property name="defaultEncoding" value="utf-8"></property> 
</bean>  
```

### 编写Controller方法

```java
public String toRegister(HttpServletRequest request, @RequestParam("pic") MultipartFile pic) {
	String filePath = request.getSession().getServletContext().getRealPath("upload/img");
	String picName = UUID.randomUUID()+ pic.getOriginalFilename().substring(pic.getOriginalFilename().lastIndexOf('.'));
	File file = new File(filePath + File.separator + picName);
	try {
		pic.transferTo(file);
	} catch (IllegalStateException | IOException e) {
		e.printStackTrace();
	}
	return "register";
}
```

### MultipartFile接口

方法:

- String getOriginalFileName() 返回文件名.后缀
- void transferTo(File file) 把文件存储到file里

### 虚拟目录

在tomcat上配置图片虚拟目录，在tomcat下conf/server.xml中添加：

```xml
<Context docBase="F:\develop\upload\temp" path="/pic" reloadable="false"/>
```

访问`http://localhost:8080/pic`即可访问`F:\develop\upload\temp`下的图片

## 拦截器

### 实现HandlerInterceptor接口

```java
public class HandlerInterceptor1 implements HandlerInterceptor {
	//进入 Handler方法之前执行
	//用于身份认证、身份授权
	//比如身份认证，如果认证通过表示当前用户没有登陆，需要此方法拦截不再向下执行
	@Override
	public boolean preHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler) throws Exception {
		//return false表示拦截，不向下执行
		//return true表示放行
		return false;
	}
	//进入Handler方法之后，返回modelAndView之前执行
	//应用场景从modelAndView出发：将公用的模型数据(比如菜单导航)在这里传到视图，也可以在这里统一指定视图
	@Override
	public void postHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
	}
	//执行Handler完成执行此方法
	//应用场景：统一异常处理，统一日志处理
	@Override
	public void afterCompletion(HttpServletRequest request,
			HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
	}
}
```

 

### 执行顺序

1. preHandle按拦截器定义顺序调用
2. postHandler按拦截器定义逆序调用
3. afterCompletion按拦截器定义逆序调用
4. preHandle在拦截器链内它之前的所有拦截器方法返回true时调用。
5. postHandler在拦截器链内它之前的所有拦截器方法返回true时调用。
6. afterCompletion只要其自身的preHandle返回true就调用，反之返回false或不执行时都不调用。

 

 

### 配置拦截器

方式一(不推荐)

```xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
    <property name="interceptors">
        <list>
            <ref bean="handlerInterceptor1"/>
            <ref bean="handlerInterceptor2"/>
        </list>
    </property>
</bean>
<bean id="handlerInterceptor1" class="springmvc.intercapter.HandlerInterceptor1"/>
<bean id="handlerInterceptor2" class="springmvc.intercapter.HandlerInterceptor2"/>
```

方式二

```xml
<!--拦截器 -->
<mvc:interceptors>
    <!--多个拦截器,顺序执行 -->
    <mvc:interceptor>
        <!--匹配mapping的才会被拦截器拦截-->
        <mvc:mapping path="/**"/>
        	<bean class="cn.itcast.springmvc.filter.HandlerInterceptor1"></bean>
    </mvc:interceptor>
    <mvc:interceptor>
    	<mvc:mapping path="/**"/>
        <bean class="cn.itcast.springmvc.filter.HandlerInterceptor2"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

