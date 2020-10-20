### 数据库设计三范式——减少数据冗余



#### 第一范式1NF（确保每列保持原子性）

* 第一范式是最基本的范式。如果数据库表中的所有字段值都是不可分解的原子值，就说明该数据库满足了第一范式

#### 第二范式2NF（确保表中的每列都和主键唯一相关，数据不存在部分依赖）

* 第二范式在第一范式的基础之上更进一层。第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。也就是说在一个数据库表中，一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。

#### 第三范式3NF（数据不能存在传递依赖）

* 满足第三范式必须先满足第二范式。第三范式要求一个数据库表中不包含已在其他表中已包含的非主键字段





### JDBC



JDBC(Java Database Connectivity Java数据库连接)是一种基于Java语言访问数据库的一种技术，是用于执行SQL语句的Java API，可以为多种关系型数据库提供统一访问。

* JDBC的设计思想：由sun公司提供访问数据库的接口，由数据库厂商提供对这些接口的实现，程序员编程时都是针对接口进行编程的。JDBC是面向接口的
* JDBC包括一套JDBC的API和一套程序员和数据库厂商都必须去遵守的规范：
  * java.sql包：提供访问数据库基本的功能
  * javax.sql包：提供扩展的功能
* JDBC 可以连接到数据库，在Java app中执行SQL命令，并且处理结果。



#### 常用数据库连接方式



* MySQL

```java
String Driver="com.mysql.jdbc.Driver"; //驱动程序
String URL="jdbc:mysql://localhost:3306/db_name"; //连接的URL,db_name为数据库名
String Username="username"; //用户名
String Password="password"; //密码
Class.forName(Driver);
Connection con=DriverManager.getConnection(URL,Username,Password);
```



* Oracle

```java
String Driver="oracle.jdbc.driver.OracleDriver"; //连接数据库的方法
String URL="jdbc:oracle:thin:@localhost:1521:orcl"; //orcl为数据库的SID
String Username="username"; //用户名
String Password="password"; //密码
Class.forName(Driver) ; //加载数据库驱动
Connection con=DriverManager.getConnection(URL,Username,Password); 
```



* PostgreSQL

```java
String Driver="org.postgresql.Driver"; //连接数据库的方法
String URL="jdbc:postgresql://localhost/db_name"; //db_name为数据库名
String Username="username"; //用户名
String Password="password"; //密码
Class.forName(Driver) ;
Connection con=DriverManager.getConnection(URL,Username,Password);
```



* DB2

```java
String Driver="com.ibm.dbjdbc.app.DBDriver"; //连接具有DB2客户端的Provider实例
//String Driver="com.ibm.dbjdbc.net.DBDriver"; //连接不具有DB2客户端的Provider实例
String URL="jdbc:db2://localhost:5000/db_name"; //db_name为数据库名
String Username="username"; //用户名
String Password="password"; //密码
Class.forName(Driver) ;
Connection con=DriverManager.getConnection(URL,Username,Password);
```



* Microsoft SQL Server

```java
String Driver="com.microsoft.sqlserver.jdbc.SQLServerDriver"; //连接SQL数据库的方法
String URL="jdbc:sqlserver://localhost:1433;DatabaseName=db_name"; //db_name为数据库名
String Username="username"; //用户名
String Password="password"; //密码
Class.forName(Driver).new Instance(); //加载数据可驱动
Connection con=DriverManager.getConnection(URL,Username,Password);
```



#### JDBC使用（Oracle为例）



```java

//1.加载驱动————反射
Class.forName("oracle.jdbc.driver.OracleDriver");




//2.建立连接
//thin：是连接Oracle数据库的一种方式
//orcl：表示一个Oracle数据库实例
//getConnection(数据库url， 用户名， 密码);
Connection connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:orcl", “Scott”, "123456");



//3.测试连接是否成功：能否获取到Connection对象
System.out.println(connection);
//4.定义sql语句
String sql = "select * from emp";
//5.准备静态处理块对象，用于存放sql语句
Statement statement = connection.createStatement();
//6.执行sql语句,返回值对象是结果集合
ResultSet rs = statement.execute(sql);
//7.循环处理结果
while(rs.next()) {
    int anInt = rs.getInt(1); //索引从1开始
    System.out.println(anInt);
    String ename = rs.getString("ename");
    System.out.println(ename);
}
//8.关闭连接
statement.close();
connection.close();
```
