# 1 springboot 入门

## 1、Spring Boot 简介

> 简化Spring应用开发的一个框架；
>
> 整个Spring技术栈的一个大整合；
>
> J2EE开发的一站式解决方案；

## 2、微服务

2014，martin fowler

微服务：架构风格（服务微化）

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；

单体应用：ALL IN ONE

微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

[详细参照微服务文档](https://martinfowler.com/articles/microservices.html#MicroservicesAndSoa)

## 3、环境准备

<http://www.gulixueyuan.com/> 谷粒学院

环境约束

- jdk1.8：Spring Boot 推荐jdk1.7及以上；java version "1.8.0_112"
- maven3.x：maven 3.3以上版本；Apache Maven 3.3.9
- IntelliJIDEA2017：IntelliJ IDEA 2017.2.2 x64、STS
- SpringBoot 1.5.9.RELEASE：1.5.9；

统一环境；

### 1、MAVEN设置；

给maven 的settings.xml配置文件的profiles标签添加

```
<profile>
  <id>jdk-1.8</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>1.8</jdk>
  </activation>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>
```

### 2、IDEA设置

整合maven进来；

## 4 springboot hello world

### 1. 创建maven工程

### 2. 导入spring boot相关依赖

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

### 3. 编写主程序，启动spring boot应用

```java
/**
 * @SpringBootApplication 标记为一个spring boot 应用入口类
 * 具有以下三个注解的功能:
 *  @SpringBootConfiguration
 * 	@EnableAutoConfiguration
 * 	@ComponentScan 组件扫描时扫描入口类同级别目录下的包，超出这个目录下的包无法扫描
 */
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        // spring 应用启动入口
        SpringApplication.run(MainApplication.class, args);
    }
}
```

### 4. 编写相关业务逻辑，controller,service, dao等

```java
/**
 * @RestController 具有restful风格的控制器
 *具有以下两个个注解的功能:
 * @Controller
 * @ResponseBody
 */
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello() {
        return "hello world";
    }
}
```

###  5. 运行主程序并测试

1.启动MainApplication类

2.浏览器访问：http://localhost:8080/hello

### 6. 简化部署

1.导入maven插件

在pom.xml文件引用插件

```xml
<!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

2.打开maven projects ->lifecycle运行package

3.找到jar,在命令行执行java -jar xxx.jar

Spring Boot 使用嵌入式的Tomcat无需再配置Tomcat

## 5 hello world探究

#### 1、父项目

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>
```

他的父项目是：

```
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>1.5.9.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

这是真正管理Spring Boot应用里面所依赖的版本

Spring Boot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

#### 2、启动器

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**spring-boot-starter-web**：

spring-boot-starter：spring-boot场景启动器，帮我们导入了web模块正常运行所依赖的组件；

点击进去可以看到帮我们引入很多web相关的依赖

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starters</artifactId>
		<version>1.5.4.RELEASE</version>
	</parent>
	<artifactId>spring-boot-starter-web</artifactId>
	<name>Spring Boot Web Starter</name>
	<description>Starter for building web, including RESTful, applications using Spring
		MVC. Uses Tomcat as the default embedded container</description>
	<url>http://projects.spring.io/spring-boot/</url>
	<organization>
		<name>Pivotal Software, Inc.</name>
		<url>http://www.spring.io</url>
	</organization>
	<properties>
		<main.basedir>${basedir}/../..</main.basedir>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
		</dependency>
	</dependencies>
</project>
```

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景的启动器

### 2、主程序类，主入口类

```
/**
 * @Author: cuzz
 * @Date: 2018/9/20 18:06
 * @Description: @SpringBootApplication 来标注一个主程序，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class Application {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(Application.class, args);
    }
}
```

@**SpringBootApplication** : Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
      @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

- @**SpringBootConfiguration** : Spring Boot的配置类，标注在某个类上，表示这是一个Spring Boot的配置类
- @**Configuration** : 配置类上来标注这个注解，配置类也是容器中的一个组件@Component
- @**EnableAutoConfiguration**：开启自动配置功能

以前我们需要配置的东西，Spring Boot帮我们自动配置；@**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；

```
@AutoConfigurationPackage
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```

- @**AutoConfigurationPackage**：自动配置包

  - @**Import**(AutoConfigurationPackages.Registrar.class)：
  - Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；

- 将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；

- @**Import**(EnableAutoConfigurationImportSelector.class)，给容器中导入组件

  - EnableAutoConfigurationImportSelector：导入哪些组件的选择器；
  - 将所有需要导入的组件以全类名的方式返回，这些组件就会被添加到容器中； 会给容器中导入非常多的自动配置类（xxxAutoConfiguration），就是给容器中导入这个场景需要的所有组件，并配置好这些组件；[![自动配置类](https://github.com/cuzz1/springboot-learning/raw/master/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180129224104.png)](https://github.com/cuzz1/springboot-learning/blob/master/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180129224104.png)

- 有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

- 调用了`SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)`；

- Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；

- 以前我们需要自己配置的东西，自动配置类都帮我们；

- J2EE的整体整合解决方案和自动配置都在`spring-boot-autoconfigure-1.5.9.RELEASE.jar`；

  

[Spring注解版视频](https://pan.baidu.com/s/1Y3t24jisu3LJvSR8-jiRCw)

## 6、使用Spring Initializer快速创建Spring Boot项目

### 1、IDEA：使用 Spring Initializer快速创建项目

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源； js css images；
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；

### 2、STS使用 Spring Starter Project快速创建项目

# 4. Web开发

使用springboot的步骤：

 	1. 创建springboot应用，选中项目中需要的模块，添加依赖
 	2. 在application.proterties配置文件中添加少量配置，springboot默认配置了很多场景配置
 	3. 编写业务逻辑

## 1. springboot对静态资源的规则

org\springframework\boot\autoconfigure\web\servlet\WebMvcAutoConfiguration.java

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
                CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
                if (!registry.hasMappingForPattern("/webjars/**")) {
                    this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                }

                String staticPathPattern = this.mvcProperties.getStaticPathPattern();
                if (!registry.hasMappingForPattern(staticPathPattern)) {
                    this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                }

            }
        }
```

https://github.com/cuzz1/springboot-learning/blob/master/SpringBoot%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B.md



