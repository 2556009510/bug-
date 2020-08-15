**目录**



[利用JdbcTemplate实现增删改查：](#利用JdbcTemplate实现增删改查：)

[1.插入](#1.插入)

[2.批量插入](#2.批量插入)

[3.删](#3.删)

[4.更](#4.更)

[5.查询某个值，并以对象的方式返回](#5.查询某个值，并以对象的方式返回)

[6.查询返回集合对象](#6.查询返回集合对象)

[7.返回组合函数的值](#7.返回组合函数的值)

[8.使用具备具名函数的JdbcTemplate](#8.使用具备具名函数的JdbcTemplate)

[9.整合EmpDao](#9.整合EmpDao)

[10.也可以去官方API文档](#10.也可以去官方API文档)

------

# 利用JdbcTemplate实现增删改查：

1、将上一个项目的pom.xml文件内的插件复制过来：

pom.xml

```XML
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>

   <groupId>com.mashibing</groupId>
   <artifactId>spring_demo</artifactId>
   <version>1.0-SNAPSHOT</version>

   <dependencies>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.2.3.RELEASE</version>
       </dependency>

       <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.1.21</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.47</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/cglib/cglib -->
       <dependency>
           <groupId>cglib</groupId>
           <artifactId>cglib</artifactId>
           <version>3.3.0</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
       <dependency>
           <groupId>org.aspectj</groupId>
           <artifactId>aspectjweaver</artifactId>
           <version>1.9.5</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/aopalliance/aopalliance -->
       <dependency>
           <groupId>aopalliance</groupId>
           <artifactId>aopalliance</artifactId>
           <version>1.0</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-aspects</artifactId>
           <version>5.2.3.RELEASE</version>
       </dependency>
   </dependencies>

</project>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**并且导入：**

```XML
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-orm</artifactId>
           <version>5.2.3.RELEASE</version>
       </dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

2、再添加MySQL和Druid（阿里巴巴）

3、

![img](https://img-blog.csdnimg.cn/20200708231333103.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

4、创建：

![img](https://img-blog.csdnimg.cn/20200708231345568.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**db.properties**

```java
jdbc.username=root
jdbc.password=123456
jdbc.url=jdbc:mysql://localhost:3306/deom  //deom是数据库名称
jdbc.driverName=com.mysql.jdbc.Driver
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**xml****文件：applicationContext** ![img](https://img-blog.csdnimg.cn/20200708231436449.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="
http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"        
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd               
       http://www.springframework.org/schema/aop        
       https://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:component-scan base-package="com.mashibing"></context:component-scan> //组件扫描
    <context:property-placeholder location="classpath:db.properties">
</context:property-placeholder>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
        <property name="driverClassName" value="${jdbc.driverName}"></property>
        <property name="url" value="${jdbc.url}"></property>
    </bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708231635773.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
//jdbcTemplate注册为bean对象
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708231703184.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
//配置事务管理器的bean对象
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
//开启基于注解的事务管理器的配置
    <tx:annotation-driven transaction-manager="transactionManager">
</tx:annotation-driven>
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
("applicationContext.xml");
    @Test
    public void test01() throws SQLException {
        DruidDataSource dataSource = context.getBean("dataSource", DruidDataSource.class);
        System.out.println(dataSource.getConnection());
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
      System.out.println(jdbcTemplate);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708231837261.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 1.插入

```java
    @Test //增
    public void test02(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "insert into emp(empno,ename) values(?,?)";
        int zhangsan = jdbcTemplate.update(sql, 1111, "zhangsan");
        System.out.println(zhangsan);  
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708231905280.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 2.批量插入

```java
@Test //批量增加
    public void test03(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "insert into emp(empno,ename) values(?,?)";
        List<Object[]> list = new ArrayList<Object[]>();
        list.add(new Object[]{2222,"lisi"});
        list.add(new Object[]{3333,"wangwu"});
        list.add(new Object[]{4444,"maliu"});
        int[] result = jdbcTemplate.batchUpdate(sql,list);
        for (int i : result) {
            System.out.println(i);
        }
    }

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708231933706.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200708231937569.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.删

```java
    @Test //删
    public void test04(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "delete from emp where empno = ?";
        int zhangsan = jdbcTemplate.update(sql, 1111);
        System.out.println(zhangsan);  
    }

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708232008622.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 4.更

```java
    @Test //改
    public void test05(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "update  emp set ename=? where empno=?";
        int wangwu= jdbcTemplate.update(sql,"mashibing",2222);
        System.out.println(wangwu);  
    }

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708232044855.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 5.查询某个值，并以对象的方式返回

**java****文件：****Emp（容器）** ![img](https://img-blog.csdnimg.cn/20200708232052272.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
public class Emp {

    private Integer empno;
    private String ename;
    private String job;
    private Integer mgr;
    private Date hiredate;
    private Double sal;
    private Double comm;
    private Integer deptno;

    public Emp() {
    }
    public Emp(Integer empno, String ename) {
        this.empno = empno;
        this.ename = ename;
    }
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

**java****文件：****MyTest****（测试）**

```java
    @Test //查
    public void test06(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "select * from emp where empno=?";
        Emp result = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<>(Emp.class),7369);
        System.out.println(result);
    }

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708232154766.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 6.查询返回集合对象

```java
    @Test //查询所有
    public void test07(){
        JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "select * from emp where sal >?";
        List<Emp> result = jdbcTemplate.query(sql,new BeanPropertyRowMapper<>(Emp.class),1500);
        for (Emp emp : result) {
            System.out.println(emp);
        }
    }

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 7.返回组合函数的值

**MyTest.java**

```java
public class MyTest {
   public static void main(String[] args) throws SQLException {
       ApplicationContext context = new ClassPathXmlApplicationContext("jdbcTemplate.xml");
       JdbcTemplate jdbcTemplate = context.getBean("jdbcTemplate", JdbcTemplate.class);

       String  sql = "select max(sal) from emp";
       Double aDouble = jdbcTemplate.queryForObject(sql, Double.class);
       System.out.println(aDouble);
  }
}

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 8.使用具备具名函数的JdbcTemplate

**MyTest.java**

```java
public class MyTest {
   public static void main(String[] args) throws SQLException {
       ApplicationContext context = new ClassPathXmlApplicationContext("jdbcTemplate.xml");
       NamedParameterJdbcTemplate jdbcTemplate = context.getBean("namedParameterJdbcTemplate", NamedParameterJdbcTemplate.class);

       String  sql = "insert into emp(empno,ename) values(:empno,:ename)";
       Map<String,Object> map = new HashMap<>();
       map.put("empno",2222);
       map.put("ename","sili");
       int update = jdbcTemplate.update(sql, map);
       System.out.println(update);
  }
}

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 9.整合EmpDao

**添加：**

![img](https://img-blog.csdnimg.cn/20200708232248335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****EmpDao** ![img](https://img-blog.csdnimg.cn/20200708232257956.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
@Repository仓库
public class EmpDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public void save(Emp emp){
        String sql = "insert into emp(empno,ename) values(?,?)";
        int update = jdbcTemplate.update(sql,emp.getEmpno(),emp.getEname());
        System.out.println(update);
    }
}

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
    @Test //插入
    public void test08(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        empDao.save(new Emp(1111,"zhangsan"));
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815130809527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200815130813919.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200708232359416.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 10.也可以去官方API文档

![img](https://img-blog.csdnimg.cn/20200815130833323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)
