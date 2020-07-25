**目录**

[1、AOP:将某段代码动态切入到指定方法的指定位置进行运行（代理）](#1、AOP%3A将某段代码动态切入到指定方法的指定位置进行运行（代理）)

[案例一：](#案例一：)

[方式一：（普通方式）](#方式一：（普通方式）)

[方式二：（AOP方式）](#方式二：（AOP方式）)

[方式三：（动态代理，常用方式）](#方式三：（动态代理，常用方式）)

[案例二：](#案例二：)

[反射：](#反射：)

[一般这样写：](#一般这样写：)

------



![img](https://img-blog.csdnimg.cn/20200706214141192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214140253.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**AOP：**Aspect Oriented Programming 面向切面编程

**OOP：**Object Oriented Programming 面向对象编程

# 1、**AOP:将****某段代码动态****切入到****指定方法****的****指定位置****进行运行（代理）**

![img](https://img-blog.csdnimg.cn/2020070621414183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  ![img](https://img-blog.csdnimg.cn/20200706214140942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214140224.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **案例一：**

### **方式一：（普通方式）**

![img](https://img-blog.csdnimg.cn/20200706214140196.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****Calculator****（计算器）**

```java
public interface Calculator {
    public Integer add(Integer i,Integer j);
    public Integer sub(Integer i,Integer j);
    public Integer mul(Integer i,Integer j);
    public Integer div(Integer i,Integer j);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyCalculator****（计算器）**

```java
@Service
public class MyCalculator implements Calculator {
    public Integer add(Integer i, Integer j) {
        Integer result = i+j;
        return result;
    }
    public Integer sub(Integer i, Integer j) {
        Integer result = i-j;
        return result;
    }
    public Integer mul(Integer i, Integer j) {
        Integer result = i*j;
        return result;
    }
    public Integer div(Integer i, Integer j) {
        Integer result = i/j;
        return result;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
public class MyTest {
    @Test
    public void test01() throws NoSuchMethodException {
        MyCalculator myCalculator = new MyCalculator();
        System.out.println(myCalculator.add(1, 2)); 
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**========================================================**

### **方式二：（AOP方式）**

![img](https://img-blog.csdnimg.cn/20200706214406655.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706214412327.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706214429663.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214437315.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**j****ava****文件：****MyCalculator****（计算器）**

**添加：**

```java
@Service
public class MyCalculator implements Calculator {
    public Integer add(Integer i, Integer j){
        LogUtil.start(i,j); //开始
        Integer result = i+j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer sub(Integer i, Integer j) {
        LogUtil.start(i,j); //开始
        Integer result = i-j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer mul(Integer i, Integer j) {
        LogUtil.start(i,j); //开始
        Integer result = i*j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer div(Integer i, Integer j) {
        LogUtil.start(i,j); //开始
        Integer result = i/j;
        LogUtil.stop(result); //结束
        return result;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214524965.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**j****ava****文件：****MyCalculator****（计算器）**

**添加：**

```java
@Service
public class MyCalculator implements Calculator {
    public Integer add(Integer i, Integer j) throws NoSuchMethodException {
添加：  Method add = MyCalculator.class.getMethod("add", Integer.class, Integer.class);
        LogUtil.start(add,i,j); //开始
        Integer result = i+j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer sub(Integer i, Integer j) throws NoSuchMethodException {
添加：  Method sub = MyCalculator.class.getMethod("sub", Integer.class, Integer.class);
        LogUtil.start(sub,i,j); //开始
        Integer result = i-j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer mul(Integer i, Integer j) throws NoSuchMethodException {
添加：  Method mul = MyCalculator.class.getMethod("mul", Integer.class, Integer.class);
        LogUtil.start(mul,i,j); //开始
        Integer result = i*j;
        LogUtil.stop(result); //结束
        return result;
    }
    public Integer div(Integer i, Integer j) throws NoSuchMethodException {
添加：  Method div = MyCalculator.class.getMethod("div", Integer.class, Integer.class);
        LogUtil.start(div,i,j); //开始
        Integer result = i/j;
        LogUtil.stop(result); //结束
        return result;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****LogUtil****（日志工具）**

![img](https://img-blog.csdnimg.cn/20200706214618752.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****Calculator****（计算器）**

![img](https://img-blog.csdnimg.cn/20200706214628925.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200706214641528.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706214654359.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706214657251.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**========================================================**

### **方式三：（动态代理，常用方式）** 

![img](https://img-blog.csdnimg.cn/20200706214720194.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****CalculatorProxy****（计算器代理）**

![img](https://img-blog.csdnimg.cn/20200706214731498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

**加入：**

```java
    Calculator calculator = CalculatorProxy.getCalculator (new MyCalculator());
    calculator.add(1, 1);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214757483.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**j****ava****文件：****MyCalculator****（计算器）**

**关掉：**

![img](https://img-blog.csdnimg.cn/20200706214806319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
        Calculator calculator = (Calculator) CalculatorProxy.getCalculator (new MyCalculator());
        System.out.println(calculator.add(1, 1));
        calculator.sub(1,1);
        calculator.mul(1,1);
        calculator.div(1,1); //如果改（1,0），该行数据会出错
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706214837590.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**如果改（1,0）：**

**java****文件：****CalculatorProxy****（计算器代理）**

```java
public class CalculatorProxy {

    public static Calculator getCalculator ( final Calculator calculator){
        //获取被代理对象的类加载器
        ClassLoader loader = calculator.getClass().getClassLoader();
        //被代理对象的所有接口
        Class<?>[] interfaces = calculator.getClass().getInterfaces();
        //用来执行被代理类需要执行的方法
        InvocationHandler handler = new InvocationHandler() {
            public Object invoke(Object proxy（代理）, Method method（方法）, Object[] args（参数）) throws Throwable {
               Object result = null;
               try{
================================ 只改这里 =====================================
                   System.out.println(method.getName()+"方法开始执行，参数列表是："+ Arrays.asList(args));
                   //开始调用被代理类的方法
                   result = method.invoke(calculator,args);
                   System.out.println(method.getName()+"方法执行完成，结果是："+result);
===============================================================================
               }catch (Exception e){
                   System.out.println(method.getName()+"方法抛出异常："
+e.getMessage());
                   LogUtil.logException(method,e);
               }finally {
                   System.out.println(method.getName()+"方法执行结束。。。。。over");
                   LogUtil.logFinally(method);
               }
               return result;
            }
        };
        Object o = Proxy.newProxyInstance(loader, interfaces, handler);
        return (Calculator) o; 
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**//写了处理异常后，解决此问题，但是只是跳过了异常**

![img](https://img-blog.csdnimg.cn/20200706214912129.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**解决方式：**

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
             开始：LogUtil.start(method,args);
                   //开始调用被代理类的方法
                   result = method.invoke(calculator,args);
             结束：LogUtil.stop(method,result);
               }catch (Exception e){
                   System.out.println(method.getName()+"方法抛出异常："
+e.getMessage());
                   LogUtil.logException(method,e);
               }finally {
                   System.out.println(method.getName()+"方法执行结束。。。。。over");
                   LogUtil.logFinally(method);
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

**java****文件：****LogUtil****（日志工具）**

**添加：**

![img](https://img-blog.csdnimg.cn/20200706214944901.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706214948438.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//该行未报错**

![img](https://img-blog.csdnimg.cn/20200706215006409.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706215013815.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**===========================================================**

## **案例二：**

![img](https://img-blog.csdnimg.cn/2020070621503174.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **反射：**

**java****文件：****SecondCalculator****（****第二个计算器）**

```java
public class SecondCalculator implements Calculator {
    public Integer add(Integer i, Integer j) throws NoSuchMethodException {
        return null;
    }
    public Integer sub(Integer i, Integer j) throws NoSuchMethodException {
        return null;
    }
    public Integer mul(Integer i, Integer j) throws NoSuchMethodException {
        return null;
    }
    public Integer div(Integer i, Integer j) throws NoSuchMethodException {
        return null;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyInterface****（接口）**

```java
public interface MyInterface {
    
    public void show();
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MySubClass****（下标类）**

**添加：**

![img](https://img-blog.csdnimg.cn/20200706215136514.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

**添加：**

![img](https://img-blog.csdnimg.cn/20200706215146780.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**1****、**必须要有接口，如果没有接口，不能使用，这种方式是用jdk提供的reflect（反射）包下的类

**2****、**但是在生产环境中不能保证每个类都有实现的接口，所以有第二种方式cglib（基于类的代理），cglib在实现的时候有没有接口都无所谓
**3****、**在spring中使用了两种动态代理的方式，一种是jdk提供的reflect（反射）下面写的，另外一种就是cglib（基于类的代理）



**java****文件：****MyTest****（测试）**

![img](https://img-blog.csdnimg.cn/20200706215244193.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706215247487.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**========================================================**

![img](https://img-blog.csdnimg.cn/20200706215259639.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### **一般这样写：**

**java****文件：****DynamicProxy****（动态代理）**

```java
public class DynamicProxy implements InvocationHandler {
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        return null;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**=============================================================**

**java****文件：****MyInterface****（接口）**

```java
public interface MyInterface {

    public void show(Integer i);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MySubClass****（下标类）**

```java
public class MySubClass implements MyInterface {
    public void show(Integer i) {
        System.out.println("show.......");
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706215428111.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200706215430993.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyTest****（测试）**

```java
System.out.println("------------------");
  MyInterface proxy = (MyInterface) CalculatorProxy.getProxy(new MySubClass());
  proxy.show(100);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200706215502470.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**=======================================================================**

![img](https://img-blog.csdnimg.cn/20200706215515232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020070621552015.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)