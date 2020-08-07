# 2、select元素

## 5、联合查询

```XML
    <groupId>com.mashibing</groupId>
    <artifactId>mybatis_helloworld</artifactId>
    <version>1.0-SNAPSHOT</version>

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
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

    </dependencies>
<!--    <build>-->
<!--        <resources>-->
<!--            <resource>-->
<!--                <directory>src/main/java</directory>-->
<!--                <includes>-->
<!--                    <include>**/*.xml</include>-->
<!--                </includes>-->
<!--            </resource>-->
<!--        </resources>-->
<!--    </build>-->
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

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
    private Dept dept;     //部门（点进去会跳转到Dept.java）

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

**Dept****.****java****（部门）**

```java
public class Dept {
    private Integer deptno;//部门编号
    private String dname;  //部门名称
    private String loc;    //地方

    public Integer getDeptno() {return deptno;}
    public void setDeptno(Integer deptno) {this.deptno = deptno;}

    public String getDname() {return dname;}
    public void setDname(String dname) {this.dname = dname;}

    public String getLoc() {return loc;}
    public void setLoc(String loc) {this.loc = loc;}

    @Override
    public String toString() {
        return "Dept{" +
                "deptno=" + deptno +
                ", dname='" + dname + '\'' +
                ", loc='" + loc + '\'' +
                ", emps=" + emps +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace命名空间="com.mashibing.dao.EmpDao">
//将每一个属性值都映射成对象中的数据，如果有实体类对象，就写成对象.属性的方式
    <resultMap结果映射 id="myEmp" type="com.mashibing.bean.Emp">
        <id column="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        <result column="deptno" property="dept.deptno"></result>
        <result column="dname" property="dept.dname"></result>
        <result column="loc" property="dept.loc"></result>
    </resultMap>

    <select id="selectEmpByEmpno" resultMap结果映射="myEmp">
        select * from emp where empno = #{empno}
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
public class MyTest {

    SqlSessionFactory sqlSessionFactory =  null;

    @Before
    public void init(){
        String resource = "mybatis-config.xml";
        InputStream inputStream = null;
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
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203709567.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080620371320.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****该集合中还存在null**

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
//将每一个属性值都映射成对象中的数据，如果有实体类对象，就写成对象.属性的方式
    <resultMap结果映射 id="myEmp" type="com.mashibing.bean.Emp">
        <id column="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        <result column="deptno" property="dept.deptno"></result>
        <result column="dname" property="dept.dname"></result>
        <result column="loc" property="dept.loc"></result>
    </resultMap>

    <select id="selectEmpByEmpno" resultMap结果映射="myEmp">
================================== 换成 =======================================
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
===============================================================================
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通过dept.XXX方式将主键拿出来

![img](https://img-blog.csdnimg.cn/20200806203744617.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 6、获取集合元素

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
//将每一个属性值都映射成对象中的数据，如果有实体类对象，就写成对象.属性的方式
<!--    <resultMap id="myEmp" type="com.mashibing.bean.Emp">-->
<!--        <id column="empno" property="empno"></id>-->
<!--        <result column="ename" property="ename"></result>-->
<!--        <result column="job" property="job"></result>-->
<!--        <result column="mgr" property="mgr"></result>-->
<!--        <result column="hiredate" property="hiredate"></result>-->
<!--        <result column="sal" property="sal"></result>-->
<!--        <result column="comm" property="comm"></result>-->
<!--        <result column="deptno" property="dept.deptno"></result>-->
<!--        <result column="dname" property="dept.dname"></result>-->
<!--        <result column="loc" property="dept.loc"></result>-->
<!--    </resultMap>-->
================================== 添加 =======================================
//推荐使用第二种方式
    <resultMap结果映射 id="myEmp" type="com.mashibing.bean.Emp">
        <id column="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        <association关联 property="dept" javaType="com.mashibing.bean.Dept">
            <id property属性="deptno" column列="deptno"></id>
            <result property="dname" column="dname"></result>
            <result property="loc" column="loc"></result>
        </association>
    </resultMap>
===============================================================================
    <select id="selectEmpByEmpno" resultMap结果映射="myEmp">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203808973.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```bash
//将上面的 * 改为empno，ename，job，dname
   <select id="selectEmpByEmpno" resultMap结果映射="myEmp">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203832284.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//取不到**

**//****改回\***

![img](https://img-blog.csdnimg.cn/20200806203847632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DeptDao.****java**

```java
public interface DeptDao {

    public Dept selectDeptByDeptno(Integer deptno);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DeptDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace命名空间="com.mashibing.dao.DeptDao">
    <resultMap结果映射 id="myDept" type="com.mashibing.bean.Dept">
        <id column="deptno" property="deptno"></id>
        <result property="dname" column="dname"></result>
        <result property="loc" column="loc"></result>
        <collection收集 property="emps" ofType类型筛选="com.mashibing.bean.Emp">
            <id column列="empno" property属性="empno"></id>
            <result column="ename" property="ename"></result>
            <result column="job" property="job"></result>
            <result column="mgr" property="mgr"></result>
            <result column="hiredate" property="hiredate"></result>
            <result column="sal" property="sal"></result>
            <result column="comm" property="comm"></result>
        </collection>
    </resultMap>
   <select id="selectDeptByDeptno" resultMap结果映射="myDept">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
   </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test02() throws IOException {

//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        DeptDao mapper映射 = sqlSession.getMapper(DeptDao.class);
        Dept dept = mapper.selectDeptByDeptno(10);
        System.out.println(dept);
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203941732.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 7、分步查询

​    在上述逻辑的查询中，是由我们自己来完成sql语句的关联查询的，那么，我们能让mybatis帮我们实现自动的关联查询吗?

**DeptDao.****java**

```java
public interface DeptDao {

    public Dept selectDeptByDeptno(Integer deptno);
================================== 添加 =======================================
    public Dept selectDeptByStep(Integer deptno);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DeptDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace命名空间="com.mashibing.dao.DeptDao">
    <resultMap结果映射 id="myDept" type="com.mashibing.bean.Dept">
        <id column="deptno" property="deptno"></id>
        <result property="dname" column="dname"></result>
        <result property="loc" column="loc"></result>
        <collection收集 property="emps" ofType类型筛选="com.mashibing.bean.Emp">
            <id column列="empno" property属性="empno"></id>
            <result column="ename" property="ename"></result>
            <result column="job" property="job"></result>
            <result column="mgr" property="mgr"></result>
            <result column="hiredate" property="hiredate"></result>
            <result column="sal" property="sal"></result>
            <result column="comm" property="comm"></result>
        </collection>
    </resultMap>
   <select id="selectDeptByDeptno" resultMap结果映射="myDept">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
   </select>
================================== 添加 =======================================
<select id="selectDeptByStep" resultType结果类型="com.mashibing.bean.Dept">
        select * from dept where deptno = #{deptno}
    </select>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
================================== 添加 =======================================
public Emp selectEmpByStep(Integer empno);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
//将每一个属性值都映射成对象中的数据，如果有实体类对象，就写成对象.属性的方式
<!--    <resultMap id="myEmp" type="com.mashibing.bean.Emp">-->
<!--        <id column="empno" property="empno"></id>-->
<!--        <result column="ename" property="ename"></result>-->
<!--        <result column="job" property="job"></result>-->
<!--        <result column="mgr" property="mgr"></result>-->
<!--        <result column="hiredate" property="hiredate"></result>-->
<!--        <result column="sal" property="sal"></result>-->
<!--        <result column="comm" property="comm"></result>-->
<!--        <result column="deptno" property="dept.deptno"></result>-->
<!--        <result column="dname" property="dept.dname"></result>-->
<!--        <result column="loc" property="dept.loc"></result>-->
<!--    </resultMap>-->

//推荐使用第二种方式
    <resultMap结果映射 id="myEmp" type="com.mashibing.bean.Emp">
        <id column="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        <association关联 property="dept" javaType ="com.mashibing.bean.Dept">
            <id property属性="deptno" column列="deptno"></id>
            <result property="dname" column="dname"></result>
            <result property="loc" column="loc"></result>
        </association>
    </resultMap>
<select id="selectEmpByEmpno" resultMap结果映射="myEmp">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
    </select>
================================== 添加 =======================================
    <select id="selectEmpByStep" resultMap结果映射="empDept">
        select * from emp where empno = #{empno}
    </select>

<resultMap结果映射 id="empDept" type="com.mashibing.bean.Emp">
        //将上面的复制下来
        <id column列="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        //dept是对象，不是属性（鼠标点进去转到另一个表）
        <association关联 property="dept" select="com.mashibing.dao.DeptDao.selectDeptByStep" column列="deptno"></association>
    </resultMap>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test03() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByStep(7369);
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093322614.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093326735.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 8、延迟查询（延迟加载方式）

​    当我们在进行表关联的时候，有可能在查询结果的时候不需要关联对象的属性值，那么此时可以通过延迟加载来实现功能。在全局配置文件中添加如下属性

**DeptDao.****java**

```java
public interface DeptDao {

    public Dept selectDeptByDeptno(Integer deptno);
    public Dept selectDeptByStep(Integer deptno);
================================== 添加 =======================================
    public Dept selectDeptByStemp2(Integer deptno);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DeptDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace命名空间="com.mashibing.dao.DeptDao">
    <resultMap结果映射 id="myDept" type="com.mashibing.bean.Dept">
        <id column="deptno" property="deptno"></id>
        <result property="dname" column="dname"></result>
        <result property="loc" column="loc"></result>
        <collection收集 property="emps" ofType类型筛选="com.mashibing.bean.Emp">
            <id column列="empno" property属性="empno"></id>
            <result column="ename" property="ename"></result>
            <result column="job" property="job"></result>
            <result column="mgr" property="mgr"></result>
            <result column="hiredate" property="hiredate"></result>
            <result column="sal" property="sal"></result>
            <result column="comm" property="comm"></result>
        </collection>
    </resultMap>
   <select id="selectDeptByDeptno" resultMap结果映射="myDept">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
   </select>
<select id="selectDeptByStep" resultType结果类型="com.mashibing.bean.Dept">
        select * from dept where deptno = #{deptno}
    </select>
================================== 添加 =======================================
    <select id="selectDeptByStemp2" resultMap="deptEmp">
        select * from dept where deptno = #{deptno}
    </select>

<resultMap id="deptEmp" type="com.mashibing.bean.Dept">
        //下面三行，是从上面直接复制的
        <id column="deptno" property="deptno"></id>
        <result property="dname" column="dname"></result>
        <result property="loc" column="loc"></result>
        <collection收藏 property="emps" ofType类型筛选="com.mashibing.bean.Emp" select="com.mashibing.dao.EmpDao.selectEmpByStep2" column列="deptno">
        </collection>
    </resultMap>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
public Emp selectEmpByStep(Integer empno);
================================== 添加 =======================================
public Emp selectEmpByStep2(Integer deptno);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
//将每一个属性值都映射成对象中的数据，如果有实体类对象，就写成对象.属性的方式
<!--    <resultMap id="myEmp" type="com.mashibing.bean.Emp">-->
<!--        <id column="empno" property="empno"></id>-->
<!--        <result column="ename" property="ename"></result>-->
<!--        <result column="job" property="job"></result>-->
<!--        <result column="mgr" property="mgr"></result>-->
<!--        <result column="hiredate" property="hiredate"></result>-->
<!--        <result column="sal" property="sal"></result>-->
<!--        <result column="comm" property="comm"></result>-->
<!--        <result column="deptno" property="dept.deptno"></result>-->
<!--        <result column="dname" property="dept.dname"></result>-->
<!--        <result column="loc" property="dept.loc"></result>-->
<!--    </resultMap>-->

//推荐使用第二种方式
    <resultMap结果映射 id="myEmp" type="com.mashibing.bean.Emp">
        <id column="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        <association关联 property="dept" javaType ="com.mashibing.bean.Dept">
            <id property属性="deptno" column列="deptno"></id>
            <result property="dname" column="dname"></result>
            <result property="loc" column="loc"></result>
        </association>
    </resultMap>
<select id="selectEmpByEmpno" resultMap结果映射="myEmp">
        select * from emp left join dept on emp.deptno = dept.deptno where empno = #{empno}
    </select>
    <select id="selectEmpByStep" resultMap结果映射="empDept">
        select * from emp where empno = #{empno}
    </select>

<resultMap结果映射 id="empDept" type="com.mashibing.bean.Emp">
        //将上面的复制下来
        <id column列="empno" property="empno"></id>
        <result column="ename" property="ename"></result>
        <result column="job" property="job"></result>
        <result column="mgr" property="mgr"></result>
        <result column="hiredate" property="hiredate"></result>
        <result column="sal" property="sal"></result>
        <result column="comm" property="comm"></result>
        //dept是对象，不是属性（鼠标点进去转到另一个表）
        <association关联 property="dept" select="com.mashibing.dao.DeptDao.selectDeptByStep" column列="deptno"></association>
    </resultMap>
================================== 添加 =======================================
    <select id="selectEmpByStep2" resultType="com.mashibing.bean.Emp">
        select * from dept where deptno = #{deptno}
    </select>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test04() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        DeptDao mapper映射 = sqlSession.getMapper(DeptDao.class);
        Dept dept = mapper.selectDeptByStemp2(10);
//关闭会话
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093502464.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093505781.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093510213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080709351548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**去除无关的东西：****延时查询（延时加载）**

### **方式一：** 

**mybatis-config.****xml**

```XML
//可以影响mybatis运行时的行为，包含N多个属性，需要什么引入什么
<settings>
//开启驼峰标识验证
    <setting name="mapUnderscoreToCamelCase" value="true"/>
================================== 添加 =======================================
//配置懒加载开关
    <setting name="lazyLoadingEnabled" value="true"/>
===============================================================================
</settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093539119.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080709354250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093546995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807093550681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//除了lasy懒惰之外，还有eager急切**

**lasy****懒惰（默认）：**

![img](https://img-blog.csdnimg.cn/20200807093607717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果设置了全局加载，但是希望在某一个sql语句查询的时候不适用延时策略，可以添加如下属性：

**eager****急切：**

![img](https://img-blog.csdnimg.cn/20200807093615897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **方式二：**

**mybatis-config.****xml**

```XML
//可以影响mybatis运行时的行为，包含N多个属性，需要什么引入什么
<settings>
//开启驼峰标识验证
    <setting name="mapUnderscoreToCamelCase" value="true"/>
//配置懒加载开关
    <setting name="lazyLoadingEnabled" value="true"/>
================================== 添加 =======================================
    <setting name="aggressiveLazyLoading" value="true"/>
===============================================================================
</settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**eager****急切：**

![img](https://img-blog.csdnimg.cn/20200807093639993.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**lasy****懒惰：**

![img](https://img-blog.csdnimg.cn/20200807093646911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

aggressiveLazyLoading在3.4.1版本之前默认之前而为true

![img](https://img-blog.csdnimg.cn/20200807093656900.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//如果需要的话，可通过通过该方式应用**