# Spring-xml配置

## Beans命名空间

### 导入约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" 
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
```



### `<bean>`标签

#### 属性

- **id**：Spring容器管理的bean实例的名称标识。不能包含特殊符号。
- **name**：作用和id相同，但能包含特殊符号。（一般推荐用id）。
- **class**：创建的实例的所属类的全路径。
- **scope**：bean的作用范围。(后三个仅适用WebApplicationContext环境)
  - **singleton**(**默认值**)：在Spring容器中仅存在一个共享的Bean实例,**容器启动时实例化**
  - **prototype**：每次从容器中都获取一个新的实例,**被调用时才实例化**
  - **request**：每次HTTP请求都会创建一个新的Bean实例
  - **session**：同一个HTTP请求共享一个Bean实例
  - **global session**：同一个全局Session共享一个Bean实例
- **init-method**：初始化方法。紧随对象创建后执行。
- **destroy-method**：摧毁方法。在实例被摧毁前执行。
- **lazy-init**：当值为true时，不在随容器启动而提前实例化，但当被其他需要提前实例化的bean引用时，依然会提前实例化。



### 属性注入标签

#### `<property>`标签

有属性name，value，ref。通过setter方法注入，其内可以有以下子标签

- `<value>`基本类型和字符串值，根据bean属性自动判断类型，注意字符实体
- `<null />`和空字符串`<value></value>`
- `<ref bean="userDaoImpl" />` bean可以换成parent即父容器中的bean
- `<bean class="cn.bdqn.biz.dao.impl.UserDao" />`  内部bean，不能被外部引用，不写id



##### 集合类型的标签，都是property的子标签

`<list>`，`<set>`：其内可容纳的标签和property一样

`<map>`内部由`<entry>`对组成

```xml
<entry key="键名" key-ref="引用型键" value="基础类型值" value-ref="引用类型值" />
<entry key="0">
	<value>值</value>
<entry>
```

`<props>`键值都必须是String类型，其内部子标签是若干`<prop key="键">值</prop>`

**集合合并**：通过在集合标签内加入merge="true"属性，来使子bean继承父bean的同名属性集合元素。



### 导入外部数据

#### 导入Properties文件

```xml
<!--1.使用传统的PropertyPlaceholderConfigurer引用属性文件  -->
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" p:fileEncoding="utf-8">
    <property name="locations">
        <list>
              <value>classpath:com/smart/placeholder/jdbc.properties</value>
        </list>
    </property>
</bean>

<!--2.使用context命名空间的配置引用属性文件  -->
<context:property-placeholder location="classpath:com/smart/placeholder/jdbc.properties" file-encoding="utf8"/>
```

properties文件内可以引用自身文件内的属性值,如:

```properties
dbName=sampledb
url=jdbc:mysql://localhost:3306/${dbName} 
```

属性引用的语法

- xml配置:${属性名}，#{beanName.beanProp} 引用配置的bean的属性值或方法的返回值
- 基于java类：@Value(${属性名}，@Value(#{beanName.beanProp})

 

#### context:property-placeholder详解 

属性:

1. location="属性文件，多个之间逗号分隔,或者可以使用通配符"  

2. file-encoding="文件编码"  

3. ignore-resource-not-found="是否忽略找不到的属性文件，如果不忽略，找不到将抛出异常"  

4. ignore-unresolvable="是否忽略解析不到的属性，如果不忽略，找不到将抛出异常."

5. properties-ref="Spring内部Properties对象的Bean名" ,例如XML中配置的java.util.Properties对象

6. local-override="是否本地覆盖模式，即如果true，那么properties-ref的属性将覆盖location加载的属性，否则相反"  

7. system-properties-mode="系统属性模式"

   - ENVIRONMENT(默认):通过当前环境和本地properties解析占位符
   - NEVER:只通过本地properties,不通过系统环境参数
   - OVERRIDE:先系统环境参数后本地properties
   - FALLBACK:先本地properties后系统环境参数

8. order="顺序"  多个该标签配置之间的检索顺序

  

#### 配置多个context:property-placeholder的方法

由于一般配置多个只会有一个生效，除了在一个里配置多个**properties**文件之外，就需要做如下配置:

```xml
<context:property-placeholder location=*"classpath:config.properties"* ignore-unresolvable=*"true"* order=*"1"* />
<context:property-placeholder location=*"classpath:config2.properties"* ignore-unresolvable=*"false"* order=*"2"* /> 
```

1. order属性决定多个配置时的加载先后顺序,数字越小越先加载

2. 当spring加载的`<context:property-placeholder>`的ignore-unresolvable属性为false，则停止扫描剩余的`<context:property-placeholder>`，反之,则继续扫描.

3. 由于ignore-unresolvable="true"时会忽略解析不到的属性，无法报错，所以我们需要在**最后加载**的`<context:property-placeholder>`的ignore-unresolvable属性设为false。

4. 多个配置文件中如果有属性名相同的情况，则后加载的会覆盖先加载的。

5. PropertyPlaceholderConfigurer和<context:property-placeholder>是等效的，以上的规则对其也适用，有时错误产生的原因就在PropertyPlaceholderConfigurer的配置上。

## context命名空间

### 导入约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">
```

### 开启注解扫描

```xml
<!-- 开启注解扫描，扫描指定package内的类，属性，方法上的注解 -->
<context:component-scan base-package="me.wodahsorez">
	<!-- 加入需要扫描的注解 -->
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/> 
    <!-- 排除不需要扫描的类 -->
    <context:exclude-filter type="regex" expression=".service.*"/> 
</context:component-scan>
<!-- 启用已经被注册的bean的属性上的注解，component-scan也包含该功能 -->
<context:annotation-config></context:annotation-config>
```

#### component-scan的属性：

- base-package：扫描的包路径。

- annotation-config：默认为true。即开启属性注解，等效于annotation-config。

- resource-pattern：对资源进行筛选的正则表达式。具体细分在include-filter与exclude-filter中进行。

- use-default-filters：默认为true。即会对@Component，@Controller，@Service，@Reposity等注解扫描。若值为false，则这些注解都不被扫描，需要配合include-filter指定需要的注解。即作用相当于全选和反选。

- 先排除黑名单,再加入白名单

#### include-filter与exclude-filter的type属性：

- annotation：注解类型
- assignable_type：annotation：指定的类型
- aspectj：按照Aspectj的表达式，基本上不会用到
- regex：按照正则表达式
- custom：自定义规则


## p命名空间

### 导入约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/p http://www.springframework.org/schema/p/spring-p.xsd">
```

### 使用

- 特点是使用属性的形式而不是子元素配置Bean的属性
- 对于直接量(基本数据类型、字符串)属性：p:属性名="属性值"
- 对于引用Bean的属性：p:属性名-ref="Bean的id"，如：

```xml
<bean id="userDaoImpl" class="cn.bdqn.biz.dao.impl.UserDaoImpl" p:dao="123" p:dao-ref="userDao"/>
```

## Aop命名空间

### 导入约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop“ xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
```

### 配置切面

```xml
<aop:config>
    <!-- 配置切入点，expression写匹配方法的规则 -->
	<aop:pointcut id="servicePointcut" expression="execution(public * com.pb.service.*.*(..))" />
	<!-- ref指定增强方法所在类的bean的id -->
    <aop:aspect ref="serviceLogging">
		<!-- method为增强方法的名称，pointcut—ref指要织入增强的切点的id -->
		<aop:before method="beforeService" pointcut-ref="servicePointcut" />
		<!-- returning为增强方法的形参名，会把被增强方法的返回值传给该形参 -->
        <aop:after-returning method="afterReturning" pointcut-ref="servicePointcut" returning="returnVal" />
		<!-- throwing为增强方法的形参名，会把被增强方法的异常传给该形参 -->
        <aop:after-throwing method="afterThrowing" pointcut-ref="servicePointcut" throwing="ex" />
		<aop:after method="after" pointcut-ref="servicePointcut" />
		<aop:around method="around" pointcut-ref="servicePointcut" />
	</aop:aspect>
</aop:config>
```

### 其他配置

```xml
<!-- 开启aop自动代理，false默认使用JDK动态代理，true默认使用CGLib动态代理 -->
<aop:aspectj-autoproxy proxy-target-class="false"/>
```

## tx命名空间

### 导入约束

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd">
```

### 增强事务

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
```

启用事务注解

```xml
<tx:annotation-driven transaction-manager="txManager"/>
```

