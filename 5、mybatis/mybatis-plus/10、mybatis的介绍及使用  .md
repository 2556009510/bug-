# 2、简单的CRUD操作

如果我们下面要实现CRUD的基本操作，那么我们该如何实现呢？

​    在Mybatis中，我们需要编写对应的Dao接口，并在接口中定义相关的方法，然后提供与该接口相同名称的Dao.xml文件，在文件中填写对应的sql语句，才能完成对应的操作

​    在Mybatis-plus中，我们只需要定义接口，然后继承BaseMapper<T>泛型类即可，此前做的所有操作都是由Mybatis-plus来帮我们完成，不需要创建sql映射文件

**EmpDao.****java**



```java
public interface EmpDao extends BaseMapper<Emp> { //泛型

}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        List<Emp> list = empDao.selectLinst(null);
        for (Emp emp : list) {
            System.out.println(emp);
        }
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 ![img](https://img-blog.csdnimg.cn/20200809162401749.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//无效绑定语句**

**mybatis-config.****xml**



```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration配置>
=================================== 添加 ======================================
<mappers映射>
        <mapper name="com.mashibing.dao.EmpDao"></mapper>
</mappers>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080916243195.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****无效声明语句**

**spring.xml****顶部添加组件扫描：**![img](https://img-blog.csdnimg.cn/20200809162441885.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162445168.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//****无效声明语句，删去该行**

**mybatis-config.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration配置>
=================================== 去掉 ======================================
<mappers映射>
        <mapper name="com.mashibing.dao.EmpDao"></mapper>
</mappers>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper映射 namespace="com.mashibing.dao.EmpDao">

</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162529518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****成功**

![img](https://img-blog.csdnimg.cn/20200809162536943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**如果报错，重新粘贴过该****.xml****文件，那么需要执行这段：**

![img](https://img-blog.csdnimg.cn/20200809162544468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162547597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/2020080916255441.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

![img](https://img-blog.csdnimg.cn/20200809162602147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162605533.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//表中数据全显示出来**

![img](https://img-blog.csdnimg.cn/20200809162616158.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****主要实现不写这个来调试**

## 1、插入操作

![img](https://img-blog.csdnimg.cn/20200809162631300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162634811.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//mp是什么我也不知**

**MyTest.****java**

```java
public class MyTest {

    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");

//在mybatis-plus中，插入数据的sql语句会伴随你插入的对象的属性值进行更改，比较灵活
    @Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.seteName("zhangsan");
        emp.setJob("Teacher教师");
        emp.setMgr(100);
        emp.setSal(1000.0);
        emp.setComm(500.0);
        emp.setHiredate(new Date());
        emp.setDeptno(10);
        int insert = empDao.insert(emp);
        System.out.println(insert);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**Emp.java** 

![img](https://img-blog.csdnimg.cn/20200809162708462.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162715181.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//空的，没加日志**

**mybatis-config.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration配置>
================================== 添加日志 ===================================
    <settings设置>
        <setting name="logImpl" value="log4j"/>
    </settings>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162736838.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    当运行上述代码的时候，大家发现报错了，原因在于你写的实体类的名称跟表的名称不匹配，因此在实现的是需要添加@TableName注解，指定具体的表的名称

**Emp.****java**



![img](https://img-blog.csdnimg.cn/20200809162746275.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162758240.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****成功**

![img](https://img-blog.csdnimg.cn/20200809162805288.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****之前****tdl_emp****为空，现在已更新了**

![img](https://img-blog.csdnimg.cn/20200809162811401.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****原因是：使用了****mapUnderscoreToCamelCase****（是否开启自动驼峰命名规则）默认开启？**

![img](https://img-blog.csdnimg.cn/20200809162821416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809162825208.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**@TableName（表名）**

| **属性**         | **类型** | **必须指定** | **默认值** | **描述**                                                     |
| ---------------- | -------- | ------------ | ---------- | ------------------------------------------------------------ |
| value            | String   | 否           | ""         | 表名                                                         |
| schema           | String   | 否           | ""         | schema                                                       |
| keepGlobalPrefix | boolean  | 否           | false      | 是否保持使用全局的 tablePrefix 的值(如果设置了全局 tablePrefix 且自行设置了 value 的值) |
| resultMap        | String   | 否           | ""         | xml 中 resultMap 的 id                                       |
| autoResultMap    | boolean  | 否           | false      | 是否自动构建 resultMap 并使用(如果设置 resultMap 则不会进行 resultMap 的自动构建并注入) |

**关于****`autoResultMap`****的说明****:**

mp会自动构建一个`ResultMap`并注入到mybatis里(一般用不上).下面讲两句: 因为mp底层是mybatis,所以一些mybatis的常识你要知道,mp只是帮你注入了常用crud到mybatis里 注入之前可以说是动态的(根据你entity的字段以及注解变化而变化),但是注入之后是静态的(等于你写在xml的东西) 而对于直接指定`typeHandler`,mybatis只支持你写在2个地方:

1. 定义在resultMap里,只作用于select查询的返回结果封装
2. 定义在`insert`和`update`sql的`#{property}`里的`property`后面(例:`#{property,typehandler=xxx.xxx.xxx}`),只作用于`设置值` 而除了这两种直接指定`typeHandler`,mybatis有一个全局的扫描你自己的`typeHandler`包的配置,这是根据你的`property`的类型去找`typeHandler`并使用.

上述代码运行通过之后，大家会发现结果能够正常的进行插入，但是在控制台会打印一个警告信息，说没有@TableId的注解，原因就在于定义实体类的时候并没有声明其中的主键是哪个列，以及使用什么样的主键生成策略，因此，可以在类的属性上添加如下注解，来消除此警告

在mybatis-plus2.x版本的时候，如果设置了表自增，那么id必须制定为Auto类型，否则插入不成功，3.x不存在此问题

![img](https://img-blog.csdnimg.cn/20200809162840110.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

[#](https://mp.baomidou.com/guide/annotation.html#tableid)[@TableId](https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/TableId.java)（表）

- 描述：主键注解

| **属性** | **类型** | **必须指定** | **默认值**  | **描述**   |
| -------- | -------- | ------------ | ----------- | ---------- |
| value    | String   | 否           | ""          | 主键字段名 |
| type     | Enum     | 否           | IdType.NONE | 主键类型   |

[#](https://mp.baomidou.com/guide/annotation.html#idtype)[IdType](https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/IdType.java)（id类型）

| **值**        |      | **描述**                                                     |
| ------------- | ---- | ------------------------------------------------------------ |
| AUTO（自动）  |      | 数据库ID自增                                                 |
| NONE（默认）  |      | 无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT) |
| INPUT（输入） |      | insert前自行set主键值                                        |
| ASSIGN_ID     |      | 分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |
| ASSIGN_UUID   |      | 分配UUID,主键类型为String(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextUUID`(默认default方法) |
| ID_WORKER     |      | 分布式全局唯一ID 长整型类型(please use `ASSIGN_ID`)          |
| UUID          |      | 32位UUID字符串(please use `ASSIGN_UUID`)                     |
| ID_WORKER_STR |      | 分布式全局唯一ID 字符串类型(please use `ASSIGN_ID`)          |

​    但是大家会发现，我们在写属性的时候，实体类属性名称跟表的属性名称并没有一一对应上，那么为什么会完成对应的操作呢？

​    其实原因就在于mybatis-plus的全局配置

在进行数据插入的是，如果我们输入的时候用的是全字段，那么sql语句中就会执行如下sql语句：

**MyTest.****java**

```java
    @Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.seteName("zhangsan");
        emp.setJob("Teacher教师");
        emp.setMgr(100);
        emp.setSal(1000.0);
        emp.setComm(500.0);
        emp.setHiredate(new Date());
        emp.setDeptno(10);
        int insert = empDao.insert(emp);
        System.out.println(insert);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

INSERT INTO tbl_emp ( e_name, job, mgr, hiredate, sal, comm, deptno ) VALUES ( ?, ?, ?, ?, ?, ?, ? )

但是如果我们在插入的时候，将对象中的某些属性值设置为空，那么会是什么效果呢？

**MyTest.****java**

```java
    @Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.seteName("zhangsan");
        emp.setJob("Teacher教师");
        emp.setMgr(100);
        //emp.setSal(1000.0);
        //emp.setComm(500.0);
        //emp.setHiredate(new Date());
        //emp.setDeptno(10);
        int insert = empDao.insert(emp);
        System.out.println(insert);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

INSERT INTO tbl_emp ( e_name, job, mgr ) VALUES ( ?, ?, ? )

大家其实可以看到我们在插入的时候，mybatis-plus会根据我会输入的对象的字段的个数来动态的调整我们的sql语句插入的字段，这是就是mybatis-plus比较灵活的地方。

## 2、更新操作

**MyTest.****java**

```java
@Test
    public void test03(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(6); //表中第六行员工
        emp.seteName("lisi");
        emp.setJob("Teacher教师");
        emp.setMgr(100);
        int insert = empDao.updateById(emp); //更新
        System.out.println();
        System.out.println(insert);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3、删除操作

**MyTest.****java**

```java
//delete（删除）操作在使用的时候要使用queryWrapper查询包装
    @Test
    public void test04(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
//根据id删除数据
        //int i = empDao.deleteById(6); //表中第六行员工
        //System.out.println(i);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164122553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//根据id集合批量删除
        //int i = empDao.deleteBatchIds(Arrays.asList(1, 2, 3));
        //System.out.println(i);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164137539.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//根据map类型的数据进行删除，但是要注意，key为列的名称.value是具体的值
        //Map<String,Object> map = new HashMap<String,Object>();
        //map.put("empno",4); //empno列，4行
        //int i = empDao.deleteByMap(map);
        //System.out.println(i);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164149668.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//全部删除？
        //int delete = empDao.delete(null);
        //System.out.println(delete);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080916420139.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
================================ 推荐使用 =====================================
//delete（删除）操作在使用的时候要使用queryWrapper查询包装
        QueryWrapper wrapper包装 = new QueryWrapper();
        wrapper.eq("empno",7); //empno列，7行
        int delete = empDao.delete(wrapper);
        System.out.println(delete);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164238538.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
===============================================================================
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 4、查询操作

![img](https://img-blog.csdnimg.cn/20200809164253838.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test05(){
        EmpDao empdao = context.getBean(EmpDao.class);
//查询单条语句，需要添加对应的查询条件，封装在QueryWrapper查询包装中
//使用selectOne的时候有且仅能返回一条语句，如果是多条结果的话，会报错

        //QueryWrapper wrapper包装 = new QueryWrapper();
        //表里有几条数据，就该写几行（这里写两个）
        //wrapper.eq("empno","8");
        //wrapper.eq("e_name","zhangsan");
        //Emp emp = empdao.selectOne(wrapper);
        //System.out.println(emp);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164312233.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200809164314320.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//查询某一个结果集的数据（表中有几个返回几个）
        //List<Emp> list = empdao.selectList(null);
        //System.out.println(list); 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164328442.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//根据id的集合返回数据
        //List<Emp> list = empdao.selectBatchIds(Arrays.asList(8, 9));
        //System.out.println(list);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164340621.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//根据id进行查询
        //Emp emp = empdao.selectById(8);

//查询结果集合封装成一个list里面的对象是maps
        //@MapKey对应的结果是 Map<Object,emp>

        //List<Map<String, Object>> maps = empdao.selectMaps(null);
        //System.out.println();
        //System.out.println(maps);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//结果两个一样** ![img](https://img-blog.csdnimg.cn/20200809164401292.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//返回满足查询条件的所有行总数
        //Integer integer = empdao.selectCount(null);
        //System.out.println(integer);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164413743.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

===============================================================================

![img](https://img-blog.csdnimg.cn/20200809164425842.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
        //null是不设置条件
        //Page<Emp> empPage = empdao.selectPage(new Page<Emp>(2, 2),null);
        //System.out.println("-------------------");
        //System.out.println(empPage.getRecords记录());
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

//执行了分页，但是没用

![img](https://img-blog.csdnimg.cn/20200809164442950.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
//在使用分页的时候，必须要添加一个插件
        /**
         *   <plugins>
         * <plugin interceptor拦截器=
"com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor分页拦截器"></plugin>
         *     </plugins>
         */
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164501931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164504221.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java**

```java
@Mapper映射
public interface EmpDao extends BaseMapper<Emp> {
=================================== 添加 ======================================
    public List<Emp> selectEmpByCondition();
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164529274.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper映射 namespace="com.mashibing.dao.EmpDao">
   <select id="selectEmpByCondition" resultType="com.mashibing.bean.Emp">
       select * from tbl_emp
   </select> 
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
        //empdao.selectMapsPage() //查询并翻页
        Emp emp = empdao.selectEmpByCondition();
        System.out.println(emp);
//也可以：
        List<Emp> emp = empdao.selectEmpByCondition();
        System.out.println(emp);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200809164647146.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)