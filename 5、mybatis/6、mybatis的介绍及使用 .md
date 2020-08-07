# 3、动态sql

​    动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

​    使用动态 SQL 并非一件易事，但借助可用于任何 SQL 映射语句中的强大的动态 SQL 语言，MyBatis 显著地提升了这一特性的易用性。

​    如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

- **if**
- **choose** (when, otherwise)
- **trim** (where, set)
- **foreach**

## 1、if 

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
public Emp selectEmpByStep(Integer empno);
public Emp selectEmpByStep2(Integer deptno);
================================== 添加 =======================================
public Emp selectEmpByCondition(Emp emp);
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

    <select id="selectEmpByStep2" resultType="com.mashibing.bean.Emp">
        select * from dept where deptno = #{deptno}
    </select>
================================== 添加 =======================================
//if动态sql
    <select id="selectEmpByCondition" resultType="com.mashibing.bean.Emp">
        select * from emp where
            <if test="empno!=null">
                empno = #{empno} and
            </if>
            <if test="ename!=null">
                ename=#{ename}
            </if>
    </select>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test05() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper映射 = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(1000);
        emp.setEname("SMITH"); //职业
        Emp emp2 = mapper.selectEmpByCondition(emp);
        System.out.println(emp2);
//关闭会话
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175029688.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080717504166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080717504586.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

看起来测试是比较正常的，但是大家需要注意的是如果我们传入的参数值有缺失会怎么呢？这个时候拼接的sql语句就会变得有问题，例如不传参数或者丢失最后一个参数，那么语句中就会多一个where或者and的关键字，因此在mybatis中也给出了具体的解决方案：

![img](https://img-blog.csdnimg.cn/20200807175053554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175058520.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175103126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175107641.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//****和上面打开的结果一样，只是EmpDao.****xml****内部的内容变了**

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

**EmpDao.****xml**

![img](https://img-blog.csdnimg.cn/20200807175120405.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175123841.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175128274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175133107.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

![img](https://img-blog.csdnimg.cn/20200807175139455.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175144149.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175149217.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

可以了解一下HTML转义字符

![img](https://img-blog.csdnimg.cn/2020080717515793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    现在看起来没有什么问题了，但是我们的条件添加到了拼接sql语句的前后，那么我们该如何处理呢？

```XML
   <select id="getEmpByCondition" resultType="com.mashibing.bean.Emp">
      select * from emp
       <trim prefix="where" prefixOverrides="and" suffixOverrides="and">
           <if test="empno!=null">
              empno > #{empno} and
           </if>
           <if test="ename!=null">
              ename like #{ename} and
           </if>
           <if test="sal!=null">
              sal > #{sal} and
           </if>
       </trim>
   </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 2、choose（选择）

​    有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 选择，它有点像 Java 中的 switch（开关） 语句。

![img](https://img-blog.csdnimg.cn/20200807175327792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175333133.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175335868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175340169.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175342793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175347453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175351996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175355480.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3、foreach（相当于for）

​    动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。

​    trim:截取字符串，可以自定义where的格式
​    prefix：为sql语句整体添加一个前缀
​    prefixOverrides:去除整体sql语句前面多余的字符串
​    suffixOverriede:去除整体sql语句后面多余的字符串

![img](https://img-blog.csdnimg.cn/20200807175406459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175410302.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
    <select id="selectEmpByCondition" resultType="com.mashibing.bean.Emp">
        select * from emp
        <trim prefix前缀="where" prefixOverrides前缀重写="and">
================================== 修改 =======================================
                <if test="empno!=null">
                    empno = #{empno} 
                </if>
                <if test="ename!=null">
                    ename=#{ename} 
                </if>
===============================================================================
        </trim>
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175438949.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080717544433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175448607.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****报错**

![img](https://img-blog.csdnimg.cn/20200807175501437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175506195.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175510691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175516278.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****与上面一样**

**EmpDao.****java**

```java
public interface EmpDao {

    public Emp selectEmpByEmpno(Integer empno);
public Emp selectEmpByStep(Integer empno);
    public Emp selectEmpByStep2(Integer deptno);
public Emp selectEmpByCondition(Emp emp);
================================== 添加 =======================================
    public List<Emp> selectEmpByDeptnos(@Param("deptnos") List<Integer> deptnos);
===============================================================================
}
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
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    foreach循环是对集合进行遍历

​    collection收藏="deptnos"  指定要遍历的集合

​    close关闭="" 表示以什么结束

​    index索引="" 给定一个索引值

item逐条列出=""  遍历的每一个元素的值 ![img](https://img-blog.csdnimg.cn/20200807175605526.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 

​    open打开=""  表示以什么开始

​    separator分离器="" 表示多个元素的分隔符

**MyTest.****java**

```java
    @Test
    public void test06() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        List<Emp> list = mapper.selectEmpByDeptnos(Arrays.asList(10, 20));
        for (Emp emp : list) {
            System.out.println(emp);
        }
//关闭会话
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200807175633251.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **4、script（脚本）**

要在带注解的映射器接口类中使用动态 SQL，可以使用 *script* 元素。比如:

```XML
    @Update({"<script>",
      "update Author",
      "  <set>",
      "    <if test='username != null'>username=#{username},</if>",
      "    <if test='password != null'>password=#{password},</if>",
      "    <if test='email != null'>email=#{email},</if>",
      "    <if test='bio != null'>bio=#{bio}</if>",
      "  </set>",
      "where id=#{id}",
      "</script>"})
    void updateAuthorValues(Author author);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **5、bind（绑定）**

bind 元素允许你在 OGNL 表达式以外创建一个变量，并将其绑定到当前的上下文。比如：

```XML
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}
</select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **6、多数据库支持**

如果配置了 databaseIdProvider，你就可以在动态代码中使用名为 “_databaseId” 的变量来为不同的数据库构建特定的语句。比如下面的例子：

```XML
<insert id="insert">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    <if test="_databaseId == 'oracle'">
      select seq_users.nextval from dual
    </if>
    <if test="_databaseId == 'db2'">
      select nextval for seq_users from sysibm.sysdummy1"
    </if>
  </selectKey>
  insert into users values (#{id}, #{name})
</insert>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **7、动态 SQL 中的插入脚本语言**

MyBatis 从 3.2 版本开始支持插入脚本语言，这允许你插入一种语言驱动，并基于这种语言来编写动态 SQL 查询语句。

可以通过实现以下接口来插入一种语言：

```java
public interface LanguageDriver {
  ParameterHandler createParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql);
  SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType);
  SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

实现自定义语言驱动后，你就可以在 mybatis-config.xml 文件中将它设置为默认语言：

```XML
<typeAliases>
  <typeAlias type="org.sample.MyLanguageDriver" alias="myLanguage"/>
</typeAliases>
<settings>
  <setting name="defaultScriptingLanguage" value="myLanguage"/>
</settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

或者，你也可以使用 lang 属性为特定的语句指定语言：

```XML
<select id="selectBlog" lang="myLanguage">
  SELECT * FROM BLOG
</select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

或者，在你的 mapper 接口上添加 @Lang 注解：

```java
public interface Mapper {
  @Lang(MyLanguageDriver.class)
  @Select("SELECT * FROM BLOG")
  List<Blog> selectBlog();
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**提示** ：可以使用 Apache Velocity 作为动态语言，更多细节请参考 MyBatis-Velocity 项目。

你前面看到的所有 xml 标签都由默认 MyBatis 语言提供，而它由语言驱动 org.apache.ibatis.scripting.xmltags.XmlLanguageDriver（别名为 xml）所提供。

## **8、set**

​    用于动态更新语句的类似解决方案叫做 *set*。

​    *set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列。

```XML
<update id="updateEmpByEmpno">
  update emp
   <set>
       <if test="empno!=null">
          empno=#{empno},
       </if>
       <if test="ename!=null">
          ename = #{ename},
       </if>
       <if test="sal!=null">
          sal = #{sal}
       </if>
   </set>
   <where>
      empno = #{empno}
   </where>
</update>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

