# Mybatis

## 原理

1. 配置基础配置XML，如SqlMapConfig.xml

2. 配置映射器XML，如mapper.xml

3. 解析XML，SqlSessionFactoryBuilder构建出SqlSessionFactory对象

4. SqlSessionFactory产生SqlSession对象

5. SqlSession下的四大对象
    - Executor：执行器，由它统一调度其他三个对象来执行对应的SQL；
    - StatementHandler：使用数据库的Statement执行操作；
    - ParameterHandler：用于SQL对参数的处理；
    - ResultHandler：进行最后数据集的封装返回处理；

```java
// 配置文件
String resource = "SqlMapConfig.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
// 使用SqlSessionFactoryBuilder从xml配置文件中创建SqlSessionFactory
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
// 创建数据库会话实例sqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();
// 查询单个/多个记录，根据用户id查询用户信息(namespace.select的id)
User user = sqlSession.selectOne("test.findUserById", 10);
List<User> list = sqlSession.selectList("test.findUserByName","小明");
// 插入user用户的信息
sqlSession.insert("test.insertUser", user);
// 删除用户
sqlSession.delete("test.deleteUserById",18);
// 更新用户
sqlSession.update("test.updateUser", user);
// 提交事务
sqlSession.commit();
```



### 原始DAO开发

- 编写DAO接口类

- 编写DAO实现类，实现类注入成员属性sqlSessionFactory，方法结构如下


```java
@Override
public User findUserById(Integer id) {
    SqlSession sqlSession = sqlSessionFactory.openSession();
    User user = sqlSession.selectOne("test.findUserById", id);
    // 释放资源
    sqlSession.close();
    return user;
}
```

缺点：由于SQLSession是线程不安全的，所以必须放在方法内，但同时就导致方法内有很多重复冗余的代码。



### mapper代理开发

1. 编写mapper.xml
2. 编写mapper接口（类似于DAO接口）
3. 通过SqlSession.getMapper(接口名.class)获取实现了该接口的代理类实例，即DAO实例。
4. 两者需要遵循一些规范才能生效

    - Mapper.xml文件中的namespace与mapper接口的类全限定名相同
    - Mapper接口方法名和Mapper.xml中定义的每个statement的id相同 
    - Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同
    - Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

## 基础配置XML

### 根元素configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD SQL Map Config 3.0//EN"  
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
</configuration>
```



### 配置Properties

```xml
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

加载顺序：

- 先加载`<properties>`内的`<property>`的属性
- 然后加载resource/url指定的文件内的属性，并覆盖已读同名属性
- 最后读取parameterType传进来的属性值，并覆盖已读同名属性

使用：配置了的属性可以用`${属性名}`来调用，包括mapper.xml里也能使用



### 配置settings

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
</settings>
```

参考：[settings全部属性](http://www.mybatis.org/mybatis-3/zh/configuration.html#settings)



### 配置连接池和事务管理，如果在Spring里配置则可以不写

```xml
<environments default="development">
    <environment id="development">
        <!-- 使用jdbc事务管理，事务由Mybatis控制 -->
        <transactionManager type="JDBC"></transactionManager>
        <!-- 配置数据库连接池 -->
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jsbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </dataSource>
    </environment>
</environments>
```



### 加载映射文件

```xml
<mappers>
    <!-- resource和url都能引用资源 -->
    <mapper resource="sqlmap/mapper.xml"/>
    <mapper url="file:///var/mappers/AuthorMapper.xml"/>
    <!-- 加载mapper接口，必须满足mapper代理模式规范，且xml文件名和接口类名相同，且在同一目录下-->
    <mapper class="org.mybatis.builder.PostMapper"/>
    <!-- 批量加载包内的mapper接口-->
    <package name="org.mybatis.builder"/>
</mappers>
```



### 默认别名

| 别名       | 映射的类型 |
| :--------- | :--------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

### 自定义别名

```xml
<typeAliases>
    <!-- 单个别名定义 -->
    <typeAlias alias="user" type="cn.itcast.mybatis.po.User"/>
    <!-- 批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） -->
    <package name="cn.itcast.mybatis.po"/>
    <package name="其它包"/>
</typeAliases>
```

## 在Spring里配置Mybatis

```xml
<!-- 配置mybatis -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="classpath:mybatis/mybatis-config.xml"></property>
    <!-- mapper扫描 -->
    <property name="mapperLocations" value="classpath:mybatis/*/*.xml"></property>
</bean>

<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg ref="sqlSessionFactory" />
</bean>
```



## XML映射文件

### 根元素mapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- mapper:根标签，namespace：命名空间，随便写，一般保证命名空间唯一 -->
<mapper namespace="cn.itcast.mybatis.mapper.UserMapper">
</mapper>
```



### statement配置

```xml
<!-- 添加用户 -->
<insert  id="insertUser" parameterType="cn.itcast.mybatis.po.User">
    insert into user(id,username,birthday,sex,address) values(#{id},#{username},#{birthday},#{sex},#{address})
</insert>
<!-- 删除用户 -->
<delete id="deleteUserById" parameterType="int">
    delete from user where id=#{id}
</delete>
<!-- 更新用户 -->
<update id="updateUser" parameterType="cn.itcast.mybatis.po.User">
    update user set username=#{username},birthday=#{birthday}, sex=#{sex},address=#{address} where id=#{id}
</update>
<!-- 查询用户，也可以用resultMap来映射返回值 -->
<select id="findUserById" parameterType="int" resultType="cn.itcast.mybatis.po.User">
    select * from user where id = #{id}
</select>
```



### #{}和${}

#### `#{}`占位符

- 进行java类型和jdbc类型转换
- 传值时会带`""`
- 如果parameterType传入的是pojo类型，那么`#{}`中的变量名称必须是pojo中对应的属性.属性.属性...
- 如果parameterType传入的是基础类型值，`#{}`括号中可以是value或其它名称
- 可以有效防止sql注入 

 

#### `${}`拼接符

- 不进行java类型和jdbc类型转换 
- 传值时不带`""`
- 如果parameterType传入的是pojo类型，那么`${}`中的变量名称必须是pojo中对应的属性.属性.属性...
- 如果parameterType传输单个简单类型值，`${}`括号中只能是value

 

#### 使用情景

- mybaties排序时使用order by动态参数时需要注意，使用`${}`而不用`#{}`
-  模糊查询：`LIKE CONCAT('%',#{value},'%')` 或 `LIKE '%${value}%'`



### 定义resultMap

```xml
<resultMap id="detailedBlogResultMap" type="Blog">
    <constructor>
        <idArg column="blog_id" javaType="int"/>
    </constructor>
    <result property="title" column="blog_title"/>
    <association property="author" javaType="Author">
        <id property="id" column="author_id"/>
        <result property="username" column="author_username"/>
        <result property="password" column="author_password"/>
        <result property="email" column="author_email"/>
        <result property="bio" column="author_bio"/>
        <result property="favouriteSection" column="author_favourite_section"/>
    </association>
    <collection property="posts" ofType="Post">
        <id property="id" column="post_id"/>
        <result property="subject" column="post_subject"/>
        <association property="author" javaType="Author"/>
        <collection property="comments" ofType="Comment">
            <id property="id" column="comment_id"/>
        </collection>
        <collection property="tags" ofType="Tag" >
            <id property="id" column="tag_id"/>
        </collection>
        <discriminator javaType="int" column="draft">
            <case value="1" resultType="DraftPost"/>
        </discriminator>
    </collection>
</resultMap>
```

**resultMap标签**

- **id**：resultMap的标识，即statement内resultMap属性需要的值。
- **type**：映射的pojo的全限定类名，或类型别名。
- **autoMapping**：true（默认）或false。是否启动自动映射功能，true则自动查找与字段名小写同名的属性名，并调用setter方法。false则需要明确注明映射关系才会调用对应的setter方法。
- **extends**：继承指定标识的resultMap的映射



**id子标签**：用于设置主键字段和pojo属性的映射关系

**result子标签**：用于设置普通字段和pojo属性的映射关系

- **property**：pojo的属性名
- **column**：查询结果的字段名



**constructor标签**：使用指定参数列表的构造函数来实例化pojo。注意：其子元素顺序必须与参数列表顺序对应

- **idArg子标签**：标记该入参为主键
- **arg子标签**：标记该入参为普通字段（主键使用该子元素设置也是可以的）
  - **column**：查询结果的字段名
  - **JavaType**：入参的java类型名



**association 标签**：pojo内部pojo类型属性的映射

- **property**：该pojo类型属性的属性名
- **javaType**：该属性的pojo类型名或别名
- **resultMap**：使用指定标识的resultMap映射
- **column**：传入关联查询的字段名，多个用`,`隔开
- **select**：关联查询的statement的id



**collection标签**：对应于一对多的集合映射

- **property**：该集合属性的属性名
- **javaType**：该集合属性的java属性类名或别名
- **ofType**：该集合内存的pojo类型名或别名
- **resultMap**：使用指定标识的resultMap映射
- **column**：传入关联查询的字段名，多个用`,`隔开
- **select**：关联查询的statement的id



**延迟加载**：select指定的查询只有在该属性的get方法被调用的时候才会执行，并把结果映射给该参数。需配置以下设置。

```xml
<settings>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>
```



**discriminator标签**：鉴别器，根据某个字段的值来动态选择映射类型，如果都不符合则使用父标签定义的resultMap

- **column**：用来判断的字段的名称
- **javaType**：该字段映射的javaType，用来指定判断策略

- **case子标签**：当column字段的值符合value属性时，使用resultType或者resultMap指定的映射关系。





### INSERT返回主键

#### 第一种方式：使用useGeneratedKeys

```xml
<insert id="insertAndGetId" useGeneratedKeys="true" keyProperty="userId" parameterType="com.chenzhou.mybatis.User">
    insert into user(userName,password,comment)
    values(#{userName},#{password},#{comment})
</insert>
```

- useGeneratedKeys：默认false，true启用主键自增
- keyProperty：把主键自增后的id写入parameterType实体类的哪个属性里。
- MySQL和SQLServer支持，Oracle不支持。



#### 第二种方式：使用`<selectKey>`获取自增id

```xml
<insert id="insertProduct" parameterType="domain.model.ProductBean" >
    <selectKey resultType="java.lang.Long" order="AFTER" keyProperty="productId">
        SELECT LAST_INSERT_ID()
    </selectKey>
    INSERT INTO t_product(productName,productDesrcible,merchantId)values(#{productName},#{productDesrcible},#{merchantId});
</insert>
```

- `SELECT LAST_INSERT_ID()`是查询刚刚插入的记录自增长的id的SQL语句
- order：BEFORE或AFTER，表示`<selectKey>`在insert语句的之前还是之后执行
- keyProperty：把主键自增后的id写入parameterType实体类的哪个属性里。
- resultType：指定查询结果在java里的返回类型



#### 

#### 第三种方式：使用`<selectKey>`生成自定义id

```xml
<insert id="insertProduct" parameterType="domain.model.ProductBean" >
    <selectKey resultType="java.lang.String" order="BEFORE" keyProperty="productId">
        SELECT product_seq.nextval from dual
    </selectKey>
    INSERT INTO t_product(productId,productName,productDesrcible,merchantId)values(#{productId},#{productName},#{productDesrcible},#{merchantId});
</insert>
```



## 动态SQL

### if标签

```xml
<select id="findUserList" parameterType="user" resultType="user">
	select * from user where 1=1 
	<if test="id!=null and id!=''">
		and id=#{id}
	</if>
	<if test="username!=null and username!=''">
		and username like '%${username}%'
	</if>
</select>
```

注意：要做不等于空字符串校验。



### choose(when,otherwise)标签

```xml
<select id="findUserInfoByOneParam" parameterType="Map" resultMap="UserInfoResult">
	select * from userinfo 
	<choose>
		<when test="searchBy=='department'">
			where department=#{department}
		</when>
		<when test="searchBy=='position'">
			where position=#{position}
		</when>
		<otherwise>
			where gender=#{gender}
		</otherwise>
	</choose>
</select> 
```

按顺序判断when中的条件出否成立，如果有一个成立，则choose结束。当choose中所有when的条件都不满则时，则执行otherwise中的sql。



### where/set/trim标签

where 标签：只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句。而且，若语句的开头为“AND”或“OR”，where标签也会将它们去除。

set标签：只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“SET”子句。而且，若语句的结尾为`,`，set标签也会将它们去除。

trim标签：

- prefix：需要加在内部sql前面的前缀 
- prefixoverride：指定需要去掉的内部sql的第一个前缀
- suffix：需要加在内部sql后面的后缀
- suffixoverride：指定需要去掉的内部sql的最后一个后缀



### sql片段

sql标签：定义一个可以被反复使用的sql片段。属性id指定sql片段的唯一标识

include标签：引用一个sql片段。属性refid指定sql片段的id。通过namespace.片段id来引用其他xml内的片段



### foreach标签

collection：指定pojo的集合属性名，如果parameterType是List类型时填“list”，是数组时填“array”

item：定义每次遍历生成的对象名，当为Map时指代值

index：定义遍历的索引名，当为Map时指代键

open：开始遍历时拼接的字符串

close：结束遍历时拼接的字符串

separator：便利对象之间拼接的字符串

