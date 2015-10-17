title: Spring Boot
date: 2015-10-11 15:32:23
tags: Spring Boot
---
进公司的第一个项目在使用Spring Boot，所以这里就总结一下Spring Boot 使用时的一些小技能。
### SpringBoot简介
 我理解的SpringBoot是Spring的另一个产品，建立Spring MVC 之上的一个框架，与Spring MVC相比
 它在开始使用的时候就使用了Java 配置的方式（之前学的Spring MVC 还是使用xml进行的配置)。而且Spring
 Boot 集成了基本上所有的Web开发需要的基础jar包，比如说hibernate等。Spring Boot还为项目提供了很多的
 默认配置方式，比如说项目的静态资源的路径，视图解析器，还有ORM等。值得一提的是Spring Boot 使用的Spring Data JPA
 的ORM，这个ORM在项目使用中比Hibernate还要简单（至少在我们这个简单项目中是这个样子）。下面会有更详细的实例。

### 新建一个SpringBoot项目
详细的项目建立步骤可以参考Spring官网的 [Building an Application with Spring Boot](http://spring.io/guides/gs/spring-boot/)
#### 新建一个Gradle项目
  * 建立Gradle 项目文件结构
  * 给build.gradle 中添加设置以及依赖
```
group 'com.h5'
version '1.0-SNAPSHOT'

buildscript {
    ext {
        springbootVersion = '1.2.6.RELEASE'
    }

    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:$springbootVersion")
    }
}

ext {
    springbootVersion = '1.2.6.RELEASE'
}

apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'spring-boot'

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile "org.springframework.boot:spring-boot-starter-web:$springbootVersion"
    compile "org.springframework.boot:spring-boot-starter-data-jpa:$springbootVersion"
    compile "org.springframework.boot:spring-boot-starter-thymeleaf:$springbootVersion"
    compile "org.springframework.boot:spring-boot-starter-security:$springbootVersion"
    compile 'mysql:mysql-connector-java:5.1.36'

    testCompile 'junit:junit:4.12'
    testCompile 'commons-io:commons-io:2.4'
    testCompile "org.springframework.boot:spring-boot-starter-test:$springbootVersion"
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.5'
}
```
* 下载依赖
* 新建Application类，并添加SpringBoot配置，注意Application类要在所有其它Java类的上一层
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
* 编写第一个Controller类
```java
@RestController
public class HelloController {

    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }

}
```
* 启动项目`Spring bootRun`,在浏览器输入`http://localhost:8080`测试项目是否新建成功。
#### 为项目添加`Thymeleaf`模板引擎
* 在gradle中添加 `Thymeleaf`依赖`compile "org.springframework.boot:spring-boot-starter-security:$springbootVersion"`;
* SpringBoot 会自动配置使用 `Thymeleaf`模板引擎，并配置模板路径到`main/resources/templates`，后缀为 `.html`
* 在Controller中使用`@Controller`(不是`@RestController`),返回值为String，然后 return "template name";
