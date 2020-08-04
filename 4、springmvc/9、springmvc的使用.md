# **11****、SpringMVC异常处理机制**

​    在SpringMVC中拥有一套非常强大的异常处理机制，SpringMVC通过HandlerExceptionResolver（处理异常解析器），包括请求映射，数据绑定以及目标方法的执行时发生的异常。

![img](https://img-blog.csdnimg.cn/20200804151008661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**设置报出异常，并从其他页面显示异常的问题：**

**index.****jsp****（索引）**

```javascript
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <%
    pageContext.setAttribute("ctp",request.getContextPath());
  %>
  <body>
  <a href="${ctp}/i18n">国际化页面</a>
================================ 添加 =========================================
  <a href="${ctp}/exception1">触发异常</a>
===============================================================================
  </body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****ExceptionController****（异常控制器）**

```java
@Controller
public class ExceptionController {

    @RequestMapping("/exception1")
    public String exception(){
        System.out.println(this.getClass().getName());
        int i = 10/0;
        return "success成功";
    }
//将上面方法报的异常弄到这里
    @ExceptionHandler处理异常(value = {ArithmeticException.class})
    public ModelAndView handlerException(Exception exception异常){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("error错误");
        mv.addObject("exce操作",exception);
        return mv;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**error.****jsp****（错误）** ![img](https://img-blog.csdnimg.cn/20200804151109551.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

![img](https://img-blog.csdnimg.cn/20200804151113538.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151116250.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**当有两个一样的处理异常方法，测试执行谁：**

![img](https://img-blog.csdnimg.cn/20200804151126668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

当使用ExceptionHandler（异常处理）进行处理的时候，默认会先走小范围，然后再寻找大范围
当在某一个类中定义的ExceptionHandler只能处理当前类的异常信息，如果其他类包含的话，无法进行处理
    可以通过@ControllerAdvice（控制器建议）注解进行标注，表示为全局异常处理类

​    在不同的类中可能会包含不同的异常处理，因此我们需要定义一个全局的异常控制器,使用@ControllerAdvice（控制器建议）注解标注，如果本类跟全局都有相关异常的处理，那么会优先使用本类的（但是在下面几项调试中没弄测出来？）



**java****文件：****MyGlobalExceptionHandler****（全局异常处理）**

```java
@ControllerAdvice控制器建议
public class MyGlobalExceptionHandler {

    @ExceptionHandler异常处理(value = {ArithmeticException.class,NullPointerException.class})
    public ModelAndView handlerException(Exception exception){
        System.out.println("global-------exception1");
        ModelAndView mv = new ModelAndView();
        mv.setViewName("error");
        mv.addObject("exce",exception);
        return mv;
    }
    @ExceptionHandler异常处理(value = {Exception.class})
    public ModelAndView handlerException2(Exception exception){
        System.out.println("global-------exception2");
        ModelAndView mv = new ModelAndView();
        mv.setViewName("error");
        mv.addObject("exce",exception);
        return mv;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151200329.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151203856.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151210583.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151215216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080415122125.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151224479.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151230307.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151235310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151241393.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151243679.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151246777.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

每次进行异常处理的时候，先在本类查找，然后去查找全局配置

会默认的从DispatcherServlet（前端控制器）**.**properties（属性）中找到对应的异常处理类：



\#默认的处理类：org.springframework.web.servlet.HandlerExceptionResolver=

\#处理@ExceptionHandler（异常处理）：org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver,\

\#处理@ResponseStatus（响应状态），给自定义异常使用：org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver,\

\#判断是否是SpringMVC自带异常：org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver





@ResponseStatus（响应状态）的使用：

​    @ResponseStatus可以标注到方法上，但是标注在方法之后可能导致该方法无法被访问，因此更多的是在自定义类上

**java****文件：****ExceptionController****（异常控制器）**

```java
@Controller
public class ExceptionController {

    @RequestMapping("/exception1")
    public String exception(){
        System.out.println(this.getClass().getName());
        int i = 10/0;
        return "success成功";
    }
================================ 添加 =========================================
//@ResponseStatus（响应状态）虽然可以标注在方法上，但是不推荐使用

    @ResponseStatus响应状态(reason = "我就是错了，不知道什么原因",value = HttpStatus.NOT_ACCEPTABLE无法接受)
    @RequestMapping("/exception2")
    public String exception2(){
        System.out.println("exception2");
        return "success";
    }
===============================================================================

//将上面报的异常弄到这里
//    @ExceptionHandler处理异常(value = //{ArithmeticException.class,NullPointerException.class})
//    public ModelAndView handlerException(Exception exception异常){
//        System.out.println("exception1");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
//    @ExceptionHandler处理异常(value = {Exception.class})
//    public ModelAndView handlerException2(Exception exception){
//        System.out.println("exception2");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151354386.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151358122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151403866.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080415140613.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****UsernameException****（用户名异常）**

```java
@ResponseStatus(reason = "名字错误",value = HttpStatus.NOT_ACCEPTABLE无法接受)
public class UsernameException extends RuntimeException运行时异常 {
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****ExceptionController****（异常控制器）**

```java
@Controller
public class ExceptionController {

    @RequestMapping("/exception1")
    public String exception(){
        System.out.println(this.getClass().getName());
        int i = 10/0;
        return "success成功";
    }
//@ResponseStatus（响应状态）虽然可以标注在方法上，但是不推荐使用

    @ResponseStatus响应状态(reason = "我就是错了，不知道什么原因",value = HttpStatus.OK)
    @RequestMapping("/exception2")
    public String exception2(){
        System.out.println("exception2");
        return "success";
    }
================================ 添加 =========================================
    @RequestMapping("/exception3")
    public String exception3(String username用户名){
        System.out.println("exception3");
        if ("admin".equals(username)){
            return "success";
        }else{
            throw new UsernameException用户名异常();
        }
    }
===============================================================================

//将上面报的异常弄到这里
//    @ExceptionHandler处理异常(value = //{ArithmeticException.class,NullPointerException.class})
//    public ModelAndView handlerException(Exception exception异常){
//        System.out.println("exception1");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
//    @ExceptionHandler处理异常(value = {Exception.class})
//    public ModelAndView handlerException2(Exception exception){
//        System.out.println("exception2");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151455224.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151458137.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****MyGlobalExceptionHandler****（全局异常处理）**

![img](https://img-blog.csdnimg.cn/20200804151505465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

@ControllerAdvice（控制器建议）：如果本类跟全局都有相关异常的处理，那么会优先使用本类的（但是在调试中没弄测出来？）

![img](https://img-blog.csdnimg.cn/2020080415152694.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****ExceptionController****（异常控制器）**

```java
@Controller
public class ExceptionController {

    @RequestMapping("/exception1")
    public String exception(){
        System.out.println(this.getClass().getName());
        int i = 10/0;
        return "success成功";
    }
//@ResponseStatus（响应状态）虽然可以标注在方法上，但是不推荐使用

    @ResponseStatus响应状态(reason = "我就是错了，不知道什么原因",value = HttpStatus.NOT_ACCEPTABLE无法接受)
    @RequestMapping("/exception2")
    public String exception2(){
        System.out.println("exception2");
        return "success";
    }
@RequestMapping("/exception3")
    public String exception3(String username用户名){
        System.out.println("exception3");
        if ("admin".equals(username)){
            return "success";
        }else{
            throw new UsernameException用户名异常();
        }
    }
================================ 添加 =========================================
    @RequestMapping(value = "/exception4",method = RequestMethod.POST)
    public String exception4(){
        System.out.println("exception3");
        return "success";
    }
===============================================================================

//将上面报的异常弄到这里
//    @ExceptionHandler处理异常(value = //{ArithmeticException.class,NullPointerException.class})
//    public ModelAndView handlerException(Exception exception异常){
//        System.out.println("exception1");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
//    @ExceptionHandler处理异常(value = {Exception.class})
//    public ModelAndView handlerException2(Exception exception){
//        System.out.println("exception2");
//        ModelAndView mv = new ModelAndView();
//        mv.setViewName("error");
//        mv.addObject("exce",exception);
//        return mv;
//    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200804151600478.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//****默认的处理器帮我们处理**

在容器启动好，进入DispatcherServlet（前端控制器）之后，会对HandlerExceptionResolver（处理异常解析器）进行初始化操作：

![img](https://img-blog.csdnimg.cn/20200804151616831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    在一个类中可能会包含多个异常的处理方法，在不同的方法上可以使用不同范围的异常，在查找的时候会优先调用范围小的异常处理

学习SpringMVC框架最核心的就是DispatcherServlet（前端控制器）的设计，掌握好DispatcherServlet（前端控制器）是掌握SpringMVC的核心关键。

![img](https://img-blog.csdnimg.cn/20200804151625205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

