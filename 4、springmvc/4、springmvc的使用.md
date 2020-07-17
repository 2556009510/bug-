![img](https://img-blog.csdnimg.cn/20200714181200651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **4****、使用Model，Map，ModelMap传输数据到页面（回滚参数值）** 

​    在刚开始的helloworld项目中，我们传递了参数回到我们页面，但是后续的操作都只是接受用户的请求，那么在SpringMVC中除了可以使用原生servlet的对象传递数据之外，还有什么其他的方式呢？

​    可以在方法的参数上传入Model，ModelMap,Map类型，此时都能够将数据传送回页面

![img](https://img-blog.csdnimg.cn/20200716130351976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716130353941.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**导入依赖：**

```html
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.2.3.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.3.RELEASE</version>
    </dependency>
</dependencies>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130413247.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716130415156.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**web.****xml**

```html
================================= 过滤器 ======================================
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>
org.springframework.web.filter.CharacterEncodingFilter
    </filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
//过滤器映射
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
===============================================================================
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
org.springframework.web.servlet.DispatcherServlet
</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
</servlet>
//控制器映射
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**springmvc.****xml**

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans    
       http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context 
       https://www.springframework.org/schema/context/spring-context.xsd 
       http://www.springframework.org/schema/mvc 
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.mashibing">
</context:component-scan>
    <bean class="
org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="suffix" value=".jsp"></property> //后缀
        <property name="prefix" value="/WEB-INF/page/"></property> //前缀
    </bean>
</beans>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
public class OutputController {

    @RequestMapping请求路径("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,springmvc ");
        return "success";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**success.****jsp** ![img](https://img-blog.csdnimg.cn/20200716130525382.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200716130535752.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**设置服务器：**

![img](https://img-blog.csdnimg.cn/20200716130546955.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130553519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130557806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130601338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130606851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130614965.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130618975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130625884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130640953.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
public class OutputController {

    @RequestMapping请求路径("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,springmvc ");
        return "success";
    }
================================== 添加 =======================================
    @RequestMapping("/output2")
    public String output2(Model model){
        model.addAttribute("msg","hello,springmvc");
        return "success";
    }
    @RequestMapping("/output3")
    public String output3(ModelMap modelMap){
        modelMap.addAttribute("msg","hello,springmvc");
        return "success";
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**下面三个结果都一样：**

![img](https://img-blog.csdnimg.cn/20200716130704193.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613070777.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130709270.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在向页面回显数据的时候，可以在方法的参数中显示的声明      
  Map:
  ModelMap:
  Model:

**success.****jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
欢迎<br>
${msg}<br>
================================== 添加 =======================================
page:${pageScope.msg}<br> //当前页面范围
request:${requestScope.msg}<br> //请求范围
session:${sessionScope.msg}<br> //会话范围
application:${applicationScope.msg}<br> //应用范围
===============================================================================
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**下面三个结果都一样：**

![img](https://img-blog.csdnimg.cn/20200716130741566.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130743570.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130746506.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

都可以将数据进行回显：回显之后数据是保存在哪个作用域中的？（使用范围）
  page:  当前页面
  request： 当前请求
  session： 当前会话
  application：当前应用



当使用上述参数传递数据的时候回把数据都放置到Request作用域

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
public class OutputController {

    @RequestMapping请求路径("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,springmvc ");
        return "success";
    }
    @RequestMapping("/output2")
    public String output2(Model model){
        model.addAttribute("msg","hello,springmvc");
        return "success";
    }
    @RequestMapping("/output3")
    public String output3(ModelMap modelMap){
        modelMap.addAttribute("msg","hello,springmvc");
        return "success";
    }
================================== 添加 =======================================
    @RequestMapping请求路径("/output4")
    public ModelAndView output4(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("success");
        mv.addObject("msg","springmvc");
        return mv;
===============================================================================
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613081416.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130818189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**下面三个结果都一样：**

![img](https://img-blog.csdnimg.cn/2020071613082749.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/2020071613083011.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130833411.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716130836867.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716130901689.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716130836867.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

当使用此方式进行设置之后，会发现所有的参数值都设置到了request作用域中，那么这三个对象是什么关系呢？

![img](https://img-blog.csdnimg.cn/20200716130952765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **5****、使用ModelAndView对象传输数据到页面**

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
================================== 添加 =======================================
@SessionAttributes会话属性(value = "username")
===============================================================================
public class OutputController {

    @RequestMapping("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,springmvc ");
        System.out.println(map.getClass());
        return "success";
    }
    @RequestMapping("/output2")
    public String output2(Model model){
        model.addAttribute("msg","hello,springmvc");
        System.out.println(model.getClass());
        return "success";
    }
    @RequestMapping("/output3")
    public String output3(ModelMap modelMap){
        modelMap.addAttribute("msg","hello,springmvc");
        System.out.println(modelMap.getClass());
        return "success";
    }
    @RequestMapping("/output4")
    public ModelAndView output4(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("success");
        mv.addObject("msg","springmvc");
        return mv;
    }
================================== 添加 =======================================
    @RequestMapping("/testSession")
    public String testSession(Model model){
        model.addAttribute("username","zhangsan");
        return "success"; //成功
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**success.****jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
欢迎<br>
${msg}<br>
page:${pageScope.msg}<br> //当前页面范围
request:${requestScope.msg}<br> //请求范围
session:${sessionScope.msg}<br> //会话范围
application:${applicationScope.msg}<br> //应用范围
================================== 添加 =======================================
<hr>
session:${sessionScope.username}<br> //会话范围
request:${requestScope.username}<br> //请求范围
===============================================================================
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131032957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    发现当使用modelAndView对象的时候，返回值的类型也是此对象，可以将要跳转的页面设置成view的名称，来完成跳转的功能，同时数据也是放到request作用中

## **6****、使用session传输数据到页面**

@SessionAttribute（会话属性）：此注解可以表示，当向request作用域设置数据的时候同时也要向session中保存一份,此注解有两个参数，一个value（表示将哪些值设置到session中），另外一个type（表示按照类型来设置数据，一般不用，因为有可能会将很多数据都设置到session中，导致session异常）

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
================================ 添加"msg" ====================================
@SessionAttributes会话属性(value = {"username","msg"}) //测试两个谁的优先级高
===============================================================================
public class OutputController {

    @RequestMapping("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,output"); //改output
        System.out.println(map.getClass());
        return "success";
    }
    @RequestMapping("/output2")
    public String output2(Model model){
        model.addAttribute("msg","hello,output2"); //改output2
        System.out.println(model.getClass());
        return "success";
    }
    @RequestMapping("/output3")
    public String output3(ModelMap modelMap){
        modelMap.addAttribute("msg","hello,output3"); //改output3
        System.out.println(modelMap.getClass());
        return "success";
    }
    @RequestMapping("/output4")
    public ModelAndView output4(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("success");
        mv.addObject("msg","output4"); //改output4
        return mv;
    }
    @RequestMapping("/testSession")
    public String testSession(Model model){
        model.addAttribute("username","zhangsan");
        return "success"; //成功
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131101540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131104774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131106963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131109935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​     当需要向session（会话）中设置数据的时候，可以在当前Controller（控制器）上添加@SessionAttributes（会话属性）
​     此注解表示，每次在向request作用中设置属性值的时候，顺带着向session（会话）中保存一份

![img](https://img-blog.csdnimg.cn/20200716131118708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131123792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131127201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131132290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131138412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​     value和type（类型）都写上之后表示session（会话）中可以存储名字为value值的参数以及类型为Integer的参数
​     value和type可以分开单独使用，但是尽量不要一起使用，因为type类型会把所有符合类型的数据都设置到session（会话）中，导致session（会话）异常

**推荐这样写：**

**java****文件：****OutputController****（输出控制器）**

```java
@Controller
================================== 修改 =======================================
@SessionAttributes(types = String.class) //把所有符合类型的数据都设置到session（会话）中
===============================================================================
public class OutputController {

    @RequestMapping("/output")
    public String output(Map<String,String> map){
        map.put("msg","hello,output"); //改output
        System.out.println(map.getClass());
        return "success";
    }
    @RequestMapping("/output2")
    public String output2(Model model){
        model.addAttribute("msg","hello,output2"); //改output2
        System.out.println(model.getClass());
        return "success";
    }
    @RequestMapping("/output3")
    public String output3(ModelMap modelMap){
        modelMap.addAttribute("msg","hello,output3"); //改output3
        System.out.println(modelMap.getClass());
        return "success";
    }
    @RequestMapping("/output4")
    public ModelAndView output4(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("success");
        mv.addObject("msg","output4"); //改output4
        return mv;
    }
    @RequestMapping("/testSession")
    public String testSession(Model model){
        model.addAttribute("username","zhangsan");
        return "success"; //成功
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)