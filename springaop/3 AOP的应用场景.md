**目录**



[AOP的应用场景](#AOP的应用场景)

[Spring AOP的简单配置](#Spring AOP的简单配置)

[1.添加依赖](#1.添加依赖)

[2.编写配置](#2.编写配置)

[3.测试](#3.测试)

[4.通过cglib（字节码生成）来创建代理对象](#4.通过cglib（字节码生成）来创建代理对象)

[切入点表达式：](#切入点表达式：)

[1、*](#1、*)

[（1）可以用来匹配一个或者多个字符](#（1）可以用来匹配一个或者多个字符)

[（2）匹配任意类型的参数，但只能匹配一个](#（2）匹配任意类型的参数，但只能匹配一个)

[（3）*在进行匹配的时候只能匹配一层路径，不匹配多层](#（3）*在进行匹配的时候只能匹配一层路径，不匹配多层)

[（4）*不能够匹配访问修饰符，如果不确定访问修饰符是什么，可以直接省略不写](#（4）*不能够匹配访问修饰符，如果不确定访问修饰符是什么，可以直接省略不写)

[（5）返回值void可以使用*来代替](#（5）返回值void可以使用*来代替)

[2、..（点点）](#2、..（点点）)

[（1）可以匹配多个参数，任意类型](#（1）可以匹配多个参数，任意类型)

[（2）可以匹配多层路径](#（2）可以匹配多层路径)

[3、||（或）、&&（与）、！（非）](#3、||（或）、%26%26（与）、！（非）)

[（1）||（或）：多个条件只要满足其中一个即可](#（1）||（或）：多个条件只要满足其中一个即可)

[（2）&&（与）：多个条件必须同时满足](#（2）%26%26（与）：多个条件必须同时满足)

[（3）！（非）：取反,除了这种情况的其他都满足](#（3）！（非）：取反%2C除了这种情况的其他都满足)

[4、通知方法的执行顺序](#4、通知方法的执行顺序)

[5、获取方法的详细信息](#5、获取方法的详细信息)

[1、获取结果值：getName](#1、获取结果值：getName)

[2、获取异常信息：getMessage](#2、获取异常信息：getMessage)

[3、其他获取方式](#3、其他获取方式)

[6、spring对通过方法的要求](#6、spring对通过方法的要求)

[7、表达式的抽取](#7、表达式的抽取)

[8、环绕通知的使用](#8、环绕通知的使用)

[9、多切面运行的顺序](#9、多切面运行的顺序)

[基于配置的AOP配置](#基于配置的AOP配置)

------

## AOP的应用场景

- **日志管理**
- **权限认证**
- **安全检查**
- **事务控制**

# Spring AOP的简单配置

## 1.添加依赖

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

## 2.编写配置

**将目标类和切面类加入到IOC容器中，在对应的类上添加组件注解**

给LogUtil.java添加@Component组件

给MyCalculator.java添加@Service服务



添加自动扫描的配置

```XML
<!--别忘了添加context命名空间-->
<context:component-scan base-package="com.mashibing"></context:component-scan>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**设置程序中的切面类**

在LogUtil.java中添加@Aspect（方面）



**设置切面类中的方法是什么时候在哪里执行：**

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
通知注解有以下几种类型：
@Before：前置通知，在方法执行之前完成
@After：后置通知，在方法执行完成之后执行
@AfterReturing：返回通知：在返回结果之后运行
@AfterThrowing：异常通知：出现异常的时候使用
@Around：环绕通知
在方法的参数列表中不要随便添加参数值，会有异常信息
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

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

## 3.测试

**java****文件：****MyTest****（测试）**

```java
@Test
public void test02() throws NoSuchMethodException {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
("applicationContext.xml");
    Calculator calculator = context.getBean(Calculator计算器.class);
    calculator.add(1,1); 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
 //输出顺序没弄

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
    System.out.println(calculator.getClass()); 
}

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
通知的执行顺序：开始>异常>结束>结果是什么

通知的正常执行顺序：
如果正常执行：@Before à @After à @AfterReturning
如果异常结束：@Before à @After à @AfterThrowing

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622325643.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 4.通过cglib（字节码生成）来创建代理对象

**j****ava****文件：****MyCalculator****（计算器）**

![img](https://img-blog.csdnimg.cn/20200706223315629.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706223328911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**（这个区别问题后面讲）**

**如果有异常：**

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200815104713638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 切入点表达式：

## 1、*

### **（1）可以用来匹配一个或者多个字符**

execution( public Integer com.mashibing.service.M**y**Calculator.*****(Integer,**Integer**)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706223436283.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223442677.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706223445723.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622350611.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223511511.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//也能显示**

### **（2）匹配任意类型的参数，但只能匹配一个**

execution( public Integer com.mashibing.service.M*****Calculator.*****(Integer,*****))

![img](https://img-blog.csdnimg.cn/20200706223529873.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223533890.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706223541699.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **（3）\*****在进行匹配的时候只能匹配一层路径，不匹配多层**

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

### **（4）\*****不能够匹配访问修饰符，如果不确定访问修饰符是什么，可以直接省略不写**

execution( Integer com.mashibing.service.MyCalculator.*****(Integer,*****))

### **（5）返回值void可以使用*********来代替**

![img](https://img-blog.csdnimg.cn/20200706223650990.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

## **2、..****（点点）**

### **（1）可以匹配多个参数，任意类型**

execution(* com.mashibing.service.MyCalculator.*(**..**))

![img](https://img-blog.csdnimg.cn/2020070622370722.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223710136.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//也不会红色波浪线报错**

![img](https://img-blog.csdnimg.cn/2020070622372429.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706223727683.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **（2）可以匹配多层路径**

1. 方式一：

execution(* com.mashibing**..**MyCalculator*****.*(..))

![img](https://img-blog.csdnimg.cn/20200706223741267.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223752899.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**运行test03：**![img](https://img-blog.csdnimg.cn/20200706223801331.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 



1. 方式二：

**最偷懒的方式：（下面三种都可行）**如果表达式是以*****开头，那么可以代替所有

​      execution(* *(**..**))

​      execution(* com**..***(**..**))

​      execution(* ***.***(**..**))

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223906618.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

## 3、||（或）、&&（与）、！（非）

### **（1）||****（或）：**多个条件只要满足其中一个即可

execution(public Integer com.mashibing.service.MyCalculator.*(..)) **||** execution(* *(..))

![img](https://img-blog.csdnimg.cn/20200706223915192.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223923162.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

### **（2）&&****（与）：**多个条件必须同时满足

execution(public Integer com.mashibing.service.MyCalculator.*(..)) && execution(* *(..))

**运行test02：**![img](https://img-blog.csdnimg.cn/20200706223936201.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

### （3）！**（非）：**取反,除了这种情况的其他都满足

**!** execution(public Integer com.mashibing.service.MyCalculator.add(Integer,Integer))
**运行test02：**![img](https://img-blog.csdnimg.cn/20200706224015401.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​ **？？**

使用通配符的时候不是越简洁越好，更多的是要选择符合要求或者符合规则的匹配方式，此时就要求在定义标识符的时候必须要遵循项目规范

## 4、通知方法的执行顺序

**通知**的执行顺序：

开始@Before  

异常@AfterThrowing

结束@After

结果是@AfterReturning



**通知**的正常执行顺序**：**

如果正常执行：@Before --> @After --> @AfterReturning

如果异常结束：@Before --> @After --> @AfterThrowing

![img](https://img-blog.csdnimg.cn/2020070622403484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **5、**获取方法的详细信息

![img](https://img-blog.csdnimg.cn/20200706224051835.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**但是这种方式比较繁琐，这里用的另一种有参函数：**![img](https://img-blog.csdnimg.cn/20200706224051817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070622410987.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200706224111586.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

```bash
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

JoinPoint.getSignature()
JoinPoint.getArgs()



### **1、获取结果值：getName**

如果方法中有返回值，那么必须要在注解中添加 Returing="result" ,这个result必须要和参数列表中的参数名称保持一致：

**java****文件：****LogUtil****（日志工具）****中的****第二个：（****@AfterReturning****）**

![img](https://img-blog.csdnimg.cn/20200706224345708.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/2020070622440228.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **2、获取异常信息：getMessage**

如果需要添加异常信息，那么在注解中要添加Throwing="e"即可

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706224440405.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224444235.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706224459973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

1. ### **3、其他获取方式**

2. ###  

如果想要添加其他参数，比如下面的int k，必须要添加 args(参数列表)，ArgNames(参数列表)**：**

@Before(value = "execution(public Integer com.mashibing.service.MyCalculator.*

(Integer,Integer)) && args(joinPoint,k)",argNames = "joinPoint,k")

![img](https://img-blog.csdnimg.cn/2020070622450841.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224512185.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 6、spring对通过方法的要求

通知方法在定义的时候有没有什么特殊的要求？

通知方法在定义的时候对于访问修饰符、返回值类型都没有影响（可改可不改）

![img](https://img-blog.csdnimg.cn/20200706224523172.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706224525475.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//****两个结果都是一样**

## 7、表达式的抽取

如果有多个匹配的表达式相同，能否做抽象？

定义一个没有返回值的空方法，给该方法添加@Pointcut（切入点）,后续在使用的时候可以直接调用方法名称

![img](https://img-blog.csdnimg.cn/20200706224609729.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //此处的方法只是起一个声明的作用,提供内部的其他通知方法进行调用

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

**一样：**![img](https://img-blog.csdnimg.cn/20200706224626928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706224701453.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

定义一个没有返回值的空方法，给该方法添加@PointCut（切入点）,后续在使用的时候可以直接调用方法名称

此处的方法只是起一个声明的作用,只是供内部的其他通知方法进行调用

## 8、环绕通知的使用

![img](https://img-blog.csdnimg.cn/20200706224718677.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​       总结：环绕通知的执行顺序是优于普通通知的，具体的执行顺序如下**：**

环绕前置 

普通前置 

目标方法执行

环绕正常结束/出现异常 

环绕后置 

普通后置 

普通返回或者异常



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

1. 环绕前置通知 
2. @Before（之前）
3. 环绕后置通知 
4. 环绕返回通知 
5. @After（之后）
6. @AfterReturning（返回之后）



如果是异常结束，那么执行顺序是：

1. 环绕前置通知 
2. @Before（之前）
3. 环绕异常通知 
4. 环绕返回通知 
5. @After（之后）
6. @AfterReturing（返回之后）

![img](https://img-blog.csdnimg.cn/20200706224806635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果出现异常的时候，在环绕通知中解决了，那么普通通知是接受不到的，如果想让普通通知接收到需要进行抛出throw throwable



**执行顺序改为：**

1. 环绕前置通知 
2. @Before（之前）
3. 环绕异常通知 
4. 环绕返回通知 
5. @After（之后）
6. @AfterThrowing（抛出之后）



**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706224815871.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706224820860.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**========================================================**

**java****文件：****SecurityUtil****（安全工具）**

**复制LogUtil.java****（日志工具）**

```java
@Aspect
@Component
public class SecurityUtil {

@Pointcut("execution(public Integer com.mashibing.service.MyCalculator.*(Integer,Integer))")
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

SecurityUtil.java文件 改 ASecurityUtil.java文件

![img](https://img-blog.csdnimg.cn/20200706224943885.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//位置发送变化**

![img](https://img-blog.csdnimg.cn/2020070622495611.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  **//****名字改回来即可解决**



## 9、多切面运行的顺序

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

# 基于配置的AOP配置

**java文件：MyCalculator****（计算器）**

**关掉：**

![img](https://img-blog.csdnimg.cn/20200815091710271.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

**关掉：**

![img](https://img-blog.csdnimg.cn/20200815091717277.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****SecurityUtil****（安全工具）**

**关掉：**

![img](https://img-blog.csdnimg.cn/20200815091724319.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**xml****文件：aop**

![img](https://img-blog.csdnimg.cn/20200815091732882.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
">
    <bean id="logUtil" class="com.mashibing.util.LogUtil"></bean>
    <bean id="securityUtil" class="com.mashibing.util.SecurityUtil"></bean>
    <bean id="myCalculator" class="com.mashibing.service.MyCalculator"></bean>
//配置aop
    <aop:config>
        <aop:pointcut id="globalPoint" expression（表达式）="execution
( Integer com.mashibing.service.MyCalculator.*(..))"/>
//声明切面
        <aop:aspect ref="logUtil">
//将一样的表达式给抽取出来
            <aop:pointcut id="myPoint" expression（表达式）="execution
( Integer com.mashibing.service.MyCalculator.*(..))"/>
===============================================================================
//定义通知在哪些方法上使用
            <aop:before method="start" pointcut-ref="myPoint"></aop:before>
            <aop:after method="logFinally" pointcut-ref="myPoint"></aop:after>
            <aop:after-returning method="stop" pointcut-ref="myPoint" returning="result"></aop:after-returning>
            <aop:after-throwing method="logException" pointcut-ref="myPoint" throwing="e"></aop:after-throwing>

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815091809854.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //该e是前面设定的有参

```XML
            <aop:around method="around" pointcut-ref="myPoint"></aop:around>
===============================================================================
        </aop:aspect>
        <aop:aspect ref="securityUtil">
============================= 将上面内容复制下来 ==============================
//定义通知在哪些方法上使用
           <aop:before method="start" pointcut-ref="globalPoint"></aop:before>
           <aop:after method="logFinally" pointcut-ref="globalPoint">
</aop:after>
           <aop:after-returning method="stop" pointcut-ref="globalPoint" returning="result"></aop:after-returning>
           <aop:after-throwing method="logException" pointcut-ref="globalPoint" throwing="e"></aop:after-throwing>
           <aop:around method="around" pointcut-ref="globalPoint"></aop:around>
===============================================================================
        </aop:aspect>
    </aop:config>
</beans>

```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200815091843913.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815091851394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815091853720.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815091857176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****一样**

![img](https://img-blog.csdnimg.cn/20200815091909120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200815091912372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****一样**
