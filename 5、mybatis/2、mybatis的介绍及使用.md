## （2）驼峰标识验证： 

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
================================== 添加 =======================================
        <mapper resource资源="UserDao.xml" />
===============================================================================
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**User.****java****（用户）**

```java
public class User {
    private Integer id;
    private String userName;

    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}

    public String getUserName() {return userName;}
    public void setUserName(String userName) {this.userName = userName;
}
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**UserDao.****java****（用户接口）**

```java
public interface UserDao {

    public User selectUserById(Integer id);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**UserDao.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mashibing.dao.UserDao">
    <select id="selectUserById" resultType="com.mashibing.bean.User">
        select * from user where id = #{id}
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141401390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.java** ![img](https://img-blog.csdnimg.cn/20200805141406207.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  

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
        Emp emp = mapper.selectEmpByEmpno(7369);
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
        emp.setEname("zhangsan");
        Integer save = mapper.save(emp);
        System.out.println(save保存);
        sqlSession.commit();
        sqlSession.close();
    }
    @Test
    public void test03(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Emp emp = new Emp();
        emp.setEmpno(3333);
        emp.setEname("zhangsan");
        emp.setSal(500.0);
        Integer update = mapper.update(emp);
        System.out.println(update更新);
        sqlSession.commit();
        sqlSession.close();
    }
    @Test
    public void test04(){
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
        EmpDao mapper = sqlSession.getMapper(EmpDao.class);
        Integer delete = mapper.delete(3333);
        System.out.println(delete删除);
        sqlSession.commit();
        sqlSession.close();
    }
================================== 添加 =======================================
    @Test
    public void test05() throws IOException {
//获取与数据库相关的会话
        SqlSession sqlSession线程 = sqlSessionFactory.openSession();
//获取对应的映射接口对象
        UserDao mapper = sqlSession.getMapper(UserDao.class);
//执行具体的sql语句
        User user = mapper.selectUserById(1);
        System.out.println(user用户);
//关闭会话
        sqlSession.close();
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080514142645.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**自动映射：**

**mybatis-config.****xml****（mybatis配置）** 

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
//在填写标签的时候一定要注意相关配置的顺序
<configuration配置>
<properties属性 resource资源="db.properties"></properties>
================================== 添加 =======================================
//可以影响mybatis运行时的行为，包含N多个属性，需要什么引入什么
    <settings设置>
//开启驼峰标识验证
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
===============================================================================
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
        <mapper resource资源="UserDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141445600.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## （3）typeAliases类型命名 

### 方式一：

在项目开发过程中，会包含开发环境，测试环境，生产环境，有可能会使用不同的数据源进行连接操作
在此配置文件中可以指定多个环境
default默认值:表示选择哪个环境作为运行时环境

![img](https://img-blog.csdnimg.cn/20200805141454433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**mybatis-config.****xml****（mybatis配置）**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
//在填写标签的时候一定要注意相关配置的顺序
<configuration配置>

    当需要引入外部的配置文件的时候，可以使用这样的方式：
    类似于<context:property属性-placeholder占位符 location地点>
resource资源:表示从当前项目的类路径中进行加载，如果用的是idea指的是
resource资源目录下的配置文件
    url:可以从当前文件系统的磁盘目录查找配置，也可以从网络上的资源进行引入
<properties属性 resource资源="db.properties"></properties>
    //可以影响mybatis运行时的行为，包含N多个属性，需要什么引入什么
    <settings>
        //开启驼峰标识验证
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
================================== 添加 =======================================
    typeAliases类型命名:表示在引入实体类的名称时候，可以使用别名，而不需要写完全限定名
    <typeAliases类型命名>
        <typeAlias type="com.mashibing.bean.Emp" alias="Emp"></typeAlias>
    </typeAliases>
===============================================================================
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

    <mappers映射>
        <mapper resource资源="EmpDao.xml" />
        <mapper resource资源="UserDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141520267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**右击****test01()****，点运行：**

![img](https://img-blog.csdnimg.cn/20200805141528322.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****跟上面一样**

### 方式二： 

![img](https://img-blog.csdnimg.cn/20200805141535283.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141538709.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080514154341.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****一样**

### 方式三： 

```XML
    typeAliases类型命名:表示在引入实体类的名称时候，可以使用别名，而不需要写完全限定名
<typeAliases类型命名>
        <typeAlias type="com.mashibing.bean.Emp" alias="Emp"></typeAlias>
================================== 添加 =======================================
        //当每一个具体的类都需要单独来写，如果有100个类呢？使用下面方式：
        //可以指定具体的包来保证实体类不需要写完全限定名
        <package name="com.mashibing.bean"/>
===============================================================================
    </typeAliases>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141610519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080514161769.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//一样（这里没写一百个类，但是测试是能通过）**

## **（4）typeHandlers类型处理器：** 

**mybatis-config.****xml****（mybatis配置）**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
//在填写标签的时候一定要注意相关配置的顺序
<configuration配置>

    当需要引入外部的配置文件的时候，可以使用这样的方式：
    类似于<context:property属性-placeholder占位符 location地点>
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
================================== 添加 =======================================
    //设置定义自己的类型处理器，mybatis中默认内置了很多类型处理器，一般不需要自己来实现
    <typeHandlers类型处理器>
        <typeHandler handler处理器="" ></typeHandler>
        <package name=""/>
    </typeHandlers>
===============================================================================
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

    <mappers映射>
        <mapper resource资源="EmpDao.xml" />
        <mapper resource资源="UserDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## （5）objectFactory对象工厂： 

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
    <typeHandlers类型处理器>
        <typeHandler handler处理器="" ></typeHandler>
        <package name=""/>
    </typeHandlers>
================================== 添加 =======================================
//当需要自定义对象工厂的时候实现此标签，完成结果集到java对象实例化的过程
    <objectFactory对象工厂 type=""></objectFactory>
===============================================================================
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

    <mappers映射>
        <mapper resource资源="EmpDao.xml" />
        <mapper resource资源="UserDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **（6）分页插件：** 

https://pagehelper.github.io/**（内部文档有操作步骤）**

## （7）databaseIdProvider数据库厂商标识： 

（根据不同的数据库来执行不同的sql语句）

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
================================== 添加 =======================================
    <databaseIdProvider数据库厂商标识 type="DB_VENDOR">
        <property name="MySQL" value="mysql"/>
        <property name="SQL Server" value="sqlserver"/>
        <property name="Oracle" value="oracle"/>
    </databaseIdProvider>
===============================================================================
    <mappers映射>
        <mapper resource资源="EmpDao.xml" />
        <mapper resource资源="UserDao.xml" />
    </mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080514174511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805141750215.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//也可用**

提供了不同的数据库厂商的标识，当有数据库移植的需求的时候，可以根据不同的数据库来执行不同的sql语句
用来扩展数据库的移植性

## （8）mapper映射： 

### ①直接输入类的完全限定名： 

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

### ②

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

### **方式一：**如果是maven的项目的话，需要添加如下配置，因为maven默认只会编译java文件，需要把xml文件也添加到指定目录中

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

### **方式二：**在resource（资源）目录下，创建跟dao层一样的同级目录即可，将配置文件放到指定的目录

![img](https://img-blog.csdnimg.cn/20200805142046537.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805142049362.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142054904.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200805142057174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142101456.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200805142120628.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### ③直接定义包的名称方式映射：

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

