# Maven

## 基本概念

Apache Maven是一个基于项目对象模型（POM）的概念，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。通过中心仓库来管理依赖。

1. 仓库类型：local本地仓库，remote私服(局域网)，central中央仓库(互联网)
2. 生命周期

    - 清洁生命周期：pre-clean，clean，post-clean。
    - 默认生命周期：compile，test，package，install，deploy。（当后面的命令执行时，会按顺序先把它之前的命令执行）
    - 网站的生命周期：pre-site，site，post-site，site-deploy。
3. 打包类型：pom(聚合工程)，jar(java工程)，war(web工程)
4. group Id：即域名。如：org.apache。artifact Id：即项目名。如maven
5. version：snapshot测试版本，release正式版本。

### 安装配置

1. [官网下载](http://maven.apache.org)，解压。
2. 配置path环境变量指向maven的bin文奸夹。

3. 配置conf/settings文件
   - `<localRepository>`配置本地仓库的路径。
   - `<mirror> `配置镜像服务器。属性有id，name，url，mirrorof（被镜像服务器替代的原始服务器的id）。

#### 阿里的镜像服务器配置

```xml
<mirror>
    <id>alimaven</id>
    <mirrorOf>central</mirrorOf>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
```
#### eclipse配置

1. 配置本地maven：Eclipse→Maven→Installation和User Setting

2. 创建索引，加快检索效率：Show View→Maven→右键rebuild index


### 目录结构

```
src                         源码目录
|---main                    项目主体目录
|---|---java                项目主体源代码目录
|---|---resources           项目主体配置文件目录
|---test                    项目测试目录
|---|---java                项目测试源代码目录
|---|---|---resources       项目测试配置文件目录
pom.xml                     Maven工程核心配置文件
```

## 命令

mvn tomcat:run  部署项目

mvn -v  版本信息

mvn clean  清理（删除target目录下编译内容）

mvn compile  编译源代码，一般编译模块下的src/main/java目录

mvn package  项目打包工具,会在模块下的target目录生成jar或war等文件

mvn test  测试命令,执行src/test/java/下所有XxxxTest.java的测试用例.

mvn install  将打包的jar/war文件复制到你的本地仓库中,供其他模块使用

## 创建项目

- maven module：子模块

- maven project：普通maven项目或父工程。



## 依赖

- 插件配置在`<project>`的`<dependencies>`。
- 参考：[maven依赖查询](https://mvnrepository.com)。

### dependencies和dependencyManagement的区别

- dependencies：本项目会引入依赖。即使子项目不写该依赖项，仍会从父项目中继承该依赖（完全继承）。
- dependencyManagement：本项目不会引入依赖，只是声明依赖的版本。因此子项目需要显示的声明需要用的依赖。
  - 在子项目中**不声明依赖**，不会从父项目中继承依赖下来；
  - 在子项目中**声明该依赖项，并且没有指定具体版本**，才会从父项目中继承该项，并且version和scope都读取自父pom;
  - 在子项目中**声明并指定了版本号**，那么会使用子项目中指定的jar版本。



### 依赖范围

即`<dependency>`内的`<scope>`。

| 依赖范围        | 编译时使用该依赖 | 测试时使用该依赖 | 运行时使用该依赖 |
| --------------- | ---------------- | ---------------- | ---------------- |
| compile（默认） | Y                | Y                | Y                |
| test            | -                | Y                | -                |
| provided        | Y                | Y                | -                |
| runtime         | -                | Y                | Y                |
| system          | Y                | Y                | -                |



### 依赖传递

A依赖B且scope为X，B依赖C且scope为Y。A与B的依赖称为**第一直接依赖**，B与C的依赖称为**第二直接依赖**。A与C的依赖称为**传递性依赖**，其scope如下表所示。`-`表示依赖不传递，即A不依赖C。

| 下边是X/右边是Y | compile  | test | provided | runtime  |
| --------------- | -------- | ---- | -------- | -------- |
| compile         | compile  | -    | -        | runtime  |
| test            | test     | -    | -        | test     |
| provided        | provided | -    | provided | provided |
| runtime         | runtime  | -    | -        | runtime  |

#### 总结

- Y是compile时，A依赖C且其scope与X相同。
- Y是test时，A不依赖C。
- Y是provided时，只有X是provided时，A依赖C，且scope依然为provided。
- Y是runtime时，当X为compile和runtime时，A依赖C且其scope为runtime，否则与X相同。



### 依赖冲突

```
A→B→C→D→E→X（version 0.1）
A→F→X（version 0.2）
```

如上情况所示A对于X的依赖不只有一种，处理这种冲突的方法如下：

- 就近原则：即依赖的层级少的优先于依赖层级多的。（直接依赖优先于传递依赖就是这一规则的特殊体现）。

- 声明优先原则：即在依赖层级相同的情况下，声明靠前的优先于靠后的。

- 版本锁定：配置dependencyManagement。

- 排除依赖：`<exclusions><exclusion><exclusion></exclusion>`


```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-core</artifactId>
	<version>${spring.version}</version>
	<!-- 排除spring-core里的commons-logging依赖 -->
	<exclusions>
		<exclusion>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```




### 常用依赖配置

#### JSTL

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

## 插件

- 插件配置在`<build>`的`<plugins>`。
- pluginManagement类似于dependencyManagement，都是用于控制版本的。



### 常用插件配置

#### 编译环境

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.0</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

#### tomcat配置

```xml
<plugin>
	<!-- 配置tomcat插件 -->
	<groupId>org.apache.tomcat.maven</groupId>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<version>2.2</version>
	<configuration>
		<port>8080</port>
		<!-- 项目名，即域名后的项目路径名 -->
        <path>/</path>
        <uriEncoding>UTF-8</uriEncoding>
        <!-- 远程部署，需要填写部署服务器URL和用户，密码 -->
        <url>http://xxx.xxx.xxx.xxx:8080/manager/text</url>
        <username>username</username>
        <password>password</password>
	</configuration>
</plugin>
```

Goals命令：tomcat7:run，tomcat7:deploy，tomcat7:redeploy



##### 远程部署

配置到远程tomcat服务器需修改该服务器的conf下的tomcat-users.xml，进行角色权限配置。

```xml
<tomcat-users>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <role rolename="manager"/>
    <role rolename="manager-gui"/>
    <user username="username" password="password" roles="manager,manager-gui,manager-script,manager-jmx,manager-status"/>  
</tomcat-users>
```

 **配置说明：**

- manager-gui：允许访问html接口(即URL路径为/manager/html/)
- manager-script：允许访问纯文本接口(即URL路径为/manager/text/)
- manager-jmx：允许访问JMX代理接口(即URL路径为/manager/jmxproxy/)
- manager-status：允许访问Tomcat只读状态页面(即URL路径为/manager/status/)

从Tomcat Manager内部配置文件中可以得知，manager-gui、manager-script、manager-jmx均具备manager-status的权限，也就是说，manager-gui、manager-script、manager-jmx三种角色权限无需再额外添加manager-status权限，即可直接访问路径"/manager/status/*"。

## 其他配置

### properties标签

```xml
<properties>
    <struts.version>2.3.15</struts.version>
    <mysql.version>5.1.29</mysql.version>
    <hibernate.version>4.3.1.Final</hibernate.version>
</properties>
```

pom.xml中配置properties标签，在pom.xml中可以通过${struts.version}来取得2.3.15这个值。

## 常见错误整理

**No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?**

原因：maven编译需要JDK，而eclipse配置的是JRE。

解决：【Window】-->【Prefrences】-->【Java】-->【Installed JREs】--->【add】--->【Standard VM】--->【Directory...】--->JDK路径--->勾选配置的JDK然后确认。