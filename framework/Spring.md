# Spring

Spring是一个轻量级控制反转(IoC)和面向切面(AOP)的容器框架。

## IoC理论

控制反转（Inversion of Control）是将组件对象的控制权从代码本身转移到外部容器的一种设计思想，主要作用是用来降低耦合度。其主要实现方式就是依赖注入（Dependency Injection，简称DI）。所谓依赖注入，就是由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中。

### IoC底层原理

- dom4j(导入并解析xml文件，获取类名)
- 反射机制（通过类名创建对象实例）
- 工厂模式（动态的注入对象依赖）


### IoC属性注入方式

- 属性注入(Setter方法注入)：灵活性好，但setter方法数量较多，时效性差，通过无参构造实例化
- 构造函数注入：灵活性差，重载限制太多，时效性好，通过匹配的构造方法实例化
- 工厂方法注入：不推荐使用，需要额外的类和代码

### Spring的bean管理

#### xml配置bean创建

使用无参构造创建bean对象

```xml
<bean id="bean1" class="me.wodahsorez.bean.Bean1"></bean>
```

使用工厂的静态方法创建bean对象

```xml
<!-- getBean1为静态方法，返回Bean1的实例对象 -->
<bean id="bean2" class="me.wodahsorez.bean.Bean2Factory" factory-method=“getBean2” ></bean>
```

使用工厂的实例方法创建bean对象

```xml
<!-- 先创建工厂类的实例 -->
<bean id="bean3Factory" class="me.wodahsorez.bean.Bean3Factory"></bean>
<!-- factory-bean写工厂类的实例的id，factory-method写工厂类的实例方法 -->
<bean id="bean3" factory-bean="bean3Factory" factory-method=“getBean3”></bean>
```

#### Spring的属性注入

构造注入

```xml
<bean id="User" class="me.wodahsorez.bean.User">
	<!-- 通过匹配构造方法的参数名匹配 -->
    <constructor-arg name="name" value="张三" ></constructor-arg>
    <!-- 通过index索引匹配参数 -->
    <constructor-arg index="2" value="23" ></constructor-arg>
    <!-- ref注入对象类型的属性，值为对象实例的id，type为属性的类型全路径 -->
    <constructor-arg type="me.wodahsorez.bean.Role" ref="role1"></constructor-arg>
</bean>
```

因为一个bean代表一个对象，所以只需配一个构造方法，其他构造方法需要另配一个bean。

setter方法注入（常用）

```xml
<bean id="User" class="me.wodahsorez.bean.User">
	<property name="name" value="张三"></property>
    <!-- ref注入对象类型的属性，值为对象实例的id -->
    <property name="role" ref="role1"></property>
    <!-- name值可以通过.来选择对象属性内部的属性 -->
    <property name="role.name" value="rolename"></property>
</bean>
```

## AOP理论

AOP(Aspect-Oriented Programming)面向切面编程，是一种通过预编译方式和运行期动态代理实现在不修改源代码的情况下给程序动态添加功能的技术。

Spring采用动态代理织入，AspectJ采用编译期织入和类装载织入



### AOP相关术语

- 增强（Advice）(给目标对象增加功能，即织入的代码，同时它拥有执行点的方位)

- 切入点（Pointcut）(匹配连接点的规则，指出哪些方法需要增强)

- 连接点（Joinpoint）(实际匹配到切入点的方法加方位信息的具体程序执行点)

- 切面（Aspect）(增强和切入点组合起来就是切面，既包括对逻辑的定义，又包括连接点的定义)

- 代理（Proxy）(目标类被aop增强后产生的结果类，称为代理类)

- 目标对象（Target）(得到增强的目标类)

- 织入（Weaving）(增强的这个动作过程)

- 引介（Introduction）(类级别增强，为类动态添加接口实现逻辑)


 连接点是所有能够被增强的方法，而切入点是规则指定要增强的方法。



#### 增强处理类型:

**Before**：前置增强处理，在目标方法前织入增强处理

**AfterReturning**：后置增强处理，在目标方法正常执行（不出现异常）后织入增强处理

**AfterThrowing**：异常增强处理，在目标方法抛出异常后织入增强处理

**After**：最终增强处理，不论方法是否抛出异常，都会在目标方法最后织入增强处理

**Around**：环绕增强处理，在目标方法的前后都可以织入增强处理

[不同增强处理类型执行顺序](http://www.xuebuyuan.com/2116977.html)

```
不同通知的执行顺序：
    @Around环绕通知，方法前...
    @Before前置通知
    执行对象方法...
    @Around环绕通知，方法后...
    @AfterReturning后置通知
    @After最终通知 执行...
    @AfterThrowing异常通知，程序出现异常了吗？
    退出方法...
相同通知的执行顺序是：
	从上向下
```



### 两种动态代理

#### JDK动态代理

必须实现接口,才能使用

njava.lang.reflect.InvocationHandler

njava.lang.reflect.Proxy

#### CGLib动态代理

通过利用规则直接生成对应的子类代理.class，所以可以不需要实现接口，不能对final类代理。

添加CGLIB库，并在spring配置中加入`<aop:aspectj-autoproxy proxy-target-class="true"/>`



### 切入点的表达式

```java
execution(<修饰符模式>? <返回类型模式> <方法名模式>(<参数模式>) <异常模式>?)  //?为可选项
//参数模式:如果入参类型为java.lang包下的类,可以直接使用类名,否则必须使用全限定名
//*代表类型任意，..代表包及其子包，.*(..)表示方法名任意且参数任意
execution(public * *(..) )  任意public方法
execution(* *To(..))  任意以To为后缀的方法
execution(* com.smart.Waiter.*(..))  所有com.smart.Waiter接口定义的方法
execution(* com.smart.Waiter+.*(..))  所有com.smart.Waiter接口和其实现类的方法
execution(* com.smart.*.*(..))  所有com.smart包下的类的所有方法
execution(* com.smart..*.*(..))  所有com.smart包下及其子包下的类的所有方法
execution(* com..*.*Dao.find*(..))  所有com包下及其子包下以Dao为后缀的类的以find为前缀的方法
execution(* joke(String,int))  匹配名为joke,入参类型为(String,int)的方法
execution(* joke(String,*))  匹配名为joke,第一个入参为String,后一个任意的方法,但有且仅有两个入参
execution(* joke(String,..))  匹配名为joke,第一个入参为String,后面可以有任意个(包括零)任意类型的入参
execution(* joke(Object+))  匹配名为joke,入参类型为Object或其子类
```

## Spring事务

### 配置事务管理器

```xml
<bean id="transactionManager" 
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource" />
</bean>
```

#### 支持的事务管理器：

- JDBC或MyBatis：org.springframework.jdbc.datasource.DataSourceTransactionManager
- Hibernate：org.springframework.orm.hibernate5.HibernateTransactionManager
- Jpa：org.springframework.orm.jpa.JpaTransactionManager
- Jdo：org.springframework.orm.jdo.JdoTransactionManager
- JTA：org.springframework.transaction.jta.JtaTransactionManager

### 声明式事务xml配置

```xml
<!--事务增强 -->
<tx:advice id="txAdvice" transaction-manager="txManager">
	<tx:attributes>
		<!--事务属性定义 -->
		<tx:method name="get*" read-only="false" />
		<tx:method name="add*" rollback-for="PessimisticLockingFailureException" />
		<tx:method name="update*" />
	</tx:attributes>
</tx:advice>
<!--使用强大的切点表达式语言轻松定义目标方法 -->
<aop:config>
	<!--通过aop定义事务增强切面 -->
	<aop:pointcut id="serviceMethod" expression="execution(* com.yyq.service.*Forum.*(..))" />
	<!--引用事务增强 -->
	<aop:advisor pointcut-ref="serviceMethod" advice-ref="txAdvice" />
</aop:config>
```



## Spring容器的启动配置

在web.xml里配置Listener，代码如下：  

```xml
<listener>   
    <listener-class> org.springframework.web.context.ContextLoaderListener </listener-class>listener-class>
</listener>
```

如果在web.xml里给该Listener指定要加载的xml，如：

```xml
<context-param>
    <param-name>contextConfigLocation</param-name> 
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

则会去加载相应的xml，而不会去加载/WEB-INF/下的applicationContext.xml。

但是，如果没有指定的话，默认会去/WEB-INF/下加载applicationContext.xml。

注意：这里的contextConfigLocation内是对整个Spring有感知的，但是@controller会无法被MVC识别，而MVC的contextConfigLocation内只对MVC有用二者范围有区别.

### 路径写法

```
/WEB-INF/*-context.xml 
com/mycompany/**/applicationContext.xml  
file:C:/some/path/*-context.xml
classpath:com/mycompany/**/applicationContext.xml  
```

1. 会查找到WEB-INF目录下的以"-context.xml"结尾的文件  在WEB-INF下的 a-context.xml b-context.xml都会被找到

2. com/mycompany/目录下所有的applicationContext.xml都会被找到

3. file 表示会根据文件系统的路径查找 这个条会找到 c盘下的/some/path目录以"-context.xml"的文件都会被找到

4. 查找classpath下的com/mycompany/包中所有子包的applicationContext.xml文件

5. classpath：只会到你的class路径中查找找文件; 

6. classpath*：不仅包含class路径，还包括jar文件中(class路径)进行查找.

有时候会用模糊匹配的方式配置多配置文件。但是如果配置文件是在jar包里，模糊匹配就找不到了。可以用**逗号或空格**隔开的方式配置多个配置文件。或者配置文件在jar文件根目录除外的其他任何地方，然后根据路径名称模糊匹配即可找到。

没有资源前缀默认路径相对于WEB的部署根路径，Ant风格的匹配符：?匹配一个字符,*匹配任意字符,**匹配多层路径

参考：[spring配置文件路径你知多少](http://name327.iteye.com/blog/1628884)