# 3、Mybatis-plus的相关配置

​    在mybatis中我们可以在mybatis-config配置文件中可以添加<settings设置>标签，设置全局的默认策略，在MP中也具备相同的功能，只不过配置方式有所不同，我们可以在spring.xml文件中添加配置。

https://mp.baomidou.com/config/

![img](https://img-blog.csdnimg.cn/20200810183512357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在此链接中包含了非常多的配置项，用户可以按照自己的需求添加需要的配置，配置方式如下

**部分配置演示：**

## （1）configuration配置时间：

​    通过这个配置文件的配置，大家可以进行回想上述问题的出现，mybatis-plus是如何解决这个问题的呢？

​    在mybatis-plus中会引入写默认的配置，这个选项的默认配置为true，因此可以完成对应的实现。

我们可以通过如下配置来禁用驼峰标识的操作，如下所示：

**mybatis-config.****xml**

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration配置>
===============================================================================
    <settings设置>
        <setting name="logImpl" value="log4j"/>
    </settings>
===============================================================================
    <plugins插件>
        <plugin interceptor拦截器="
com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor"></plugin>
    </plugins>
===============================================================================
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**spring.****xml**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"        
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/tx
https://www.springframework.org/schema/tx/spring-tx.xsd">

//引入外部配置文件
    <context:property-placeholder location地点="classpath类路径:db.properties"></context:property-placeholder>
===============================================================================
//配置数据源
    <bean id="dataSource数据源" class="com.alibaba.druid.pool.DruidDataSource德鲁伊数据源">
        <property name="driverClassName" value="${driverClassName}"></property>
        <property name="url" value="${url}"></property>
        <property name="username" value="${username}"></property>
        <property name="password" value="${password}"></property>
    </bean>
===============================================================================
//添加事务管理器
    <bean id="transactionManager事务管理器" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource数据源" ref="dataSource数据源"></property>
    </bean>
===============================================================================
//添加事务注解配置
    <tx:annotation-driven transaction-manager="transactionManager事务管理器"/>
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​        ![img](https://img-blog.csdnimg.cn/20200810183611627.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //如果没设置，则*.xml会爆红

```XML
     </bean>

//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包" value="com.mashibing.dao"></property>
    </bean>
=================================== 添加 ======================================   
    <bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
    </bean>
===============================================================================
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.seteName("zhangsan");
        emp.setJob("Teacher");
        emp.setMgr(100);
        emp.setSal(1000.0);
        emp.setComm(500.0);
        emp.setHiredate(new Date());
        emp.setDeptno(10);
        int insert = empDao.insert(emp);
        System.out.println();
        System.out.println(insert);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202008101836469.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //成功

**spring.****xml**

```XML
·
·
·
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>
=================================== 添加 ======================================
        <property name="configuration配置时间" ref="configuration配置时间"></property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
    <bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
    </bean>
===============================================================================
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//****报错（不能同时存在）**



​    表示这两个标签无法同时使用，因此我们可以选择将configLocation配置位置给去掉，就是不使用mybatis的配置，此时就能够正常使用了，但是放置属性的时候又报错了，原因就在于我们把驼峰标识给禁用了，重新开启即可。除此之外，我们还可以在属性的上面添加@TableField属性

![img](https://img-blog.csdnimg.cn/20200810183724268.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**spring.****xml**

![img](https://img-blog.csdnimg.cn/20200810183731410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810183736190.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //报错（未知的eName在字段列表中），因为上面写了false？

configLocation、configuration都代表的是mybatis-config.xml文件中相关的配置不能同时存在：
configLocation配置位置:指定具体的配置文件的位置
configuration配置时间：将mybatis-config.xml文件中的属性可以拿出来进行直接的配置  

## （2）globalConfig全局配置：

### ①修改图像：

**spring.****xml**

```XML
·
·
·
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        //<property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>

        <property name="configuration配置时间" ref="configuration配置时间"></property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
    <bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
</bean>
=================================== 添加 ======================================
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

//可以修改图像![img](https://img-blog.csdnimg.cn/20200810183814819.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

```XML
        <property name="banner广告" value="true"></property> 
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810183832171.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //一样

**spring.****xml**

```XML
·
·
·
底部：
============================== true改false ===================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        //可以修改图像
        <property name="banner广告" value="false"></property> 
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810183858853.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  //一样

**spring.****xml**

```XML
·
·
·
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        //<property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>
        <property name="configuration配置时间" ref="configuration配置时间"></property>
=================================== 添加 ======================================
        <property name="globalConfig全局配置" ref="globalConfig全局配置"></property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
    <bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
    </bean>
============================== false改true ===================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        <property name="banner广告" value="true"></property> 
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810183921496.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //初始化成功

![img](https://img-blog.csdnimg.cn/20200810183931252.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //此前该表设置了自增主键，直接执行即可

### ②dbConfig日志配置：

**spring.****xml**

```XML
·
·
·
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        //<property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>
        <property name="configuration配置时间" ref="configuration配置时间"></property>
        <property name="globalConfig全局配置" ref="globalConfig全局配置"></property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
    <bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
    </bean>
===============================================================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        <property name="banner广告" value="true"></property> 
================================== 添加 =======================================
        <property name="dbConfig日志配置" ref="dbConfig日志配置"></property>
    </bean>
================================== 添加 =======================================
<bean id="dbConfig日志配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig（全局配置$日志配置）">
        <property name="tablePrefix表前缀" value="tbl_"></property>
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202008101840044.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810184007394.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //初始化成功

![img](https://img-blog.csdnimg.cn/20200810184017580.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //此前该表设置了自增主键，直接执行即可

**spring.****xml**

```XML
·
·
·
===============================================================================
//整合spring和mybatis
    <bean id="sqlSessionFactoryBean线程工厂豆" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        //<property name="configLocation配置位置" value="classpath类路径=:mybatis-config.xml"></property>
        //<property name="mapperLocations映射位置" value="classpath:com/mashibing/dao/*.xml"></property>
        <property name="configuration配置时间" ref="configuration配置时间"></property>
        <property name="globalConfig全局配置" ref="globalConfig全局配置"></property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
<bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>
================================== 添加日志 ===================================
        <property name="logImpl" value="org.apache.ibatis.logging.log4j.Log4jImpl"></property>
    </bean>
===============================================================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        <property name="banner广告" value="true"></property> 
        <property name="dbConfig" ref="dbConfig"></property>
    </bean>
===============================================================================  
<bean id="dbConfig日志配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig（全局配置$日志配置）">
        <property name="tablePrefix表前缀" value="tbl_"></property>
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810184046420.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
@Test
    public void test02(){
        EmpDao empDao = context.getBean("empDao", EmpDao.class);
        Emp emp = new Emp();
        emp.seteName("zhangsan");
        emp.setJob("Teacher");
        emp.setMgr(100);
        emp.setSal(1000.0);
        emp.setComm(500.0);
        emp.setHiredate(new Date());
        emp.setDeptno(10);
        int insert = empDao.insert(emp);
        System.out.println();
        System.out.println(insert);
================================== 添加日志 ===================================
        System.out.println(emp.getEmpno());
===============================================================================  
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810184107332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //创建一个员工，并显示其id值（第18行）

## （3）过程介绍：

### 1、当添加上述配置之后，大家发现运行过程中报错，

​              Property 'configuration' and 'configLocation' can not specified with together

​    表示这两个标签无法同时使用，因此我们可以选择将configLocation配置位置给禁用掉，就是不使用mybatis的配置，此时就能够正常使用了，但是放置属性的时候又报错了，原因就在于我们把驼峰标识给禁用了，重新开启即可。除此之外，我们还可以在属性的上面添加@TableField属性

```
    @TableField(value = "e_name")
    private String eName;
```

### 2、此时发现日志功能又无法使用了，只需要添加如下配置即可

```
<bean id="configuration" class="com.baomidou.mybatisplus.core.MybatisConfiguration">

   <property name="mapUnderscoreToCamelCase" value="true"></property>

   <property name="logImpl" value="org.apache.ibatis.logging.log4j.Log4jImpl"></property>

</bean>
```

### 3、我们在刚刚插入数据的时候发现每个类可能都需要写主键生成策略，这是比较麻烦的，因此可以选择将主键配置策略设置到全局配置中。

```
<bean id="dbConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">

       <property name="idType" ref="idType"></property>

   </bean>

   <util:constant id="idType" static-field="com.baomidou.mybatisplus.annotation.IdType.AUTO"></util:constant>
```

### 4、如果你的表的名字都具备相同的前缀，那么可以设置默认的前缀配置策略，此时的话可以将实体类上的@TableName表名标签省略不写

```
<bean id="dbConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">

       <property name="idType" ref="idType"></property>

       <property name="tablePrefix" value="tbl_"></property>

   </bean>

   <util:constant id="idType" static-field="com.baomidou.mybatisplus.annotation.IdType.AUTO"></util:constant>
```

### 5、在mybatis-plus中如果需要获取插入的数据的主键的值，那么直接获取即可，原因就在于配置文件中指定了默认的属性为true



## （4）条件构造器Wrapper（看官网即可）

https://mp.baomidou.com/guide/wrapper.html

**很多：**

![img](https://img-blog.csdnimg.cn/20200810184238800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**写法跟前面的****CRUD****差不多**

## （5）代码生成器

[https://mp.baomidou.com/guide/generator.html](https://mp.baomidou.com/config/generator-config.html)

![img](https://img-blog.csdnimg.cn/20200810184332648.gif)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

​    其实在学习mybatis的时候我们就使用过逆向工程，根据我们的数据表来生成的对应的实体类，DAO接口和Mapper映射文件，而MyBatis-plus提供了更加完善的功能，下面来针对两种方式做一个基本的对比

​    1、MyBatis-plus是根据java代码开生成代码的，而Mybatis是根据XML文件的配置来生成的

​    2、MyBatis-plus能够生成实体类、Mapper接口、Mapper映射文件，Service层，Controller层，而Mybatis只能生成实体类，Mapper接口，Mapper映射文件

### （1）添加依赖

 ![img](https://img-blog.csdnimg.cn/20200810184359465.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)//导入依赖

```XML
    <dependencies>
=================================== 添加 ======================================
        <!-- https://mvnrepository.com/artifact/com.baomidou/mybatis-plus-generator -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.3.1</version>
        </dependency>
===============================================================================
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus</artifactId>
            <version>3.3.1</version>
        </dependency>
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
            <version>8.0.19</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.3.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.2.3.RELEASE</version>
        </dependency>
====================== 默认的模板引擎（也有自定义的） =========================
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.2</version>
        </dependency>        
================================ spring Web MVC ========================================
         
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.3.RELEASE</version>
        </dependency>
    </dependencies>
===============================================================================
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
===============================================================================
</project>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

模板引擎依赖在MyBatis-Plus支持有：Velocity（默认）、Freemarker、Beetl，用户可以选择自己熟悉的模板引擎，如果都不满足您的要求，可以采用自定义模板引擎。

### （2）编写生成类

**TestGenerator.java**

```java
public class TestGenerator {
    public static void main(String[] args) {
//代码生成器
        AutoGenerator autoGenerator自动生产 = new AutoGenerator();

//全局配置，也要配上前面的全局配置
        GlobalConfig globalConfig全局配置 = new GlobalConfig();
        globalConfig.setAuthor("lian")//设置作者
        .setOutputDir("D:\\IdeaProjects\\mybatis_plus_generator\\src\\main\\java")                                 //设置输出路径
        .setFileOverride(true)      //设置文件覆盖
        .setIdType(IdType.AUTO)     //设置主键生成策略
        .setServiceName("%sService")//service服务接口的名称
        .setBaseResultMap(true)     //基本结果集合
        .setBaseColumnList(true)    //设置基本的列，也可设成所有
        .setControllerName("%sController");//Controller服务接口的名称
//配置数据源
        DataSourceConfig dataSourceConfig数据源配置 = new DataSourceConfig();
        dataSourceConfig.setDriverName("com.mysql.cj.jdbc.Driver")
.setUrl("jdbc:mysql://192.168.85.111:3306/mp?serverTimezone=UTC")
                        .setUsername("root").setPassword("123456");
//策略配置
        StrategyConfig strategyConfig策略配置 = new StrategyConfig();
        strategyConfig.setInclude()                   //设置要包含的表
        .setTablePrefix("tbl_")                       //设置表名的前缀
        .setNaming(NamingStrategy.underline_to_camel);//映射实体类的时候命名的策略
//包名配置
        PackageConfig packageConfig包名配置 = new PackageConfig();
        packageConfig.setParent("com.mashibing")
                     .setMapper("mapper")
                     .setService("service")
                     .setController("controller")
                     .setEntity("bean")
                     .setXml("mapper");

autoGenerator.setGlobalConfig(globalConfig)
             .setDataSource(dataSourceConfig)
             .setStrategy(strategyConfig)
             .setPackageInfo(packageConfig);

        autoGenerator.execute执行();
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810184515799.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //成功

​    注意，当通过上述代码实现之后，大家发现可以在Controller层可以直接实现调用，这些调用的实现最核心的功能就在于ServiceImpl类，这个类中自动完成mapper的注入，同时提供了一系列CRUD的方法。

![img](https://img-blog.csdnimg.cn/20200810184526609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```XML
================================== 导入依赖 ===================================
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
===============================================================================
</project>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810184643509.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//刷新**