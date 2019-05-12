# 1. JDBC访问数据库

### 1. jdbc访问数据库步骤

1. 加载数据库驱动类，指定数据库连接URL地址，指定用户名及密码
2. 通过DriverMannager打开数据库连接
3. 通过数据库连接创建Statement对象
4. 通过Statement对象执行SQL语句，得到ResultSet对象
5. 通过ResultSet读取数据，并将数据转换为JavaBean对象
6. 关闭ResultSet,Statement对象以及数据库连接，释放相关资源

```java
import java.sql.*;

public class JDBCTest {
    public static void main(String[] args) throws Exception {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 1.加载数据库驱动类、指定数据库连接URL地址、指定数据库名、连接密码等信息
            // 加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 获取数据库连接
            String url = "jdbc:mysql://127.0.0.1:3306/test";
            // 设置用户名与密码
            String user = "root";
            String password = "123456";
            // 2.通过DriverMannager打开数据库连接
            connection = DriverManager.getConnection(url, user, password);
            // 3.编写sql语句，通过数据库连接创建Statement对象，并设置参数
            String sql = "select * from tbl_user where id=?";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setLong(1, 1l);
            // 4.通过Statement对象执行SQL语句，得到ResultSet对象
            resultSet = preparedStatement.executeQuery();
            // 5.处理结果,通过ResultSet读取数据，并将数据转换为JavaBean对象
            while (resultSet.next()) {
                System.out.println(resultSet.getString("user_name"));
                System.out.println(resultSet.getString("name"));
                System.out.println(resultSet.getInt("age"));
                System.out.println(resultSet.getDate("birthday"));
            }
        } catch (SQLException e) {
            // 处理异常
            e.printStackTrace();
        }finally {
            // 6.关闭连接、释放资源
            if (resultSet != null) {
                resultSet.close();
            }
            if (preparedStatement != null) {
                preparedStatement.close();
            }
            if (connection != null) {
                connection.close();
            }
        }
    }
}
```

2.mybatis配置

创建数据库表

```sql
DROP TABLE IF EXISTS tbl_user;
CREATE TABLE tbl_user (
id int(32) NOT NULL AUTO_INCREMENT,
username varchar(32) DEFAULT NULL,
password varchar(32) DEFAULT NULL,
email varchar(32) default null ,
PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO tbl_user ( username, password, email) VALUES ('张三', '123456', '124324356@qq.com');
INSERT INTO tbl_user ( username, password, email) VALUES ('李四', '123456', '124354352@qq.com');
```

引入依赖

```xml
        <!--引入java的mysql连接依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.32</version>
        </dependency>
        <!--引入mybatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <!--引入日志依赖-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>

        <!--引入单元测试依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
```

1.配置全局配置文件（mybatis-config.xml）,日志文件等

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <properties>
        <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&amp;characterEncoding=utf-8&amp;allowMultiQueries=true"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </properties>
    
    <!--数据库环境配置，指定数据库驱动，连接URL，用户名，密码等-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <!--注册映射器-->
    <mappers>
        <mapper resource="mybatis/mapper/TestMapper.xml"/>
    </mappers>
</configuration>
```



2.编写数据库表对应的实体类

```java
package com.xouan.entity;

public class User {
    private Integer id;
    private String username;
    private String password;

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    private String email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

```



3.编写实体类到数据库的映射器Map.xml（TestMapper.xml）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--指定命名空间，编写sql语句-->
<mapper namespace="TestMapper">
    <select id="selectAll" resultType="com.xouan.entity.User">
        select * from tbl_user;
    </select>
</mapper>
```



4.修改全局配置文件（mybatis-config.xml）,注册映射器

```xml
    <!--注册映射器-->
    <mappers>
        <mapper resource="mybatis/mapper/TestMapper.xml"/>
    </mappers>
```



5.使用mybatis

 	1. 读取全局配置文件
 	2. 构建sql会话工厂
 	3. 读取sql会话
 	4. 执行业务
 	5. 关闭sql会话

```java
public class MybatisTest {
    public static void main(String[] args) throws Exception{
        
        // 读取全局配置文件
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        // 构建会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 开启会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
        try {
            // 执行sql查询
            List<User> userList = sqlSession.selectList("TestMapper.selectAll");
            System.out.println(userList);
        } finally {
            // 关闭会话
            sqlSession.close();
        }
    }
}

```



