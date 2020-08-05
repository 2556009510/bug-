# 1、数据库操作框架的历程

## (1)  JDBC

​       JDBC(Java Data Base Connection,java数据库连接)是一种用于执行SQL语句的Java API可以为多种关系数据库提供统一访问,它由一组用Java语言编写的类和接口组成.JDBC提供了一种基准,据此可以构建更高级的工具和接口,使数据库开发人员能够编写数据库应用程序

**优点：**运行期：快捷、高效

**缺点：**编辑期：代码量大、繁琐异常处理、不支持数据库跨平台

![img](https://img-blog.csdnimg.cn/20200805140019623.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## (2) DBUtils

​              DBUtils是Java编程中的数据库操作实用工具，小巧简单实用。

​              DBUtils封装了对JDBC的操作，简化了JDBC操作，可以少写代码。

​              DBUtils三个核心功能介绍：

​                     1、QueryRunner中提供对sql语句操作的API

​                     2、ResultSetHandler接口，用于定义select操作后，怎样封装结果集

​                     3、DBUtils类，它就是一个工具类，定义了关闭资源与事务处理的方法

## (3)Hibernate

​              Hibernate 是由 Gavin King 于 2001 年创建的开放源代码的对象关系框架。它强大且高效的构建具有关系对象持久性和查询服务的 Java 应用程序。

​              Hibernate 将 Java 类映射到数据库表中，从 Java 数据类型中映射到 SQL 数据类型中，并把开发人员从 95% 的公共数据持续性编程工作中解放出来。

​              Hibernate 是传统 Java 对象和数据库服务器之间的桥梁，用来处理基于 O/R 映射机制和模式的那些对象。

![img](https://img-blog.csdnimg.cn/20200805140044177.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**Hibernate** **优势：**

1. Hibernate 使用 XML 文件来处理映射 Java 类别到数据库表格中，并且不用编写任何代码。
2. 为在数据库中直接储存和检索 Java 对象提供简单的 API。
3. 如果在数据库中或任何其它表格中出现变化，那么仅需要改变 XML 文件属性。
4. 抽象不熟悉的 SQL 类型，并为我们提供工作中所熟悉的 Java 对象。
5. Hibernate 不需要应用程序服务器来操作。
6. 操控你数据库中对象复杂的关联。
7. 最小化与访问数据库的智能提取策略。
8. 提供简单的数据询问。

**Hibernate****劣势：**

1. hibernate的完全封装导致无法使用数据的一些功能。
2. Hibernate的缓存问题。
3. Hibernate对于代码的耦合度太高。
4. Hibernate寻找bug困难。
5. Hibernate批量数据操作需要大量的内存空间而且执行过程中需要的对象太多

## (4) JDBCTemplate（jdbc模板）

JdbcTemplate针对数据查询提供了多个重载的模板方法,你可以根据需要选用不同的模板方法.如果你的查询很简单，仅仅是传入相应SQL或者相关参数，然后取得一个单一的结果，那么你可以选择如下一组便利的模板方法。

**优点：**运行期：高效、内嵌Spring框架中、支持基于AOP的声明式事务             

**缺点：**必须于Spring框架结合在一起使用、不支持数据库跨平台、默认没有缓存

# 2、什么是Mybatis？

​       MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

​       **优点：**

​              1、与JDBC相比，减少了50%的代码量

​              2、最简单的持久化框架，简单易学

​              3、SQL代码从程序代码中彻底分离出来，可以重用

​              4、提供XML标签，支持编写动态SQL

​              5、提供映射标签，支持对象与数据库的ORM字段关系映射

​       **缺点：**

​              1、SQL语句编写工作量大，熟练度要高

​              2、数据库移植性比较差，如果需要切换数据库的话，SQL语句会有很大的差异

# 3、第一个Mybatis项目

## 3.1、创建普通的maven项目

![img](https://img-blog.csdnimg.cn/2020080514013695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140146138.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805140148535.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.2、导入相关的依赖

**导入依赖：**

```XML
<dependencies>
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.4</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.19</version>
    </dependency>
</dependencies>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140223970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140228533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140233151.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805140235117.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.3、创建对应的数据表，数据表我们使用之前的demo数据库，脚本文件在群里，大家自行去下载安装

## 3.4、创建与表对应的实体类对象

**Emp.****java**

```java
public class Emp {

    private Integer empno; //员工编号
    private String ename;  //员工姓名
    private String job;    //职业
    private Integer mgr;   //上级编号
    private Date hiredate; //入职日期
    private Double sal;    //工资
    private Double comm;   //商品
    private Integer deptno;//部门编号

    public Integer getEmpno() {return empno;}
    public void setEmpno(Integer empno) {this.empno = empno;}

    public String getEname() {return ename;}
    public void setEname(String ename) {this.ename = ename;}

    public String getJob() {return job;}
    public void setJob(String job) {this.job = job;}

    public Integer getMgr() {return mgr;}
    public void setMgr(Integer mgr) {this.mgr = mgr;}

    public Date getHiredate() {return hiredate;}
    public void setHiredate(Date hiredate) {this.hiredate = hiredate;}

    public Double getSal() {return sal;}
    public void setSal(Double sal) {this.sal = sal;}

    public Double getComm() {return comm;}
    public void setComm(Double comm) {this.comm = comm;}

    public Integer getDeptno() {return deptno;}
    public void setDeptno(Integer deptno) {this.deptno = deptno;}

    @Override
    public String toString() {
        return "Emp{" +
                "empno=" + empno +
                ", ename='" + ename + '\'' +
                ", job='" + job + '\'' +
                ", mgr=" + mgr +
                ", hiredate=" + hiredate +
                ", sal=" + sal +
                ", comm=" + comm +
                ", deptno=" + deptno +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.5、创建对应的dao类

**EmpDao.****java****（Emp接口）**

```java
public interface EmpDao {

    public void save(Emp emp);           //保存
    public Integer update(Emp emp);      //更新
    public Integer delete(Integer empno);//删除
    public Emp selectEmpByEmpno(Integer empno);//选择
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.6、编写配置文件

**mybatis-config.****xml****（mybatis配置）**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
//在填写标签的时候一定要注意相关配置的顺序 
<configuration配置>
<properties属性 resource资源="db.properties"></properties>
    <environments环境s default默认值="development开发">
        <environment环境 id="development开发">
            <transactionManager事务管理器 type="JDBC"/>
            <dataSource数据源 type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers映射>
        <mapper resource资源="EmpDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140348609.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)//在填写标签的时候一定要注意相关配置的顺序 

**db.****properties**

```java
driver=com.mysql.cj.jdbc.Driver
//低于MySQL8.0的版本需要加serverTimezone=UTC，并重启
url=jdbc:mysql://192.168.85.111:3306/demo?serverTimezone=UTC
username=root
password=123456
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
// namespace（命名空间）：编写接口的全类名，就是告诉要实现该配置文件是哪个接口的具体实现
<mapper namespace命名空间="com.mashibing.dao.EmpDao">
    <select id="selectEmpByEmpno" resultType="Emp">
        select * from emp where empno = #{empno}
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**id****：**表示的是要匹配的方法的名称
**resultType****：**表示返回值的类型，查询操作必须要包含返回值的类型
**#{****属性名}：**表示要传递的参数的名称

## 3.7、编写测试类

 ![img](https://img-blog.csdnimg.cn/20200805140747778.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **导入依赖**

**MyTest.java** ![img](https://img-blog.csdnimg.cn/20200805140747722.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

```java
public class MyTest {

    @Test
    public void test01() throws IOException {
        String resource资源 = "mybatis-config.xml";
        InputStream inputStream字节输入流 = Resources.getResourceAsStream(resource); 
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
//执行具体的sql语句
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140824981.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140828492.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

//增删改查操作不需要返回值，增删改返回的是影响的行数，mybatis会自动做判断

**MyTest.java** ![img](https://img-blog.csdnimg.cn/20200805140838981.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  

```java
public class MyTest {

    @Test
    public void test01() throws IOException {
        String resource资源 = "mybatis-config.xml";
        InputStream inputStream字节输入流 = Resources.getResourceAsStream(resource); 
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
//执行具体的sql语句
        Emp emp = mapper.selectEmpByEmpno(7369); //7369是员工编号
        System.out.println(emp);
================================ 添加 =========================================
        Integer i = mapper.save(class Emp());
        System.out.println(i);
===============================================================================
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test01()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805140858819.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 4、增删改查的基本操作

![img](https://img-blog.csdnimg.cn/20200805140908501.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//如果SQL语句较复杂，建议使用该方式，但这里先不用**

## 增加员工：

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
    <select id="selectEmpByEmpno" resultType="Emp">
        select * from emp where empno = #{empno}
    </select>
================================ 添加 =========================================
<insert id="save" >
        insert into emp(empno,ename) values(#{empno},#{ename})
    </insert>
    <update id="update">
        update emp set sal=#{sal} where empno = #{empno}
    </update>
    <delete id="delete">
        delete from emp where empno = #{empno}
    </delete>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805140940483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805140946160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141004246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test02()****，点运行：**

![img](https://img-blog.csdnimg.cn/2020080514101191.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

![img](https://img-blog.csdnimg.cn/20200805141015634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 增加该员工属性值：

**MyTest.java** ![img](https://img-blog.csdnimg.cn/2020080514101939.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

```java
public class MyTest {

    SqlSessionFactory sqlSessionFactory（线程工厂） =  null;

    @Before以前
    public void init(){
        String resource资源 = "mybatis-config.xml";
        InputStream inputStream字节输入流 = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    }
    @Test
    public void test01() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
//执行具体的sql语句
        Emp emp = mapper.selectEmpByEmpno(7369); //7369是员工编号
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
@Test
    public void test02(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");//传的是具其的对象，不是属性值，刚刚是属性值？
        Integer save = mapper.save(emp);
        System.out.println(save保存);
        sqlSession.commit提交();
        sqlSession.close();
    }
============================ 复制上面方法的内容 ===============================
@Test
    public void test03(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");
        emp.setSal(500.0);  //添加
        Integer update = mapper.update(emp); //添加
        System.out.println(update更新);
        sqlSession.commit提交();
        sqlSession.close();
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test03()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141046466.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 删除员工：

**MyTest.java** ![img](https://img-blog.csdnimg.cn/20200805141051196.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

```java
public class MyTest {

    SqlSessionFactory sqlSessionFactory（线程工厂） =  null;

    @Before以前
    public void init(){
        String resource资源 = "mybatis-config.xml";
        InputStream inputStream字节输入流 = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    }
    @Test
    public void test01() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
//执行具体的sql语句
        Emp emp = mapper.selectEmpByEmpno(7369); //7369是员工编号
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
@Test
    public void test02(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");//传的是具其的对象，不是属性值，刚刚是属性值？
        Integer save = mapper.save(emp);
        System.out.println(save保存);
        sqlSession.commit提交();
        sqlSession.close();
    }
@Test
    public void test03(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");
        emp.setSal(500.0);                  //添加
        Integer update = mapper.update(emp);//添加
        System.out.println(update更新);
        sqlSession.commit提交();
        sqlSession.close();
    }
============================ 复制上面方法的内容 ===============================
    @Test
    public void test04(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        //Emp emp = new Emp();
        //emp.setEmpno(3333);
        //emp.setEname("zhangsan");
        Integer delete = mapper.delete(3333); //添加
        System.out.println(delete删除);
        sqlSession.commit提交();
        sqlSession.close();
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test04()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141116386.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 5、配置文件详解

​    在mybatis的项目中，我们发现了有一个mybatis-config.xml的配置文件，这个配置文件是mybatis的全局配置文件，用来进行相关的全局配置，在任何操作下都生效的配置。下面我们要针对其中的属性做详细的解释，方便大家在后续使用的时候更加熟练。

## **（1）日志配置文件：**

**导入依赖：**

```XML
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**log4j.****properties**

```bash
# 全局日志
log4j.rootLogger=INFO信息, stdout输出  
# MyBatis 日志
log4j.logger.com.mashibing=TRACE跟踪
# 控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test01()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141209759.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141213899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141217636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141223285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141227308.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****与第一个一样**

![img](https://img-blog.csdnimg.cn/20200805141234276.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //到现在为止，讲了四种配置文件

===========================================================

![img](https://img-blog.csdnimg.cn/20200805141246481.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805141248183.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

