### 1 Dao层整合

1. 编写实体类
2. 编写dao层接口
3. 编写dao接口道数据库映射sql映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.seckill.dao.SeckillDao">

</mapper>
```



4. 配置mybatis全局配置文件mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--使用jdbc的getGeneratedKeys取得数据库自增主键值-->
        <setting name="useGeneratedKeys" value="true"/>
        <!--使用列别名替换列名，默认为true-->
        <setting name="useColumnLabel" value="true"/>
        <!--开启驼峰命名转换-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>
```

5. spring整合mybatis

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--整合mybatis配置-->
    <!--1. 配置数据库相关配置，数据库驱动、url、用户名、密码-->
    <context:property-placeholder location="classpath:jdbc.properties"/>
    <!--2. 配置数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <!--配置连接池属性-->
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <!--druid连接池私有属性-->
        <property name="maxActive" value="30"/>
        <property name="minIdle" value="10"/>
        <property name="connectionErrorRetryAttempts" value="2"/>
        <property name="loginTimeout" value="1000"/>
    </bean>
    <!--3. 配置sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--注入数据库连接池-->
        <property name="dataSource" ref="dataSource"/>
        <!--p配置mybatis全局配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!--扫描实体包，使用别名-->
        <property name="typeAliasesPackage" value="com.seckill.entity"/>
        <!--扫描sql配置文件：mapper需要的xml文件-->
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>
    <!--4. 配置扫描dao接口包, 动态实现dao注入到spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入sqlSessionFactory，使用bean的时候才会加载sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--给出扫描Dao接口包-->
        <property name="basePackage" value="com.seckill.dao"/>
    </bean>
</beans>

```

jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/seckill
jdbc.username=root
jdbc.password=123456
```

## 2 service层

配置服务处bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!--扫描service包及其子包下所有使用注解的类型-->
    <context:component-scan base-package="com.seckill.service"/>
    <!--如果需要可配置事务管理器-->
    <!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置基于注解的声明式事务-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

## 3 web层

1 在web.xml中配置前端控制器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1"
         metadata-complete="true">
  <!--配置前端控制器-->
  <servlet>
    <servlet-name>seckill-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--配置初始化参数,加载了spring-web.xml,spring-service.xml配置文件-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring/spring-*.xml</param-value>
    </init-param>
  </servlet>
  <!--配置控制器映射到所有/下-->
  <servlet-mapping>
    <servlet-name>seckill-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>

```

2 配置spring mvc 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">
    <!--1.配置mvc-->
    <!--（1）开启springmvc注解模式，自动注册DefaultAnnotationHandlerMapping和AnnotationMethodHandlerAdapter
        （2）提供一系列：数据绑定，数字和日期的format，@Numberformat，@DataTimeFormat,xml,json的支持
    -->
    <mvc:annotation-driven/>

    <!--2.加入对静态资源的处理-->
    <mvc:default-servlet-handler/>

    <!--3.配置jsp-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <!--4.扫描web相关的bean扫描包-->
    <context:component-scan base-package="com.seckill.web"/>
</beans>
```

