**目录**

[（8）配置映射的三种方式：](#（8）mapper映射：)

[①直接输入类的完全限定名：](#①直接输入类的完全限定名：)

[②](#②)

[    方式一：如果是maven的项目的话，需要添加如下配置，因为maven默认只会编译java文件，需要把xml文件也添加到指定目录中](#    方式一：如果是maven的项目的话，需要添加如下配置，因为maven默认只会编译java文件，需要把xml文件也添加到指定目录中)

[    方式二：在resource（资源）目录下，创建跟dao层一样的同级目录即可，将配置文件放到指定的目录](#    方式二：在resource（资源）目录下，创建跟dao层一样的同级目录即可，将配置文件放到指定的目录)

[③直接定义包的名称方式映射：](#    ③直接定义包的名称方式映射：)

[（9）Mybatis SQL映射文件详解：](#（9）Mybatis SQL映射文件详解：)

[1、insert插入、update更新、delete删除](#1、insert插入、update更新、delete删除)

[useGeneratedKeys使用生成的主键：](#useGeneratedKeys使用生成的主键：)

[keyProperty主键属性：](#keyProperty主键属性：)

[id:](#id%3A)

[resultType结果类型:](#resultType结果类型%3A)

[resultMap返回值映射:](#resultMap返回值映射%3A)

------



------

# （8）配置映射的三种方式：

## ①直接输入类的完全限定名：

**UserDaoAnnotation.****java****（用户注解接口）**

```java
public interface UserDaoAnnotation {

    @Select("select * from user where id = #{id}")
    public User selectUserById(Integer id);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**mybatis-config.****xml****（mybatis配置）**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
//在填写标签的时候一定要注意相关配置的顺序
<configuration配置>

    当需要引入外部的配置文件的时候，可以使用这样的方式：
    类似于<context:property-placeholder location>
resource资源:表示从当前项目的类路径中进行加载，如果用的是idea指的是
resource资源目录下的配置文件
    url:可以从当前文件系统的磁盘目录查找配置，也可以从网络上的资源进行引入
<properties属性 resource资源="db.properties"></properties>
    //可以影响mybatis运行时的行为，包含N多个属性，需要什么引入什么
    <settings>
        //开启驼峰标识验证
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    typeAliases类型命名:表示在引入实体类的名称时候，可以使用别名，而不需要写完全限定名
<typeAliases类型命名>
        <typeAlias type="com.mashibing.bean.Emp" alias="Emp"></typeAlias>
        //当每一个具体的类都需要单独来写，如果有100个类呢？使用下面方式：
        //可以指定具体的包来保证实体类不需要写完全限定名
        <package name="com.mashibing.bean"/>
</typeAliases>
    //设置定义自己的类型处理器，mybatis中默认内置了很多类型处理器，一般不需要自己来实现
    <typeHandlers>
        <typeHandler handler="" ></typeHandler>
        <package name=""/>
    </typeHandlers>
//当需要自定义对象工厂的时候实现此标签，完成结果集到java对象实例化的过程
    <objectFactory type=""></objectFactory>
    <environments环境s default默认值="development开发">
        配置具体的环境属性：
        id:表示当前环境的名称
        <environment环境 id="development开发">
            事务管理器，每一种数据源都需要配置具体的事务管理器：JDBC、MANAGED 
            JDBC:使用jdbc原生的事务控制
            MANAGED管理:什么都没做
            <transactionManager事务管理器 type="JDBC"/> 
            配置具体的数据源的类型：pooled、unpooled 、jndi（不用）
            pooled合并:使用数据库连接池
            unpooled：每次都打开和关闭一个链接
            <dataSource数据源 type="POOLED">
                //链接数据的时候需要添加的必备的参数，一般是四个，如果是连接池
                的话，可以设置连接最大个数等
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
//提供了不同的数据库厂商的标识，当有数据库移植的需求的时候，可以根据不同的数据库来执行不同的sql语句
用来扩展数据库的移植性
    <databaseIdProvider数据库厂商标识 type="DB_VENDOR">
        <property name="MySQL" value="mysql"/>
        <property name="SQL Server" value="sqlserver"/>
        <property name="Oracle" value="oracle"/>
    </databaseIdProvider>
============================== 添加 =================================
//是来将mapper映射文件引入到配置文件中，方便程序启动的时候进行加载
//每次在进行填写的时候需要注意，写完xml映射之后一定要添加到mybatis-config文件中
resource资源:从项目的类路径下加载对应的映射文件
url:从本地磁盘目录或者网络中引入映射文件
class:可以直接引入类的完全限定名，可以使用注解（接口类）的方式进行使用
    <mappers>
        //<mapper resource="EmpDao.xml" />
        //<mapper resource="UserDao.xml"/>
添加：  <mapper class="com.mashibing.dao.UserDaoAnnotation"></mapper>
    </mappers>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141842364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test05()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141849708.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## ②

**mybatis-config.****xml****（mybatis配置）**

```XML
//是来将mapper映射文件引入到配置文件中，方便程序启动的时候进行加载
//每次在进行填写的时候需要注意，写完xml映射之后一定要添加到mybatis-config文件中

resource:从项目的类路径下加载对应的映射文件
url:从本地磁盘目录或者网络中引入映射文件
class:可以直接引入类的完全限定名，可以使用注解（接口类）的方式进行使用
============================== 添加 =================================
//如果不想以注解（接口类）的方式引入呢？
//如果想要class的方式引入配置文件，可以将xml文件添加到具体的类的同级目录下
 
    <mappers>
        //<mapper resource="EmpDao.xml" />
        //<mapper resource="UserDao.xml"/>
        //<mapper class="com.mashibing.dao.UserDaoAnnotation"></mapper>
添加：  <mapper class="com.mashibing.dao.UserDao"></mapper>
    </mappers>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141912779.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141919475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test05()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141935491.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//报错**

###     **方式一：**如果是maven的项目的话，需要添加如下配置，因为maven默认只会编译java文件，需要把xml文件也添加到指定目录中

**加入依赖：**

```XML
<builds建立>
    <resources资源s>
        <resource资源>
            <directory目录>src/main/java</directory>
            <includes包含s>
                <include包含>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142000337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//自动配置成功**

![img](https://img-blog.csdnimg.cn/20200805142008300.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

###     **方式二：**在resource（资源）目录下，创建跟dao层一样的同级目录即可，将配置文件放到指定的目录

![img](https://img-blog.csdnimg.cn/20200805142046537.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805142049362.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142054904.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805142057174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142101456.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## ③直接定义包的名称方式映射：

**mybatis-config.****xml****（mybatis配置）**

```XML
//是来将mapper映射文件引入到配置文件中，方便程序启动的时候进行加载
//每次在进行填写的时候需要注意，写完xml映射之后一定要添加到mybatis-config文件中
resource:从项目的类路径下加载对应的映射文件
url:从本地磁盘目录或者网络中引入映射文件
class:可以直接引入类的完全限定名，可以使用注解（接口类）的方式进行使用

//如果不想以注解（接口类）的方式引入呢？
//如果想要class的方式引入配置文件，可以将xml文件添加到具体的类的同级目录下

    <mappers>
        //<mapper resource="EmpDao.xml" />
        //<mapper resource="UserDao.xml"/>
        //<mapper class="com.mashibing.dao.UserDaoAnnotation"></mapper>
        //<mapper class="com.mashibing.dao.UserDao"></mapper>
============================== 添加 =================================
        //如果需要引入多个配置文件，可以直接定义包的名称
        resource目录下配置的映射文件必须要具体相同的目录
        <package name="com.mashibing.dao"/>
===============================================================================
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142146258.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# （9）Mybatis SQL映射文件详解：

| **属性**                           | **描述**                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| `id`                               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType``输入类型`          | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过TypeHandler类型处理器推断出具体传入语句的参数，默认值为unset未设置。 |
| `parameterMap``输入映射`           | 用于引用外部 parameterMap输入映射 的属性，目前已被废弃。请使用行内参数映射和 parameterType输入类型 属性。 |
| `flushCache``冲洗缓存`             | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：true（对 insert、update 和 delete 语句）。 |
| `timeout`超时                      | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为unset未设置（依赖数据库驱动）。 |
| `statementType``声明类型`          | 可选 STATEMENT声明，PREPARED准备 或 CALLABLE可调用。这会让 MyBatis 分别使用 Statement陈述、PreparedStatement准备声明或 CallableStatement可调用语句，默认值：PREPARED准备。 |
| `useGeneratedKeys``使用生成的主键` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys()获取生成的主键 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty``主键属性`            | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys()获取生成的键 的返回值或 insert 语句的 selectKey选择键 子元素设置它的值，默认值：unset未设置。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`键列                    | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`数据库标识符           | 如果配置了databaseIdProvider数据库厂商标识，MyBatis 会加载所有不带 databaseId数据库标识符 或匹配当前databaseId数据库标识符 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

​    在之前我们学习了mybatis的全局配置文件，下面我们开始学习mybatis的映射文件，在映射文件中，可以编写以下的顶级元素标签：

```html
cache缓存 – 该命名空间的缓存配置。

cache-ref缓存参数 – 引用其它命名空间的缓存配置。

resultMap结果映射 – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。

parameterMap参数映射 – 老式风格的参数映射。此元素已被废弃，并可能在将来被移除！请使用行内参数映射。文档中不会介绍此元素。

sql – 可被其它语句引用的可重用语句块。

insert – 映射插入语句。

update – 映射更新语句。

delete – 映射删除语句。

select – 映射查询语句。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



在每个顶级元素标签中可以添加很多个属性，下面我们开始详细了解下具体的配置。

id:用来标识跟dao接口中匹配的方法，必须与方法的名字一一对应上
flushCache冲洗缓存：用来标识当前sql语句的结果是否进入二级缓存
statementType声明类型：用来选择执行sql语句的方式
    （1）statement声明：最基本的jdbc的操作，用来表示一个sql语句，不能防止sql注入
    （2）PREPARED准备**：**preareStatement准备声明：采用预编译的方式，能够防止sql注入，设置参数的时候需要该对象来进行设置
    （3）CALLABLE可调用：调用存储过程


useGeneratedKeys使用生成的主键：完成插入操作的时候，可以将自增生成的主键值返回到具体的对象
keyProperty主键属性：指定返回的主键要赋值到哪个属性中

## 1、insert插入、update更新、delete删除

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.EmpDao">
    <select id="selectEmpByEmpno" resultType="Emp" databaseId="mysql">
        select * from emp where empno = #{empno}
    </select>
//insert delete update分别用来标识不同类型的sql语句
<insert id="save" >
        insert into emp(empno,ename) values(#{empno},#{ename})
    </insert>
    <update id="update">
        update emp set sal=#{sal} where empno = #{empno}
    </update>
    <delete id="delete">
        delete from emp where empno = #{empno}
    </delete>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133406387.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test02(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");
        Integer save = mapper.save(emp);
        System.out.println(save保存);
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133442504.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**UserDao****.****java**

```java
public interface UserDao {

    public User selectUserById(Integer id);
=================================== 添加 ======================================
    public Integer saveUser(User user);
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**UserDao****.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.UserDao">
    <select id="selectUserById" resultType="com.mashibing.bean.User">
        select * from user where id = #{id}
    </select>
=================================== 添加 ======================================
    <insert id="saveUser">
        insert into user(user_name) values(#{userName})
    </insert>
===============================================================================
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test06(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        User user = new User();
        user.setUserName("lisi");
        Integer save = mapper.saveUser(user用户);
        System.out.println(save);
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133543816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200806133549449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133554772.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133558126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//报错**

![img](https://img-blog.csdnimg.cn/20200806133607385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080613361278.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133628834.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133640376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133644346.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //id是null

### useGeneratedKeys使用生成的主键：

完成插入操作的时候，可以将**自增**生成的主键值返回到具体的对象

###  

### keyProperty主键属性：

指定返回的主键要赋值到哪个属性中

![img](https://img-blog.csdnimg.cn/20200806133702884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202008061337050.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在编写sql语句的过程中，无论你是否配置了驼峰标识的识别settting点取设置，都需要在sql语句中写具体的表的属性名，不能写对象名称

```XML
=======================================================================
<!--如果数据库支持自增可以使用这样的方式-->
   <insert id="insertUser" useGeneratedKeys使用生成的主键="true" keyProperty主键属性="id">
      insert into user(user_name) values(#{userName})
   </insert>
=======================================================================
   <!--如果数据库不支持自增的话，那么可以使用如下的方式进行赋值查询-->
   <insert id="insertUser2" >
       <selectKey order命令="BEFORE之前" keyProperty主键属性="id" resultType结果类型="integer整数">
          select max(id)+1 from user
       </selectKey>
      insert into user(id,user_name) values(#{id},#{userName})
   </insert>
=======================================================================
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

| **属性**                           | **描述**                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| `id`                               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType``输入类型`          | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过TypeHandler类型处理器推断出具体传入语句的参数，默认值为unset未设置。 |
| `parameterMap``输入映射`           | 用于引用外部 parameterMap输入映射 的属性，目前已被废弃。请使用行内参数映射和 parameterType输入类型 属性。 |
| `flushCache``冲洗缓存`             | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：true（对 insert、update 和 delete 语句）。 |
| `timeout`超时                      | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为unset未设置（依赖数据库驱动）。 |
| `statementType``声明类型`          | 可选 STATEMENT声明，PREPARED准备 或 CALLABLE可调用。这会让 MyBatis 分别使用 Statement陈述、PreparedStatement准备声明或 CallableStatement可调用语句，默认值：PREPARED准备。 |
| `useGeneratedKeys``使用生成的主键` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys()获取生成的主键 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty``主键属性`            | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys()获取生成的键 的返回值或 insert 语句的 selectKey选择键 子元素设置它的值，默认值：unset未设置。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`键列                    | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`数据库标识符           | 如果配置了databaseIdProvider数据库厂商标识，MyBatis 会加载所有不带 databaseId数据库标识符 或匹配当前databaseId数据库标识符 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

```html
cache缓存 – 该命名空间的缓存配置。

cache-ref缓存参数 – 引用其它命名空间的缓存配置。

resultMap结果映射 – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。

parameterMap参数映射 – 老式风格的参数映射。此元素已被废弃，并可能在将来被移除！请使用行内参数映射。文档中不会介绍此元素。

sql – 可被其它语句引用的可重用语句块。

insert – 映射插入语句。

update – 映射更新语句。

delete – 映射删除语句。

select – 映射查询语句。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



### id:

用来设置当前sql语句匹配的dao接口的方法，必须要跟方法的名字统一

###  resultType结果类型:

表示返回的结果的类型,一般使用的并不多，此类型只能返回单一的对象，而我们在查询的时候特别是关联查询的时候，需要自定义结果集



当返回的结果是一个集合的时候，并不需要resultMap返回值映射，只需要使用resultType结果类型 指定集合中的元素类型即可

**EmpDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace命名空间="com.mashibing.dao.EmpDao">
============================== 添加 =================================
    <select id="selectAll" resultType结果类型="Emp">
        select * from emp
    </select>
=====================================================================
    <select id="selectEmpByEmpno" resultType结果类型="com.mashibing.bean.Emp" databaseId="mysql">
        select * from emp where empno = #{empno}
    </select>
//insert delete update分别用来标识不同类型的sql语句
<insert id="save">
        insert into emp(empno,ename) values(#{empno},#{ename})
    </insert>
    <update id="update">
        update emp set sal=#{sal} where empno = #{empno}
    </update>
    <delete id="delete">
        delete from emp where empno = #{empno}
    </delete>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java****接口**

```java
public interface EmpDao {

    public Integer save(Emp emp);
    public Integer update(Emp emp);
    public Integer delete(Integer empno);
    public Emp selectEmpByEmpno(Integer empno);
============================== 添加 =================================
    public List<Emp> selectAll();
=====================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test07(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        List<Emp> list = mapper.selectAll();
        for (Emp emp : list) {
            System.out.println(emp);
        }
        sqlSession.commit();
        sqlSession.close();
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200806133951554.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### resultMap返回值映射:

当进行关联查询的时候，在返回结果的对象中还包含另一个对象的引用时，此时需要自定义结果集合，才使用resultMap 