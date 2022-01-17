# JDBC

* 什么是JDBC呢? 其实JDBC就是一群用于操作数据库的一系列的接口
* 不用的数据库都要实现这个接口这样java程序就可以来通过接口来操作数据库
* 具体的关系看文件JDBC.xmind

## 常用的类与接口

1. 第一步就是注册驱动要用到 ----- driver  不过可以使用DriverManager 来 集中管理Driver

2. 创建连接通道 : 使用 Connection 在java程序和 数据库之间创建一个数据通道

3. 发生sql指令 : 使用 Statement  或者 PreparedStatement  一般不会用Statement 因为他存在sql注入的问题很危险

4. **关闭资源**  : 首先要分析一下 那些是资源

   1. Statement是资源 --- 他相当于一个在java程序和数据库之间的传话人一样 
   2. Connection 是资源 ---- 他就是在java和数据库之间的通信的通道 是 Statement要走的路
   3. 还有一种是 ResultSet 是结果集 是一种数据库给java查询的反馈 可以理解为一张表也是一种资源

   * 注意一下 : 资源一定要关闭 否则在以后的多用户中会让程序 或者数据库崩溃的

## 批处理

批处理的意思就是 : 将多条sql语句不像之前那样一条一条的通过通道发送给数据库 而是将一定量的**指令**打包成一个集合 在将这个集合通过数据通道发送给数据库  一般在将指令发送数据通道会很耗时间的 所以通过这样就可以减轻时间的浪费了

* 具体调用的API有:
  1. addBatch() //加入到集合内
  2. executionBatch() //将集合的全部指令发送出去
  3. learnBatch() //清空集合里的指令

## 数据库连接池

* 什么是数据库连接池呢 : 其实 数据库连接池就是事先将和数据库的连接通道打通 等待java程序来连接
* java程序拿到连接后就可以传输sql语句了 当传输完毕后将这个连接归还给连接池 但是连接不会和数据库断开
* 连接池能解决的问题就是 : 避免了 连接数量过多而导致系统或者数据库崩溃,也可以避免了频繁的连接和断开连接

### 怎么使用数据库连接池呢

这里讲解两种常用的数据库连接池 

1. c3p0
2. druid (阿里的)

```java
/**
* 演示怎么使用c3p0数据库连接池
*/


//方式一:
public class DataSource_ {
    public static void main(String[] args) throws Exception {
        ComboPooledDataSource comboPooledDataSource
                = new ComboPooledDataSource();// 这个就是数据库连接池


        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\mysql.properties"));
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driver = properties.getProperty("driver");

        //让连接池能够去连接数据库
        comboPooledDataSource.setDriverClass(driver);
        comboPooledDataSource.setJdbcUrl(url);
        comboPooledDataSource.setUser(user);
        comboPooledDataSource.setPassword(password);

        comboPooledDataSource.setInitialPoolSize(10);//初始化连接数
        comboPooledDataSource.setMaxPoolSize(50); // 最大的连接数

        Connection connection = comboPooledDataSource.getConnection(); // 获取连接
        connection.close();  //这个关闭并没有真正的关闭连接只是将引用断开而已

    }
}


//方式二 (这里是配置了文件的)
public class DataSource02_ {
    public static void main(String[] args) throws SQLException {
        ComboPooledDataSource liwenhao =
                new ComboPooledDataSource("liwenhao");
        Connection connection = liwenhao.getConnection();
        System.out.println("连接成功了");
        connection.close();
    }
}
```

```xml
<c3p0-config>

  <named-config name="liwenhao">
<!-- 驱动类 -->
  <property name="driverClass">com.mysql.jdbc.Driver</property>
  <!-- url-->
  	<property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/hsp_03</property>
  <!-- 用户名 -->
  		<property name="user">root</property>
  		<!-- 密码 -->
  	<property name="password">hsp</property>
  	<!-- 每次增长的连接数-->
    <property name="acquireIncrement">5</property>
    <!-- 初始的连接数 -->
    <property name="initialPoolSize">10</property>
    <!-- 最小连接数 -->
    <property name="minPoolSize">5</property>
   <!-- 最大连接数 -->
    <property name="maxPoolSize">50</property>

	<!-- 可连接的最多的命令对象数 -->
    <property name="maxStatements">5</property> 
    
    <!-- 每个连接对象可连接的最多的命令对象数 -->
    <property name="maxStatementsPerConnection">2</property>
  </named-config>
</c3p0-config>
```





* 下面是阿里的druid

```java 
package DataSource;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.Connection;
import java.util.Properties;

public class Druid_ {
    public static void main(String[] args) throws Exception {
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\druid.properties"));

        DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);//这里直接传入一个配置文件就可以得到一个连接池了
        Connection connection = dataSource.getConnection();
        System.out.println("成功了");
        connection.close();
    }
}

```

```properties
#key=value
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/hsp_03?rewriteBatchedStatements=true
#url=jdbc:mysql://localhost:3306/girls
username=root
password=hsp
#initial connection Size
initialSize=10
#min idle connecton size
minIdle=5
#max active connection size
maxActive=50
#max wait time (5000 mil seconds)
maxWait=5000
```



* 上面这两种只是在连接池上动了手脚
* 但是和数据库交互的基本步骤没有改变

1. 注册驱动
2. 创建连接
3. 发送sql语句  这里的查询 和 dml是不一样的方法 **原因就在于查询是有返回值的**
4. **关闭资源**

## Apache DBUtils

* 这是一个工具类 里面封装了一系列JDBC相关的方法可以大大减少我们的开发时间

一般用到的两个类是 : QueryRunner  and  ResultSetHandler(接口)

```java
package DataSource;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
//这个就是查询
public class DB_Test {
    public static void main(String[] args) throws SQLException {
        Connection connection = JDBCUtilsByDruid.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        String sql = "select * from account where id >= ?";
        List<Account> list = queryRunner.query(connection, sql, new BeanListHandler<>(Account.class), 1);
        System.out.println(list);
        
        //关闭资源
        JDBCUtilsByDruid.close(null,null,connection); // 这一步关闭资源真的很重要

    }
}

```



```java
package DataSource;

import org.apache.commons.dbutils.QueryRunner;

import java.sql.Connection;
import java.sql.SQLException;

public class DB_Test02 {
    public static void main(String[] args) throws Exception {
        //得到连接(连接池中)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //创建一个传输的人
        QueryRunner queryRunner = new QueryRunner();

        //执行sql语句  这里执行的就是dml语句 对于dml语句用update来发送而对于查询用query
        String sql = "insert into account values (?,?,?)";
        int liwenhao = queryRunner.update(connection, sql, 3, "liwenhao", 1000000);
        System.out.println(liwenhao>0?"表受到了影响":"表没有受到了影响");
    }
}

```

* **对于dml语句用update来发送而对于查询用query**

1. 对于查询语句 在返回是 如果是多行数据 用 BeanListHandler 返回的是一个集合ArrayList
2. 单行数据 用 BeanHandler 返回的是一个对应的对象
3. 单行单列数据用 ScalarHandler  返回的只是这个属性的对象实例

## 对 DBUtils 的进一步优化 *DAO*

![image-20220117092047866](C:/Users/%E6%95%85%E4%BA%8B%E4%B8%8E%E9%85%92/AppData/Roaming/Typora/typora-user-images/image-20220117092047866.png)

* 这就是一个基本的DAO示意图
  1. 整体的思想就是每一张表对应一个类 和 一个DAO类 通过DAO类来和数据库交互

