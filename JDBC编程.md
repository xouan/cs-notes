# JDBC编程

**jdbc**:全称Java数据库连接(Java Database Connectivity),是一套用于执行SQL的Java API。应用程序可以根据这套API连接关系数据库，并使用SQL语句完成CRUD等操作。

## 1 JDBC连接数据库步骤

JDBC连接数据库需要经过6步：

1. 加载并注册数据库驱动
2. 通过DriverManger类获取数据库连接
3. 通过Connection接口创建Statement对象
4. 使用Statement或PreparedStatement接口执行SQL语句
5. 操作ResultSet结果集
6. 关闭连接，释放资源

```java
    public static void main(String[] args) throws Exception{
        /**
         * jdbc连接数据库步骤
         * 1. 加载并注册数据库驱动
         * 2. 通过驱动管理（DriverManager)获取连接
         * 3. 通过Connection对象获取Statement
         * 4. 使用Statement接口执行SQL语句
         * 5. 操作ResultSet结果集处理数据
         * 6. 关闭连接，释放资源*/

        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
//            1. 加载并注册数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            String url = "jdbc:mysql://127.0.0.1:3306/jdbctest?useSSL=false";
            String user = "root";
            String password = "123456";
//            2. 通过驱动管理（DriverManager)获取连接
            connection = DriverManager.getConnection(url, user, password);
//            3. 通过Connection对象获取Statement
            statement = connection.createStatement();
//            4. 使用Statement接口执行SQL语句
            String sql = "select * from user";
            resultSet = statement.executeQuery(sql);
//            5. 操作ResultSet结果集处理数据
            System.out.println("userID   |   userName   |   userEmail");
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String email = resultSet.getString("email");
                System.out.println(id + "    " + name + "    " + email);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
//            6. 关闭连接，释放资源*/
            if (resultSet != null) {
                resultSet.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        }
    }
}

```

## 2 数据库基本操作

#### 1. 查询数据

在查询数据时可以通过Statement实列进行静态查询，也可以通过PreparedStatement进行动态查询。

动态查询：从多个查询条件中随机选择若干个组合成一个DQL语句进行查询，这一过程叫做动态查询。

1. 通过Statement查询

```java
Statement stmt = conn.createStatement(); // 创建Statement对象
String sql = "select * from  user where id = 2"; // 定义静态查询语句
ResultSet rs = stmt.executeQuery(sql); // 直接执行查询
```

2. 通过PreparedStatement进行查询

```java
Statement stmt = conn.createStatement(); // 创建Statement对象
String sql = "select * from user where id = ?"; // 定义查询语句,？作为sql语句的占位符
PreparedStatement ps = conn.prepareStatement(sql); // 预处理sql语句
ps.setInt(1, 2); // 为占位符指定具体值。1表示sql语句中的第一个占位符，2表示具体值
ResultSet rs = ps.executeQuery(); // 执行查询语句
```

注：

1. 为动态SQL语句中参数赋值时，蚕食索引从1开始，而不是0

Statement直接执行sql语句只能执行一次，PreparedStatement先将sql语句预处理，然后设置参数组成具体的sql语句，可以重复使用。

```java
            Statement stmt = conn.createStatement();
            String sql = "select * from user where id = ?";
            PreparedStatement ps = conn.prepareStatement(sql);
            // 第一次执行
            ps.setInt(1, 2);
            ResultSet rs = ps.executeQuery(); 
            System.out.println("userID   |   userName   |   userEmail");
            while (rs.next()) {
                int id = rs.getInt(1);
                String name = rs.getString(2);
                String email = rs.getString(3);
                System.out.println(id + "    " + name + "    " + email);
            }
            // 第二次执行，参数值不同
            ps.setInt(1, 1);
            ResultSet rs2 = ps.executeQuery();
            while (rs2.next()) {
                int id = rs2.getInt(1);
                String name = rs2.getString(2);
                String email = rs2.getString(3);
                System.out.println(id + "    " + name + "    " + email);
            }
```

#### 2. 插入语句

#### 3. 更新语句

#### 4. 删除语句

## 3 事物处理

