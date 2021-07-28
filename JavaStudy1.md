# ali

## cachedThreadPool问题

线程池底层实现

线程池的底层实现是：ThreadPoolExecutor

使用工厂类Executors快速创建预定义线程池

IDEA插件Alibaba Java Coding Guidelines，代码提示如下（马云不推荐使用啊）

线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。
说明：Executors 返回的线程池对象的弊端如下：
1） FixedThreadPool 和 SingleThreadPool：
允许的请求队列长度为 Integer.MAX_VALUE（无限），可能会堆积大量的请求，从而导致 OOM。
2） CachedThreadPool：
允许的创建线程数量为 Integer.MAX_VALUE（无限），可能会创建大量的线程，从而导致 OOM。

预定义线程池

2. CachedThreadPool（典型的最大线程数是无限的线程池）

允许的创建线程数量为 Integer.MAX_VALUE（无限），可能会创建大量的线程，从而导致 OOM。

ExecutorService executorService = Executors.newCachedThreadPool();

//点击进入代码，查看实现：
     public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
第一种写法（最简单）：使用工厂类Executors快速创建线程池
ExecutorService executorService = Executors.newCachedThreadPool();

第二种写法（最原始）：使用ExecutorService创建线程池
ExecutorService pool = new ThreadPoolExecutor(
        0,      //corePoolSize = 0
        Integer.MAX_VALUE,  //maximumPoolSize = 2147483647
        60L,   //keepAliveTime = 60
        TimeUnit.MILLISECONDS,
        new SynchronousQueue<Runnable>(),
        Executors.defaultThreadFactory(),
        new ThreadPoolExecutor.AbortPolicy());

第三种写法（适中）：使用ThreadPoolTaskExecutor简洁创建线程池
ThreadPoolTaskExecutor poolTaskExecutor = new ThreadPoolTaskExecutor();
poolTaskExecutor.setQueueCapacity(0); //这个地方随便填
poolTaskExecutor.setCorePoolSize(0);  //核心线程数是0，阻塞队列会选择new SynchronousQueue()
poolTaskExecutor.setMaxPoolSize(2147483647);  // Integer.MAX_VALUE = 2147483647
poolTaskExecutor.setRejectedExecutionHandler(new ThreadPoolExecutor.AbortPolicy());
poolTaskExecutor.setKeepAliveSeconds(60);
poolTaskExecutor.initialize()
————————————————
版权声明：本文为CSDN博主「敬勤」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_40133108/article/details/109861426

简介
CachedThreadPool是一个根据需要创建线程的线程池。

创建方法
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
1
2
3
4
5
corePoolSize = 0，maximumPoolSize设置为Integer.MAX_VALUE，代表没有核心线程，非核心线程是无界的；keepAliveTime = 60L，空闲线程等待新任务的最长时间是60s；用了阻塞队列SynchronousQueue,是一个不存储元素的阻塞队列，每一个插入操作必须等待另一个线程的移除操作，同理一个移除操作也得等待另一个线程的插入操作完成；

execute方法执行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424131202663.png#pic_center)

执行execute方法时，首先会先执行SynchronousQueue的offer方法提交任务，并查询线程池中是否有空闲线程来执行SynchronousQueue的poll方法来移除任务。如果有，则配对成功，将任务交给这个空闲线程。否则，配对失败，创建新的线程去处理任务；当线程池中的线程空闲时，会执行SynchronousQueue的poll方法等待执行SynchronousQueue中新提交的任务。若超过60s依然没有任务提交到SynchronousQueue，这个空闲线程就会终止；因为maximumPoolSize是无界的，所以提交任务的速度 > 线程池中线程处理任务的速度就要不断创建新线程；每次提交任务，都会立即有线程去处理，因此CachedThreadPool适用于处理大量、耗时少的任务。
————————————————
版权声明：本文为CSDN博主「墨玉浮白」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_36299025/article/details/89490858

## JDBC连接数据库

###### 1、加载JDBC驱动程序



```csharp
在连接数据库之前，手续要加载想要连接的数据库驱动到JVM（java虚拟机）
通过java.lang.Class类的静态方法forName(String className)实现
成功加载后，会将Dirver类的实例注册到DriverManager类中  
例如：    
    try {    
        // 加载MySql的驱动类    
        Class.forName("com.mysql.jdbc.Driver");    
    } catch(ClassNotFoundException e) {    
        System.out.println("找不到驱动程序类 ，加载驱动失败！");    
        e.printStackTrace();    
    }    
```

###### 2、拼接JDBC需要连接的URL



```ruby
1、mysql URL格式如下：
jdbc:mysql://[host:port],[host:port].../[database][?参数名1][=参数值1][&参数名2][=参数值2]...
2、例如：（MySql的连接URL）    
jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=gbk;    
useUnicode=true：表示使用Unicode字符集。
如果characterEncoding设置为gb2312或GBK，本参数必须设置为true。
characterEncoding=gbk：字符编码方式。    
```

###### 3、创建数据库的连接



```dart
要连接数据库，需要向java.sql.DriverManager请求并获得Connection对象
该对象就代表着一个数据库的连接
使用DriverManager的getConnectin(String url, String username, String password)方法
传入指定的连接的数据库的路径、数据库的用户名和密码来获得。
    // 连接MySql数据库，用户名和密码都是root    
     String url = "jdbc:mysql://localhost:3306/test";     
     String username = "root";    
     String password = "root";    
     try {    
       Connection con = DriverManager.getConnection(url , username , password);    
     } catch(SQLException se) {    
       System.out.println("数据库连接失败！");    
       se.printStackTrace();    
     }    
```

###### 4、创建一个Statement



```bash
要执行SQL语句，必须获得java.sql.Statement实例，Statement实例分为以下3种类型：    
 1、执行静态SQL语句。通常通过Statement实例实现。    
 2、执行动态SQL语句。通常通过PreparedStatement实例实现。    
 3、执行数据库存储过程。通常通过CallableStatement实例实现。  
 具体的实现方式：   
  Statement stmt = con.createStatement();   
  PreparedStatement pstmt = con.prepareStatement(sql);   
  CallableStatement cstmt = con.prepareCall("{CALL demoSp(? , ?)}");      
```

###### 5、执行SQL语句



```dart
Statement接口提供了三种执行SQL语句的方法：executeQuery 、executeUpdate和execute    
 1、ResultSet executeQuery(String sqlString)：
         执行查询数据库的SQL语句，返回一个结果集（ResultSet）对象。    
 2、int executeUpdate(String sqlString)：
         用于执行INSERT、UPDATE或DELETE语句以及SQL DDL语句，如：CREATE TABLE和DROP TABLE等    
 3、execute(sqlString):
         用于执行返回多个结果集、多个更新计数或二者组合的语句。    

具体实现的代码：    
   ResultSet rs = stmt.executeQuery("SELECT * FROM ...");    
   int rows = stmt.executeUpdate("INSERT INTO ...");    
   boolean flag = stmt.execute(String sql);    
```

###### 6、处理执行完SQL之后的结果



```dart
两种情况：    
1、执行更新返回的是本次操作影响到的记录数。    
2、执行查询返回的结果是一个ResultSet对象。    
ResultSet包含符合SQL语句中条件的所有行，并且它通过一套get方法提供了对这些行中数据的访问。    
使用结果集（ResultSet）对象的访问方法获取数据：    
   while(rs.next()) {    
         String name = rs.getString("name");    
         String pass = rs.getString(1); // 此方法比较高效    
       }    
 （列是从左到右编号的，并且从列1开始）   
```

###### 7、关闭使用的JDBC对象



```csharp
操作完成以后要把所有使用的JDBC对象全都关闭，以释放JDBC资源，关闭顺序和声明顺序相反：    
1、关闭记录集    
2、关闭声明    
3、关闭连接对象    
 if (rs != null) {   // 关闭记录集    
        try {    
            rs.close();    
        } catch(SQLException e) {    
            e.printStackTrace();    
        }    
  }    
 if (stmt != null) {   // 关闭声明    
        try {    
            stmt.close();    
        } catch(SQLException e) {    
            e.printStackTrace();    
        }    
 }    
 if (conn != null) {  // 关闭连接对象    
         try {    
            conn.close();    
         } catch(SQLException e) {    
            e.printStackTrace();    
         }    
}  
```



作者：Charles___
链接：https://www.jianshu.com/p/776b8d354a1b
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## sql注入

一、SQL注入
SQL注入是一种比较常见的网路攻击方式，一些恶意人员在需要用户输入的地方，恶意输入SQL语句的片段，通过SQL语句，实现无账号登录，甚至篡改数据库。

二、SQL注入实例
登录场景：
在一个登录界面，要求用户输入账号和密码。

我们的代码中会有如下SQL语句：

String sql="select * from user where account='"+account+"' and password='"+password+"'";
1
情形1：（免账号登录）
账号：' or 1=1-- （--后面有一个空格）
密码：
当我们输入了这个账号和密码，那么SQL语句就变成：

String sql="select * from user where account='' or 1=1 -- ' and password=''";
1
2
3
4
5

这个查询条件就变成account=‘’ 或者1=1，这个条件是恒成立的。
后面的-- ，就是注释。
这样就可以实现免账号登录。

情形2：（删除数据库）
账号：';drop database test;-- （--后面有一个空格）
密码：
当我们输入了这个账号和密码，那么SQL语句就变成：

String sql="select * from user where account='';drop database test;-- ' and password=''";
1
2
3
4
5

这个查询条件就变成account=‘’ 或者1=1，这个条件是恒成立的。
后面的-- ，就是注释。
这样就能删除数据库。

三、防止SQL注入方法
使用PreparedStatement可以防止SQL注入。sql语句中的参数需要用？代替。PreparedStatement对sql预编译后,然后调用setXX()方法设置sql语句中的参数。这样再传入特殊值，也不会出现sql注入的问题了。

四、登录项目
1、用户表
用户的账号信息：

create table user(
id int primary key auto_increment,
account varchar(20),
password varchar(20),
nickname varchar(20)
);

insert into user(account,password,nickname) values('Jack','123456','杰克');
insert into user(account,password,nickname) values('Mary','888888','玛丽');

select*from user;

1
2
3
4
5
6
7
8
9
10
11
12


2、项目结构
创建一个javaweb项目。Login类实现登录功能，用Junit进行单元测试，创建LoginTest类实现登录测试功能。


3、登录实现
Login.java

package net.jdbc.test;

import java.math.BigDecimal;
import java.sql.*;

public class Login {
    //数据库url、用户名和密码
    static final String DB_URL="jdbc:mysql://localhost:3306/test?";
    static final String USER="root";
    static final String PASS="root123";
    public static void login(String account,String password) {
        try {
            //1、注册JDBC驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2、获取数据库连接
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASS);
            //3、操作数据库
            String sql="select * from user where account=? and password=?";

            //获取操作数据库的对象
            //用PreparedStatement可以预防SQL注入
            PreparedStatement preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1,account);//设置参数
            preparedStatement.setString(2,password);
            ResultSet resultSet = preparedStatement.executeQuery();//执行查询sql，获取结果集
    
            if (resultSet.next()){ //遍历结果集，取出数据
                String name = resultSet.getString("nickname");
                //输出数据
                System.out.println(name+"登录成功");
            }else{
                System.out.println("用户登录失败......");
            }
            //4、关闭结果集、数据库操作对象、数据库连接
            resultSet.close();
            preparedStatement.close();
            connection.close();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch(SQLException e){
            e.printStackTrace();
        } catch(Exception e){
            e.printStackTrace();
        }
    }

}

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
4、登录测试
LoginTest.java

package net.jdbc.test;

import org.junit.Test;

import static org.junit.Assert.*;

public class LoginTest {

    @Test
    public void login() {
        Login.login("' or 1=1-- ","");//SQL注入测试
        Login.login("Jack","123");//正确账号，错误密码
        Login.login("Jack","123456");
        Login.login("Mary","888888");
    
    }
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
运行结果：

————————————————
版权声明：本文为CSDN博主「龟的小号」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/hju22/article/details/87538733

预编译语句:

select * from user where username like #{name}

预处理PrepatedStatement的参数占位符 :select * from user where username like ? 

#{}是 sql 的参数占位符，Mybatis 会将 sql 中的#{}替换为?号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的?号占位符设置参数值，

预编译语句可以重复使用这些计划，减少 SQL 编译所需要的时间，还可以解决动态 SQL 所带来的 SQL 注入的问题。
只传参数，比传递 SQL 语句更高效。
相同语句可以一次解析，多次使用，提高处理效率。
Statement

Class.forName(com.mysql.jdbc.Driver);       //加载驱动
Connection con = DriverManager.getConnection("jdbc:mysql://....");  //创建与mysql中某个数据库的连接

Statement st = con.CreateStatement();    //创建一个Statement对象

String id = "03";                        //
String sq = "delete from table1 where id="+id; //构建sql语句
st.execute(sq);          //执行sql语句
上面这段代码的本意是要删除id=03的记录，但是如果有人将id的内容改为“03 or 1=1”。那么表中的任何记录都将被删除，后果十分严重。

  这是Statement的一大缺点：不能防止sql注入。

  另一个缺点是，每次执行都需要重新编译sql语句，效率低下。而PreparedStatement则解决了这些问题。

Prestatement

Class.forName(com.mysql.jdbc.Driver);       //加载驱动
Connection con = DriverManager.getConnection("jdbc:mysql://....");  //创建与mysql中某个数据库的连接


String sq = "delete from table1 where id=? and name=?"//构建sql语句，以？作为占位符，这个位置的值待设置
PreparedStatement ps = con.prepareStatement(sq);    //创建PreparedStatement时就传入sql语句，实现了预编译

ps.setString(1,"03");
ps.setString(2,"mao");          //设置sql语句的占位符的值，注意第一个参数位置是1不是0

ps.execute();          //执行这个PreparedStatement

//设置占位符的值还可以通过setObject(int,object)完成


//上面代码的作用时删除表中id=03且name=mao的记录
为什么PreparedStatement能防止sql注入呢？

  因为sql语句是预编译的，而且语句中使用了占位符，规定了sql语句的结构。用户可以设置"?"的值，但是不能改变sql语句的结构，因此想在sql语句后面加上如“or 1=1”实现sql注入是行不通的。

实际开发中，一般采用PreparedStatement访问数据库，它不仅能防止sql注入，还是预编译的(不用改变一次参数就要重新编译整个sql语句，效率高)，此外，它执行查询语句得到的结果集是离线的，连接关闭后，仍然可以访问结果集。
————————————————
版权声明：本文为CSDN博主「原野的稻草人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/QGhurt/article/details/107012843

## Mybatis防止sql注入

一、什么是Sql注入
sql注入是一种代码注入技术，将恶意的sql插入到被执行的字段中，以不正当的手段多数据库信息进行操作。

在jdbc中使用一般使用PreparedStatement就是为了防止sql注入。

比如代码 ："select * from user where id =" + id;
正常执行不会出现问题，一旦被sql注入，比如将传入参数id=“3 or 1 = 1”，那么sql会变成
"select * from user where id = 3 or 1 = 1",这样全部用户信息就一览无遗了。
二、MyBatis中防止sql注入
下面是Mybatis的两种sql。

  <select id="selectByPrimaryKey" resultMap="BaseResultMap"         
                        parameterType="java.lang.Integer" >
    select 
    <include refid="Base_Column_List" />
    from user
    where id = #{id,jdbcType=INTEGER}
  </select>
  <select id="selectByPrimaryKey" resultMap="BaseResultMap" 
                         parameterType="java.lang.Integer" >
    select 
    <include refid="Base_Column_List" />
    from user
    where id = ${id,jdbcType=INTEGER}
  </select>
可以很清楚地看到，主要是#和$的区别。

将sql进行预编译"where id = ?"，然后底层再使用PreparedStatement的set方法进行参数设置。

$ 将传入的数据直接将参数拼接在sql中，比如 “where id = 123”。

因此，#与$相比，#可以很大程度的防止sql注入，因为对sql做了预编译处理，因此在使用中一般使用#{}方式。

三、演示
在项目中配置log4j。方便查看sql日志。

#方式

查看日志，sql进行了预编译，使用了占位符，防止了sql注入。  
[cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - ==>  Preparing: select id, stuId, username, password, type, info from user where id = ? 
  [cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - ==> Parameters: 1(Integer)
  [cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - <==      Total: 1
$方式

$传入的数据直接显示在sql中
 [cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - ==>  Preparing: select id, stuId, username, password, type, info from user where id = 1 
  [cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - ==> Parameters: 
  [cn.jxust.cms.dao.UserMapper.selectByPrimaryKey] - <==      Total: 1


附：使用$方式时，需要在mapper接口中使用@Param注解。否则会报There is no getter for property named 'id' in 'class java.lang.Integer'错误。

User selectByPrimaryKey(@Param(value = "id") Integer id);
————————————————
版权声明：本文为CSDN博主「包子林黄辉冯」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zh137289/article/details/90551305

### 1、首先看一下下面两个sql语句的区别：

```
<select id="selectByNameAndPassword" parameterType="java.util.Map" resultMap="BaseResultMap">
select id, username, password, role
from user
where username = #{username,jdbcType=VARCHAR}
and password = #{password,jdbcType=VARCHAR}
</select>
<select id="selectByNameAndPassword" parameterType="java.util.Map" resultMap="BaseResultMap">
select id, username, password, role
from user
where username = ${username,jdbcType=VARCHAR}
and password = ${password,jdbcType=VARCHAR}
</select>
```

**mybatis中的#和$的区别：**

1、#将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。
如：where username=#{username}，如果传入的值是111,那么解析成sql时的值为where username="111", 如果传入的值是id，则解析成的sql为where username="id".　
2、$将传入的数据直接显示生成在sql中。
如：where username=${username}，如果传入的值是111,那么解析成sql时的值为where username=111；
如果传入的值是;drop table user;，则解析成的sql为：select id, username, password, role from user where username=;drop table user;
3、#方式能够很大程度防止sql注入，$方式无法防止Sql注入。
4、$方式一般用于传入数据库对象，例如传入表名.
5、一般能用#的就别用$，若不得不使用“${xxx}”这样的参数，要手工地做好过滤工作，来防止sql注入攻击。
6、在MyBatis中，“${xxx}”这样格式的参数会直接参与SQL编译，从而不能避免注入攻击。但涉及到动态表名和列名时，只能使用“${xxx}”这样的参数格式。所以，这样的参数需要我们在代码中手工进行处理来防止注入。
【结论】在编写MyBatis的映射语句时，尽量采用“#{xxx}”这样的格式。若不得不使用“${xxx}”这样的参数，要手工地做好过滤工作，来防止SQL注入攻击。



### 2、什么是sql注入

　　[ sql注入解释](https://en.wikipedia.org/wiki/SQL_injection)：是一种代码注入技术，用于攻击数据驱动的应用，恶意的SQL语句被插入到执行的实体字段中（例如，为了转储数据库内容给攻击者）

　　**SQL****注入**，大家都不陌生，是一种常见的攻击方式。**攻击者**在界面的表单信息或URL上输入一些奇怪的SQL片段（例如“or ‘1’=’1’”这样的语句），有可能入侵**参数检验不足**的应用程序。所以，在我们的应用中需要做一些工作，来防备这样的攻击方式。在一些安全性要求很高的应用中（比如银行软件），经常使用将**SQL****语句**全部替换为**存储过程**这样的方式，来防止SQL注入。这当然是**一种很安全的方式**，但我们平时开发中，可能不需要这种死板的方式。



### 3、mybatis是如何做到防止sql注入的

　　[MyBatis](https://mybatis.github.io/mybatis-3/)框架作为一款半自动化的持久层框架，其SQL语句都要我们自己手动编写，这个时候当然需要防止SQL注入。其实，MyBatis的SQL是一个具有“**输入+输出**”的功能，类似于函数的结构，参考上面的两个例子。其中，parameterType表示了输入的参数类型，resultType表示了输出的参数类型。回应上文，如果我们想防止SQL注入，理所当然地要在输入参数上下功夫。上面代码中使用#的即输入参数在SQL中拼接的部分，传入参数后，打印出执行的SQL语句，会看到SQL是这样的：

```
select id, username, password, role from user where username=? and password=?
```

　　不管输入什么参数，打印出的SQL都是这样的。这是因为MyBatis启用了预编译功能，在SQL执行前，会先将上面的SQL发送给数据库进行编译；执行时，直接使用编译好的SQL，替换占位符“?”就可以了。因为SQL注入只能对编译过程起作用，所以这样的方式就很好地避免了SQL注入的问题。

　　【底层实现原理】MyBatis是如何做到SQL预编译的呢？其实在框架底层，是JDBC中的PreparedStatement类在起作用，PreparedStatement是我们很熟悉的Statement的子类，它的对象包含了编译好的SQL语句。这种“准备好”的方式不仅能提高安全性，而且在多次执行同一个SQL时，能够提高效率。原因是SQL已编译好，再次执行时无需再编译。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//安全的，预编译了的
Connection conn = getConn();//获得连接
String sql = "select id, username, password, role from user where id=?"; //执行sql前会预编译号该条语句
PreparedStatement pstmt = conn.prepareStatement(sql); 
pstmt.setString(1, id); 
ResultSet rs=pstmt.executeUpdate(); 
......
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//不安全的，没进行预编译
private String getNameByUserId(String userId) {
    Connection conn = getConn();//获得连接
    String sql = "select id,username,password,role from user where id=" + id;
    //当id参数为"3;drop table user;"时，执行的sql语句如下:
    //select id,username,password,role from user where id=3; drop table user;  
    PreparedStatement pstmt =  conn.prepareStatement(sql);
    ResultSet rs=pstmt.executeUpdate();
    ......
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

【 **结论**：】

| #{}：相当于JDBC中的PreparedStatement |
| ------------------------------------ |
| ${}：是输出变量的值                  |

简单说，#{}是经过预编译的，是安全的；${}是未经过预编译的，仅仅是取变量的值，是非安全的，存在SQL注入。
如果我们order by语句后用了${}，那么不做任何处理的时候是存在SQL注入危险的。你说怎么防止，那我只能悲惨的告诉你，你得手动处理过滤一下输入的内容。如判断一下输入的参数的长度是否正常（注入语句一般很长），更精确的过滤则可以查询一下输入的参数是否在预期的参数集合中。

# 数据在网络中如何传输

在发送主机A上，发送的数据经过应用层时，应用层对数据进行了包装，它在要传输的数据上加了一个应用层首部AH后，继续向传输层传送。
传输层接收到应用层的数据后，将数据+应用层AH当做数据，给它进行包装，加上自己的首部，此时的数据变为数据+应用层AH+传输层PH，继续向会话层传送。
依此类推，数据每传递一层，便增加相应协议的首部。
直到传输至数据链路层，数据链路层将加了自己首部的数据交给物理层后，转换为高低跳跃的比特流，这时候的数据才能在线路上传输。

接收端的接收过程与发送过程相反，在接收主机B上，能够通过电信号识别出比特流识别，将收到的信息递交给数据链路层。
数据链路层收到数据后，剥离发送时添加的数据链路层首部DH，把数据提取出来，递交给网络层。
同样的，网络层剥离自己的首部NH，还原后将数据递交给传输层。依此类推，至应用层将其首部AH剥离后，即可还原成最原始的发送数据了。

# xiecheng

## 分布式场景下的超卖问题

防止秒杀重复下单
在Redis中记录一个hash值，用户每次秒杀，值加1，是否大于1作为判断

![img](https://img-blog.csdnimg.cn/2021022417583226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2F3b2R3ZGU=,size_16,color_FFFFFF,t_70)

 

 

使用分布式锁解决超卖的问题


分布式锁
为了保证一个方法 或属性在高并发情况下的同一时间只能被同一个线程执行，在传统单体应用单机部署的情况下，可以使用java并发处理相关的API（ReentrantLock或Synchronized）进行互斥控制，在单击环境汇总，java中提供了很多并发处理的API。但是，随着业务发展的需要，原单体单机部署的系统被演化成分布式集群系统后，由于分布式系统多线程。多进程并且分布在不同机器上，这使原单击部署情况下的并发控制锁策略失效，单纯的java Api并不能提供分布式的能力。为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问 ，这就是分布式锁要解决的问题

 

分布式锁应该具备的条件
在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行
高可用的获取锁与释放锁
高性能的获取锁与释放锁
具备可重入的特性
具备锁失效机制，防止死锁
 具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败


分布式的实现的方式
1、基于数据库的实现方式
基于数据库的实现的核心思想是：在数据库中创建一个表，表中包含方法名等字段，并在方法名字端上创建唯一索引，想要执行某个方法，就使用这个方法名向表中插入数据，成功插入则获取锁，执行完成后删除对应的行数据释放锁。

数据库实现有以下几个问题：

这把锁强依赖数据库可用性，数据库是一个单点，一旦数据库挂掉，会导致业务不可用
这把锁没有失效时间，一旦解锁操作失败，就会导致锁记录一直在数据库中，其他线程无法再获得这把锁
这把锁只能是非阻塞的，因为数据的insert操作，一旦插入失败就会直接报错。没有获得锁的线程并不会进入排队队列，要想再次获得锁就要再次触发获得锁操作
这把锁是非重入的，同一个线程没有释放锁之前无法再次获得该锁。因为数据中数据已经存在了


基于数据库排他锁


除了可以通过增删操作数据表中的记录以外，其实还可以借助数据库中自带的锁来实现分布式的锁。我们还用刚刚创建的那张数据库表。可以通过数据库的排它锁来实现分布式锁。



 ![img](https://img-blog.csdnimg.cn/20210225154502624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2F3b2R3ZGU=,size_16,color_FFFFFF,t_70)

在查询语句后面增加 for update，数据库在查询过程中给数据表增加排他锁（InnoDb引擎在加锁的时候，只有通过索引进行检索的时候才会使用行级锁，否则会使用表级锁。我们希望使用行级锁，就要给method_name添加索引，这个索引一定要创建成唯一索引，否则会出现多个重载方法之间无法同时访问的问题）当某条记录被加上排他锁之后，其他线程无法再在该行记录上增加排他锁。

 

我们可以认为获得排他锁的线程即可获得分布式锁，当获取到锁之后，可以执行方法的业务逻辑，执行完方法之后，在通过以下方法解锁

![img](https://img-blog.csdnimg.cn/2021022516132824.png)

通过connection.commit（）操作来释放锁

这种方法可以有效的解决上面提到的无法释放锁和阻塞锁的问题。

阻塞锁？ for update 语句会在执行成功后立即返回，在执行失败时一直处于阻塞状态，直到成功
锁定之后服务宕机，无法释放？使用这种方式，服务宕机之后数据库会把自己释放掉
但是还是⽆法直接解决数据库单点问题。

这⾥还可能存在另外⼀个问题，虽然我们对 method_name 使⽤了唯⼀索引，并且显示使⽤ for  update 来使⽤⾏级锁。但是，MySql会对查询进⾏优化，即便在条件中使⽤了索引字段，但是否使⽤索

引来检索数据是由 MySQL 通过判断不同执⾏计划的代价来决定的，如果 MySQL 认为全表扫效率更⾼， ⽐如对⼀些很⼩的表，它就不会使⽤索引，这种情况下 InnoDB 将使⽤表锁，⽽不是⾏锁。如果发⽣这

种情况就悲剧了。。。

还有⼀个问题，就是我们要使⽤排他锁来进⾏分布式锁的lock，那么⼀个排他锁⻓时间不提交，就会占

⽤数据库连接。⼀旦类似的连接变得多了，就可能把数据库连接池撑爆

 

数据库实现分布式的缺点：
会有各种各样的问题，在解决问题的过程中会使整个方案变得越来越复杂
操作数据库需要一定的开销，性能问题需要考虑
使用数据库的行级锁并不一定靠谱，尤其是当我们的锁表并不大的时候（有可能锁住整张表）


基于Redis的实现方式
redis实现分布式的优点：
1、Redis有很高的性能

2、Redis命令对此支持较好，实现起来比较方便

 

命令介绍
1、SETNX

SETNX key val : 当且仅当key不存在时，set一个key为val的字符串，返回1，若key存在，则什么都不做，返回0

 

2、expire 

expire key timeout : 为key设置一个超时时间，单位为second，超过这个时间锁会自动释放，避免死锁。

 

3、delete

delete key ： 删除key

使用redis分布式的时候，主要就会使用到这三个命令

 

实现思想
1、获取锁的时候，使用setnx加锁，并使用expire命令为锁添加一个超时时间，超过该时间自动释放锁，所得value值为一个随机生成的UUID，通过此在释放锁的时候判断

2、获取锁的时候还设置一个获取超时时间，若超过这个时间则放弃获取锁

3、释放锁的时候，通过UUID判断是不是该锁，若是该锁，则执行delete进行所释放

 

总结
可以使用缓存来替代数据库来实现分布式锁，这个可以提供更好的性能，同时，很多缓存服务器都是集群部署的，可以避免单点问题。并且很多缓存服务都提供了可以用来实现分布式的方法，比如Tair的put ⽅法，redis的setnx⽅法等。并且，这些缓存服务也都提供了对数据的过期⾃动删除的⽀持，可以直接设置超时时间来控制锁的释放

 

使用缓存实现分布式锁的优点

性能好，实现起来较为方便
使用缓存实现分布式锁的缺点
使用缓存实现分布式锁的优点

通过超时时间来控制锁的失效时间并不是十分的靠谱

 

 

基于ZooKeeper的实现方式
实现分析
ZooKeeper是一个为分布式应用提供一致性服务的开源组件，它内部是一个分层的文件系统目录数结构，规定同一个目录下文件名不能重复。基于ZooKeeper实现分布式锁的步骤如下：

创建一个目录 mylock
线程A想获取锁就在mylock目录下创建临时顺序节点
获取mylock目录下的子节点，然后获取比自己小的兄弟节点，如果不存在，则说明当前线程顺序号最小，获得锁
线程B获取锁节点，判断自己不是最小节点，设置监听比自己小的节点
线程A处理完，删除自己的节点，线程B监听到变更事件，判断自己是不是最小的节点，如果是则获得锁
————————————————
版权声明：本文为CSDN博主「你怎么敢的呀」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/awodwde/article/details/114032040

## 分布式锁的优化

## 首先，我们一起来看看这个问题的背景？

前段时间有个朋友在外面面试，然后有一天找我聊说：有一个国内不错的电商公司，面试官给他出了一个场景题：

- 假如下单时，用分布式锁来防止库存超卖，但是是每秒上千订单的高并发场景，如何对分布式锁进行高并发优化来应对这个场景？
- 他说他当时没答上来，因为没做过没什么思路。其实我当时听到这个面试题心里也觉得有点意思，因为如果是我来面试候选人的话，应该会给的范围更大一些
- 比如，让面试的同学聊一聊电商高并发秒杀场景下的库存超卖解决方案，各种方案的优缺点以及实践，进而聊到分布式锁这个话题。
- 因为库存超卖问题是有很多种技术解决方案的，比如悲观锁，分布式锁，乐观锁，队列串行化，Redis原子操作，等等吧。
- 但是既然那个面试官兄弟限定死了用分布式锁来解决库存超卖，我估计就是想问一个点：在高并发场景下如何优化分布式锁的并发性能
- 我觉得，面试官提问的角度还是可以接受的，因为在实际落地生产的时候，分布式锁这个东西保证了数据的准确性，但是他天然并发能力有点弱。
- 刚好我之前在自己项目的其他场景下，确实是做过高并发场景下的分布式锁优化方案，因此正好是借着这个朋友的面试题，把分布式锁的高并发优化思路，给大家来聊一聊。

## 库存超卖现象是怎么产生的？

先来看看如果不用分布式锁，所谓的电商库存超卖是啥意思？大家看看下面的图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123093451423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzOTEzNDI=,size_16,color_FFFFFF,t_70)

- 这个图，其实很清晰了，假设订单系统部署两台机器上，不同的用户都要同时买10台iphone，分别发了一个请求给订单系统。
- 接着每个订单系统实例都去数据库里查了一下，当前iphone库存是12台。
- 俩大兄弟一看，乐了，12台库存大于了要买的10台数量啊！
- 于是乎，每个订单系统实例都发送SQL到数据库里下单，然后扣减了10个库存，其中一个将库存从12台扣减为2台，另外一个将库存从2台扣减为-8台。
- 现在完了，库存出现了负数！泪奔啊，没有20台iphone发给两个用户啊！这可如何是好。

## 用分布式锁如何解决库存超卖问题？

我们用分布式锁如何解决库存超卖问题呢？其实很简单，回忆一下上次我们说的那个分布式锁的实现原理：

同一个锁key，同一时间只能有一个客户端拿到锁，其他客户端会陷入无限的等待来尝试获取那个锁，只有获取到锁的客户端才能执行下面的业务逻辑。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123093606240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzOTEzNDI=,size_16,color_FFFFFF,t_70)

代码大概就是上面那个样子，现在我们来分析一下，为啥这样做可以避免库存超卖？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123093631322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzOTEzNDI=,size_16,color_FFFFFF,t_70)

- 大家可以顺着上面的那个步骤序号看一遍，马上就明白了。
- 从上图可以看到，只有一个订单系统实例可以成功加分布式锁，然后只有他一个实例可以查库存、判断库存是否充足、下单扣减库存，接着释放锁。
- 释放锁之后，另外一个订单系统实例才能加锁，接着查库存，一下发现库存只有2台了，库存不足，无法购买，下单失败。不会将库存扣减为-8的。

## 有没有其他方案可以解决库存超卖问题？

- 当然有啊！比如悲观锁，分布式锁，乐观锁，队列串行化，异步队列分散，Redis原子操作，等等，很多方案，我们对库存超卖有自己的一整套优化机制。
- 但是前面说过了，这篇文章就聊一个分布式锁的并发优化，不是聊库存超卖的解决方案，所以库存超卖只是一个业务场景而已

## 分布式锁的方案在高并发场景下

好，现在我们来看看，分布式锁的方案在高并发场景下有什么问题？

- 问题很大啊！兄弟，不知道你看出来了没有。分布式锁一旦加了之后，对同一个商品的下单请求，会导致所有客户端都必须对同一个商品的库存锁key进行加锁。
- 比如，对iphone这个商品的下单，都必对“iphone_stock”这个锁key来加锁。这样会导致对同一个商品的下单请求，就必须串行化，一个接一个的处理。
- 大家再回去对照上面的图反复看一下，应该能想明白这个问题。
- 假设加锁之后，释放锁之前，查库存 -> 创建订单 -> 扣减库存，这个过程性能很高吧，算他全过程20毫秒，这应该不错了。
- 那么1秒是1000毫秒，只能容纳50个对这个商品的请求依次串行完成处理。
- 比如一秒钟来50个请求，都是对iphone下单的，那么每个请求处理20毫秒，一个一个来，最后1000毫秒正好处理完50个请求。

大家看一眼下面的图，加深一下感觉。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123093818790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzOTEzNDI=,size_16,color_FFFFFF,t_70)

- 所以看到这里，大家起码也明白了，简单的使用分布式锁来处理库存超卖问题，存在什么缺陷。
- 缺陷就是同一个商品多用户同时下单的时候，会基于分布式锁串行化处理，导致没法同时处理同一个商品的大量下单的请求。
- 这种方案，要是应对那种低并发、无秒杀场景的普通小电商系统，可能还可以接受。
- 因为如果并发量很低，每秒就不到10个请求，没有瞬时高并发秒杀单个商品的场景的话，其实也很少会对同一个商品在一秒内瞬间下1000个订单，因为小电商系统没那场景

## 如何对分布式锁进行高并发优化？

好了，终于引入正题了，那么现在怎么办呢？

- 面试官说，我现在就卡死，库存超卖就是用分布式锁来解决，而且一秒对一个iphone下上千订单，怎么优化？
- 现在按照刚才的计算，你一秒钟只能处理针对iphone的50个订单。
- 其实说出来也很简单，相信很多人看过java里的ConcurrentHashMap的源码和底层原理，应该知道里面的核心思路，就是分段加锁！
- 把数据分成很多个段，每个段是一个单独的锁，所以多个线程过来并发修改数据的时候，可以并发的修改不同段的数据。不至于说，同一时间只能有一个线程独占修改ConcurrentHashMap中的数据。
- 另外，Java 8中新增了一个LongAdder类，也是针对Java
  7以前的AtomicLong进行的优化，解决的是CAS类操作在高并发场景下，使用乐观锁思路，会导致大量线程长时间重复循环。
- LongAdder中也是采用了类似的分段CAS操作，失败则自动迁移到下一个分段进行CAS的思路。
- 其实分布式锁的优化思路也是类似的，之前我们是在另外一个业务场景下落地了这个方案到生产中，不是在库存超卖问题里用的。

但是库存超卖这个业务场景不错，很容易理解，所以我们就用这个场景来说一下。大家看看下面的图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181123093945970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzOTEzNDI=,size_16,color_FFFFFF,t_70)

- 其实这就是分段加锁。你想，假如你现在iphone有1000个库存，那么你完全可以给拆成20个库存段，要是你愿意，可以在数据库的表里建20个库存字段，比如stock_01，stock_02，类似这样的，也可以在redis之类的地方放20个库存key。
- 总之，就是把你的1000件库存给他拆开，每个库存段是50件库存，比如stock_01对应50件库存，stock_02对应50件库存。
- 接着，每秒1000个请求过来了，好！此时其实可以是自己写一个简单的随机算法，每个请求都是随机在20个分段库存里，选择一个进行加锁。
- bingo！这样就好了，同时可以有最多20个下单请求一起执行，每个下单请求锁了一个库存分段，然后在业务逻辑里面，就对数据库或者是Redis中的那个分段库存进行操作即可，包括查库存 -> 判断库存是否充足 -> 扣减库存。
- 这相当于什么呢？相当于一个20毫秒，可以并发处理掉20个下单请求，那么1秒，也就可以依次处理掉20 * 50 = 1000个对iphone的下单请求了。
- 一旦对某个数据做了分段处理之后，有一个坑大家一定要注意：就是如果某个下单请求，咔嚓加锁，然后发现这个分段库存里的库存不足了，此时咋办？
- 这时你得自动释放锁，然后立马换下一个分段库存，再次尝试加锁后尝试处理。这个过程一定要实现。

## 分布式锁并发优化方案有没有什么不足？

不足肯定是有的，最大的不足，大家发现没有，很不方便啊！实现太复杂了。

- 首先，你得对一个数据分段存储，一个库存字段本来好好的，现在要分为20个分段库存字段；
- 其次，你在每次处理库存的时候，还得自己写随机算法，随机挑选一个分段来处理；
- 最后，如果某个分段中的数据不足了，你还得自动切换到下一个分段数据去处理
- 这个过程都是要手动写代码实现的，还是有点工作量，挺麻烦的。
- 不过我们确实在一些业务场景里，因为用到了分布式锁，然后又必须要进行锁并发的优化，又进一步用到了分段加锁的技术方案，效果当然是很好的了，一下子并发性能可以增长几十倍。

## 该优化方案的后续改进

- 以我们本文所说的库存超卖场景为例，你要是这么玩，会把自己搞的很痛苦！
- 再次强调，我们这里的库存超卖场景，仅仅只是作为演示场景而已，以后有机会，再单独聊聊高并发秒杀系统架构下的库存超卖的其他解决方案。

## 上篇文章的补充说明

- 本文最后做个说明，笔者收到一些朋友留言，说有朋友在技术群里看到上篇文章之后，吐槽了一通上一篇文章（《拜托，面试请不要再问我Redis分布式锁的实现原理》），说是那个Redis分布式锁的实现原理把人给带歪了。
- 在这儿得郑重说明一下，上篇文章，明确说明了是Redisson那个开源框架对Redis锁的实现原理，并不是我个人YY出来的那一套原理。
- 实际上Redisson作为一款优秀的开源框架，我觉得他整体对分布式锁的实现是OK的，虽然有一些缺陷，但是生产环境可用。
- 另外，有的兄弟可能觉得那个跟Redis官网作者给出的分布式锁实现思路不同，所以就吐槽，说要遵循Redis官网中的作者的分布式锁实现思路。
- 其实我必须指出，Redis官网中给出的仅仅是Redis分布式锁的实现思路而已，记住，那是思路！思路跟落地生产环境的技术方案之间是有差距的。
- 比如说Redis官网给出的分布式锁实现思路，并没有给出到分布式锁的自动续期机制、锁的互斥自等待机制、锁的可重入加锁与释放锁的机制。但是Redisson框架对分布式锁的实现是实现了一整套机制的
- 所以重复一遍，那仅仅是思路，如果你愿意，你完全可以基于Redis官网的思路自己实现一套生产级的分布式锁出来。
- 另外Redis官网给出的RedLock算法，一直是我个人并不推崇在生产使用的。
- 因为那套算法中可能存在一些逻辑问题，在国外是引发了争议的，连Redis作者自己都在官网中给出了因为他的RedLock算法而引发争议的文章，当然他自己是不太同意的。
- 但是这个事儿，就搞成公说公有理，婆说婆有理了。具体请参加官网原文
- 因此下回有机会，我会通过大量手绘图的形式，给大家写一下Redis官方作者自己提出的RedLock分布式锁的算法，以及该算法基于Redisson框架如何落地生产环境使用，到时大家可以再讨论。

## StringBuilder.toString()

```java
String str = "test";
String sb = new StringBuilder("test").toString();
str == sb // false
```

StringBuilder类的toString()方法会新建一个String返回，所以str和sb不相等

# teng

## 有抽象类为什么还要接口

## 线程池怎样让线程一直处在运行状态

