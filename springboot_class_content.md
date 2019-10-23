<!-- 如果springboot项目在没有网络的情况下，可以创建普通的maven项目，手动根据springboot官网提供的说明，复制相应代码 -->

# bootstrap相关引用：

~~~xml
<!-- 新 Bootstrap 核心 CSS 文件 -->
 
<link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
 
<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
 
<script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
 
<!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
 
<script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
~~~



# 1.创建springboot

### 当前无网络且仓库有jar的情况的详细步骤

1.创建一个maven工程
2.导入相关POM.XML的jar dependencies
3.创建springboot主程序@SpringbootApplication，且创建main方法，SpringBootApplication.run(当前类.class,args);
4.相关项目建在项目的package下，创建类时可以使用 包名+类名

# 2.场景启动器查看方式

1. 官网：spring.io 
2. 导航栏：projects 
3. 网页内容中心：springboot 
4. tab切换面板：learn 
5. 选择版本：current 2.1.9GA 
6. 文档：Reference DOC 
7. 搜索 using spring boot 
8. 选择：Starters 

# **3.SpringBoot启动类分解**

**@SpringBootApplication**注解拆分：
	**@SpringBootConfiguration**:Springboot的配置类
		标注在某个类上，表示这是一个springboot的配置类
		@Configuration:配置类上来标注这个注解
		配置类==配置文件：配置类也是容器中的一个组件
	**@EnabledAutoConfiguration**:开启自动配置类
		以前我们需要手动配置的东西springboot帮我们自动配置
		@AutoConfigurationPackage:自动配置包

***需要注意@SpringBootApplication所在包下以及下面的所有子包中的组件扫描到spring容器中***

resources目录结构：
	statics：保存js，css，img等静态资源
	templates:保存所有的模板页面，（Springboot默认jar包使用嵌入式的tomcat，默认不支持JSP）；可以使用模板引擎(freemarker，thymeleaf)
	application.properties:springboot应用配置文件,可以修改默认设置
		重新指定端口：server.port = 8080

配置文件
springboot全局配置文件:application.properties or application.yml 
	YAML：是一个标记语言，
	以前的配置文件大多使用xml配置
	以数据为中心，比Json，Xml更适合做配置文件
多环境配置文件：dev，qu，prod(多环境切换：spring.profiles.active=dev)


## 4.Mybatis 配置
##### mybaits别名以及扫描xml

1. mybatis.typeAliasesPackage=org.spring.springboot.domain
2. mybatis.mapperLocations=classpath:mapper/*.xml

##### 热部署：

1.pom文件添加依赖

~~~xml
<!-- 热部署 -->
<!-- devtools可以实现页面热部署（即页面修改后会立即生效，这个可以直接在application.properties文件中配置spring.thymeleaf.cache=false来实现） -->
<!-- 实现类文件热部署（类文件修改后不会立即生效），实现对属性文件的热部署。 -->
<!-- 即devtools会监听classpath下的文件变动，并且会立即重启应用（发生在保存时机），注意：因为其采用的虚拟机机制，该项重启是很快的 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <!-- optional=true,依赖不会传递，该项目依赖devtools；之后依赖boot项目的项目如果想要使用devtools，需要重新引入 -->
    <optional>true</optional>
</dependency>
~~~

2.左上角依次找到【File】——【Settings...】——【Build,Execution,Deployment】——【Compiler】，

勾选"Build project automatically",然后右下角【Apply】——【OK】

3.Ctrl+Shift+A : 搜索 Registry 找到"compiler.automake.allow.when.app.running"，勾选，【Close】关闭：

4.重启项目

##### JSP访问处理

```xml
属性文件处理
properties写法(application.properties  )
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

yaml写法(application.yml)
spring:
 mvc:
 view:
 	prefix: /WEB-INF/jsp/
	suffix: .jsp
```

```xml
<!-- 添加servlet依赖模块 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <scope>provided</scope>
</dependency>
<!-- 添加jstl标签库依赖模块 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
<!--添加tomcat依赖模块.-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
<!-- 使用jsp引擎，springboot内置tomcat没有此依赖 -->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
```

##### <!--DEBUG-->

```js
表示不能在默认包下创建SpringBootApplication
** WARNING ** : Your ApplicationContext is unlikely to start due to a @ComponentScan of the default package.
```



# 5.使用新的模板引擎

**Thymeleaf**是用于Web和独立环境的现代服务器端Java模板引擎。Thymeleaf的主要目标是将优雅的自然模板带到您的开发工作流程中—HTML能够在浏览器中正确显示，并且可以作为静态原型，从而在开发团队中实现更强大的协作。Thymeleaf能够处理HTML，XML，JavaScript，CSS甚至纯文本。

Spring-boot-starter-web集成了Tomcat以及Spring MVC，会自动配置相关东西，Thymeleaf是用的比较广泛的模板引擎.

```xml
<!-- 依赖导入 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

application.yml配置

~~~yaml
#thymeleaf 不配置执行默认选项
spring.thymeleaf.cache=false #缓存是否开启
spring.thymeleaf.prefix=classpath:/templates/ #网页文件指定位置，前缀
spring.thymeleaf.check-template-location=true #检查模板地址是否正确
spring.thymeleaf.suffix=.html #后缀
spring.thymeleaf.encoding=UTF-8 #网页格式
spring.thymeleaf.mode=HTML5 #网页类型
~~~

***注意：***

~~~htm
1. 使用@Controller 注解，在对应的方法上，视图解析器可以解析return 的jsp,html页面，并且跳转到相应页面。若返回json等内容到页面，则需要加@ResponseBody注解

2. @RestController注解，相当于@Controller+@ResponseBody两个注解的结合，返回json数据不需要在方法前面加@ResponseBody注解了，但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面
~~~

网页写法：

~~~html
<!DOCTYPE html>
<html lang="en"  xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>thymeleaf模板</title>
    <meta name="viewport" content="width=device-width,initial-scale=1"/>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap-theme.min.css" rel="stylesheet">
</head>
<body>
<table class="table table-striped table-hover">
    <tr>
        <td>编号</td>
        <td>姓名</td>
        <td>年龄</td>
    </tr>
    <tr th:each="item : ${list}">
        <td th:text="${item.id}"></td>
        <td th:text="${item.name}"></td>
        <td th:text="${item.age}"></td>
    </tr>
    <tr>
        <td><a th:href="@{/toOther}">随便点的</a> </td>
    </tr>
</table>
</body>
</html>
~~~

> 相关文档：<https://www.e-learn.cn/index.php/thymeleaf>

请求Get传参：<a th:href="@{/order/details(id=3)}"> 

​	多个参数：<a th:href="@{/order/details(id=3,action='show_all')}">

# 6.SpringBoot配置文件

SpringBoot采纳了建立生产就绪Spring应用程序的观点。 Spring Boot优先于配置的惯例，旨在让您尽快启动和运行。在一般情况下，我们不需要做太多的配置就能够让spring boot正常运行。在一些特殊的情况下，我们需要做修改一些配置，或者需要有自己的配置属性。

## 一、自定义属性

当我们创建一个springboot项目的时候，系统默认会为我们在src/main/java/resources目录下创建一个application.properties。个人习惯，我会将application.properties改为application.yml文件，两种文件格式都支持。

在application.yml自定义一组属性：

```
my:
 name: forezp
 age: 12


```

如果你需要读取配置文件的值只需要加@Value(“${属性名}”)：

```
@RestController
public class MiyaController {

    @Value("${my.name}")
    private String name;
    @Value("${my.age}")
    private int age;

    @RequestMapping(value = "/miya")
    public String miya(){
        return name+":"+age;
    }

}

```

启动工程，访问：localhost:8080/miya,浏览器显示：

> forezp:12

## 二、将配置文件的属性赋给实体类

当我们有很多配置属性的时候，这时我们会把这些属性作为字段来创建一个javabean，并将属性值赋予给他们,比如：

```yaml
my:
 name: forezp
 age: 12
 number:  ${random.int}
 uuid : ${random.uuid}
 max: ${random.int(10)}
 value: ${random.value}
 greeting: hi,i'm  ${my.name}


```

其中配置文件中用到了${random} ，它可以用来生成各种不同类型的随机值。

怎么讲这些属性赋于给一个javabean 呢，首先创建一个javabean ：

```java
@ConfigurationProperties(prefix = "my")
@Component
public class ConfigBean {

    private String name;
    private int age;
    private int number;
    private String uuid;
    private int max;
    private String value;
    private String greeting;
    
    省略了getter setter....


```

需要加个注解@ConfigurationProperties，并加上它的prrfix。另外@Component可加可不加。另外spring-boot-configuration-processor依赖可加可不加，具体原因不详。

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>


```

另外需要在应用类或者application类，加EnableConfigurationProperties注解。

```java
@RestController
@EnableConfigurationProperties({ConfigBean.class})
public class LucyController {
    @Autowired
    ConfigBean configBean;

    @RequestMapping(value = "/lucy")
    public String miya(){
        return configBean.getGreeting()+" >>>>"+configBean.getName()+" >>>>"+ configBean.getUuid()+" >>>>"+configBean.getMax();
    }
    


```

启动工程，访问localhost:8080/lucy,我们会发现配置文件信息读到了。

## 三、自定义配置文件

上面介绍的是我们都把配置文件写到application.yml中。有时我们不愿意把配置都写到application配置文件中，这时需要我们自定义配置文件，比如test.properties:

```properties
com.forezp.name=forezp
com.forezp.age=12


```

怎么将这个配置文件信息赋予给一个javabean呢？

```java
@Configuration
@PropertySource(value = "classpath:test.properties")
@ConfigurationProperties(prefix = "com.forezp")
public class User {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}



```

在最新版本的springboot，需要加这三个注解。@Configuration @PropertySource(value = “classpath:test.properties”) @ConfigurationProperties(prefix = “com.forezp”);在1.4版本需要 PropertySource加上location。

```java
@RestController
@EnableConfigurationProperties({ConfigBean.class,User.class})
public class LucyController {
    @Autowired
    ConfigBean configBean;

    @RequestMapping(value = "/lucy")
    public String miya(){
        return configBean.getGreeting()+" >>>>"+configBean.getName()+" >>>>"+ configBean.getUuid()+" >>>>"+configBean.getMax();
    }

    @Autowired
    User user;
    @RequestMapping(value = "/user")
    public String user(){
        return user.getName()+user.getAge();
    }

}


```

启动工程，打开localhost:8080/user;浏览器会显示：

> forezp12

## 四、多个环境配置文件

在现实的开发环境中，我们需要不同的配置环境；格式为application-{profile}.properties，其中{profile}对应你的环境标识，比如：

- application-test.properties：测试环境
- application-dev.properties：开发环境
- application-prod.properties：生产环境

怎么使用？只需要我们在application.yml中加：

```yaml
 spring:
  profiles:
    active: dev
 

```

其中application-dev.yml:

```yaml
 server:
  port: 8082
 

```

启动工程，发现程序的端口不再是8080,而是8082。

# 7.SpringBoot做定时任务

## 构建工程

创建一个Springboot工程，在它的程序入口加上@EnableScheduling,开启调度任务。

```java
@SpringBootApplication
@EnableScheduling
public class SpringbootSchedulingTasksApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootSchedulingTasksApplication.class, args);
	}
}


```

## 创建定时任务

创建一个定时任务，每过5s在控制台打印当前时间。

```java
@Component
public class ScheduledTasks {

    private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        log.info("The time is now {}", dateFormat.format(new Date()));
    }
}

```

通过在方法上加@Scheduled注解，表明该方法是一个调度任务。

- @Scheduled(fixedRate = 5000) ：上一次开始执行时间点之后5秒再执行  1 1 1 
- @Scheduled(fixedDelay = 5000) ：上一次执行完毕时间点之后5秒再执行  5 5 5
- @Scheduled(initialDelay=1000, fixedRate=5000) ：第一次延迟1秒后执行，之后按fixedRate的规则每5秒执行一次
- @Scheduled(cron=” /5 “) ：通过cron表达式定义规则，什么是cro表达式，自行搜索引擎。

## 测试

启动springboot工程，控制台没过5s就打印出了当前的时间。

> 2017-04-29 17:39:37.672 INFO 677 — [pool-1-thread-1] com.forezp.task.ScheduledTasks : The time is now 17:39:37 2017-04-29 17:39:42.671 INFO 677 — [pool-1-thread-1] com.forezp.task.ScheduledTasks : The time is now 17:39:42 2017-04-29 17:39:47.672 INFO 677 — [pool-1-thread-1] com.forezp.task.ScheduledTasks : The time is now 17:39:47 2017-04-29 17:39:52.675 INFO 677 — [pool-1-thread-1] com.forezp.task.ScheduledTasks : The time is now 17:39:52

## 总结

在springboot创建定时任务比较简单，只需2步：

- 1.在程序的入口加上@EnableScheduling注解。
- 2.在定时方法上加@Scheduled注解

# 8.声明式事务

springboot开启事务很简单，只需要一个注解@Transactional 就可以了。因为在springboot中已经默认对jpa、jdbc、mybatis开启了事事务，引入它们依赖的时候，事物就默认开启。当然，如果你需要用其他的orm，比如beatlsql，就需要自己配置相关的事物管理器。

## 准备阶段

以上一篇文章的代码为例子，即springboot整合mybatis，上一篇文章是基于注解来实现mybatis的数据访问层，这篇文章基于xml的来实现，并开启声明式事务。

## 环境依赖

在pom文件中引入mybatis启动依赖：

```xml
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>1.3.0</version>
</dependency>

```

引入mysql 依赖

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.0.29</version>
</dependency>
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>1.3.0</version>
</dependency>


```

## 初始化数据库脚本

```sql
-- create table `account`
# DROP TABLE `account` IF EXISTS
CREATE TABLE `account` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  `money` double DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
INSERT INTO `account` VALUES ('1', 'aaa', '1000');
INSERT INTO `account` VALUES ('2', 'bbb', '1000');
INSERT INTO `account` VALUES ('3', 'ccc', '1000');


```

## 配置数据源

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
mybatis.mapper-locations=classpath*:mybatis/*Mapper.xml
mybatis.type-aliases-package=com.forezp.entity


```

通过配置mybatis.mapper-locations来指明mapper的xml文件存放位置，我是放在resources/mybatis文件下的。mybatis.type-aliases-package来指明和数据库映射的实体的所在包。

经过以上步骤，springboot就可以通过mybatis访问数据库来。

## 创建实体类

```java
public class Account {
    private int id ;
    private String name ;
    private double money;
    
    getter..
    setter..
    
  }

```

## 数据访问dao 层

接口：

```java
public interface AccountMapper2 {
   int update( @Param("money") double money, @Param("id") int  id);
}



```

mapper:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.forezp.dao.AccountMapper2">


    <update id="update">
        UPDATE account set money=#{money} WHERE id=#{id}
    </update>
</mapper>


```

## service层

```java
@Service
public class AccountService2 {

    @Autowired
    AccountMapper2 accountMapper2;

    @Transactional
    public void transfer() throws RuntimeException{
        accountMapper2.update(90,1);//用户1减10块 用户2加10块
        int i=1/0;
        accountMapper2.update(110,2);
    }
}


```

@Transactional，声明事务，并设计一个转账方法，用户1减10块，用户2加10块。在用户1减10 ，之后，抛出异常，即用户2加10块钱不能执行，当加注解@Transactional之后，两个人的钱都没有增减。当不加@Transactional，用户1减了10，用户2没有增加，即没有操作用户2 的数据。可见@Transactional注解开启了事物。

## 结语

springboot 开启事物很简单，只需要加一行注解就可以了，前提你用的是jdbctemplate, jpa, mybatis，这种常见的orm。

# 9.SpringBoot全局异常检测

### 1.知识点

在spring 3.2中，新增了@ControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping中。@ControllerAdvice[官方文档](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)。创建全局异常处理类：通过使用@ControllerAdvice定义统一的异常处理类，而不是在每个Controller中逐个定义。@ExceptionHandler用来定义函数针对的异常类型，最后将Exception对象和请求URL映射到error.html中.

### 2.新建异常捕获类

```java
package com.demo.common;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;

/**
 * Created by sunsicheng on 2019年10月18日.
 */
@ControllerAdvice
public class CatchGlobalException {
    @ExceptionHandler(value = Exception.class)
    public ModelAndView defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {
        ModelAndView mav = new ModelAndView();
        mav.addObject("exception", e);
        mav.addObject("url", req.getRequestURL());
        mav.setViewName("error");
        return mav;
    }

}
```

### 3.error page

实现error.html页面展示：在templates目录下创建error.html，将请求的URL和Exception对象的message输出。

~~~html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" >
<head lang="en">
    <meta charset="UTF-8" />
    <title>抱歉,这是一个错误页</title>
</head>
<body>
<div>很抱歉，这是我们的一个错误页</div>
<div>影响的因素有很多，我们会尽快解决的。  ﹃_﹃〣</div>
<div th:text="${url}"></div>
<div th:text="${exception.message}"></div>
</body>
</html>
~~~

### 4.效果

```java
@RequestMapping("/debug")
    public String Debug(){
        int number = 5 / 0;
        return null;
    }
```

# 10.Logback日志框架集成

#### 1.log4j logback slf4j区别？

~~~html
首先谈到日志,我们可能听过log4j logback slf4j这三个名词，那么它们之间的关系是怎么样的呢？SLF4J，即简单日志门面（Simple Logging Facade for JAVA），不是具体的日志解决方案，它只服务于各种各样的日志系统。一般来说，slf4j配合log4j、logback进行使用，可以理解为slf4j是标准，log4j和logback是实现，我们可以根据自己的需求进行选择具体的日志系统。

这里推荐使用logback,因为logback更快的执行速度，logback-classic 非常自然的实现了SLF4J，官方也推荐使用logback作为日志系统。

让我们开始使用 logback 吧！

~~~

#### 2.不需要引入依赖

为什么不需要引入依赖呢？因为 [SpringBoot](https://liuyanzhao.com/tag/springboot/) 的核心依赖里已经包含了

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
~~~

进去之后，点击放大镜按钮，放大，找到 spring-boot-starter，我们可以看到下图。已经引入了常见的日志依赖，也包括 logback



#### 3.自定义 logback 配置文件

在 resource 根目录新建 logback-spring.xml

首先，官方推荐使用的xml名字的格式为：logback-spring.xml而不是logback.xml，至于为什么，因为带spring后缀的可以使用<springProfile>这个标签。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false">
    <contextName>Logback For demo Mobile</contextName>
    <!-- 设置log日志存放地址 -->
    <!--（改） 单环境设置 -->
    <!-- name的值是变量的名称，value的值时变量定义的值。通过定义的值会被插入到logger上下文中。定义变量后，可以使“${}”来使用变量。 -->
    <property name="LOG_HOME" value="D:/logback_log" />
    <!-- 多环境设置 -->
    <!--  <springProfile name="dev">
         <property name="LOG_HOME" value="/Users/ssc/Desktop/temp/log" />
     </springProfile>
     <springProfile name="prod">
         <property name="LOG_HOME" value="/logger/log" />
     </springProfile> -->
    <!-- 控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!-- encoder默认配置为PartternLayoutEncoder -->
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{80} -%msg%n</pattern>
        </encoder>
        <target>System.out</target>
    </appender>
    <!-- 按照每天生成日志文件 -->
    <appender name="ROLLING_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名 ,每天保存（侧翻）一次 -->
            <FileNamePattern>${LOG_HOME}/%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数 -->
            <MaxHistory>20</MaxHistory>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{25} -%msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小 -->
        <triggeringPolicy
                class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>20MB</MaxFileSize>
        </triggeringPolicy>
    </appender>
    <!-- （改）过滤器，可以指定哪些包，哪个记录到等级， -->
    <!-- 运用的场景比如，你只需要com.demo.controller包下的error日志输出。定义好name="com.demo.controller" level="ERROR" 就行了 -->
    <logger name="com" level="ERROR">
        <appender-ref ref="ROLLING_FILE" />
    </logger>
    <!-- 全局，控制台遇到INFO及以上级别就进行输出 TRACE、DEBUG、INFO、WARN 和 ERROR -->
    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
~~~

代码写法：

~~~java

package com.md.demo.rest;
 
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
 
/**
 * @author Minbo
 */
@RestController
public class InitRest {
 
	protected static Logger logger = LoggerFactory.getLogger(InitRest.class);
 
	/**
	 * http://localhost:9090/hello
	 * 
	 * @return
	 */
	@GetMapping("/hello")
	public String hello() {
		logger.info("hello");
		return "Hello greetings from spring-boot2-logback";
	}
}
~~~

#### 4.开始使用

导入包

~~~java
1. import org.slf4j.Logger;
2. import org.slf4j.LoggerFactory;
~~~

获得实例

~~~java
private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
~~~

直接使用

~~~java
logger.error("参数验证失败",);
~~~

# 11.404,405等页面的友好提示处理

此处省略...

# 12.SpringBoot整合mybatis

### 1.导入依赖

~~~xml

<!--mybatis起步依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>

<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
~~~

### 2.添加数据库连接信息

~~~properties
#DB Configuration:
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=123
~~~

### 3.创建表

~~~sql

- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
 
-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', 'zhangsan', '123', '张三');
INSERT INTO `user` VALUES ('2', 'lisi', '123', '李四');
~~~

### 4.创建实体

~~~java
public class User {
    // 主键
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
 
    //此处省略getter和setter方法 .. ..
    
}

~~~

### 5.编写Mapper @Mapper标记该类是一个mybatis的mapper接口，可以被spring boot自动扫描到spring上下文中

~~~java
@Mapper
public interface UserMapper {
    public List<User> queryUserList();
}
~~~

### 6.配置Mapper映射文件(在src\main\resources\mapper路径下加入UserMapper.xml配置文件")

~~~xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="queryUserList" resultType="user">
        select * from user
    </select>
</mapper>
~~~

### 7.在application.properties中添加mybatis信息

~~~properties

#spring集成Mybatis环境
#pojo别名扫描包
mybatis.type-aliases-package=com.itheima.domain
#加载Mybatis映射文件
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
~~~

### 8.编写测试Controller

~~~java
@Controller
public class MapperController {
 
    @Autowired
    private UserMapper userMapper;
 
    @RequestMapping("/queryUser")
    @ResponseBody
    public List<User> queryUser(){
        List<User> users = userMapper.queryUserList();
        return users;
    }
 
}
~~~

# 13.Springboot整合Junit

### 1.添加Junit依赖

~~~xml

<!--测试的起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
~~~

### 2.编写测试类

~~~java
package com.neuq.test;
 
import com.neuq.MySpringBootApplication;
import com.neuq.domain.User;
import com.neuq.mapper.UserMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
 
import java.util.List;
 
@RunWith(SpringRunner.class)
@SpringBootTest(classes = MySpringBootApplication.class)
public class MapperTest {
 
    @Autowired
    private UserMapper userMapper;
 
    @Test
    public void test() {
        List<User> users = userMapper.queryUserList();
        System.out.println(users);
    }
 
}
~~~

***SpringRunner继承自SpringJUnit4ClassRunner，使用哪一个Spring提供的测试测试引擎都可以***

***@SpringBootTest的属性指定的是引导类的字节码对象***

# 14.SpringBoot整合Spring Data JPA

ex:什么是JPA

JPA(Java Persistence API)是Sun官方提出的Java持久化规范。它为Java开发人员提供了一种对象/关联映射工具来管理Java应用中的关系数据。

JPA的出现主要是为了简化现有的持久化开发工作和整合ORM技术，结束现在Hibernate，TopLink，JDO等ORM框架各自为营的局面。值得注意的是，JPA是在充分吸收了现有Hibernate，TopLink，JDO等ORM框架的基础上发展而来的，具有易于使用，伸缩性强等优点。

从目前的开发社区的反应上看，JPA受到了极大的支持和赞扬，其中就包括了Spring与EJB3.0的开发团队。

注意:JPA是一套规范，不是一套产品，那么像Hibernate,TopLink,JDO他们是一套产品，如果说这些产品实现了这个JPA规范，那么我们就可以叫他们为JPA的实现产品

使用`Spring-Data-Jpa`,JPA(Java Persistence API)定义了一系列对象持久化的标准，目前实现这一规范的产品有Hiberbate、TopLink。`Spring-Data-Jpa`是对Hibernate的整合

### 1.添加Spring Data JPA的起步依赖

~~~xml

<!-- springBoot JPA的起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
   <version>5.1.38</version>
</dependency>
~~~

### 2.application.properties中配置jpa属性

~~~properties
# mysql
spring.datasource.url=jdbc:mysql://localhost:3306/java4all?useSSL=false&characterEncoding=utf-8&autoReconnect=true
spring.datasource.username=root  
spring.datasource.password=123
spring.datasource.driver-class-name=com.mysql.jdbc.Driver 
#JPA Configuration:
spring.jpa.database=MySQL
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true #是否自动生成
spring.jpa.hibernate.ddl-auto=update #自动生成时检查机制
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy #命名策略
~~~

1. naming_strategy有两种值可以配置分别为：

   ```xml
   ***DefaultNamingStrategy***:
   这个直接映射，不会做过多的处理（前提没有设置@Table，@Column等属性的时候）。如果有@Column则以@Column为准  
   ***ImprovedNamingStrategy***:
   表名，字段为小写，当有大写字母的时候会转换为分隔符号“_”。 

   #此为hibernate4的策略
   第一：org.hibernate.cfg.DefaultNamingStrategy 
   第二：org.hibernate.cfg.ImprovedNamingStrategy  

   #此为hibernate5的策略
   隐式命名策略，使用此属性当我们使用的表或列没有明确指定一个使用的名称 。
   spring.jpa.hibernate.naming.implicit-strategy= # Hibernate 5 implicit naming strategy fully qualified name.
   物理命名策略，用于转换“逻辑名称”(隐式或显式)的表或列成一个物理名称。
   spring.jpa.hibernate.naming.physical-strategy= # Hibernate 5 physical naming strategy fully qualified name.

   物理命名策略有两个常用的配置:
   org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy
   org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl

   对于PhysicalNamingStrategyStandardImpl有DefaultNamingStrategy的效果；
   对于SpringPhysicalNamingStrategy  有ImprovedNamingStrategy的效果。
   ```

2. ddl-auto:

   1. create表示每次应用启动的时候，都会将之前的表全部drop掉，重新根据实体类生成一遍。
   2. create-drop在create的基础上，在应用关闭的时候还会drop一次。
   3. update可能是比较常用的，每次启动的时候会看看实体类有什么变化，然后看需不需要更改表结构。
   4. validate不会对表进行更改，但是会看看他和实体类是否对应
   5. none什么都不做

3. show-sql表示在控制台显示sql语句



### 3.创建实体配置实体

~~~java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
//使用JPA注解配置映射关系
@Entity //告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "user") //@Table来指定和哪个数据表对应;如果省略默认表名就是user；
public class User {
    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
  	//JPA提供的四种标准用法为：TABLE,SEQUENCE,IDENTITY,AUTO。
	//TABLE：使用一个特定的数据库表格来保存主键。
    //SEQUENCE：根据底层数据库的序列来生成主键，条件是数据库支持序列。
    //IDENTITY：主键由数据库自动生成（主要是自动增长型）
    //AUTO：主键由程序控制。
    private Long id;
    // 用户名
  	@Column(name = "username",length = 50) //这是和数据表对应的一个列
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
 
    //此处省略setter和getter方法... ...
}
~~~

### 4.编写UserRepository

~~~java
public interface UserRepository extends JpaRepository<User,Long>{
    public List<User> findAll();
}
~~~

**解释：**这个**JpaRepository**里面已经包括了基本的crud方法

### 5. 创建service进行简单的测试

~~~java
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserRepository userDao;

    @Override
    public void add(String username, String password) {
        User user = new User();
        user.setPassword(password);
        user.setUsername(username);
        userDao.save(user);
    }

    @Override
    public void deleteByName(String userName) {
        User user = new User();
        user.setUsername(userName);
        userDao.delete(user);
    }

    @Override
    public List<User> getAllUsers() {
        List<User> users = userDao.findAll();
        return users;
    }
}
~~~



### 6. 编写测试类

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest(classes=MySpringBootApplication.class)
public class JpaTest {
 
    @Autowired
    private UserService userService;
 
    @Test
    public void test(){
        List<User> users = userService.getAllUsers();
        System.out.println(users);
    }
  
    @Test
    public void testAdd(){

        userService.add("sihai","abc");
        userService.add("yan","abc");
    }
  
    @Test
    public void testQuery(){

        List<User> users = userService.getAllUsers();

        Assert.assertEquals(2, users.size());
    }

}
~~~

***注意：如果是jdk9，执行报错如下：***

![14error](D:\mytools\14error.png)

解决方案：手动导入对应的maven坐标，如下：

~~~xml
<!--jdk9需要导入如下坐标-->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
~~~

***JPA常见链接数据库错误***

##### 1.SSL连接安全错误：

~~~xml
Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set.For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'.You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification
~~~

##### 2.此错误需要修改spring.datasource.url的值：

~~~properties
错误前：spring.datasource.url=jdbc:mysql://localhost:3306/test
修改为：spring.datasource.url=jdbc:mysql://localhost:3306/test?useSSL=false
~~~

##### 3.乱码问题：

~~~properties
可能是数据库编码问题，或者插入的数据是乱码
spring.datasource.url=jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=utf-8
~~~

##### 4.事务错误:

***在持久层上添加上 @Transactional 注解即可。***

~~~java
javax.persistence.TransactionRequiredException: No EntityManager with actual transaction available for current thread - cannot reliably process 'remove' call
~~~

### 7.JPA分页

~~~java
package com.example.jpa.jpa_demo.controller;

import com.example.jpa.jpa_demo.dao.UserRepository;
import com.example.jpa.jpa_demo.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Manager {

    @Autowired
    private UserRepository userRepository;

    @RequestMapping("/getList")
    public String getList(Model model, Integer pageNum) {
        if (pageNum == null) {
            pageNum = 1;
        }
	   // 排序方式，这里是以“id”为标准进行降序
        Sort sort = Sort.by(Sort.Direction.ASC, "id");// 这里的"id"是实体类的主键，记住一定要是实体类的属性，而不能是数据库的字段

        Pageable pageable = PageRequest.of(pageNum - 1, 6, sort);// （当前页， 每页记录数， 排序方式）

        Page<User> all = userRepository.findAll(pageable);

        model.addAttribute("list",all);
        return "index";
    }
}

~~~

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!-- 新 Bootstrap 核心 CSS 文件 -->
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<table class="table table-striped table-hover">
    <tr>
        <td>id</td>
        <td>姓名</td>
        <td>年龄</td>
    </tr>
    <tr th:each="item : ${list}">
        <td th:text="${item.id}"></td>
        <td th:text="${item.name}"></td>
        <td th:text="${item.age}"></td>
    </tr>
    <tr>
        <td colspan="3">
            当前<span th:text="${list.number}+1"></span>页 |  总<span th:text="${list.totalPages}"></span>页 | 共<span th:text="${list.totalElements}"></span>条 | <a class="btn btn-primary" th:href="@{/getList}">首页</a> | <a class="btn btn-primary" th:href="@{/getList/(pageNum = ${list.hasPrevious()} ? ${list.number} : 1)}">上一页</a> | <a class="btn btn-primary" th:href="@{/getList/(pageNum = ${list.hasNext()} ? ${list.number} + 2 : ${list.totalPages})}">下一页</a> | <a class="btn btn-primary" th:href="@{/getList/(pageNum = ${list.totalPages})}">尾页</a>
        </td>
    </tr>
</table>
</body>
</html>
~~~

# 15.SpringBoot使用AOP

回顾AOP：

~~~html
AOP是Spring提供的两个核心功能之一：IOC（控制反转）,AOP（Aspect Oriented Programming 面向切面编程）；IOC有助于应用对象之间的解耦，AOP可以实现横切关注点和它所影响的对象之间的解耦；

AOP，它通过对既有的程序定义一个横向切入点，然后在其前后切入不同的执行内容，来拓展应用程序的功能，常见的用法如：打开事务和关闭事物，记录日志，统计接口时间等。AOP不会破坏原有的程序逻辑，拓展出的功能和原有程序是完全解耦的，因此，它可以很好的对业务逻辑的各个部分进行隔离，从而使业务逻辑的各个部分之间的耦合度大大降低，提高了部分程序的复用性和灵活性。
~~~

#### 1.创建一个SpringBoot的web项目

​	此处省略。。

#### 2.引入依赖

~~~xml
<!--aop-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~

**:引入此依赖后，我们不需要做其他特殊的配置，SpringBoot中的aop默认配置是默认开启的，我们引入即可使用。

#### 3.UserController

~~~java
package com.ssc.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Description:登陆
 */
@RestController
@RequestMapping("user")
public class UserController {

    @RequestMapping(value = "login",method = RequestMethod.GET)
    public String login(String userName){
        return "登陆成功！";
    }

    @RequestMapping(value = "register",method = RequestMethod.GET)
    public String register(){
        try {
            Thread.sleep(3000);
        }catch (InterruptedException ex){
            ex.printStackTrace();
        }
        return "注册成功！";
    }
}
~~~

#### 4.几个注解

~~~html
实现aop切面，主要有以下几个关键点需要了解：

@Aspect，此注解将一个类定义为一个切面类；

@Pointcut，此注解可以定义一个切入点，可以是规则表达式，也可以是某个package下的所有函数，也可以是一个注解等,其实就是执行条件，满足此条件的就切入；

然后可以定义切入位置，我们可以选择在切入点的不同位置进行切入：

@Before在切入点开始处切入内容；

@After在切入点结尾处切入内容；

@AfterReturning在切入点return内容之后切入内容（可以用来对返回值做一些处理）；

@Around在切入点前后切入内容，并自己控制何时执行切入点自身的内容；

@AfterThrowing用来处理当切入内容部分抛出异常之后的处理逻辑；
~~~

#### 5.实现切面

我们定义一个切面，然后定义一个切入点，切入点这个方法，不需要方法体，返回值是void，在切入点后定义一下切入条件，当满足条件时，就会在切入点的指定位置织入我们自定义的内容。

~~~java
package com.ssc.myAop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.jboss.logging.Logger;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Arrays;

/**
 * Description:切面类
 */

//定义一个切面类
@Aspect
@Component
public class WebLogAspect {

    private Logger logger = Logger.getLogger(getClass());

    /**
     * 定义一个切入点，可以是规则表达式，也可以是某个package下的所有函数，也可以是一个注解,其实就是执行条件，满足此条件的就切入
     * 这里是：com.ssc.controller包以及子包下的所有类的所有方法，返回类型任意，方法参数任意
     * 从前到后顺序解释：
     *  execution：在满足后面的条件的方法执行时触发；
     *  第一个*：表示返回值任意；
     *  com.ssc.controller..*..*：表示此路径下的任意类的任意方法
     *  (..):表示方法参数任意；
     */
    @Pointcut("execution(* com.ssc.controller..*.*(..))")
    public void webLog(){}

    /**
     * 在切入点之前执行
     * @param joinPoint 连接点，实在应用执行过程中能够插入切面的一个点，这个点可以是调用方法时，抛出异常时，甚至是修改一个字段时，切面代码可以利用这些连接点插入到应用的正常流程中，并添加新的行为，如日志、安全、事务、缓存等。
     */
    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint){

        ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = requestAttributes.getRequest();

        //记录请求日志
        logger.info("======>url；"+request.getRequestURL().toString());
        logger.info("======>httpMethod:"+request.getMethod());
        logger.info("======>ip:"+ request.getRemoteAddr());
        logger.info("======>classMethod : " + joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());
        logger.info("======>args："+ Arrays.toString(joinPoint.getArgs()));

    }


    /**
     * 在切入点之后执行
     * @param response
     */
    @AfterReturning(returning = "response",pointcut = "webLog()")
    public void doAfterReturn(Object response){
        //记录返回值
        logger.info("======>response:"+response);
    }


}
~~~



#### 6.启动项目，验证

我们启动项目，然后访问接口：http://localhost:8080/user/login?userName=ssc

#### 7.是aop来记录每个接口的调用时间

这个也非常简单，我们只需要在切入点前记录方法调用前的时间，再在切入点后使用当前时间减去调用开始的时间即可，我们这里引入ThreadLocal类，来记录方法开始调用的时间。

~~~java
package com.ssc.myAop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.jboss.logging.Logger;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Arrays;

/**
 * Description:切面类
 */

//定义一个切面类
@Aspect
@Component
public class WebLogAspect {

    private Logger logger = Logger.getLogger(getClass());

    /**记录方法调用开始时间*/
    ThreadLocal<Long> startTime = new ThreadLocal<>();

    /**
     * 定义一个切入点，可以是规则表达式，也可以是某个package下的所有函数，也可以是一个注解,其实就是执行条件，满足此条件的就切入
     * 这里是：com.ssc.controller包以及子包下的所有类的所有方法，返回类型任意，方法参数任意
     */
    @Pointcut("execution(* com.ssc.controller..*.*(..))")
    public void webLog(){}

    /**
     * 在切入点之前执行
     * @param joinPoint 连接点，实在应用执行过程中能够插入切面的一个点，这个点可以是调用方法时，抛出异常时，甚至是修改一个字段时，
     *                  切面代码可以利用这些连接点插入到应用的正常流程中，并添加新的行为，如日志、安全、事务、缓存等。
     */
    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint){
        //记录调用开始时间
        startTime.set(System.currentTimeMillis());

        ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = requestAttributes.getRequest();

        //记录请求日志
        logger.info("======>url；"+request.getRequestURL().toString());
        logger.info("======>httpMethod:"+request.getMethod());
        logger.info("======>ip:"+ request.getRemoteAddr());
        logger.info("======>classMethod : " + joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());
        logger.info("======>args："+ Arrays.toString(joinPoint.getArgs()));

    }


    /**
     * 在切入点之后执行
     * @param response
     */
    @AfterReturning(returning = "response",pointcut = "webLog()")
    public void doAfterReturn(JoinPoint joinPoint,Object response){

        //获取调用事件
        logger.info("======>"+joinPoint.getSignature().getDeclaringTypeName()+"."
                    +joinPoint.getSignature().getName()+"方法耗时："
                    +(System.currentTimeMillis()-startTime.get())/1000+"s");

        //记录返回值
        logger.info("======>response:"+response);
    }


}
~~~



#### 8.启动项目，验证

#### 9.AOP切面的优先级

~~~html
当我们对web层做多个切面时，会有一个问题：究竟先去切入谁？这里引入一个优先级的问题。处理方法非常简单，我们定义切面时，使用一个注解@Order(i),这个i决定着优先级的高低。

比如，现在定义了两个切面，一个@Order(3)，一个@Order(8)，那么执行时：

在切入点前的操作，按order的值由小到大执行，即：先@Order(3)，后@Order(8)；

在切入点后的操作，按order的值由大到小执行，即：先@Order(8)，后@Order(3)；
~~~

# 16.全文检索ElasticSearch

### 1.全文搜索

数据结构：

结构化：指具有固定格式或有限长度的数据，如数据库，元数据等

非结构化：指不定长或无固定格式的数据，如邮件，word文档等

非结构化数据的检索 例如字典

​	顺序扫描法(Serial Scanning) ：例如计算机查找某个文件，适用于文件较少的情况

​	全文搜索（Full-text Search）：将非结构化中的一部分信息提取出来，重新组合，使其变得有一定结构，实际就是将非结构化转变为有结构化数据，**<u>并建立索引</u>**

#### 1.1 概念

​	全文检索是一种将文件中所有文本与搜索项匹配的文字资料检索方法

#### 1.2 实现原理

​	建文本库->建立索引->执行搜索->过滤结果

#### 1.3 全文搜索实现技术

基于java的开源实现

- Lucene:有名的搜索引擎(巨人，发动机)
- ElasticSearch:基于Lucene引擎建立(踩着巨人攀登，车)，自身带有分布式协调管理功能，只支持json，主要提供RESTful接口，侧重于核心
- Solr:与Es同类型，利用zookeeper管理系统，支持json，xml等，传统搜索好于ES，实时搜索不如ES
- 场景适用不同，各有优势，目前市场上ES相对火爆一些

### 2.ElasticSearch简介

#### 2.1ElasticSearch是什么？

- 高度可扩展的开源全文搜索和分析引擎
- 快速地，近实时地对大数据进行存储，搜索和分析
- 用来支撑有复杂的数据搜索需求的企业级应用

#### 2.2ElasticSearch的特点

- 分布式实时文件存储，可将每一个字段存入索引，使其可以被检索到
- 实时分析的分布式搜索引擎
- 分布式：索引分拆成多个分片，每个分片可有零个或多个副本。集群中的每个数据节点都可承载一个或多个分片，并且协调和处理各种操作，每个分片都可进行读取和搜索；
- 高可用：基于分布式，其中一个分片有问题，其余分片可继续使用，不会因为单个有问题导致功能奔溃
- 多类型：支持多种数据类型操作
- 多用API：支持RESTful风格(/test/{id})，支持java原生api(/test?param=value)
- 面向文档：是用于存储、检索和管理面向文档和半结构化的数据。它是NoSQL数据库。其核心概念就是文档的观念，虽然不同的面向文档数据在实现这个定义上有差别，（在一般情况下）但它们在文档封装和数据编码上有一些标准格式。编码包括 XML、YAML、JSON 和 BSON，还有二进制格式（诸如PDF和MS office 文档）。
- 异步写入：相对于同步效率更高
- 近实时：接近实时速度读取，但还不是真正的实时，有轻微延时，在1s左右
- 基于Lucene(某一个功能割舍掉，然后加速另外一个功能)
- Apache协议(Apache Hadoop子项目)

### 3.ElasticSearch核心概念

- 近实时：接近实时速度读取，但还不是真正的实时，有轻微延时，在1s左右
- 集群：一个或者多个节点的集合，用来保存应用的全部数据，并提供基于全部节点的集成式的索引的搜索功能，每一个集群都有一个名称，默认为ElasticSearch
- 节点：集群中的一个单台服务器，用来保存数据，并参与整个集群的索引的搜索的操作，节点也有默认名称，可以自定义
- 索引：加快搜索速度
- 类型：对一个索引中包含文档进一步的细分，一般根据产品的特征，来划分不同的类型，例如：一般产品，虚拟产品等
- 文档：进行索引的基本单位，与索引类型是相对应的，每个具体的产品可以有一个文档与之对应，文档用json来表示
- 分片：存储数量较大时，会超出单个节点所能处理的范围，ES可以把索引分成多个分片，用来存储索引的部分数据，ES会自动负责分配和聚合，从可靠性角度出发，对于分片中的数据，还需要一个副本，ES每个索引都可以分成多个分片，并且会有多个副本，ES会自动管理这些节点中的分片和副本，对开发人员来说是透明的。
- 为什么要有这个分片：一方面需要水平的来分割或者缩放内容卷，其次可以并行在多个节点上，提高性能和吞吐量
- 副本：1.故障不可避免，为了使系统提供高可用性，将副本分配到不同的节点上 2.增加副本可以增加搜索量和吞吐量
- 总而言之：索引可以划分成多个分片，分片又可以设置多个副本，默认情况，ES每个索引会分配5个分片1个副本，意味着集群至少会有两个节点，会拥有5个分片，5个副本

### 4.ElasticSearch与springboot集成

#### 4.1配置环境：

- ElasticSearch 5.6.8
- Spring Data ElasticSearch 2.1.9.RELEASE
- JNA 5.3.1 访问操作系统原生应用，ElasticSearch 需要安装此依赖

~~~xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.1.9.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <version>2.1.9.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
  <version>2.1.9.RELEASE</version>
</dependency>
<!--添加对应依赖-->
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-elasticsearch -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
  <version>2.1.9.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/net.java.dev.jna/jna -->
<dependency>
  <groupId>net.java.dev.jna</groupId>
  <artifactId>jna</artifactId>
  <version>5.3.1</version>
</dependency>
<!-- springBoot JPA的起步依赖 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<!-- MySQL连接驱动 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <!-- optional=true,依赖不会传递，该项目依赖devtools；之后依赖boot项目的项目如果想要使用devtools，需要重新引入 -->
  <optional>true</optional>
</dependency>

~~~

#### 4.2修改application.properties

~~~properties
# elasticSearch服务地址
spring.data.elasticsearch.cluster-nodes=localhost:9300
# 设置连接超时时间
spring.data.elasticsearch.properties.transport.tcp.connect_timeout=120s
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.datasource.driver-class-name= com.mysql.jdbc.Driver
spring.datasource.url= jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=123


~~~

### 5.ElasticSearch实战

#### 5.1.编写 文档类EsBlog

~~~java
package com.ssc.domain.es;

import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;

import java.io.Serializable;

//文档EsBlog
//定义blog类实现序列化
@Document(indexName="blog",type="blog")// 文档
public class EsBlog implements Serializable {
    @Id
    private String id;//ES文档中的id定义为string
    private String title;
    private String summary;//摘要
    private String content;//正文内容

    protected EsBlog(){//JPA规范要求，防止直接使用
    }
    public EsBlog(String title,String summary,String content){
        this.title = title;
        this.summary = summary;
        this.content = content;
    }
    @Override
    public String toString(){
        return String.format("EsBlog[id='%s',title='%s',summary='%s',content='%s']",id,title,summary,content);
    }

    public String getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public String getSummary() {
        return summary;
    }

    public String getContent() {
        return content;
    }
}
~~~

#### 5.2资源库EsBlogRepository

##### 解析方法名创建查询，自定义查询

规则：find+全局修饰+By+实体的属性名称+限定词+连接词+ …(其它实体属性)+OrderBy+排序属性+排序方向。例如：

~~~java
//分页查询出符合姓名的记录,同理Sort也可以直接加上
public List<User> findByName(String name, Pageable pageable);

//正确
public List<User> findByUsernameLike(String username);
//错误
public List<User> aaa();
继承jpa，必须满足一些规则，规则如下：
全局修饰： Distinct， Top， First
关键词： IsNull， IsNotNull， Like， NotLike， Containing， In， NotIn，
IgnoreCase， Between， Equals， LessThan， GreaterThan， After， Before…
排序方向： Asc， Desc
连接词： And， Or
And — 等价于 SQL 中的 and 关键字，比如 findByUsernameAndPassword(String user, Striang pwd)；
Or — 等价于 SQL 中的 or 关键字，比如 findByUsernameOrAddress(String user, String addr)；
Between — 等价于 SQL 中的 between 关键字，比如 findBySalaryBetween(int max, int min)；
LessThan — 等价于 SQL 中的 “<”，比如 findBySalaryLessThan(int max)；
GreaterThan — 等价于 SQL 中的”>”，比如 findBySalaryGreaterThan(int min)；
IsNull — 等价于 SQL 中的 “is null”，比如 findByUsernameIsNull()；
IsNotNull — 等价于 SQL 中的 “is not null”，比如 findByUsernameIsNotNull()；
NotNull — 与 IsNotNull 等价；
Like — 等价于 SQL 中的 “like”，比如 findByUsernameLike(String user)；
NotLike — 等价于 SQL 中的 “not like”，比如 findByUsernameNotLike(String user)；
OrderBy — 等价于 SQL 中的 “order by”，比如 findByUsernameOrderBySalaryAsc(String user)；
Not — 等价于 SQL 中的 “！ =”，比如 findByUsernameNot(String user)；
In — 等价于 SQL 中的 “in”，比如 findByUsernameIn(Collection userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；
NotIn — 等价于 SQL 中的 “not in”，比如 findByUsernameNotIn(Collection userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数
//解释
Spring Data JPA框架在进行方法名解析时，会先把方法名多余的前缀截取掉，比如find，findBy，read，readBy，get，getBy，然后对剩下的部分进行解析。

假如创建如下的查询：findByUserName（），框架在解析该方法时，首先剔除findBy，然后对剩下的属性进行解析，假设查询实体为User

1：先判断userName（根据POJO规范，首字母变为小写）是否为查询实体的一个属性，如果是，则表示根据该属性进行查询;如果没有该属性，继续第二步;

2：从右往左截取第一个大写字母开头的字符串此处是Name），然后检查剩下的字符串是否为查询实体的一个属性，如果是，则表示根据该属性进行查询;如果没有该属性，则重复第二步，继续从右往左截取;最后假设用户为查询实体的一个属性;

3：接着处理剩下部分（UserName），先判断用户所对应的类型是否有userName属性，如果有，则表示该方法最终是根据“User.userName”的取值进行查询;否则继续按照步骤2的规则从右往左截取，最终表示根据“User.userName”的值进行查询。

4：可能会存在一种特殊情况，比如User包含一个的属性，也有一个userNameChange属性，此时会存在混合。可以明确在属性之间加上“_”以显式表达意思，比如“findByUser_NameChange ）“或者”findByUserName_Change（）“
~~~



~~~java
package com.ssc.repository.es;

import com.ssc.domain.es.EsBlog;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

//资源库EsBlogRepository
public interface EsBlogRepository extends ElasticsearchRepository<EsBlog,String> {
    //分页查询博客并去重
    Page<EsBlog> findDistinctEsBlogByTitleContainingOrSummaryContainingOrContentContaining(String title, String summary, String content, Pageable pageable);
}

~~~

#### 5.3资源库测试用例EsBlogRepositoryTest,开始测试前启动ElasticSearch

~~~java
package com.ssc;

import com.ssc.domain.es.EsBlog;
import com.ssc.repository.es.EsBlogRepository;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = ElasticserachDemoApplication.class)
public class ElasticserachDemoApplicationTests{
    @Autowired
    private EsBlogRepository esBlogRepository;

    @Before
    public void initRepositoryData(){
        //清除所有数据
        esBlogRepository.deleteAll();
        //初始化数据
        esBlogRepository.save(new EsBlog("登鹳雀楼","王之涣的登鹳雀楼","白日依山尽，黄河入海流。欲穷千里目，更上一层楼。"));
        esBlogRepository.save(new EsBlog("相思","王维的相思","红豆生南国,春来发几枝。愿君多采撷,此物最相思。"));
        esBlogRepository.save(new EsBlog("静夜思","李白的静夜思","床前明月光，疑是地上霜。举头望明月，低头思故乡。"));
    }
    @Test
    public void testFind(){
        Pageable pageable = new PageRequest(0,20);
        String title = "思";
        String summary = "相思";
        String content = "相思";
        Page<EsBlog> page = esBlogRepository.findDistinctEsBlogByTitleContainingOrSummaryContainingOrContentContaining
                (title,summary,content,pageable);
        Assert.assertEquals(page.getTotalElements(),2);

        for(EsBlog esblog : page.getContent()){
            System.out.println(esblog.toString());
        }
    }


}

~~~

#### 5.5控制器BlogController

~~~java
package com.ssc.controller.es;

import com.ssc.domain.es.EsBlog;
import com.ssc.repository.es.EsBlogRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class BlogController {
    @Autowired
    private EsBlogRepository esBlogRepository;

    @GetMapping("/list") //访问时记住传参
    public List<EsBlog> list(@RequestParam(value = "title") String title,
                             @RequestParam(value = "summary") String summary,
                             @RequestParam(value = "content") String content,
                             @RequestParam(value = "pageIndex", defaultValue = "0") Integer pageIndex,
                             @RequestParam(value = "pageSize", defaultValue = "10") Integer pageSize
    ) {
        //数据在测试用例中初始化
        Pageable pageable = new PageRequest(pageIndex, pageSize);
        Page<EsBlog> page = esBlogRepository.findDistinctEsBlogByTitleContainingOrSummaryContainingOrContentContaining
                (title, summary, content, pageable);
        return page.getContent();
    }
}

~~~

