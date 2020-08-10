# 6、插件

https://mp.baomidou.com/guide/auto-fill-metainfo.html

![img](https://img-blog.csdnimg.cn/2020081022361729.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

MyBatis 允许你在映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

## 1、分页插件

在spring.xml文件中添加如下配置引入插件

```XML
<property name="plugins">
    <array数组>
        <bean class="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor"></bean>
    </array>
</property>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

编写测试类

```java
   @Test
   public void TestPage(){
       Page page = new Page(2,2);
       Page page1 = empDao.selectPage(page, null);
       List records = page1.getRecords();
       for (Object record : records) {
           System.out.println(record);
      }
       System.out.println("==============");
       System.out.println("获取总条数："+page.getTotal());
       System.out.println("当前页码："+page.getCurrent());
       System.out.println("总页码："+page.getPages());
       System.out.println("每页显示的条数："+page.getSize());
       System.out.println("是否有上一页："+page.hasPrevious());
       System.out.println("是否有下一页："+page.hasNext());
  }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 2、乐观锁插件（当要更新一条记录的时候，希望这条记录没有被别人更新）

**乐观锁实现方式：**

1. 取出记录时，获取当前version 
2. 更新时，带上这个version 
3. 执行更新时， set version = newVersion where version = oldVersion 
4. 如果version不对，就更新失败

添加配置：

```XML
<bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor"</bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

修改实体类添加version字段并在表中添加version字段

![img](https://img-blog.csdnimg.cn/20200810223802545.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810223804872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

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
==================================== 添加 =====================================  
        <property name="plugins插件">
            <array数组>
                //分页插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor分页拦截器"></bean>
                //乐观锁插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor乐观锁拦截器"></bean>
            </array>
        </property>
     </bean>
===============================================================================
//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
<bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>

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

**MyTest.****java**

```java
    @Test
    public void test06(){
        Emp emp = new Emp();
        emp.setEmpno(18);
        emp.seteName("haha");
        emp.setSal(10000.0);
        emp.setVersion(3);
        EmpDao empDao = context.getBean(EmpDao.class);
        empDao.updateById(emp);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810223849283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //当前方式被限制

![img](https://img-blog.csdnimg.cn/2020081022385890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810223902382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3、SQL执行分析插件，避免出现全表更新和删除

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
===============================================================================
        <property name="plugins插件">
            <array数组>
                //分页插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor分页拦截器"></bean>
                //乐观锁插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor乐观锁拦截器"></bean>
==================================== 添加 =====================================  
                //禁止全表更新和删除操作
                <bean class="com.baomidou.mybatisplus.extension.plugins.SqlExplainInterceptor（SQL解释拦截器）">
                </bean>
===============================================================================
            </array>
        </property>
     </bean>

//设置下面方式时，可不使用mybatis-config.xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer映射扫描配置">
        <property name="basePackage基本包 " value="com.mashibing.dao"></property>
    </bean>
===============================================================================  
<bean id="configuration配置时间" class="com.baomidou.mybatisplus.core.MybatisConfiguration（mybatis配置时间）">
        <property name="mapUnderscoreToCamelCase驼峰标识" value="false"></property>

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

**MyTest.****java**

```java
@Test
    public void test07(){
        EmpDao empDao = context.getBean(EmpDao.class);
        Emp emp = new Emp();
        emp.setSal(2222.2);
        int update更新 = empDao.update(emp, null);
        System.out.println(update);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810223936654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //这样是全表更新操作

**spring.****xml**

```XML
·
·
·
===============================================================================
·
·
·
        <property name="plugins插件">
            <array数组>
                //分页插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor分页拦截器"></bean>
                //乐观锁插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor乐观锁拦截器"></bean>
===============================================================================
                //禁止全表更新和删除操作
                <bean class="com.baomidou.mybatisplus.extension.plugins.SqlExplainInterceptor（SQL解释拦截器）">
==================================== 添加 =====================================  
                    <property name="sqlParserList（SQL解析器列表）">
                        <list>
                            <bean class="com.baomidou.mybatisplus.extension.parsers.BlockAttackSqlParser（攻击SQL阻塞解析器）"></bean>
                        </list>
                    </property>
===============================================================================
            </array>
        </property>
     </bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224000951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224003915.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //成功

## 4、非法sql检查插件

**spring.****xml**

```XML
·
·
·
===============================================================================
·
·
·
        <property name="plugins插件">
            <array数组>
                //分页插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor分页拦截器"></bean>
                //乐观锁插件
                <bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor乐观锁拦截器"></bean>
===============================================================================
                //禁止全表更新和删除操作
                <bean class="com.baomidou.mybatisplus.extension.plugins.SqlExplainInterceptor（SQL解释拦截器）">
                    <property name="sqlParserList（SQL解析器列表）">
                        <list>
                            <bean class="com.baomidou.mybatisplus.extension.parsers.BlockAttackSqlParser（阻塞攻击SQL解析器）"></bean>
                        </list>
                    </property>
                </bean>
==================================== 添加 =====================================  
                //非法sql阻断
                <bean class="com.baomidou.mybatisplus.extension.plugins.IllegalSQLInterceptor非法SQL拦截器"></bean>
===============================================================================
            </array>
        </property>
     </bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test08(){
        EmpDao empDao = context.getBean(EmpDao.class);
        QueryWrapper queryWrapper = new QueryWrapper();
        queryWrapper.or();
        List list = empDao.selectList(queryWrapper);
        System.out.println(list);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224059619.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //是用来将代码标准化，去掉此代码是可以执行，但要看公司要求

# 7、SQL注入器

​       全局配置 `sqlInjector``（SQL注入器）`用于注入 `ISqlInjector` 接口的子类，实现自定义方法注入。也就是说我们可以将配置在xml中的文件使用注入的方式注入到全局中，就不需要再编写sql语句

https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-core/src/main/java/com/baomidou/mybatisplus/core/injector/DefaultSqlInjector.java

   在mybatis操作的时候，我们需要自己定义接口中实现的方法，并添加与之对应的EmpDao.xml文件，编写对应的sql语句

   在mybatis-plus操作的时候，我们只需要继承BaseMapper接口即可，其中的泛型T表示我们要实际操作的实体类对象

**自定义注入器：**![img](https://img-blog.csdnimg.cn/20200810224138698.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**DeleteAll.****java****（全部删除）**

```java
public class DeleteAll extends AbstractMethod抽象方法 {

    @Override
    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {
        String sql;
        MySqlMethod mySqlMethod = MySqlMethod.DELETE_ALL;
        if (tableInfo.isLogicDelete()) {
            sql = String.format(mySqlMethod.getSql(), tableInfo.getTableName(),  tableInfo,
                    sqlWhereEntityWrapper(true,tableInfo));
        } else {
            mySqlMethod = MySqlMethod.DELETE_ALL;
            sql = String.format(mySqlMethod.getSql(), tableInfo.getTableName(),
                    sqlWhereEntityWrapper(true,tableInfo));
        }
        SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql, modelClass);
        return addUpdateMappedStatement(mapperClass, modelClass, mySqlMethod.getMethod(), sqlSource);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyInjector.****java****（我的注入器）**

```java
public class MyInjector extends  AbstractSqlInjectorjava抽象SQL注入器{

    @Override
    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {
        return Stream.of(new DeleteAll()).collect(Collectors.toList());
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MySqlMethod.****java****（我的SQL方法）**

```java
//自定义全局删除方法
    public enum MySqlMethod {

//删除全部
    DELETE_ALL("deleteAll", "根据 entity 条件删除记录", "<script>\nDELETE FROM %s %s\n</script>");

    private final String method;
    private final String desc;
    private final String sql;

    MySqlMethod(String method, String desc, String sql) {
        this.method = method;
        this.desc = desc;
        this.sql = sql;
    }

    public String getMethod() {return method;}
    public String getDesc() {return desc;}
    public String getSql() {return sql;}
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**EmpDao.****java**

```java
@Mapper映射
public interface EmpDao extends BaseMapper<Emp> {

    public List<Emp> selectEmpByCondition();
=================================== 添加 ======================================
    Integer deleteAll();
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**spring.****xml**

```XML
·	
·
·
底部：
===============================================================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        <property name="banner广告" value="true"></property> 
        <property name="dbConfig" ref="dbConfig"></property>
=================================== 添加 ======================================                
        <property name="sqlInjector" ref="myInject"></property>
    </bean>

    <bean id="myInject" class="com.mashibing.inject.MyInjector我的注射器"></bean>
===============================================================================  
    <bean id="dbConfig日志配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig（全局配置$日志配置）">
        <property name="tablePrefix表前缀" value="tbl_"></property>
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test09(){
        EmpDao empDao = context.getBean(EmpDao.class);
        Integer integer整数 = empDao.deleteAll();
        System.out.println(integer);
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224307212.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 8、公共字段填充

注解填充字段 `@TableField``(.. fill = FieldFill.``INSERT``插入``)` 生成器策略部分也可以配置！

metaobject:元对象，是mybatis提供的一个用于更加方便，更加优雅的访问对象的属性，给对象的属性设置值的一个对象，还会用于包装对象，支持Object,Map,Collection等对象进行包装。本质上metaobject元对象是给对象的属性设置值，最终还是要通过Reflect反射获取到属性的对应方法的invoker调用，最终执行。

**编写自定义的公共字段填充:**![img](https://img-blog.csdnimg.cn/20200810224318135.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

![img](https://img-blog.csdnimg.cn/20200810224321645.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224326781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyMetaObjectHandler****.****java****（我的元对象处理）**

```java
public class MyMetaObjectHandler implements MetaObjectHandler {
//插入
    @Override
    public void insertFill(MetaObject metaObject) {
        this.strictInsertFill(metaObject, "eName", String.class, "lian");
    }
//更新
    @Override
    public void updateFill(MetaObject metaObject) {
        this.strictUpdateFill(metaObject, "eName", String.class, "lian");
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**spring.****xml**

```XML
·	
·
·
底部：
===============================================================================  
    <bean id="globalConfig全局配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig全局配置">
        <property name="banner广告" value="true"></property> 
        <property name="dbConfig" ref="dbConfig"></property>
        //<property name="sqlInjector" ref="myInject"></property>
=================================== 添加 ======================================  
        <property name="metaObjectHandler" ref="myMeta"></property>
</bean>

    <bean id="myMeta" class="com.mashibing.fill.MyMetaObjectHandler我的元对象处理"></bean>
===============================================================================  
<bean id="myInject" class="com.mashibing.inject.MyInjector我的注射器"></bean>

    <bean id="dbConfig日志配置" class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig（全局配置$日志配置）">
        <property name="tablePrefix表前缀" value="tbl_"></property>
    </bean>
===============================================================================  
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**MyTest.****java**

```java
    @Test
    public void test10(){
        EmpDao empDao = context.getBean(EmpDao.class);
        empDao.insert(new Emp());
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200810224530167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //Default就是拒绝？

**spring.xml****中关掉插件：**

1. 乐观锁插件
2. 禁止全表更新和删除操作
3. 非法sql阻断
4. ![img](https://img-blog.csdnimg.cn/20200810224543582.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

![img](https://img-blog.csdnimg.cn/20200810224547888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)