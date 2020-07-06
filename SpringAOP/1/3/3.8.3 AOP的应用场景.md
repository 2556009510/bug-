1. **日志管理**
2. **权限认证**
3. **安全检查**
4. **事务控制**

**xml****文件：****pom**

```html
<groupId>com.mashibing</groupId>
<artifactId>spring_aop_study</artifactId>
<version>1.0-SNAPSHOT</version>

<dependencies>
    <!-- https://mvnrepository.com/artifact/junit/junit -->
     <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
     </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
     <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.3.RELEASE</version>
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
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

**java****文件：****CalculatorProxy****（计算器代理）**

```java
public class CalculatorProxy {

    public static Calculator getCalculator( final Calculator calculator){
        //获取被代理对象的类加载器
        ClassLoader loader = calculator.getClass().getClassLoader();
        //被代理对象的所有接口
        Class<?>[] interfaces = calculator.getClass().getInterfaces();
        //用来执行被代理类需要执行的方法
        InvocationHandler handler = new InvocationHandler() {
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
               Object result = null;
               try{
================================ 只改这里 =====================================
                   //开始调用被代理类的方法
                   result = method.invoke(calculator,args);
               }catch (Exception e){
               }finally {
               }
===============================================================================
               return result;
            }
        };
        Object o = Proxy.newProxyInstance(loader, interfaces, handler);
        return (Calculator) o; 
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**j****ava****文件：****MyCalculator****（计算器）**

```java
@Service
public class MyCalculator implements Calculator {
    public Integer add(Integer i, Integer j) throws NoSuchMethodException {
        Integer result = i+j;
        return result;
    }
    public Integer sub(Integer i, Integer j) throws NoSuchMethodException {
        Integer result = i-j;
        return result;
    }
    public Integer mul(Integer i, Integer j) throws NoSuchMethodException {
        Integer result = i*j;
        return result;
    }
    public Integer div(Integer i, Integer j) throws NoSuchMethodException {
        Integer result = i/j;
        return result;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
通知注解有以下几种类型：@Before：前置通知，在方法执行之前完成@After：后置通知，在方法执行完成之后执行@AfterReturing：返回通知：在返回结果之后运行@AfterThrowing：异常通知：出现异常的时候使用@Around：环绕通知在方法的参数列表中不要随便添加参数值，会有异常信息
```

**java****文件：****LogUtil****（日志工具）**

```java
@Aspect（方面）
@Component（组件）
public class LogUtil {
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622273530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**xml****文件：****applicationContext****（应用上下文、配置文件）**

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"

       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
">
//开启包的扫描
    <context:component-scan base-package="com.mashibing"></context:component-scan>
//开启aop的注解功能
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
@Test
public void test02() throws NoSuchMethodException {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
("applicationContext.xml");
    Calculator calculator = context.getBean(Calculator.class);
    calculator.add(1,1); 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
 //输出顺序没弄
    System.out.println(calculator.getClass()); 
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
通知的执行顺序：开始>异常>结束>结果是什么通知的正常执行顺序：如果正常执行：@Before à @After à @AfterReturning如果异常结束：@Before à @After à @AfterThrowing
```

![img](https://img-blog.csdnimg.cn/2020070622325643.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**j****ava****文件：****MyCalculator****（计算器）**

![img](https://img-blog.csdnimg.cn/20200706223315629.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706223328911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**（这个区别问题后面讲）**



------

**java****文件：****MyCalculator****（计算器）**

![img](https://img-blog.csdnimg.cn/20200706223343473.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706223350594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**在实际的生产环境中，更多的时候使用通配符的方式：**

**1****、****可以用来匹配一个或者多个字符**
execution( public Integer com.mashibing.service.M**y**Calculator.*****(Integer,**Integer**)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706223436283.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223442677.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706223445723.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622350611.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223511511.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//也能显示**

​    **2、匹配任意类型的参数，但只能匹配一个**
execution( public Integer com.mashibing.service.M*****Calculator.*****(Integer,*****))

![img](https://img-blog.csdnimg.cn/20200706223529873.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223533890.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706223541699.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

   **3****、\*****在进行匹配的时候只能匹配一层路径，不匹配多层**

![img](https://img-blog.csdnimg.cn/20200706223550976.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//\*是自动匹配**

![img](https://img-blog.csdnimg.cn/20200706223601221.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

**添加：**

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
("applicationContext.xml");

@Test
public void test03() throws NoSuchMethodException {
    MyCalculator2 myCalculator2 = context.getBean("myCalculator2", MyCalculator2.class);
    myCalculator2.add(1,1);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223626126.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//只显示该类的，其它显示不了**

​    **4****、\*****不能够匹配访问修饰符，如果不确定访问修饰符是什么，可以直接省略不写**
execution( Integer com.mashibing.service.MyCalculator.*****(Integer,*****))
​    **5****、****返回值void可以使用*********来代替**

![img](https://img-blog.csdnimg.cn/20200706223650990.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

**..****（点点）：**
**1****、****可以匹配多个参数，任意类型**
      execution(* com.mashibing.service.MyCalculator.*(**..**))

![img](https://img-blog.csdnimg.cn/2020070622370722.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223710136.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//也不会红色波浪线报错**

![img](https://img-blog.csdnimg.cn/2020070622372429.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223727683.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**2****、****可以匹配多层路径**

​      execution(* com.mashibing**..**MyCalculator*****.*(..))

![img](https://img-blog.csdnimg.cn/20200706223741267.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223752899.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**运行test03：**![img](https://img-blog.csdnimg.cn/20200706223801331.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**最偷懒的方式：（下面三种都可行）**如果表达式是以*****开头，那么可以代替所有

​      execution(* *(**..**))

​      execution(* com**..***(**..**))

​      execution(* ***.***(**..**))

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223906618.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**||****（或）：**多个条件只要满足其中一个即可
      execution(public Integer com.mashibing.service.MyCalculator.*(..)) **||** execution(* *(..))

![img](https://img-blog.csdnimg.cn/20200706223915192.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223923162.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**&&****（与）：**多个条件必须同时满足
      execution(public Integer com.mashibing.service.MyCalculator.*(..)) && execution(* *(..))

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223936201.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**&&****（与）：**多个条件必须同时满足
      execution(public Integer com.mashibing.service.MyCalculator.*(..)) && execution(* *(..))

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706224001397.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

！**（非）：**取反,除了这种情况的其他都满足
      **!** execution(public Integer com.mashibing.service.MyCalculator.add(Integer,Integer))
**运行test02：**![img](https://img-blog.csdnimg.cn/20200706224015401.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​ **？？**

使用通配符的时候不是越简洁越好，更多的是要选择符合要求或者符合规则的匹配方式，此时就要求在定义标识符的时候必须要遵循项目规范



**通知**的执行顺序：开始>异常>结束>结果是什么

**通知**的正常执行顺序**：**

如果正常执行：@Before à @After à @AfterReturning

如果异常结束：@Before à @After à @AfterThrowing

![img](https://img-blog.csdnimg.cn/2020070622403484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

**如果将数据写在该类里面，一般是：**

![img](https://img-blog.csdnimg.cn/20200706224047641.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224051835.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**但是这种方式比较繁琐，这里用的另一种有参函数：**![img](https://img-blog.csdnimg.cn/20200706224051817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622410987.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706224111586.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

```java
@Before("execution(public Integer com.mashibing.service.MyCalculator.*(..))")
public static start(JoinPoint joinPoint){
    //获取方法签名
    Signature signature = joinPoint.getSignature();
    //获取参数信息
    Object[] args = joinPoint.getArgs();
    System.out.println(signature.getName()+"方法开始执行：参数是"
+Arrays.asList(args));
}
@AfterReturning("execution(public Integer com.mashibing.service.MyCalculator.*
(Integer,Integer))")
public static void stop(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println(signature.getName()+"方法执行结束，结果是："
+result);
}
@AfterThrowing("execution(public Integer com.mashibing.service.MyCalculator.*
(Integer,Integer))")
public static void logException(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println(signature.getName()+"方法抛出异常：");
}
@After("execution(public Integer com.mashibing.service.MyCalculator.*
(Integer,Integer))")
public static void logFinally(JoinPoint joinPoint){
   Signature signature = joinPoint.getSignature();
   System.out.println(signature.getName()+"方法执行结束。。。。over");
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224213424.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果想要在方法中获取对应的参数或者方法名称等信息的时候，必须要使用JoinPoint对象，并且此参数必须是第一个

1、getSignature()
2、getArgs()



获取结果值：getName

如果方法中有返回值，那么必须要在注解中添加 Returing="result" ,这个result必须要和参数列表中的参数名称保持一致：

**java****文件：****LogUtil****（日志工具）****中的****第二个：（****@AfterReturning****）**

![img](https://img-blog.csdnimg.cn/20200706224345708.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/2020070622440228.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

获取异常信息：getMessage

如果需要添加异常信息，那么在注解中要添加Throwing="e"即可

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706224440405.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224444235.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706224459973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果想要添加其他参数，比如下面的int k，必须要添加 args(参数列表)，ArgNames(参数列表)**：**

@Before(value = "execution(public Integer com.mashibing.service.MyCalculator.*

(Integer,Integer)) && args(joinPoint,k)",argNames = "joinPoint,k")

![img](https://img-blog.csdnimg.cn/2020070622450841.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224512185.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通知方法在定义的时候有没有什么特殊的要求？

通知方法在定义的时候对于访问修饰符、返回值类型都没有影响（可改可不改）

![img](https://img-blog.csdnimg.cn/20200706224523172.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706224525475.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//****两个个结果都是一样**

------

如果有多个匹配的表达式相同，能否做抽象？

定义一个没有返回值的空方法，给该方法添加@Pointcut（切入点）,后续在使用的时候可以直接调用方法名称

![img](https://img-blog.csdnimg.cn/20200706224609729.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

//此处的方法只是起一个声明的作用,提供内部的其他通知方法进行调用

**都一样：**![img](https://img-blog.csdnimg.cn/20200706224626928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706224645786.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
@Before(value = "myPointCut()") //修改括号内容
public static start(JoinPoint joinPoint){
    //获取方法签名
    Signature signature = joinPoint.getSignature();
    //获取参数信息
    Object[] args = joinPoint.getArgs();
    System.out.println(signature.getName()+"方法开始执行：参数是"
+Arrays.asList(args)); 
}
@AfterReturning(value = "myPointCut()",returning = "result") //修改括号内容
public static void stop(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println(signature.getName()+"方法执行结束，结果是："
+result); 
}
@AfterThrowing(value = "myPointCut()",throwing = "e") //修改括号内容
public static void logException(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println(signature.getName()+"方法抛出异常：");
}
@After("myPointCut()") //修改括号内容
public static void logFinally(JoinPoint joinPoint){
   Signature signature = joinPoint.getSignature();
   System.out.println(signature.getName()+"方法执行结束。。。。over");
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224701453.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

定义一个没有返回值的空方法，给该方法添加@PointCut注解,后续在使用的时候可以直接调用方法名称

此处的方法只是起一个声明的作用,只是供内部的其他通知方法进行调用

------

![img](https://img-blog.csdnimg.cn/20200706224718677.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​       总结：环绕通知的执行顺序是优于普通通知的，具体的执行顺序如下**：**

环绕前置 à 普通前置 à 目标方法执行 à 环绕正常结束/出现异常 à 环绕后置 à 普通后置 à 普通返回或者异常

但是需要注意的是，如果出现了异常，那么环绕通知会处理或者捕获异常，普通异常通知是接收不到的，因此最好的方式是在环绕异常通知中向外抛出异常

**java****文件：****LogUtil****（日志工具）**

```java
@Around("myPointCut()")
public Object around(ProceedingJoinPoint pjp) throws Throwable {
    Signature signature（签名） = pjp.getSignature();
    Object[] args（参数） = pjp.getArgs();
    Object result（结果） = null;
    try {
==============================================================================
        System.out.println("环绕通知start:"+signature.getName()+"方法开始执行，参数为："+Arrays.asList(args));
        result = pjp.proceed(args); 
//通过反射的方式调用目标的方法，相当于执行method.invoke(),可以自己修改结果值
       //result = 100; 
         
        System.out.println("环绕通知stop"+signature.getName()+"方法执行结束");
==============================================================================
    } catch (Throwable throwable) {
        System.out.println("环绕异常通知:"+signature.getName()+"出现异常");
    }finally {
        System.out.println("环绕返回通知:"+signature.getName()+"方法返回结果是："+result);
    }
    return result;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706224753126.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

环绕通知：环绕通知在执行的时候是优先于普通通知

如果是正常结束，那么执行顺序是：
环绕前置通知 à @Before（之前）à 环绕后置通知 à 环绕返回通知 à @After（之后）à @AfterReturning（返回之后）

如果是异常结束，那么执行顺序是：
 环绕前置通知 à @Before（之前）à 环绕异常通知 à 环绕返回通知 à @After（之后）à @AfterReturing（返回之后）

![img](https://img-blog.csdnimg.cn/20200706224806635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果出现异常的时候，在环绕通知中解决了，那么普通通知是接受不到的，如果想让普通通知接收到需要进行抛出throw throwable
    执行顺序改为：
    环绕前置通知 à @Before（之前）à 环绕异常通知 à 环绕返回通知 à @After（之后）à @AfterThrowing（抛出之后）

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706224815871.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706224820860.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

**java****文件：****SecurityUtil****（安全工具）**

**复制LogUtil.java****（日志工具）**

```java
@Aspect
@Component
public class SecurityUtil {

@Pointcut("execution(public Integer com.mashibing.service.MyCalculator.*
(Integer,Integer))")
public void myPointCut(){}
@Pointcut("execution(* *(..))")
public void myPointCut1(){}
=================================== 改 =======================================
@Before(value = "myPointCut()")
public static start(JoinPoint joinPoint（连接点）){
    //获取方法签名
    Signature signature = joinPoint.getSignature();
    //获取参数信息
    Object[] args = joinPoint.getArgs();
    System.out.println("Security---"+signature.getName()+"方法开始执行：参数是"
+Arrays.asList(args)); 
        return = 100;
}
@AfterReturning(value = "myPointCut()",returning = "result") 
public static void stop(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println("Security---"+signature.getName()+"方法执行结束，结果是："
+result); 
}
@AfterThrowing(value = "myPointCut()",throwing = "e") 
public static void logException(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println("Security---"+signature.getName()+"方法抛出异常：");
}
@After("myPointCut()")
public static void logFinally(JoinPoint joinPoint){
   Signature signature = joinPoint.getSignature();
   System.out.println("Security---"+signature.getName()+"方法执行结束。。。。over");
}
====================================================================

@Around("myPointCut()")
public Object around(ProceedingJoinPoint pjp) throws Throwable {
    Signature signature（签名） = pjp.getSignature();
    Object[] args（参数） = pjp.getArgs();
    Object result（结果） = null;
    try {
==============================================================================
        System.out.println("环绕通知start:"+signature.getName()+"方法开始执行，参数为："+Arrays.asList(args));
        result = pjp.proceed(args); 
//通过反射的方式调用目标的方法，相当于执行method.invoke(),可以自己修改结果值
       //result = 100; 
        System.out.println("环绕通知stop"+signature.getName()+"方法执行结束");
==============================================================================
    } catch (Throwable throwable) {
        System.out.println("环绕异常通知:"+signature.getName()+"出现异常");
        throw throwable;
    }finally {
        System.out.println("环绕返回通知:"+signature.getName()+"方法返回结果是："+result);
    }
    return result;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

**只留：**

```java
@Aspect
@Component
public class LogUtil {

@Before(value = "myPointCut()")
private int start(JoinPoint joinPoint（连接点）){
    //获取方法签名
    Signature signature（签名）= joinPoint.getSignature();
    //获取参数信息
    Object[] args（参数）= joinPoint.getArgs();
    System.out.println("log---"+signature.getName()+"方法开始执行：参数是"
+Arrays.asList(args));
}
@AfterReturning(value = "myPointCut()",returning = "result")
public static void stop(JoinPoint joinPoint,Object result){
    Signature signature = joinPoint.getSignature();
    System.out.println("log---"+signature.getName()+"方法执行结束，结果是："
+result);
}
@AfterThrowing(value = "myPointCut()",throwing = "e")
public static void logException(JoinPoint joinPoint,Exception e){
    Signature signature = joinPoint.getSignature();
    System.out.println("log---"+signature.getName()+"方法抛出异常："
+e.getMessage());
}
@After("myPointCut()")
public static void logFinally(JoinPoint joinPoint){
    Signature signature = joinPoint.getSignature();
    System.out.println("log---"+signature.getName()+"方法执行结束。。。。。over");
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/2020070622492356.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224926388.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224934324.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**改名：**SecurityUtil.java改ASecurityUtil.java

![img](https://img-blog.csdnimg.cn/20200706224943885.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//位置发送变化**

![img](https://img-blog.csdnimg.cn/2020070622495611.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//****名字改回来即可解决**

当应用程序中包含多个切面类的时候，具体的执行顺序是什么样？


按照切面类默认字典序（按首字母顺序）
如果需要认为的规定顺序，可以在切面类上添加@Order注解同时可以添加具体的值，值越小，越优先（值越小，越最外）

**解决顺序方式：**

**java****文件：****SecurityUtil****（安全工具）**

![img](https://img-blog.csdnimg.cn/20200706225012833.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/2020070622502532.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706225028806.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**LogUtil.java****与****SecurityUtil.java****内容一样（不包括上面刚写的顺序），只改名称****:**

![img](https://img-blog.csdnimg.cn/20200706225037371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706225042213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)