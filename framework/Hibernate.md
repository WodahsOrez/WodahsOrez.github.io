# Hibernate

## Spring配置

```xml
<!--配置C3P0连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driverClass}" />
    <property name="jdbcUrl" value="${jdbc.url}" />
    <property name="user" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>

<!--配置Hibernate的SessionFactory -->
<bean id="factory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="classpath:hibernate.cfg.xml"></property>
    <!-- 扫描实体所在的包，配合注解映射 -->
    <property name="packagesToScan" value="my.hibernate.entity" />
</bean>

<!--4.配置Hibernate的事务管理器 -->
<bean id="transaction" class="org.springframework.orm.hibernate4.HibernateTransactionManager">
	<property name="sessionFactory" ref="factory" />
</bean>
```

## 配置hibernate.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!-- 运行时输出底层sql语句 -->
        <property name="hibernate.show_sql">true</property>
        <!-- 对输出的sql语句进行格式化 -->
        <property name="hibernate.format_sql">true</property>
        <!--  update: 首次加载hibernate时根据model类会自动建立起表的结构，
              以后加载hibernate时根据 model类自动更新表结构
        -->
        <property name="hibernate.hbm2ddl.auto">update</property>
        <!-- 配置数据库方言
            在mysql里面实现分页 关键字 limit，只能使用mysql里面
            在oracle数据库，实现分页rownum
            让hibernate框架识别不同数据库的自己特有的语句
         -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        
        <!-- 实现与本地线程绑定session -->
        <property name="hibernate.current_session_context_class">thread</property>
        
        <!-- 把映射文件放到核心配置文件中必须的-->
        <mapping resource="my/hibernate/entity/User.hbm.xml"/>
    </session-factory>
</hibernate-configuration>
```

## 配置映射文件hbm.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
 "-//Hibernate/Hibernate Mapping DTD//EN"
 "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd"> 
<hibernate-mapping>
    <!-- 配置类和表对应 name属性：实体类全路径 table属性：数据库表名称
    -->
    <class name="my.hibernate.entity.User" table="user_t">
        <!-- id标签 name属性：实体类里面id属性名称 column属性：生成的表字段名称
         -->
        <id name="id" column="id">
            <!-- 设置数据库表id增长策略 native:自动选择策略，生成表id值就是主键自动增长，须int
                uuid:生成uuid主键,实体类字段必须为字符串类型
            -->
            <generator class="native"></generator>
        </id>
        <!-- 配置其他属性和表字段对应 name属性：实体类属性名称 column属性：生成表字段名称
        -->
        <property name="username" column="username"></property>
        <property name="password" column="password"></property>
        <property name="email" column="email"></property>
        <property name="telephone" column="telephone"></property>
        
        <!-- 一对多的映射 -->
		<!-- 多个联系人对应一个客户 name属性：联系人实体类中客户实体类的变量名称 class属性：customer全路径，column属性：联系人中外键名称
		-->
		<many-to-one name="customer" class="cn.itcast.entity.Customer" column="clid"></many-to-one>
		
		<!-- 一个客户对应多个联系人， 使用set标签表示所有联系人
			 name属性：属性值写在客户实体类里面表示联系人的set集合名称  
			 inverse属性默认值：false不放弃双向外键维护,true表示放弃双向外键维护(推荐)
			 cascade属性:save-update级联保存 delete级联删除
		-->
		<set name="setLinkMan" inverse="true" cascade="save-update,delete">
			<!-- 一对多建表，有外键，hibernate机制：双向维护外键，在一和多两边都配置外键，column属性值：外键名称 -->
			<key column="clid"></key>
			<!-- class里面写联系人实体类全路径 -->
			<one-to-many class="cn.itcast.entity.LinkMan"/>
		</set>
        
        <!-- 多对多的映射 -->
		<!-- 在用户里面表示所有角色，使用set标签，name属性：角色set集合名称，table属性：第三张表名称 -->
		<set name="setRole" table="user_role" cascade="save-update,delete">
			<!-- key标签里面配置，配置当前映射文件在第三张表外键名称 -->
			<key column="userid"></key>
			<!-- class：角色实体类全路径 column：角色在第三张表外键名称 -->
			<many-to-many class="cn.itcast.manytomany.Role" column="roleid"></many-to-many>
		</set>
        
    </class>
</hibernate-mapping>
```

PS：id标签和property标签，column属性可以省略的,不写值和name属性值一样的

## 基本操作

```java
//第一步 加载hibernate核心配置文件，默认到src下面找到名称是hibernate.cfg.xml
Configuration cfg = new Configuration();
cfg.configure();
//第二步 创建SessionFactory对象，读取hibernate核心配置文件内容，创建sessionFactory，在过程中，根据映射关系，在配置数据库里面把表创建
SessionFactory sessionFactory = null;
//第三步 使用SessionFactory创建session对象，类似于连接
Session session = null;
//第四步 开启事务
Transaction tx = null;                       
try {
    sessionFactory = HibernateUtils.getSessionFactory();
    session = sessionFactory.openSession();
    tx = session.beginTransaction();
    //第五步 写具体逻辑 crud操作
    //第六步 提交事务
    tx.commit();
}catch(Exception e) {
    e.printStackTrace();
    //回滚事务
    tx.rollback();
}finally {
    //第七步 关闭资源
    session.close();
    sessionFactory.close();
```

### 增删改查

```java
//新增实体类数据,相当于insert
session.save(user)
//删除实体类数据,相当于delect,需要传一个带id值得实体类对象
User user = session.get(User.class,2);
//或者
User user = new User();
user.setId(2);
session.delete(user);

//修改数据,相当于update,先查后改
User user = session.get(User.class,2);
user.setUsername("东方不败");
session.update(user);

//通过主键id获取实体类对象数据
User user = session.get(User.class,1);

//saveOrUpdate方法,瞬时态添加,持久态和托管态修改,配合unsaved-value使用
session.saveOrUpdate(user);
```

### 复杂查询操作

#### Query对象

```java
//1 创建Query对象
//方法里面写hql语句,查询所有(from 实体类名称)hql语句
Query query = session.createQuery("from User");
//2 调用query对象里面的方法得到结果
List<User> list = query.list();
```

##### 条件查询

```java
//from  实体类名称 where 实体类属性名称=? and 实体类属性名称=?
//from  实体类名称 where 实体类属性名称 like ?
//1 创建query对象
//SELECT * FROM t_customer WHERE cid=? AND custName=?
Query query = session.createQuery("from Customer c where c.cid=? and c.custName=?");
//2 设置条件值
// 向？里面设置值
// setParameter方法两个参数
// 第一个参数：int类型是？位置，？位置从0开始
// 第二个参数：具体参数值
//设置第一个？值
query.setParameter(0, 1);
//设置第二个？值
query.setParameter(1, "百度");
//3 调用方法得到结果
List<Customer> list = query.list();

//1 创建query对象
Query query = session.createQuery("from Customer c where c.custName like ?");
//2 设置？的值 %浪%
query.setParameter(0, "%浪%");
//3 调用方法得到结果
List<Customer> list = query.list();
```

##### 排序查询

```java
//from 实体类名称 order by 实体类属性名称 asc/desc
//1 创建query对象
Query query = session.createQuery("from Customer order by cid desc");
//2 调用方法得到结果
List<Customer> list = query.list();
```

##### 分页查询

```java
//hibernate的Query对象封装两个方法实现分页操作
//1 创建query对象
//写查询所有的语句
Query query = session.createQuery("from Customer");
//2 设置分页数据
//2.1 设置开始位置
query.setFirstResult(0);
//2.2 设置每页记录数
query.setMaxResults(3);
//3 调用方法得到结果
List<Customer> list = query.list();
```

##### 投影查询

```java
//select 实体类属性名称1, 实体类属性名称2  from 实体类名称
//PS:select 后面不能写 * ，不支持的
//1 创建query对象
Query query = session.createQuery("select custName,age from Customer");
//2 调用方法得到结果
List<Object[]> list = query.list();
```

##### 聚合函数

```java
//常用聚合函数:count、sum、avg、max、min
//查询表记录数
//select count(*) from 实体类名称
//1 创建query对象
Query query = session.createQuery("select count(*) from Customer");
//2 调用方法得到结果
//query对象里面有方法，直接返回对象形式
Object obj = query.uniqueResult();
//返回int类型
//int count = (int) obj; 会报错，因为返回类型是Long
//首先把object变成long类型，再变成int类型
Long lobj = (Long) obj;
int count = lobj.intValue();
```

##### group by子句

```java
//select 实体类属性名称1,count(实体类属性名称2),实体类属性名称3 from 实体类名称 group by 实体类属性名称1
//1 创建query对象
Query query = session.createQuery("select count(id),name from Event group by name");
//2 调用方法得到结果
List<Object> list = query.list();
```

##### 内连接

```java
//创建query对象
Query query = session.createQuery("from Customer c inner join c.setLinkMan");
//调用方法获得结果，返回结果是数组，分别是客户和联系人的对象
List list = query.list();
```

##### 迫切内连接

```java
//使用内连接返回list中每部分是数组，迫切内连接返回list每部分是对象
//1 创建query对象
Query query = session.createQuery("from  Customer  c  inner  join  fetch  c.setLinkMan");
//调用方法获得结果，返回客户对象其中包含了一个联系人对象
List<Customer> list = query.list();
```

##### 左/右外连接

```sql
from  Customer  c  left/right  outer  join  c.setLinkMan
```

##### 迫切左/右外连接

```sql
from  Customer  c  left/right  outer  join  fetch  c.setLinkMan
```

### Criteria对象

```java
//创建criteria对象
//方法里面参数是实体类class
Criteria criteria = session.createCriteria(User.class);
//调用方法得到结果
List<User> list = criteria.list();
```

#### 条件查询

```java
//创建对象
Criteria criteria = session.createCriteria(Customer.class);
//使用Criteria对象里面的方法设置条件值
//首先使用add方法，表示设置条件值
//在add方法里面使用类的方法实现条件设置
//类似于 cid=?
//criteria.add(Restrictions.eq("cid", 1));
//criteria.add(Restrictions.eq("custName", "百度"));
criteria.add(Restrictions.like("custName", "%百%"));
//调用方法得到结果
List<Customer> list = criteria.list();
```

#### 排序查询

```java
//创建对象
Criteria criteria = session.createCriteria(Customer.class);
//设置对哪个属性进行排序，设置排序规则 
criteria.addOrder(Order.desc("cid"));
//调用方法得到结果
List<Customer> list = criteria.list();
```

#### 分页查询

```java
//创建对象
Criteria criteria = session.createCriteria(Customer.class);
//设置分页数据
//设置开始位置
criteria.setFirstResult(0);
//每页显示记录数
criteria.setMaxResults(3);
//调用方法得到结果
List<Customer> list = criteria.list();
```

#### 统计查询

```java
//创建对象
Criteria criteria = session.createCriteria(Customer.class);
//设置操作
criteria.setProjection(Projections.rowCount());
//调用方法得到结果
Object obj = criteria.uniqueResult();
```

### SQLQuery对象

```java
//创建对象
//参数普通sql语句
SQLQuery sqlQuery = session.createSQLQuery("select * from t_user");
//设置返回的list中每部分是对象形式
sqlQuery.addEntity(User.class);
//调用sqlQuery里面的方法
List<User> list = sqlQuery.list();
//不设置对象格式时.返回list集合，默认里面每部分数组结构
//List<Object[]> list = sqlQuery.list();
```

### 一对多操作

#### 一对多级联保存

```java
//简化操作,配置<set>的cascaded="save-update"时.
//把联系人放到客户里面
customer.getSetLinkMan().add(linkman);
//保存客户
session.save(customer);
```

#### 一对多级联删除

```java
//需配置<set>的cascaded="delete"
//根据id查询客户
Customer customer = session.get(Customer.class,3);
//调用方法删除,同时删除客户和其对应的联系人
session.delete(customer);
```

#### 一对多修改

```java
//根据id查询lucy联系人，根据id查询百度的客户
Customer baidu = session.get(Customer.class, 1);
LinkMan lucy = session.get(LinkMan.class, 2);
//修改持久态对象的值
//把联系人放到客户里面
baidu.getSetLinkMan().add(lucy);
//把客户放到联系人里面
lucy.setCustomer(baidu);
```

### 多对多的操作

##### 多对多级联保存

```java
//建立关系，把角色放到用户里面
//user1 -- r1/r2
user1.getSetRole().add(r1);
user1.getSetRole().add(r2);
//保存用户
session.save(user1);
```

##### 多对多级联删除

```java
//一般不用，删除用户会把角色也删除
User user = session.get(User.class, 1);
session.delete(user);
```

##### 多对多修改

```java
//让lucy有经纪人角色
//查询lucy和经纪人
User lucy = session.get(User.class, 1);
Role role = session.get(Role.class, 1);
//把角色放到用户的set集合里面
lucy.getSetRole().add(role);
//让某个用户没有有某个角色
User user = session.get(User.class, 2);
Role role = session.get(Role.class, 3);
//从用户里面把角色去掉
user.getSetRole().remove(role);
```

