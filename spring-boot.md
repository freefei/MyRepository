###Spring Boot

####Spring Boot 是什么?

&nbsp;&nbsp;&nbsp;&nbsp;Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，即Spring Boot充分利用了JavaConfig的配置模式以及“约定优于配置”的理念，能够极大的简化基于Spring MVC的Web应用和REST服务开发。从而使开发人员不再需要定义样板化的配置。
之前我们创建基于Spring的项目需要考虑添加哪些Spring依赖和第三方的依赖。使用Spring Boot后，我们可以以最小化的依赖开始spring应用。大多数Spring Boot应用需要很少的配置即可运行，比如我们可以创建独立独立大Java应用，然后通过java -jar运行启动或者传统的WAR部署。其也提供了命令行工具来直接运行Spring脚本（如groovy脚本），Boot致力于在蓬勃发展的快速应用开发领域（rapid application development）成为领导者。

&nbsp;&nbsp;&nbsp;&nbsp;在追求开发体验的提升方面，Spring Boot，甚至可以说整个Spring生态系统都使用到了**Groovy编程语言**。Boot所提供的众多便捷功能，都是借助于Groovy强大的MetaObject协议、可插拔的AST转换过程以及内置的依赖解决方案引擎所实现的。在其核心的编译模型之中，Boot使用Groovy来构建工程文件，所以它可以使用通用的导入和样板方法（如类的main方法）对类所生成的字节码进行装饰（decorate）。这样使用Boot编写的应用就能保持非常简洁，却依然可以提供众多的功能。

####环境准备
* 一个称手的文本编辑器（例如Vim、Emacs、Sublime Text）或者IDE（Eclipse、Idea Intellij）
* Java环境（JDK 1.7或以上版本）
* Maven 3.0+（Eclipse和Idea IntelliJ内置，如果使用IDE并且不使用命令行工具可以不安装）

####命令行环境
&nbsp;&nbsp;&nbsp;&nbsp;从最根本上来讲，Spring Boot就是一些库的集合，它能够被任意项目的构建系统所使用。简便起见，该框架也提供了命令行界面，它可以用来运行和测试Boot应用。
框架的发布版本，包括集成的CLI（命令行界面），可以在Spring仓库中手动下载和安装。一种更为简便的方式是使用Groovy环境管理器（Groovy enVironment Manager，GVM），它会处理Boot版本的安装和管理。Boot及其CLI可以通过GVM的命令行gvm install springboot进行安装。在OS X上安装Boot可以使用Homebrew包管理器。为了完成安装，首先要使用brew tap pivotal/tap切换到Pivotal仓库中，然后执行brew install springboot命令。

####Example

&nbsp;&nbsp;&nbsp;&nbsp;Spring Boot在刚刚公开宣布之后就将一个样例发布到了**Twitter**上，它目前成为了最流行的一个应用样例。它的全部描述如程序清单1.2所示，一个非常简单的Groovy文件可以生成功能强大的以Spring为后端的web应用。

```java
@RestController
class App {
  @RequestMapping("/")
  String home() {
    "hello word"
  }
}
```

&nbsp;&nbsp;&nbsp;&nbsp;这个应用可以通过spring run App.groovy命令在Spring Boot CLI中运行。Boot会分析文件并根据各种“编译器自动配置（compiler auto-configuration）”标示符来确定其意图是生成Web应用。然后，它会在一个嵌入式的Tomcat中启动Spring应用上下文，并且使用默认的**8080**端口。打开浏览器并导航到给定的URL，随后将会加载一个页面并展现简单的文本响应：“hello”。提供默认应用上下文以及嵌入式容器的这些过程，能够让开发人员更加关注于开发应用以及业务逻辑，从而不用再关心繁琐的样板式配置

####IDEA环境
&nbsp;&nbsp;&nbsp;&nbsp;要进行打包和分发的工程会依赖于像Maven或Gradle这样的构建系统。为了简化依赖图，Boot的功能是模块化的，通过导入Boot所谓的“starter”模块，可以将许多的依赖添加到工程之中。为了更容易地管理依赖版本和使用默认配置，框架提供了一个parent POM，工程可以继承它。Spring Boot工程的样例POM文件定义如程序清单1所示。

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
&nbsp;&nbsp;&nbsp;&nbsp;运行应用：mvn spring-boot:run或在IDE中运行main()方法，在浏览器中访问http://localhost:8080/sayHello，Hello World!就出现在了页面中。只用了区区十几行Java代码，一个Hello World应用就可以正确运行了，那么这段代码究竟做了什么呢？我们从程序的入口SpringApplication.run(Application.class, args);开始分析：

SpringApplication是Spring Boot框架中描述Spring应用的类，它的run()方法会创建一个Spring应用上下文（Application Context）。另一方面它会扫描当前应用类路径上的依赖，例如本例中发现spring-webmvc（由 spring-boot-starter-web传递引入）在类路径中，那么Spring Boot会判断这是一个Web应用，并启动一个内嵌的Servlet容器（默认是Tomcat）用于处理HTTP请求。

@Controller表示这个一个controller类（from spring mvc）；（from spring mvc）；

@EnableAutoConfiguration声明让spring boot自动给程序进行必要的配置（from spring boot）；

@RequestMapping("/sayHello")表示通过/sayHello可以访问的方法（from spring mvc）；

@ResponseBody 表示将结果直接返回给调用者（from spring mvc）.

####运行多个控制器

上边通过加上@EnableAutoConfiguration开启自动配置，然后通过SpringApplication.run(UserController.class);运行这个控制器；这种方式只运行一个控制器比较方便，
如果存在多个控制器即多个bean时，通过@Configuration+@ComponentScan开启注解扫描并自动注册相应的注解Bean，也可只加@SpringBootApplication一个注解，整个项目只需一个启动main函数。

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

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class);
    }
}

```

####模板渲染

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

接下来需要在默认的模板文件夹src/main/resources/templates/目录下添加一个模板文件hello.html：

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

####jar文件启动

&nbsp;&nbsp;&nbsp;&nbsp;为了发布版本而构建工程时，Boot的Maven和Gradle插件可以嵌入（hook）到这些构建系统的打包过程中，以生成可执行的“胖jar包（fat jar）”，这种jar包含了工程的所有依赖并且能够以可运行jar的方式执行。使用Maven打包Boot应用只需运行mvn package命令，与之类似，使用Gradle时，执行gradle build命令将会在构建的目标地址下生成可运行的jar，我们使用java –jar命令就可以运行这个JAR包了。解压对应的jar文件可以可看到如下结构jar xf *.jar

![jar](http://7u2myi.com1.z0.glb.clouddn.com/spring-boot-jar.png)


















