# **1****、SpringMVC的返回JSON数据**



到目前为止我们编写的所有Controller的方法的返回值都是String类型，但是大家应该都知道，我们有时候数据传递特别是在ajax中，我们返回的数据经常需要使用json，那么如何来保证返回的数据的是json格式呢？

**解决方式：使用****@ResponseBody****（响应体）**

![img](https://img-blog.csdnimg.cn/2020080319595324.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803195956648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080320000118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200004247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200009249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200012801.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200015981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200020528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**pom.xml**

导入依赖：

```html
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
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.10.3</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.3</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.10.3</version>
</dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**springmvc.****xml**

```html
<context:component-scan base-package="com.mashibing"></context:component-scan>
<mvc:default-servlet-handler/>
<mvc:annotation-driven></mvc:annotation-driven>
//视图解析器
<bean class="
org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="suffix" value=".jsp"></property>
    <property name="prefix" value="/WEB-INF/page/"></property>
</bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****User****（用户）**

```java
public class User {
    private Integer id;    //id
    private String name;   //姓名
    private Integer age;   //年龄
    private String gender; //性别

    public User() {}
    public User(Integer id, String name, Integer age, String gender) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}

    public String getName() {return name;}
    public void setName(String name) {this.name = name;}

    public Integer getAge() {return age;}
    public void setAge(Integer age) {this.age = age;}

    public String getGender() {return gender;}
    public void setGender(String gender) {this.gender = gender;}

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200148585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200152449.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****JsonController****（字符串控制器）**

```java
@Controller
public class JsonController {

    @ResponseBody（响应体） //表示把当前请求返回的内容直接作为响应体，用来接收
    @RequestMapping("/json")
    public List<User> json(){
        List<User> list = new ArrayList<User>();
        list.add(new User(1,"zhangsan",12,"man"));
        list.add(new User(2,"zhangsan2",13,"woman"));
        list.add(new User(3,"zhangsan3",14,"man"));
        return list;
    }
=================================== 添加 ======================================
    @ResponseBody（响应体）
    @RequestMapping("/json2")
    public String json2(){
        return "<h1>hello,JSON</h1>";
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200229257.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# **2****、发送ajax请求获取json数据** 

![img](https://img-blog.csdnimg.cn/20200803200243388.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//导入jquery配置文件**

![img](https://img-blog.csdnimg.cn/20200803200316437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200321391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080320033096.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//数据未显示出来**

**ajax.****jsp**

```bash
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<script src="scripts/jquery-1.9.1.min.js"></script>
<head>
    <title>Title</title>
</head>
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
<a href（链接）="${ctp}/json">获取用户信息</a>
<div>

//这里是页面，将返回的数据传到这里

</div>
</body>

//获取上面的<a></a>内的内容
<script脚本 type="text/javascript">
    $("a:first第一").click(function(){  //单击事件
        $.ajax({
            url:"${ctp}/json",
            type:"GET",
            success成功:function (data) {
================================= 换成 ========================================
              $.each(data,function(){
                  var user = this.id+"--"+this.name+"--"+this.age+"--"+this.gender;  //将java文件中的属性加进来
                  $("div").append添加(user+'</br>');
              })
===============================================================================
            }
        });
        return false;
    })
</script>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200426598.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# **3****、使用@****Request****Body****（请求体）获取请求体信息**

**创建requestbody.****jsp****（请求体）**

```bash
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
//这里可以写其他的
<form action（作用）="${ctp}/testRequestBody" method="post">
    <input name="username" value="zhangsan"><br>
    <input name="password" value="123456"><br>
    <input type="submit" value="提交">
</form>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****JsonController****（字符串控制器）**

```java
@Controller
public class JsonController {

//@ResponseBody表示把当前请求返回的内容直接作为响应体，用来接受
    @ResponseBody（响应体）
    @RequestMapping("/json")
    public List<User> json(){
        List<User> list = new ArrayList<User>();
        list.add(new User(1,"zhangsan",12,"man"));
        list.add(new User(2,"zhangsan2",13,"woman"));
        list.add(new User(3,"zhangsan3",14,"man"));
        return list;
    }
    @ResponseBody（响应体）
    @RequestMapping("/json2")
    public String json2(){
        return "<h1>hello,JSON</h1>";
    }
================================= 添加 ========================================
//所有东西只需用一个有参来获取请求
@RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody（请求体） String body){
        System.out.println(body);
        return "success";
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080320055981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200602394.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200605753.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****成功**

**requestbody.****jsp****（请求体）**

![img](https://img-blog.csdnimg.cn/20200803200620832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//添加功能**

![img](https://img-blog.csdnimg.cn/20200803200632446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200635347.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200640274.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//成功**

![img](https://img-blog.csdnimg.cn/20200803200650914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



同时@RequestBody能够接受json格式的请求数据：

**requestbody.****jsp****（请求体）**

```bash
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
================================= 添加 ========================================
<script src="scripts/jquery-1.9.1.min.js"></script>
===============================================================================
<head>
    <title>Title</title>
</head>
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
//这里可以写其他的
<form action（作用）="${ctp}/testRequestBody" method="post">
    <input name="username" value="zhangsan"><br>
    <input name="password" value="123456"><br>
    <input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
================================= 添加 ========================================
<hr/>
<a href="${ctp}/testRequestJson">发送接送数据</a>  //超链接
</body>

<script脚本 type="text/javascript">
    $("a:first第一").click(function(){
        var user = {id:"1",name:"zhangsan",age:"12",gender:"man"};
        var jsonuser = JSON.stringify字符串化(user);
        $.ajax({
            url:"${ctp}/testRequestJson",
            type:"post",
            data:jsonuser,
            contentType:"application/json",
            success成功:function(data) {
                alert警告(data)
            }
        });
        return false;
    })
</script>
===============================================================================
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****JsonController****（字符串控制器）**

```java
@Controller
public class JsonController {

    @ResponseBody（响应体） //表示把当前请求返回的内容直接作为响应体，用来接收
    @RequestMapping("/json")
    public List<User> json(){
        List<User> list = new ArrayList<User>();
        list.add(new User(1,"zhangsan",12,"man"));
        list.add(new User(2,"zhangsan2",13,"woman"));
        list.add(new User(3,"zhangsan3",14,"man"));
        return list;
    }
    @ResponseBody（响应体）
    @RequestMapping("/json2")
    public String json2(){
        return "<h1>hello,JSON</h1>";
    }
@RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody（请求体） String body){
        System.out.println(body);
        return "success";
    }
================================= 添加 ========================================
@RequestMapping("/testRequestJson")
    public String testRequestJson(User user){
        System.out.println(user);
        return "success";
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200758885.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200803100.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020080320080692.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200809232.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//成功**



另外在接受请求的时候也可以使用HttpEntity（http实体）对象，用来接受参数，可以获取请求头信息

获取请求头信息：

```java
@Controller
public class OtherController {

   @RequestMapping("/testHttpEntity")
   public String testRequestBody(HttpEntity<String> httpEntity){
       System.out.println(httpEntity);
       return "success";
  }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# **4****、使用****Respsonse****Entity****（响应实体）可以用来定制响应内容**

**java****文件：****EntiryController****（实体控制器）**

```java
@Controller
public class EntiryController {

    @RequestMapping("test")
    public String test(HttpEntity<String> httpEntity){
        System.out.println(httpEntity);
        return "success";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200905123.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**success.jsp****（成功）**

![img](https://img-blog.csdnimg.cn/20200803200912596.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200918509.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200921552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200926194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**requestbody.****jsp****（请求体）**

![img](https://img-blog.csdnimg.cn/20200803200934369.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//跳转到test页面**

![img](https://img-blog.csdnimg.cn/20200803200942690.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200945766.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//跳转到该页面**

![img](https://img-blog.csdnimg.cn/20200803200953758.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803200957241.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****EntiryController****（实体控制器）**

```java
@Controller
public class EntiryController {

    @RequestMapping("test")
    public String test(HttpEntity<String> httpEntity){
        System.out.println(httpEntity);
        String body = httpEntity.getBody();
        return "success";
    }
================================== 添加 =======================================
//自定义响应相关的信息，包含body（体）和header（头信息）

    @RequestMapping("/testResponseEntity")
    public ResponseEntity响应体<String> testResponseEntity(){
        String str = "<h1>hello,springmvc</h1>";

        HttpHeaders httpHeaders头信息 = new HttpHeaders();
        httpHeaders.add("Set-Cookie","name=zhangsan");
        return new ResponseEntity<String>(str,httpHeaders,HttpStatus（http状态）.OK);
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201024390.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201026794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# **5****、文件下载**

**java****文件：****DownController****（下载控制器）**

```java
@Controller
public class DownController {

    @RequestMapping("/download")
    public ResponseEntity<byte[]> download(HttpServletRequest request) throws Exception {

//获取要下载的路径
        ServletContext servletContext = request.getServletContext();
        String realPath = servletContext.getRealPath("/scripts/jquery-1.9.1.min.js");

//通过io流对文件进行读写
        FileInputStream fileInputStream = new FileInputStream(realPath);
        byte[] bytes = new byte[fileInputStream.available()]; //缓冲区
        fileInputStream.read(bytes); //将数据读到缓冲区里去
        fileInputStream.close();     //关闭流
        HttpHeaders httpHeaders（http头信息） = new HttpHeaders();
        httpHeaders.set("Content-Disposition","attachment;filename=jquery-1.9.1.min.js");
        return new ResponseEntity<byte[]>(bytes体,httpHeaders头信息,
HttpStatus.OK);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201237883.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201239952.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//下载**

# **6****、文件上传**

![img](https://img-blog.csdnimg.cn/20200803201255887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**导入依赖：**

```html
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
<!-https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## **6.1****、上传一个文件：**

**upload.****jsp****（上传）**

```bash
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
================================ 添加 =========================================
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
<form action="${ctp}/upload" method="post" enctype="multipart多部件/form-data">
    描述：<input type="text" name="desc"><br>
    文件：<input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
</body>
===============================================================================
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****UploadController****（上传控制器）**

```java
@Controller
public class UploadController {

    @RequestMapping("/upload")
    public String upload(
@RequestParam请求参数("file文件") MultipartFile multipartFile多部分文件,
@RequestParam(value = "desc排序",required必须 = false) String desc) throws IOException {

        System.out.println(desc);
//获取文件的名称
        System.out.println(file.getOriginalFilename());
//上传路径
        multipartFile.transferTo(new File("d:\\file\\"+multipartFile.getOriginalFilename()));
        return "success";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201401205.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201404756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//报错**

**springmvc.****xml**

```html
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="UTF-8"></property>
    <property name="maxUploadSize" value="1024000"></property> //最大上传尺寸
</bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201427289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201432237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201437114.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201439154.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201445584.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201449678.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) //大文件，超过上传尺寸

## **6.2****、上传多个文件：**

**upload.****jsp****（上传）**

```bash
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
<form action="${ctp}/upload" method="post" enctype="multipart多部件/form-data">
    描述：<input type="text" name="desc"><br>
    文件：<input type="file" name="file"><br>
================================ 添加 =========================================
文件：<input type="file" name="file"><br>
文件：<input type="file" name="file"><br>
===============================================================================
    <input type="submit" value="提交">
</form>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****UploadController****（上传控制器）**

```java
@Controller
public class UploadController {

    @RequestMapping("/upload")
    public String upload(@RequestParam请求参数("file文件") MultipartFile[] multipartFile,@RequestParam(value = "desc排序",required = false) String desc) throws IOException {
        System.out.println(desc);
================================ 换成 =========================================
//获取文件的名称
        for (MultipartFile file : multipartFile) {
            if(!file.isEmpty()){ //判断是否为空
                System.out.println(file.getOriginalFilename());
                file.transferTo(new File("d:\\file\\"+ file.getOriginalFilename
()));
            }
===============================================================================
        }
        return "success";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201537617.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201540497.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201549356.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200803201551454.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)//本来是上传三个，因为有一个是重复了

​    Spring MVC 为文件上传提供了直接的支持，这种支持是通过即插即用的 MultipartResolver（多部分解析器）实现的。Spring 用 Jakarta Commons FileUpload 技术实现了一个 MultipartResolver 实现类：CommonsMultipartResovler 

​    Spring MVC 上下文中默认没有装配 MultipartResovler，因此默认情况下不能处理文件的上传工作，如果想使用 Spring 的文件上传功能，需现在上下文中配置 MultipartResolver。

