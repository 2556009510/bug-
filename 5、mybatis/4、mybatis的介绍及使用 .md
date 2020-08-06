**目录**

[2、select元素](#2、select元素)

[1、参数的获取值的方式：](#2、参数的获取值的方式：)

[#{} 与 ${}](#%23{} 与 %24{})

[${}使用场景：](#%24{}使用场景：)

[2、当查询语句中包含多个参数的时候，我们应该如何获取需要的参数  ](#3、当查询语句中包含多个参数的时候，我们应该如何获取需要的参数  )

[1、如果是单个参数：](#1、如果是单个参数：)

[2、如果是多个参数：](#2、如果是多个参数：)

[3、自定义map结构：](#3、自定义map结构：)

------



# 2、select元素

## 1、参数的获取值的方式：

每次在向sql语句中设置结果值的时候，可以使用#{}，还可以使用${}这样的方式，那么哪种比较好？

![img](https://img-blog.csdnimg.cn/2020080613401645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test01() throws IOException {
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
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### #{} 与 ${}

![img](https://img-blog.csdnimg.cn/202008061341212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134125633.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

=======================================================================

![img](https://img-blog.csdnimg.cn/2020080613413376.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134135386.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通过sql语句可以得出结论：
    \#{}的处理方式是使用了参数预编译的方式，不会引起sql注入的问题
    ${}的处理方式是直接拼接sql语句，得到对应的sql语句，会有sql注入的危险

因此，我们推荐大家使用#{}的方式，



**${}****方式：**

![img](https://img-blog.csdnimg.cn/20200806134151213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200806134157441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134207180.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**#{}****方式：**

![img](https://img-blog.csdnimg.cn/20200806134215857.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### ${}使用场景：

**上面演示完之后改回Integer与7369。**

但是要注意，${}也是有自己的使用场景的？

当需要传入动态的表名、列名的时候就需要使用${},就是最直接的拼接字符串的行为

**EmpDao.****java**

![img](https://img-blog.csdnimg.cn/20200806134227834.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134305151.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//原来是emp**

![img](https://img-blog.csdnimg.cn/20200806134321893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134326329.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//找不到标识**

![img](https://img-blog.csdnimg.cn/20200806134345887.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080613434934.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134352329.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134355901.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

当需要传入动态的表名、列名的时候就需要使用${},就是最直接的拼接字符串的行为：

![img](https://img-blog.csdnimg.cn/2020080613440464.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134407540.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 2、当查询语句中包含多个参数的时候，我们应该如何获取需要的参数   

 **1****、如果是单个参数：**
        基本数据类型：那么可以使用#{}随便获取
        引用数据类型:使用#{}获取值的是必须要使用对象的属性名


 **2****、如果是多个参数：**
        我们在获取参数值的时候，就不能简单的通过#{}来获取了,只能通过arg0、arg1、param1、param2...这样的方式来获取参数的值
        原因在于，mybatis在传入多个参数的时候，会将这些参数的结果封装到map结构中，在map中key值就是(arg0,arg1,...)
        (param1,param2...),这种方式非常不友好，没有办法根据属性名来获取具体的参数值
        如果想要使用参数的话，可以进行如下的设置：
        public List<Emp> selectEmpByEmpnoAndSal2(@Param("empno") Integer empno, @Param("sal") Double sal);
            这样的方式其实是根据@Param来进行参数的获取



### 1、如果是单个参数：

基本数据类型：那么可以使用#{}随便获取

**随便填入：**

![img](https://img-blog.csdnimg.cn/20200806134509500.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134528832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200806134533159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134538655.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

引用数据类型:使用#{}获取值的是**必须**要使用对象的属性名

![img](https://img-blog.csdnimg.cn/20200806134550502.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);

    public List<Emp> selectAll();
============================== 添加 =================================
public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
=====================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test08(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp2 = new Emp();
        emp2.put(7369);
        emp2.put(500.0);
        List<Emp> list = mapper.selectEmpByEmpnoAndSal (emp2);
        for (Emp emp : list) {
            System.out.println(emp);
        }
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
    <select id="selectEmpByEmpnoAndSal" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{empno} and sal >#{sal}
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134640214.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2、如果是多个参数：

​        我们在获取参数值的时候，就不能简单的通过#{}来获取了,只能通过arg0、arg1、param1、param2...这样的方式来获取参数的值
​        原因在于，mybatis在传入多个参数的时候，会讲这些参数的结果封装到map结构中，在map中key值就是(arg0,arg1,...)
​        (param1,param2...),这种方式非常不友好，没有办法根据属性名来获取具体的参数值

**随便填入：**![img](https://img-blog.csdnimg.cn/20200806134651524.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134656534.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);

    public List<Emp> selectAll();

public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
============================== 添加 =================================
    public List<Emp> selectEmpByEmpnoAndSal2(Initeger empno，Double sal);
=====================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
============================== 添加 =================================
    <select id="selectEmpByEmpnoAndSal2" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{empno} and sal >#{sal}
    </select>
=====================================================================
    <select id="selectEmpByEmpnoAndSal" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{aaa} and sal >#{ccc}
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134752790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134756796.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果想要使用参数的话，可以进行如下的设置：
    这样的方式其实是根据@Param参数 来进行参数的获取

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);

    public List<Emp> selectAll();
public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
============================== 修改 =================================
    public List<Emp> selectEmpByEmpnoAndSal2(@Param参数("empno") Integer empno,@Param参数("sal") Double sal);
=====================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134820292.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 3、自定义map结构：

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);

    public List<Emp> selectAll();
    public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
    public List<Emp> selectEmpByEmpnoAndSal2(@Param参数("empno") Integer empno, @Param参数("sal") Double sal);
============================== 添加 =================================
    public List<Emp> selectEmpByEmpnoAndSal3(Map<String,Object> map);
=====================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
============================== 添加 =================================
<select id="selectEmpByEmpnoAndSal3" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{empno} and sal >#{sal}
    </select>
=====================================================================    
    <select id="selectEmpByEmpnoAndSal2" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{empno} and sal >#{sal}
    </select>
    <select id="selectEmpByEmpnoAndSal" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{aaa} and sal >#{ccc}
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test08(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
============================== 添加 =================================
        Map<String,Object> map = new HashMap<>();
        map.put("empno",7369);
        map.put("sal",500.0);
=====================================================================
        List<Emp> list = mapper.selectEmpByEmpnoAndSal3(map);
        for (Emp emp : list) {
            System.out.println(emp);
        }
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806134932411.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3、处理集合返回结果

resultType结果类型:表示返回的结果的类型,一般使用的并不多，此类型只能返回单一的对象，而我们在查询的时候特别是关联查询的时候，需要自定义结果集



**上面是：**当返回的结果是一个集合的时候，并不需要resultMap结果映射，只需要使用resultType结果类型指定集合中的元素类型即可

**这里：**

**EmpDao.****xml**

```XML
//当返回值是map结构的时候，会把查询结构的字段值作为key，结果作为value
<select id="selectEmpByEmpnoReturnMap" resultType="map">
    select * from emp where empno = #{empno}
</select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);
================================== 添加 =======================================
    public Map<Object,Object> selectEmpByEmpnoReturnMap(Integer empno);
===============================================================================

    public List<Emp> selectAll();

    public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
    public List<Emp> selectEmpByEmpnoAndSal2(@Param("empno") Integer empno, @Param("sal") Double sal);
    public List<Emp> selectEmpByEmpnoAndSal3(Map<String,Object> map);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test09(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Map<Object, Object> objectObjectMap = mapper.selectEmpByEmpnoReturnMap(7369);
        System.out.println(objectObjectMap);
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806202724757.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);
    public Map<Object,Object> selectEmpByEmpnoReturnMap(Integer empno);


public List<Emp> selectAll();
================================== 添加 =======================================
    public Map<String,Emp> selectAll2();
===============================================================================
    public List<Emp> selectEmpByEmpnoAndSal(Emp emp);
    public List<Emp> selectEmpByEmpnoAndSal2(@Param("empno") Integer empno, @Param("sal") Double sal);
    public List<Emp> selectEmpByEmpnoAndSal3(Map<String,Object> map);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
//当返回值是map结构的时候，会把查询结构的字段值作为key，结果作为value
    <select id="selectEmpByEmpnoReturnMap" resultType结果类型="map">
        select * from emp where empno = #{empno}
    </select>
================================== 添加 =======================================
    <select id="selectAll2" resultType="Emp">
        select * from emp
    </select>
===============================================================================
    <select id="selectAll" resultType="Emp">
        select * from emp
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test10(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Map<String, Emp> stringEmpMap = mapper.selectAll2();
        System.out.println(stringEmpMap);
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806202809495.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806202812457.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//报错，没结果（原因：修改了一个库的值）**

![img](https://img-blog.csdnimg.cn/20200806202837326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080620284151.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

当需要返回的结果是一个map的集合的时候，同时map中包含**多个对象**，那么此时必须要在dao的方法上添加@MapKey注解，来标识到底是哪一个属性值作为key

## 4、自定义结果集---resultMap结果映射

resultMap结果映射:当进行关联查询的时候，在返回结果的对象中还包含另一个对象的引用时，此时需要自定义结果集合，使用resultMap

如果需要定义一个自定义结果集，那么该如何进行处理resultMap

**Dog.****java**

```java
public class Dog {
    private Integer id;    //id
    private String name;   //名字
    private Integer age;   //年龄
    private String gender; //性别

    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}

    public String getName() {eturn name;}
    public void setName(String name) {this.name = name;}

    public Integer getAge() {return age;}
    public void setAge(Integer age) {this.age = age;}

    public String getGender() {return gender;}
    public void setGender(String gender) {his.gender = gender;}

    @Override
    public String toString() {
        return "Dog{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DogDao.****java**

```java
public interface DogDao {
    public Dog selectDogById(Integer id);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**DogDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace命名空间="com.mashibing.dao.DogDao">
自定义结果集
    id:表示当前结果集的唯一标识
    type:表示当前结果集的类型
    <select id="selectDogById" resultMap结果映射="com.mashibing.bean.myDog">
        select * from dog where id = #{id}
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest2.****java**

```java
public class MyTest2 {

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
        SqlSession sqlSession = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        DogDao mapper = sqlSession.getMapper(DogDao.class);
//执行具体的sql语句
        Dog dog = mapper.selectDogById(1);
        System.out.println(dog);
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203000842.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//null**

**DogDao.****xml**

![img](https://img-blog.csdnimg.cn/20200806203014115.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203017984.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在使用mybatis的时候，有些情况下需要我们封装结果集，一般情况下mybatis会帮我们自动封装（字段名跟属性值必须一一对应），但是如果字段的值和类中的值不匹配的时候，怎么处理？
    1、可以在sql语句中添加别名字段，来保证赋值成功，但是太麻烦了，而且不可重用
    2、resultMap结果映射：

![img](https://img-blog.csdnimg.cn/20200806203027109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203030510.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806203034515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### resultMap结果映射

resultMap结果映射:当进行关联查询的时候，在返回结果的对象中还包含另一个对象的引用时，此时需要自定义结果集合，使用resultMap

- constructor

  构造函数

   \- 

  用于在实例化类时，注入结果到构造方法中

  - idArg - ID 参数；标记出作为 ID 的结果可以帮助提高整体性能
  - arg - 将被注入到构造方法的一个普通结果

- id – 一个 ID 结果；标记出作为 ID 的结果可以帮助提高整体性能

- result结果 – 注入到字段或 JavaBean 属性的普通结果

- association

  关联

   – 

  一个复杂类型的关联；许多结果将包装成这种类型

  - 嵌套结果映射 – 关联可以是 resultMap结果映射 元素，或是对其它结果映射的引用

- collection

  收藏

   – 

  一个复杂类型的集合

  - 嵌套结果映射 – 集合可以是 resultMap结果映射 元素，或是对其它结果映射的引用

- discriminator

  鉴别器

   – 

  使用结果值来决定使用哪个

  resultMap

  结果映射

  - case

     – 

    基于某些值的结果映射

    - 嵌套结果映射 – case 也是一个结果映射，因此具有相同的结构和元素；或者引用其它的结果映射



Dog.java

```java
public class Dog {
   private Integer id;
   private String name;
   private Integer age;
   private String gender;

   public Integer getId() {return id;}
   public void setId(Integer id) {this.id = id;}

   public String getName() {return name;}
   public void setName(String name) {this.name = name;}

   public Integer getAge() {return age;}
   public void setAge(Integer age) {this.age = age;}

   public String getGender() {return gender;}
   public void setGender(String gender) {this.gender = gender;}

   @Override
   public String toString() {
       return "Dog{" +
               "id=" + id +
               ", name='" + name + '\'' +
               ", age=" + age +
               ", gender='" + gender + '\'' +
               '}';
  }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

dog.sql ![img](https://img-blog.csdnimg.cn/2020080620343338.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```sql
SET FOREIGN_KEY_CHECKS=0;
-- ----------------------------
-- Table structure for `dog`
-- ----------------------------
DROP TABLE IF EXISTS `dog`;
CREATE TABLE `dog` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`dname` varchar(255) DEFAULT NULL,
`dage` int(11) DEFAULT NULL,
`dgender` varchar(255) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of dog
-- ----------------------------
INSERT INTO dog VALUES ('1', '大黄', '1', '雄');
INSERT INTO dog VALUES ('2', '二黄', '2', '雌');
INSERT INTO dog VALUES ('3', '三黄', '3', '雄');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

DogDao.java

```java
public interface DogDao {

   public Dog selectDogById(Integer id);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

DogDao.xml

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.DogDao">
  <!--
  在使用mybatis进行查询的时候，mybatis默认会帮我们进行结果的封装，但是要求列名跟属性名称一一对应上
  在实际的使用过程中，我们会发现有时候数据库中的列名跟我们类中的属性名并不是一一对应的，此时就需要起别名
  起别名有两种实现方式：
     1、在编写sql语句的时候添加别名
     2、自定义封装结果集
  -->
  <!--根据查询的数据进行结果的封装要使用resultMap属性，表示使用自定义规则-->
  <select id="selectDogById" resultMap="myDog">
    select * from dog where id = #{id}
  </select>

  <!--自定义结果集，将每一个列的数据跟javaBean的对象属性对应起来
  type:表示为哪一个javaBean对象进行对应
  id:唯一标识，方便其他属性标签进行引用
  -->
  <resultMap id="myDog" type="com.mashibing.bean.Dog">
     <!--
     指定主键列的对应规则：
     column：表示表中的主键列
     property:指定javaBean的属性
     -->
     <id column="id" property="id"></id>
     <!--设置其他列的对应关系-->
     <result column="dname" property="name"></result>
     <result column="dage" property="age"></result>
     <result column="dgender" property="gender"></result>
  </resultMap>
  <!--可以在sql语句中写别名-->
<!-- <select id="selectDogById" resultType="com.mashibing.bean.Dog">
     select id id,dname name,dage age,dgender gender from dog where id = #{id}
  </select>-->

  <!--这种方式是查询不到任何结果的，因为属性名跟列名并不是一一对应的-->
 <!-- <select id="selectDogById" resultType="com.mashibing.bean.Dog">
     select * from dog where id = #{id}
  </select>-->
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

