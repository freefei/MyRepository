###Spring Boot

####Spring Boot 是什么?

&nbsp;&nbsp;&nbsp;&nbsp;Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。Spring Boot充分利用了JavaConfig的配置模式以及“约定优于配置”的理念，能够极大的简化基于Spring MVC的Web应用和REST服务开发。从而使开发人员不再需要定义样板化的配置。
之前我们创建基于Spring的项目需要考虑添加哪些Spring依赖和第三方的依赖。使用Spring Boot后，我们可以以最小化的依赖开始spring应用。大多数Spring Boot应用需要很少的配置即可运行，然后通过java -jar运行启动或者传统的WAR部署。其也提供了命令行工具来直接运行Spring脚本（如groovy脚本）.

####环境准备
* 一个称手的文本编辑器（例如Vim、Emacs、Sublime Text）或者IDE（Eclipse、Idea Intellij）
* Java环境（JDK 1.7或以上版本）
* Maven 3.0+ / Gradle 1.12+

####Servlet容器

The following embedded servlet containers are supported out of the box:

|  Name |Servlet Version | Java Version |
|---------| ------|-----------|
| Tomcat 8  | 3.1 |    Java 7+   |
| Tomcat 7  | 3.0 |    Java 6+  |
| Jetty 9  | 3.1  |    Java 7+ |
| Jetty 8 | 3.0   |    Java 6+ |
| Undertow 1.1 | 3.1 | Java 7+   |

默认使用的是Tomcat如果换Jetty在pom.xml添加如下依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

Spring Boot 默认使用的是Tomcat 8、Jetty 9 如果要使用其他版本可以设置如下

```xml
<properties>
    <tomcat.version>7.0.59</tomcat.version>
</properties>

<properties>
    <jetty.version>8.1.15.v20140411</jetty.version>
    <jetty-jsp.version>2.2.0.v201112011158</jetty-jsp.version>
</properties>
```

Changing the Java version

```xml
<properties>
    <java.version>1.8</java.version>
</properties>
```

####命令行 Demo
&nbsp;&nbsp;&nbsp;&nbsp;从最根本上来讲，Spring Boot就是一些库的集合，它能够被任意项目的构建系统所使用。简便起见，该框架也提供了命令行界面，它可以用来运行和测试Boot应用。
框架的发布版本，包括集成的CLI（命令行界面），可以在Spring仓库中手动下载和安装。一种更为简便的方式是使用Groovy环境管理器（Groovy enVironment Manager，GVM），它会处理Boot版本的安装和管理。Boot及其CLI可以通过GVM的命令行gvm install springboot进行安装。在OS X上安装Boot可以使用Homebrew包管理器。为了完成安装，首先要使用brew tap pivotal/tap切换到Pivotal仓库中，然后执行brew install springboot命令，
Homebrew will install spring to /usr/local/bin。

####Example

&nbsp;&nbsp;&nbsp;&nbsp;创建一个.groovy结尾的文件。

```java
@RestController
class App {
  @RequestMapping("/")
  String home() {
    "hello world"
  }
}
```

* spring run App.groovy
* 加上 --watch参数可以实现热部署

*提供默认应用上下文以及嵌入式容器的这些过程，能够让开发人员更加关注于开发应用以及业务逻辑，从而不用再关心繁琐的样板式配置*

####IDEA Demo
&nbsp;&nbsp;&nbsp;&nbsp;要进行打包和分发的工程会依赖于像Maven或Gradle这样的构建系统。为了简化依赖图，Boot的功能是模块化的，通过导入Boot所谓的“starter”模块，可以将许多的依赖添加到工程之中。为了更容易地管理依赖版本和使用默认配置，框架提供了一个parent POM，工程可以继承它。Spring Boot工程的样例POM文件定义如下所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

	<!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.2.3.RELEASE</version>
    </parent>

    <groupId>com.renfei</groupId>
    <artifactId>spring-boot-test</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>

        <!-- Add typical dependencies for a web application --> 
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.12.2</version>
        </dependency>
    </dependencies>

   <!-- Package as an executable JAR --> 
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


</project>
```

&nbsp;&nbsp;&nbsp;&nbsp;继承spring-boot-starter-parent后我们可以继承一些默认的依赖，这样就无需添加一堆相应的依赖，把依赖配置最小化；spring-boot-starter-web提供了对web的支持，spring-boot-maven-plugin提供了直接运行项目的插件，它会对Maven打包形成的JAR进行二次修改，最终产生符合我们要求的内容结构,我们可以直接mvn spring-boot:run运行。

####Example

```java

package com.renfei.controller;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * Created with IntelliJ IDEA
 * Author: songrenfei
 * Date: 15/9/7
 * Time: 下午11:44
 */
@Controller
@EnableAutoConfiguration
public class HelloWorld {
    @RequestMapping("/sayHello")
    @ResponseBody
    public String sayHello(){
        return "Hello World";
    }
    public static void main(String[] args) {
        SpringApplication.run(HelloWorld.class, args);
    }

}

```
&nbsp;&nbsp;&nbsp;&nbsp;运行应用：mvn spring-boot:run或在IDE中运行main()方法，在浏览器中访问http://localhost:8080/sayHello，Hello World!就出现在了页面中。

SpringApplication是Spring Boot框架中描述Spring应用的类，它的run()方法会创建一个Spring应用上下文（Application Context）。另一方面它会扫描当前应用类路径上的依赖，例如本例中发现spring-webmvc（由 spring-boot-starter-web传递引入）在类路径中，那么Spring Boot会判断这是一个Web应用，并启动一个内嵌的Servlet容器（默认是Tomcat）用于处理HTTP请求。

@Controller表示这个一个controller类（from spring mvc）；

@EnableAutoConfiguration声明让spring boot自动给程序进行必要的配置根据添加的jar依赖（from spring boot）；

@RequestMapping("/sayHello")表示通过/sayHello可以访问的方法（from spring mvc）；

@ResponseBody 表示将结果直接返回给调用者（from spring mvc）.

容器的默认端口是**8080**，如果要更改端口号

只需添加src/main/resources/application.properties 文件 在文件中加入server.port:9000 即可。


####自定义Banner
 通过在classpath下添加一个banner.txt或设置banner.location来指定相应的文件可以改变启动过程中打印的banner。

 以下变量可以在banner.txt中获取

|  Variable |   说明 |
|---------| ------|
| ${application.version}  |MANIFEST.MF中声明的应用版本号，例如1.0
| ${application.formatted-version}  | MANIFEST.MF中声明的被格式化后的应用版本号（被括号包裹且以v作为前缀），用于显示，例如(v1.0)
| ${spring-boot.version}  | 正在使用的Spring Boot版本号，例如1.2.2.BUILD-SNAPSHOT 
| ${spring-boot.formatted-version} | 正在使用的Spring Boot被格式化后的版本号（被括号包裹且以v作为前缀）, 用于显示，例如(v1.2.2.BUILD-SNAPSHOT)

如果不想显示启动Banner

```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MySpringConfiguration.class);
    app.setShowBanner(false);
    app.run(args);
}
```



####组织代码

定义main应用类，通常建议将main应用类放在位于其他类上面的根包（root package）中。通常使用@EnableAutoConfiguration、@Configuration、@ComponentScan开启注解扫描并自动注册相应的注解Bean，也可只加@SpringBootApplication一个注解，整个项目只需一个启动main函数。

代码如下：
```java
package com.renfei;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.boot.autoconfigure.SpringBootApplication;


/**
 * Created with IntelliJ IDEA
 * Author: songrenfei
 * Date: 15/9/7
 * Time: 下午11:54
 */

@Configuration  //配置控制
@ComponentScan  //组件扫描
@EnableAutoConfiguration  //启用自动配置
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class);
    }
}

```

下面是一个典型的结构：

```text
com
 +- example
     +- myproject
         +- Application.java
         |
         +- domain
         |   +- Customer.java
         |   +- CustomerRepository.java
         |
         +- service
         |   +- CustomerService.java
         |
         +- web
             +- CustomerController.java
```

####模板引擎

Spring Boot为以下的模板引擎提供自动配置支持：

* FreeMarker
* Groovy
* Thymeleaf
* Velocity
* Mustache

java代码

```java
 @RequestMapping("/hello/{name}")
    public String hello(@PathVariable("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello";
    }
```
添加pom依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
当你使用这些引擎的任何一种，并采用默认的配置，模板将会从src/main/resources/templates目录下自动加载。

在默认的模板文件夹src/main/resources/templates/目录下添加一个模板文件hello.html：

```html
<!DOCTYPE HTML>
	<html xmlns:th="http://www.thymeleaf.org">
	<head>
	    <title>Getting Started: Serving Web Content</title>
	    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	</head>
	<body>
	    <p th:text="'Hello, ' + ${name} + '!'" />
	</body>
	</html>
```

####自定义错误页面

Spring Boot安装了一个'whitelabel'错误页面，当遇到一个服务器错误那就会在客户端浏览器中看到该页面，可以设置error.whitelabel.enabled=false来关闭该功能。
也可以通过一个名称为error的View来替换该页面。

####创建可执行jar

&nbsp;&nbsp;&nbsp;&nbsp;为了发布版本而构建工程时，Boot的Maven和Gradle插件可以嵌入（hook）到这些构建系统的打包过程中，以生成可执行的“胖jar包（fat jar）”，这种jar包含了工程的所有依赖并且能够以可运行jar的方式执行。使用Maven打包Boot应用只需运行mvn package命令，与之类似，使用Gradle时，执行gradle build命令将会在构建的目标地址下生成可运行的jar，我们使用java –jar命令就可以运行这个JAR包了。解压对应的jar文件可以可看到如下结构jar xf *.jar

![jar](http://7u2myi.com1.z0.glb.clouddn.com/spring-boot-jar.png)

####传统部署

创建一个可部署的war文件

主类改为继承SpringBootServletInitializer即可
```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

}
```

更新你的构建配置

```xml
<packaging>war</packaging>
```

该过程最后的一步是确保内嵌的servlet容器不能干扰war包将部署的servlet容器。为了达到这个目的，你需要将内嵌容器的依赖标记为provided。

```xml
<dependencies>
    <!-- … -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <scope>provided</scope>
    </dependency>
    <!-- … -->
</dependencies>
```




####热部署

* 方法一 利用spring-boot-devtools模块 可以参考[例子](http://blog.csdn.net/zhoujinyu0713/article/details/46843115)

由于devtools仅在[Spring milestone repository. ](http://repo.springsource.org/milestone/) 仓库中提供,所以需要在POM.XML 文件中配置仓库地址。

```xml
 <parent>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>1.3.0.M2</version>  
</parent>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>


<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>http://repo.spring.io/milestone</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```
原理是使用了两个ClassLoader，一个Classloader加载那些不会改变的类（第三方Jar包），另一个ClassLoader加载会更改的类，称为  restart ClassLoader
,这样在有代码更改的时候，原来的restart ClassLoader 被丢弃，重新创建一个restart ClassLoader，由于需要加载的类相比较少，所以实现了较快的重启时间（5秒以内）。


* 方法二 结合spring-loaded来实现热部署

**手动配置**

下载[spring-loaded](http://repo.spring.io/release/org/springframework/springloaded/1.2.4.RELEASE/springloaded-1.2.4.RELEASE.jar)

Git[spring-loaded](https://github.com/spring-projects/spring-loaded)

配置如图：-javaagent:/Users/songrenfei/Downloads/springloaded-1.2.4.RELEASE.jar -noverify

![说明](http://7u2myi.com1.z0.glb.clouddn.com/auto-load.png)

**maven配置**

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>springloaded</artifactId>
                        <version>1.2.4.RELEASE</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
```

**静态文件**

在不重启容器的情况下重新加载Thymeleaf模板
如果你正在使用Thymeleaf，那就将spring.thymeleaf.cache设置为false。

在不重启容器的情况下重新加载FreeMarker模板
如果你正在使用FreeMarker，那就将spring.freemarker.cache设置为false。

在不重启容器的情况下重新加载Groovy模板
如果你正在使用Groovy模板，那就将spring.groovy.template.cache设置为false。

在不重启容器的情况下重新加载Velocity模板
如果你正在使用Velocity，那就将spring.velocity.cache设置为false。

**修改完代码只需编译下就可以了**

####访问数据库

**Embedded Database Support**

Spring Boot can auto-configure embedded H2, HSQL and Derby databases. You don’t need to provide any connection URLs, simply include a build dependency to the embedded database that you want to use.

For example, typical POM dependencies would be:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <scope>runtime</scope>
</dependency>
```


* Using MySQL in Spring Boot via Spring Data JPA

**Dependencies** in pom.xml

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
  </dependency>
</dependencies>
```

**Properties file** in src/main/resources/application.properties

```properties
# DataSource settings: set here your own configurations for the database 
spring.datasource.url = jdbc:mysql://localhost:3306/spring_boot
spring.datasource.username = root
spring.datasource.password = anywhere
# 可以去掉，spring-boot可以根据上边的url自动推导出来
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

# Keep the connection alive if idle for a long time (needed in production)
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

# Show or not log for each sql query
spring.jpa.show-sql = true
```
**Create an entity**

```java
// Imports ...

@Entity
@Table(name = "users")
public class User {

  // Entity's fields (private)
  
  // An autogenerated id (unique for each user in the db)
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private long id;
  
  // The user's email
  @NotNull
  private String email;
  
  // The user's name
  @NotNull
  private String name;


  // Public methods
  
  public User() { }

  public User(long id) { 
    this.id = id;
  }
  
  public User(String email, String name) {
    this.email = email;
    this.name = name;
  }

  // Getter and setter methods
  // ...

}
```
为了启用代表User对象的通用数据，我们只需创建一个名为schema.sql或data.sql的文件，并将其包含在classpath之中。这个文件会在模式创建完成之后执行.
```sql
schema.sql

DROP TABLE IF EXISTS `users`;
CREATE TABLE `users` (
  `id` varchar(50) NOT NULL COMMENT 'id',
  `name` varchar(50) DEFAULT NULL COMMENT '用户名',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

data.sql

INSERT INTO `users` (`id`, `name`, `email`)
VALUES
  ('1', 'songrenfei', 'songrenfei@163.com'),
  ('2', 'lxy', 'songrenfei@163.com');

```

**The Data Access Object**

```java
package com.renfei.repository;

import com.renfei.model.User;
import org.springframework.data.jpa.repository.JpaRepository;


/**
 * User的JPA Repository
 *
 * @author songrenfei
 * @version 1.0.0
 */
//@RepositoryDefinition(domainClass=User.class,idClass = Long.class)
public interface UserRepository extends JpaRepository<User,Long>{

  public User findById(Long id);
  
}

}
```
CrudRepository提供了一些通用的接口方法来创建、查询、更新以及删除对象和对象集合。应用所需的其他特定功能可以按照Spring Data的Repository[开发约定](http://docs.spring.io/spring-data/jpa/docs/1.5.0.RC1/reference/html/jpa.repositories.html#jpa.query-methods)进行定义。一旦UserRepository接口创建成功，Boot的spring-data-jpa层会在工程中探测到它，并将其添加到Spring应用上下文之中，这样对于controller和sevice对象来说，它就成为可以进行自动注入的可选对象。

**A controller for testing**

```java
package com.renfei.controller;

import com.renfei.model.User;
import com.renfei.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * Created with IntelliJ IDEA
 * Author: songrenfei
 * Date: 15/9/7
 * Time: 下午11:52
 */

@EnableAutoConfiguration
@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    UserRepository userRepository;

    @RequestMapping("/all")
    @ResponseBody
    public List<User> findAll() {
        List<User> list = new ArrayList<User>();
        try {
            Iterable<User> users = userRepository.findAll();
            for (Iterator<User> iter =users.iterator();iter.hasNext();){
                list.add(iter.next());

            }
            return list;
        }
        catch (Exception ex) {
            System.out.print("查询失败");
        }
        return list;
    }
}

```

####输出xml格式

```xml
 <!-- out for xml-->
  <dependency>
      <groupId>com.fasterxml.jackson.dataformat</groupId>
      <artifactId>jackson-dataformat-xml</artifactId>
  </dependency> 
```


####Working with NoSQL technologies
 including MongoDB, Neo4J, Elasticsearch, Solr, Redis, Gemfire, Couchbase and Cassandra. 


* Redis

```xml
 <!-- redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
</dependency>
```
默认连接的是localhost:6379

可以在properties文件中自定义

```text
# REDIS (RedisProperties)
spring.redis.database= # database name
spring.redis.host=localhost # server host
spring.redis.password= # server password
spring.redis.port=6379 # connection port
spring.redis.pool.max-idle=8 # pool settings ...
spring.redis.pool.min-idle=0
spring.redis.pool.max-active=8
spring.redis.pool.max-wait=-1
spring.redis.sentinel.master= # name of Redis server
spring.redis.sentinel.nodes= # comma-separated list of host:port pairs

```

简单例子

```java
@Service
public class UserService {

    @Autowired
    private StringRedisTemplate template;

    public void setUserName(Long id){

        User u = userRepository.findById(id);

        template.opsForValue().set(userNameKey(id),u.getName());
    }

    public String getUserName(Long id){

        return template.opsForValue().get(userNameKey(id));

    }

    public void deleteUserName(Long id){
        template.opsForValue().getOperations().delete(userNameKey(id));
    }


    private String userNameKey(Long id){
        return "user-name-"+String.valueOf(id);
    }
}

```

####外部化配置

在application.properties中添加自己的配置

```text
my.name=songrenfei
my.age=18
```

定义配置类

```java
package com.renfei.configuration;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

/**
 * 使用自定义的properties
 * 最后注意在spring Boot入口类加上@EnableConfigurationProperties
 * Created with IntelliJ IDEA
 * Author: songrenfei
 * Date: 15/9/20
 * Time: 下午9:16
 */
@Data
@ConfigurationProperties(prefix = "my") //若新建了properties文件则通过locations指定位置
public class MyConfig {
    private String name;
    private Integer age;
}

```
最后注意在spring Boot入口类加上@EnableConfigurationProperties

```java
@EnableConfigurationProperties(MyConfig.class)
@SpringBootApplication
public class Application extends SpringBootServletInitializer {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

}
```


####生产环境运维支持

与开发和测试环境不同的是，当应用部署到生产环境时，需要各种运维相关的功能的支持，包括性能指标、运行信息和应用管理等。所有这些功能都有很多技术和开源库可以实现。Spring Boot 对这些运维相关的功能进行了整合，形成了一个功能完备和可定制的功能集，称之为 Actuator。只需要在 POM 文件中增加对 “org.springframe.boot:spring-boot-starter-actuator” 的依赖就可以添加 Actuator。Actuator 在添加之后，会自动暴露一些 HTTP 服务来提供这些信息。


```xml
<!-- Production ready features to help you monitor and manage your application. -->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
  </dependency>
```


|  名称 |说明 | 是否包含敏感信息 |
|---------| ------|-----------|
| autoconfig  | 显示 Spring Boot 自动配置的信息。 |    是   |
| beans  | 显示应用中包含的 Spring bean 的信息。 |    是 |
| configprops  | 显示应用中的配置参数的实际值。  |    是 |
| dump | 生成一个 thread dump。  |    是 |
| env | 显示从 ConfigurableEnvironment 得到的环境配置信息。 | 是  
| health | 显示应用的健康状态信息。| 否 |
| info | 显示应用的基本信息。| 否 |
| metrics | 显示应用的性能指标。 | 是 |
| mappings | 显示 Spring MVC 应用中通过“@RequestMapping”添加的路径映射。| 是|
| shutdown | 关闭应用。 | 是 |
| trace | 显示应用相关的跟踪（trace）信息。 | 是 |



在一个单独的应用中，执行器的HTTP端口默认和主HTTP端口相同。想要让应用监听不同的端口，你可以设置外部属性management.port。为了监听一个完全不同的网络地址（比如，你有一个用于管理的内部网络和一个用于用户应用程序的外部网络），你可以将management.address设置为一个可用的IP地址，然后将服务器绑定到该地址。

```text
# ----------------------------------------
# ACTUATOR PROPERTIES
# ----------------------------------------

# MANAGEMENT HTTP SERVER (ManagementServerProperties)
management.port= # defaults to 'server.port'
management.address= # bind to a specific NIC
management.context-path= # default to '/'
management.add-application-context-header= # default to true
management.security.enabled=true # enable security
management.security.role=ADMIN # role required to access the management endpoint
management.security.sessions=stateless # session creating policy to use (always, never, if_required, stateless)
```

**添加权限**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Boot会为你提供一个默认的用户账号user和默认角色USER，并且会在应用启动的时候在控制台上输出随机生成的密码。就像Boot的其他功能那样，对于内置的user账号，我们可以很容易地指定不同的用户名和密码

```text
# SECURITY (SecurityProperties)
security.user.name=user # login username
security.user.password= # login password
security.user.role=USER # role assigned to the user
security.require-ssl=false # advanced settings ...
security.enable-csrf=false
security.basic.enabled=true
security.basic.realm=Spring
security.basic.path= # /**
security.basic.authorize-mode= # ROLE, AUTHENTICATED, NONE
security.filter-order=0
security.headers.xss=false
security.headers.cache=false
security.headers.frame=false
security.headers.content-type=false
security.headers.hsts=all # none / domain / all
security.sessions=stateless # always / never / if_required / stateless
security.ignored= # Comma-separated list of paths to exclude from the default secured paths
```

####日志
Spring Boot内部日志系统使用的是Commons Logging，但开放底层的日志实现。默认为会Java Util Logging, Log4J, Log4J2和Logback提供配置。每种情况下都会预先配置使用控制台输出，也可以使用可选的文件输出。
默认情况下，如果你使用'Starter POMs'，那么就会使用Logback记录日志。

**日志格式**

Spring Boot默认的日志输出格式如下

```text
2014-03-05 10:57:51.112  INFO 45469 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/7.0.52
2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1358 ms
2014-03-05 10:57:51.698  INFO 45469 --- [ost-startStop-1] o.s.b.c.e.ServletRegistrationBean        : Mapping servlet: 'dispatcherServlet' to [/]
2014-03-05 10:57:51.702  INFO 45469 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
```

输出的节点（items）如下：

```text

Date and Time — 精确到毫秒，且易于排序。
Log Level — ERROR, WARN, INFO, DEBUG or TRACE.Process ID.
一个用于区分实际日志信息开头的---分隔符。线程名 - 包括在方括号中（控制台输出可能会被截断）。
Logger name — 通常是源class的类名（缩写）。
The log message.

```
*Logback does not have a FATAL level (it is mapped to ERROR)*

* 控制台输出

默认的日志配置会在写日志消息时将它们回显到控制台。默认，ERROR, WARN和INFO级别的消息会被记录。可以在启动应用时，通过--debug标识开启控制台的DEBUG级别日志记录。

$ java -jar myapp.jar --debug

*you can also specify debug=true in your application.properties.*

* 文件输出

set logging.file or logging.path property in your application.properties.


|  名称 |说明 | 
|---------| ------|
| logging.file  | 写到特定的日志文件里，名称可以是一个精确的位置或相对于当前目录 | 
| logging.path  | 写到特定文件夹下的spring.log里，名称可以是一个精确的位置或相对于当前目录 |

* 日志级别

Example application.properties
```text
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR
```
* 自定义日志配置

| 日志系统 |定制 | 
|---------| ------|
| Logback  | logback.xml | 
| Log4j  | log4j.properties或log4j.xml |
| Log4j2  | log4j2.xml |
| JDK (Java Util Logging)  | logging.properties|


```xml
<?xml version="1.0" encoding="UTF-8" ?>

<configuration>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>/Users/songrenfei/logs/settlement.log</file>
        <encoder>
            <pattern>%date %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily -->
            <fileNamePattern>/Users/songrenfei/logs/settlement-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>7</maxHistory>
        </rollingPolicy>
    </appender>


    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!-- encoder defaults to ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
        <encoder>
            <pattern>%date %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
        </encoder>
        <!-- Only log level INFO and above -->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>DEBUG</level>
        </filter>
    </appender>

    <!-- Strictly speaking, the level attribute is not necessary since -->
    <!-- the level of the root level is set to DEBUG by default.       -->
    <root level="INFO">
        <appender-ref ref="STDOUT"/>
    </root>

</configuration>
```

* Configure Log4j for logging

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j</artifactId>
</dependency>
```


####缓存

Supported cache providers
* Generic
* JCache
* EhCache 2.x
* Hazelcast
* Infinispan
* Redis
* Guava
* Simple


####消息服务

 JMS
* ActiveMQ support
* Artemis support
* HornetQ support
* Using a JNDI ConnectionFactory
* Sending a message
* Receiving a message
* AMQP
* RabbitMQ support
* Sending a message
* Receiving a message

####测试

 Test scope dependencies
* Testing Spring applications
* Testing Spring Boot applications
* Using Spock to test Spring Boot applications
* Test utilities





####总结

**开发微服务**

Boot对Spring应用的开发进行了简化，提供了模块化方式导入依赖的能力，强调了开发RESTful Web服务的功能并提供了生成可运行jar的能力，这一切都清晰地表明在开发可部署的微服务方面Boot框架是一个强大的工具。正如前面的例子所示，借助于Boot，让一个RESTful Web工程运行起来是一件很容易的事情；在企业级基础设施领域，微服务是一种越来越流行的应用架构，因为它能够实现快速开发、更小的代码库、企业级集成以及模块化部署

Spring Boot是新一代Spring应用的开发框架，它能够快速的进行应用开发，让人忘记传统的繁琐配置，更加专注于业务逻辑。

























