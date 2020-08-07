**目录**

[4、缓存](#4、缓存)

[1、一级缓存：](#1、一级缓存：)

[在某些情况下，一级缓存可能会失效？](#在某些情况下，一级缓存可能会失效？)

[    1、](#    1、)

[    2、](#    2、)

[    3、](#    3、)

[](#    4、)

[2、二级缓存：](#2、二级缓存：)

[（1）缓存的使用](#（1）缓存的使用)

[（2）缓存的属性](#（2）缓存的属性)

[（3）二级缓存的作用范围：](#（3）二级缓存的作用范围：)

[3、整合第三方缓存](#3、整合第三方缓存)

[（1）桌面自动创建缓存文件](#（1）桌面自动创建缓存文件)

[（2）自动创建文件、自动设置好代码（能够运行基本增删改查操作，但是除了没有toSping）](#（2）自动创建文件、自动设置好代码（能够运行基本增删改查操作，但是除了没有toSping）)

[（3）自动生成文件（@Generated系列）：](#（3）自动生成文件（%40Generated系列）：)

------

# 4、缓存

​    MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。 为了使它更加强大而且易于配置，我们对 MyBatis 3 中的缓存实现进行了许多改进。

​    默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存。 要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

当添加上该标签之后，会有如下效果：

1. 映射语句文件中的所有 select 语句的结果将会被缓存。
2. 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
3. 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
4. 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
5. 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
6. 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

在进行配置的时候还会分为一级缓存和二级缓存：

一级缓存：线程级别的缓存，是本地缓存，sqlSession级别的缓存

二级缓存：全局范围的缓存，不止局限于当前会话

mybatis中的缓存机制：
    如果没有缓存，那么每次查询的时候都需要从数据库中加载数据，这回造成io的性能问题，所以，在很多情况下
    如果连续执行两条相同的sql语句，可以直接从缓存中获取，如果获取不到，那么再去查询数据库，这意味着查询完成的结果需要放到缓存中。



缓存分类：
    1、一级缓存：表示sqlSession线程级别的缓存，每次查询的时候会开启一个会话，此会话相当于一次连接，关闭之后自动失效
    2、二级缓存：全局范围内的缓存，sqlSession线程关闭之后才会生效
    3、第三方缓存：继承第三方的组件，来充当缓存的作用

## 1、一级缓存：

一级缓存：表示将数据存储在sqlsession线程中，关闭之后自动失效，

默认情况下是开启的
    在同一个会话之内，如果执行了多个相同的sql语句，那么除了第一个之外，所有的数据都是从缓存中进行查询的

**MyTest.****java**

```java
    @Test
    public void test07() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180024209.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test07() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);

================================== 添加 =======================================
        Emp emp2 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp2);
===============================================================================
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180038882.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 在某些情况下，一级缓存可能会失效？

###     1、

在同一个方法中，可能会开启多个会话，那么会造成缓存失效。此时需要注意，会话跟方法没有关系，不是一个方法就只能由一个会话，所以严格记住,缓存的数据是保存在sqlsession线程中的

**MyTest.****java**

```java
@Test
    public void test07() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);

================================== 添加 =======================================
        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        EmpDao mapper2 = sqlSession2.getMapper(EmpDao.class);
===============================================================================
        Emp emp2 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp2);
        sqlSession.close();
        sqlSession2.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180056955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

###     2、

当传递对象的时候，如果对象中的属性值不一致，也不会走缓存

**MyTest.****java**

```java
    Emp emp = new EmpDao();
@Test
    public void test08() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        SqlSession sqlSession2 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);

        emp.setEmpno (7369);
        Emp emp2 = mapper.selectEmpCondition(emp);
        System.out.println(emp2);

        emp.setEmpno (7340);
        Emp emp3 = mapper.selectEmpCondition(emp);
        System.out.println(emp3);

        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180119596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

###     3、

在多次查询过程中，如果修改了数据，那么缓存也会失效

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
    public Emp selectEmpByStep(Integer empno);
    public Emp selectEmpByStep2(Integer deptno);
    public Emp selectEmpByCondition(Emp emp);
    public List<Emp> selectEmpByDeptnos(@Param("deptnos") List<Integer> deptnos);
================================== 添加 =======================================
    public Integer update(Emp emp);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<update id="update更新">
    update emp set ename=#{ename} where empno = #{empno}
</update>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test08() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        System.out.println("==================");

        emp.setEname("Teacher教师");
        Integer update更新 = mapper.update(emp);
        System.out.println(update);
        System.out.println("==================");
        emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180221527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### ![img](https://img-blog.csdnimg.cn/20200807232242438.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807232234672.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080723222716.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//****输出共三个**

**第一查询，第二更新，第三没查询，而是直接输出缓存的数据**

   4、如果在一个会话过程中，过程中手动清空了缓存，那么缓存也会失效

![img](https://img-blog.csdnimg.cn/20200807232208506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080723220272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test08() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        SqlSession sqlSession2 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        EmpDao mapper2 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        System.out.println("==================");

        emp.setEname("Teacher教师");
        //Integer update更新 = mapper2.update(emp);
        //System.out.println(update);
        //sqlSession.clearCache();
        System.out.println("==================");
        emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180306203.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test08() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        SqlSession sqlSession2 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        EmpDao mapper2 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        System.out.println("==================");
================================== 添加 =======================================
        try {
            Thread.sleep睡眠(10000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
===============================================================================
        //emp.setEname("Teacher教师");
        //Integer update更新 = mapper2.update(emp);
        //System.out.println(update);
        //sqlSession.clearCache();
        System.out.println("==================");
        emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807180329342.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****下面没显示，但是也是缓存失效**

## 2、二级缓存：

### （1）缓存的使用

二级缓存：表示的是全局缓存，必须要等到sqlsession线程关闭之后才会生效

默认是不开启的，如果需要开启的话，需要进行如下设置
    1、修改全局配置文件，在settings设置中添加配置
        <setting name="cacheEnabled" value="true"/>
    2、指定在哪个映射文件中使用缓存的配置
        <cache></cache>
    3、对应的java实体类必须要实现序列化的接口

默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存。 要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

**mybatis-config.****xml**

```XML
<settings>
        <!--开启驼峰标识验证-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--配置懒加载开关-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
================================== 添加 =======================================
        <setting name="cacheEnabled" value="true"/>
===============================================================================
    </settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
    foreach:遍历集合中的元素
    collection:指定要遍历的集合
    separator:分隔符
    open:以什么开始
    close：以什么结束
    item:遍历过程中的每一个元素值
    index:表示索引
    <select id="selectEmpByDeptnos" resultType="com.mashibing.bean.Emp">
        select * from emp where deptno in
        <foreach collection收藏="deptnos" separator分离器="," open打开="(" item逐条列出="deptno" index索引="idx" close关闭=")">
            #{deptno}
        </foreach>
    </select>

<update id="update">
    update emp set ename=#{ename} where empno = #{empno}
</update>
================================== 添加 =======================================
    <cache缓存></cache>
===============================================================================
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test09(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        EmpDao mapper2 = sqlSession2.getMapper(EmpDao.class);

        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close(); //一级缓存关闭
        System.out.println("====================");
        Emp emp1 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp1);
        sqlSession2.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807225646459.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//****报错**

​    3、对应的java实体类必须要实现序列化的接口

**Emp.****java**

![img](https://img-blog.csdnimg.cn/20200807225739107.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**Dept.****java**

![img](https://img-blog.csdnimg.cn/20200807225745186.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807225747426.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**缓存数越多，缓存命中率越高**

### （2）缓存的属性

eviction回收:表示缓存回收策略，

默认是LRU

​      LRU最近最少使用：移除最长时间不被使用的对象

​      FIFO先进先出：按照对象进入缓存的顺序来移除

​      SOFT软引用：移除基于垃圾回收器状态和软引用规则的对象

​      WEAK弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象



flushInternal（冲洗内部），刷新间隔:，单位毫秒

默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新

size大小：引用数目，正整数，代表缓存最多可以存储多少个对象，太大容易导致内存溢出

readonly只读：true/false

​    true：只读缓存，会给所有的调用的方法返回该对象的实例，因此这些对象不能被修改，不安全

​    false：读写缓存，会返回缓存对象的拷贝（序列化实现），比较安全，默认



一级缓存跟二级缓存有没有可能同时存在数据？
    不会同时存在，因为二级缓存生效的时候，是在sqlsession线程关闭的时候当查询数据的时候，我们是先查询一级缓存还是先查询二级缓存？
    先查询二级缓存，然后再查询一级缓存

**先一级后二级：**

**MyTest.****java**

```java
@Test
    public void test10(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();

        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        EmpDao mapper2 = sqlSession2.getMapper(EmpDao.class);
        Emp emp2 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp2);
        sqlSession2.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807225912367.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test10(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();

        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        EmpDao mapper2 = sqlSession2.getMapper(EmpDao.class);
        Emp emp2 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp2);
================================== 添加 ======================================
        Emp emp3 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp3);
===============================================================================
        sqlSession2.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807225948774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test10(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = mapper.selectEmpByEmpno(7369);
        System.out.println(emp);
        sqlSession.close();

        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        EmpDao mapper2 = sqlSession2.getMapper(EmpDao.class);
        Emp emp2 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp2);
        Emp emp3 = mapper2.selectEmpByEmpno(7369);
        System.out.println(emp3);

================================== 添加 =======================================
        Emp emp4 = mapper2.selectEmpByEmpno(7499);
        System.out.println(emp4);
        Emp emp5 = mapper2.selectEmpByEmpno(7499);
        System.out.println(emp5);
===============================================================================
        sqlSession2.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230058415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230100650.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****共5个，倒数一二个缓存命中率开始减小**

​    所以先查询二级缓存（全局），再查询一级缓存

![img](https://img-blog.csdnimg.cn/20200807230113926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### （3）二级缓存的作用范围：

​    如果设置了全局的二级缓存配置，那么在使用的时候需要注意，在每一个单独的select语句中，可以设置将查询缓存关闭，以完成特殊的设置

​    1、在setting中设置，是配置二级缓存开启，一级缓存默认一直开启

​        <setting name="cacheEnabled" value="true"/>

​    2、select标签的useCache属性：

​        在每一个select的查询中可以设置当前查询是否要使用二级缓存，只对二级缓存有效

​    3、sql标签的flushCache属性

​        增删改操作默认值为true，sql执行之后会清空一级缓存和二级缓存，

​        查询操作默认是false

​    4、sqlSession.clearCache清除缓存()

​        用来清除一级缓存



## 3、整合第三方缓存

​    在某些情况下我们也可以自定义实现缓存，或为其他第三方缓存方案创建适配器，来完全覆盖缓存行为。

### （1）桌面自动创建缓存文件 

**导入依赖：**

```XML
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache二级缓存</artifactId>
    <version>1.2.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.0-alpha1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>2.0.0-alpha1</version>
    <scope>test</scope>
</dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**ehcache.****xml**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">
//磁盘保存路径 
    <diskStore磁盘缓存 path="D:\ehcache" /> //随便写
    <defaultCache默认缓存
            //属性
            maxElementsInMemory="1"
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="true"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

属性说明：

diskStore：指定数据在磁盘中的存储位置。

defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略



以下属性是必须的：

1. maxElementsInMemory - 在内存中缓存的element的最大数目 
2. maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
3. eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
4. overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上



1. 以下属性是可选的：
2. timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大
3. timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大
4. diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区.
5. diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。
6. diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
7. memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）

**EmpDao.****xml**

```XML
<cache缓存 type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230501410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230503384.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****与上面结果一样**

![img](https://img-blog.csdnimg.cn/2020080723051081.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****存放的缓存数据**

### （2）自动创建文件、自动设置好代码（能够运行基本增删改查操作，但是除了没有toSping） 

![img](https://img-blog.csdnimg.cn/20200807230524558.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**mybatis-config.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties属性 resource资源="db.properties" ></properties>

    <environments环境s default默认="development">
        <environment环境 id="development">
            <transactionManager事务管理 type="JDBC"/>
            <dataSource数据源 type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**导入依赖：**![img](https://img-blog.csdnimg.cn/20200807230546434.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```XML
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator生产者-core核心</artifactId>
    <version>1.4.0</version>
</dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230607608.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**mbg.xml**

```XML
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
//总配置标签
<generatorConfiguration>
//具体配置的上下文环境
    <context id="simple" targetRuntime="MyBatis3DynamicSql">
//指向我们需要连接的数据库
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
connectionURL="jdbc:mysql://192.168.85.111:3306/demo?serverTimezone=UTC"
userId="root" 
password="123456"/>

        //生成对应的实体类
        targetPackage：指定存放的包
        targetProject：指定当前工程的目录
        <javaModelGenerator targetPackage="com.mashibing.bean" targetProject="src/main/java"/>

//生成对应的SQL映射文件
        <sqlMapGenerator targetPackage="com.mashibing.dao" targetProject="src/main/resources"/>

//生成对应的DAO接口
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.mashibing.dao" targetProject="src/main/java"/>

//指定需要反向生成的表
        <table tableName="emp" />
        <table tableName="dept" />
    </context>
</generatorConfiguration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.java**

```java
public class MyTest {
    public static void main(String[] args) throws Exception {

        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        File configFile = new File("mbg.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate生产(null);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230651462.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230659173.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200807230659181.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200807230659376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  

//自动创建文件、自动设置好代码（能够运行基本增删改查操作，但是除了没0有tosping）

**MyTest.****java**

```java
public class MyTest {
    SqlSessionFactory sqlSessionFactory = null;

    @Before之前
    public void init() {
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
        EmpMapper mapper映射 = sqlSession.getMapper(EmpMapper.class);
        Emp emp = mapper.selectByPrimaryKey(7369);
        System.out.println(emp);
//关闭会话
        sqlSession.close();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**mybatis-config.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties属性 resource资源="db.properties" ></properties>

    <environments环境s default默认="development">
        <environment环境 id="development">
            <transactionManager事务管理 type="JDBC"/>
            <dataSource数据源 type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
================================== 添加 =======================================
<mappers>
        <package name="com.mashibing.dao"/>
    </mappers>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230752132.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**Emp.java 添加：**![img](https://img-blog.csdnimg.cn/20200807230758387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080723080677.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**删除：**![img](https://img-blog.csdnimg.cn/20200807230809562.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200807230809461.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200807230809567.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### （3）自动生成文件（@Generated系列）： 

![img](https://img-blog.csdnimg.cn/20200807230829630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230840433.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //关掉里面的代码

![img](https://img-blog.csdnimg.cn/20200807230847344.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//用这个测试**

![img](https://img-blog.csdnimg.cn/20200807230857972.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807230901398.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**Dept.java** ![img](https://img-blog.csdnimg.cn/20200807230907437.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200807230909680.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

