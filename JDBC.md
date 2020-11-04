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
/**
 * 当执行了当前代码之后，会返回一个Class对象，在此对象的创建过程中，会调用具体类的静态代码块
 **/
Class.forName("oracle.jdbc.driver.OracleDriver");

//2.建立连接
/**
 * 第一步中已经将Driver对象注册到DriverManager中，所以此时可以直接通过DriverManager来获取数据库的连接
 * getConnection(url， username， password);需要输入连接数据库的参数：
 	* url ： 数据库的地址
 	* username ：用户名
 	* password ： 密码
 **/
//thin：是连接Oracle数据库的一种方式
//orcl：表示一个Oracle数据库实例
Connection connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:orcl", “Scott”, "123456");

//3.测试连接是否成功：能否获取到Connection对象
System.out.println(connection);
//4.定义sql语句
/**
 * 只要填写正常执行的sql语句即可
 **/
String sql = "select * from emp";

//5.准备静态处理块对象，用于存放sql语句
/**
 * 在执行sql语句的过程中，需要一个对象来存放sql语句，将对象进行执行的时候调用的是数据库的服务，数据库会从当前对象中拿到对应的sql语句进行执行
 **/
Statement statement = connection.createStatement();

//6.执行sql语句,返回值对象是结果集合
/**
 * 将结果放到ResultSet中，是返回结果的一个集合，需要经过循环迭代才能获取到其中的每一条记录
 *
 * statement在执行的时候可以选择三种方式：
 * 1. execute ：任何sql语句都可以执行
 * 2. executeQuery :只能执行查询语句
 * 3. executeUpdate：只能执行DML语句
 **/
ResultSet rs = statement.execute(sql);

//7.循环处理结果
/**
 * 使用while循环，有两种获取具体值的方式，第一种通过下标索引编号来获取，注意下标是从1开始的；第二种是通过列名来获取；
 * 推荐使用列名获取，一般不会发生变化
 **/
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





#### *SQL注入

需要注意的安全问题：当用户传入一些参数后，程序中没有做任何校验规则判断，这时用户可以将用户表中的所有数据都查询到。

最简单的防止sql注入方式：使用PrepareStatement来代替Statement保存sql语句处理



#### 批量提交batch

​	批量提交可以节省每次和数据库之间连接所耗费的时间和资源，可以在一次连接内完成需要批量执行的操作。

​	特别是当批量操作的数量巨大的时候，同时和数据库进行的连接是有限制的，每一次操作都进行一次连接是不现实的。



```java
public static void insertBatch() {
        Connection conn = DBUtil.getConnection();
        PreparedStatement pstmt = null;
        String sql = "insert into emp(empno, ename) values (?, ?)";
//准备预处理块对象
        try {
            pstmt = conn.prepareStatement(sql);
            for (int i = 0; i < 10; i++) {
                pstmt.setInt(1, i+1000);
                pstmt.setString(2, "msb" + i);
//向批处理中添加sql语句
                pstmt.addBatch();
            }
//进行批量提交
            int[] ints = pstmt.executeBatch();
            for(int anInt : ints) {
                System.out.println(anInt);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DBUtil.CloseConnection(conn, pstmt);
        }
    }
```





### 反射



​	Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法称为Java语言的反射机制。



![reflect01](reflect01.png)



​		Class类的实例表示正在运行的Java应用程序中的类和接口，每个类只会产生一个Class对象，在类加载的时候自动创建



#### 反射机制的相关类



| 类名        | 用途                                                         |
| ----------- | ------------------------------------------------------------ |
| Class       | 代表**类的实体**，在运行的Java Application中表示类和接口。使用newInstance()方法可以创建类的实例 |
| Field       | 代表类的成员变量                                             |
| Method      | 代表类中的普通方法                                           |
| Constructor | 代表类的构造方法                                             |





```java
/* 获取Class类的三种方式 */

//方法1.通过class.forname()来获取Class对象
Class clazz = Class.forName("com.mashibing.entity.Emp");
//方法2.通过类名.class来获取
Class<Emp> empClass = Emp.class;
//方法3.通过对象的getClass()方法来获取
Class empClazz = new Emp().getClass();
/*推荐使用1和2方法，方法3不建议使用，因为会重复创建对象的操作;方法2需要提前准备好类，如果该类是第三方的类必须要该类存在于项目中，如果是使用方法1，只需要知道类的完全限定名即可，无需创建类或者调用对象 */

//方法4：对于基本数据类型可以通过.TYPE的方式获取到Class对象
Class type = Integer.TYPE;
System.out.println(type.getName());
System.out.println(type.getCanonicalName());

System.out.println(clazz.getPackage());//包名
System.out.println(clazz.getName()); //完全限定名：包名+类名
System.out.println(clazz.getSimpleName()); //类名
System.out.println(clazz.getCanonicalName()); //符合Java规范的类名——一般和完全限定名相同，当类是基本数据类型时会有差别，不过差别不大，可以忽略不计

```



#### 反射在JDBC中的应用

* 思考：要查询N张表的数据，但是不想每个表都写一个对应的方法，能否写一个方法完成所有表的查询工作

```java
public List getRows(String sql,Object[] params,Class clazz){
    List list = new ArrayList();
    Connection connection = null;
    PreparedStatement pstmt = null;
    ResultSet resultSet = null;

    try {
        //自己封装的DBUtil类，方便建立连接
        connection = DBUtil.getConnection();
        //创建pstmt对象
        pstmt = connection.prepareStatement(sql);
        //给sql语句填充参数
        if(params!=null){
            for(int i = 0;i<params.length;i++){
                pstmt.setObject(i+1,params[i]);
            }
        }
        //开始执行查询操作,resultset中有返回的结果，需要讲返回的结果放置到不同的对象中
        resultSet = pstmt.executeQuery();
        //获取结果集合的元数据对象
        ResultSetMetaData metaData = resultSet.getMetaData();
        //判断查询到的每一行记录中包含多少个列
        int columnCount = metaData.getColumnCount();
        //循环遍历resultset
        while(resultSet.next()){    //循环resultset的每一行数据
            //创建放置具体结果属性的对象
            Object obj = clazz.newInstance();
            for(int i = 0;i < columnCount;i++){ //根据列名循环每一列，并调用set方法去给对象赋值
                //从结果集合中获取单一列的值
                Object objValue = resultSet.getObject(i+1);
                //获取列的名称
                String columnName = metaData.getColumnName(i+1).toLowerCase();
                //获取类中的属性
                Field declaredField = clazz.getDeclaredField(columnName);
                //获取类中属性对应的set方法
                Method method = clazz.getMethod(getSetName(columnName),declaredField.getType());
                if(objValue instanceof Number){ //因为Number从Oracle数据库对应过来时，其中包含多个基本数据类型的类，需要进行类型判断
                    Number number = (Number) objValue;
                    String fname = declaredField.getType().getName();
                    if("int".equals(fname)||"java.lang.Integer".equals(fname)){
                        method.invoke(obj,number.intValue());
                    }else if("byte".equals(fname)||"java.lang.Byte".equals(fname)){
                        method.invoke(obj,number.byteValue());
                    }else if("short".equals(fname)||"java.lang.Short".equals(fname)){
                        method.invoke(obj,number.shortValue());
                    }else if("long".equals(fname)||"java.lang.Long".equals(fname)){
                        method.invoke(obj,number.longValue());
                    }else if("float".equals(fname)||"java.lang.Float".equals(fname)){
                        method.invoke(obj,number.floatValue());
                    }else if("double".equals(fname)||"java.lang.Double".equals(fname)){
                        method.invoke(obj,number.doubleValue());
                    }
                }else{  //其他类型的可以直接对应到java中的类
                    method.invoke(obj,objValue);
                }
            }
            list.add(obj);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally {
        DBUtil.closeConnection(connection,pstmt,resultSet);
    }

    return list;
}
//拼set方法
public String getSetName(String name){
    return "set"+name.substring(0,1).toUpperCase()+name.substring(1);
}
```



* 练习：保存数据

```java
public int saveEntity(Object entity) {
    Integer count = 0;
    Connection conn = null;
    PreparedStatement pstmt = null;
    try {
        conn = MySQLDBUtil.getConnection();
        //获取到实体和表的对应信息：表名，字段名以及对应的值
        Class clazz = entity.getClass();
        String tablename = clazz.getSimpleName();
        StringBuffer fieldNames = new StringBuffer();
        StringBuffer fieldValues = new StringBuffer();

        //获取字段名
        Field[] fields = clazz.getDeclaredFields();
        for (int i = 0; i < fields.length; i++) {
            Field field = fields[i];
            fieldNames.append(field.getName());
            //根据字段名get方法获取值, 参数为空的时候使用null代替
            Method getter = clazz.getDeclaredMethod(getGetName(field.getName()), null);
            Object returnValue = getter.invoke(entity, null);
            if(returnValue instanceof String) {
                fieldValues.append("'" + returnValue + "'");
            } else {
                fieldValues.append(returnValue);
            }
            if(i != fields.length-1) {
                fieldNames.append(",");
                fieldValues.append(",");
            }
        }

        String sql = "insert into " + tablename + "(" + fieldNames.toString()
            + ") values (" + fieldValues.toString() + ")";
        pstmt = conn.prepareStatement(sql);
        count = pstmt.executeUpdate();

    } catch (SQLException e) {
        e.printStackTrace();
    } catch (Exception e) {
        e.printStackTrace();
    }

    return count;
}
//拼get方法
public String getGetName(String name){
    return "get"+name.substring(0,1).toUpperCase()+name.substring(1);
}
```



### 封装jdbc的工具包DBUtils



#### Apache的DbUtil工具包





