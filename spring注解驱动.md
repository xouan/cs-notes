

## - spring配置

成功启动Spring容器的必要条件：

1. spring框架的类包都已经放到应用程序的类路径下
2. 应用程序为spring提供了完整的Bean配置信息
3. Bean的类都已经放到应用程序的类路径下

![1547284562065](C:\Users\xyq\AppData\Roaming\Typora\typora-user-images\1547284562065.png)

## 1. spring 使用步骤

1. 编写bean POJO（普通Java类）

```java
public class Person {

    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter and Setter

    @Override
    public String toString() {
        return "[name=" + name + ", " + "age=" + age + "]";
    }
}
```

2. 配置bean及bean之间的关系

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.2.xsd ">

    <bean id="person" class="com.xouan.bean.Person">
        <property name="name" value="xouan"></property>
        <property name="age" value="20"></property>
    </bean>
</beans>
```



3. 应用程序中实例化spring容器，使用bean

```java
public class MainTest {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
        
        Person person = (Person)ac.getBean(Person.class);
        System.out.println(person);
        String[] names = ac.getBeanNamesForType(Person.class);
        for (String name : names)
            System.out.println(name);
    }
}
```

启动spring容器的方式：1 xml启动，2 java类启动



## 2. 注解方式配置spring, @Configuration,@Bean

1. 编写POJO（普通Java类）同上
2. 配置java bean

```java
@Configuration // 等同于配置文件xml
public class MainConfig {

    @Bean // 等同于配置文件中的<bean/>标签
    public Person person() {
        return new Person("xouan", 20);
    }
}

```

MainConfig配置类作为配置文件

3. 实例化spring容器，使用bean

```java
public class MainTest {
    public static void main(String[] args) {
//        ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
        ApplicationContext ac = new AnnotationConfigApplicationContext(MainConfig.class);
        Person person = (Person)ac.getBean(Person.class);
        System.out.println(person);
        String[] names = ac.getBeanNamesForType(Person.class);
        for (String name : names)
            System.out.println(name);
    }
}
```

使用AnnotationConfigApplicationContext(MainConfig.class)实列化spring容器

## 3. 组件注册与组件自动扫描

组件注册注解：@Component,@Controller,@Service,@Repository作用相同

组件自动扫描注解：@ComponentScan(value = "com.xouan")扫描该包下所以定义为组件的类

### 1. 使用列子

新建三个类：

```java
@Controller
public class BookController {
}

//@Service
@Component
public class BookService {
}

@Repository
public class BookDao {
}

```

其包结构如下：

![1547348337283](C:\Users\xyq\AppData\Roaming\Typora\typora-user-images\1547348337283.png)

引入junit单元测试包

```xml
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
```

新建测试类

```java
public class IOCTest {

    @Test
    public void testo1() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(MainConfig.class);
        String[] names = ac.getBeanDefinitionNames();
        for (String name : names) {
            System.out.println(name);
        }
    }

}
```

测试结果将打印出spring容器中所以的bean

![1547348514050](C:\Users\xyq\AppData\Roaming\Typora\typora-user-images\1547348514050.png)

### 2. 指定组件自动扫描规则

。。。。。。

