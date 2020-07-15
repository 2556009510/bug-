## **6****、@PathVariable（路径变量）**

如果需要在请求路径中的参数像作为参数应该怎么使用呢？

可以使用@PathVariable注解，此注解就是提供了对占位符URL的支持，就是将URL中占位符参数绑定到控制器处理方法的参数中

**java****文件：****HelloController****（你好控制器）**

```java
@Controller
@RequestMapping("/hello")
public class HelloController{

    @RequestMapping( value = "/hello",
headers = {"User-Agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.79 Safari/537.36"})
    public String hello(Map<String,String> map){
        map.put("hello","hello,Springmvc");
        return "hello";
    }
    @RequestMapping( value = "/hello*")
    public String hello2(Map<String,String> map){
        map.put("hello","hello,heihei");
        return "hello";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****PathVariableController****（路径变量控制器）**

```java
@Controller
public class PathVariableController {
    @RequestMapping("/testPathVariable")
    public String testPathVariable(String name){
        System.out.println(name);
        return "hello";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172538338.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200714172542133.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172610983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202007141726188.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200714172620365.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172626272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**推荐这样写：**

![img](https://img-blog.csdnimg.cn/20200714172635614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172639612.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200714172658455.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**@PathVariable****可以获取请求路径中的值：**     在路径中要使用{变量名称}做标识
     在方法参数中可以添加@PathVariable做识别，如果路径中的名称跟参数的名称不一致的时候，可以添加路径中的变量名称

**java****文件：****PathVariableController****（路径变量控制器）**

```java
@Controller
public class PathVariableController {
    /**
     * @PathVariable可以获取请求路径中的值：
     *  在路径中要使用{变量名称}做标识
     *  在方法参数中可以添加@PathVariable做识别，如果路径中的名称跟参数的名称不一致的时候，可以添加路径中的变量名称
     *  推荐添加
     * @param id
     * @param name
     * @return
     */
    @RequestMapping("/testPathVariable/{id}/{name}")
    public String testPathVariable(@PathVariable("id") Integer id ,
@PathVariable("name") String name){
        //request.getParameter("name")
        System.out.println(id);
        System.out.println(name);
        return "hello";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

​    如果需要在请求路径中的参数像作为参数应该怎么使用呢？可以使用@PathVariable注解，此注解就是提供了对占位符URL的支持，就是将URL中占位符参数绑定到控制器处理方法的参数中

## **7****、REST：表现层、资源、状态转化（一种代码写作风格）**

REST,翻译过来叫做表现层状态转化，是目前最流行的一个互联网软件架构，它架构清晰，符合标准，易于理解，扩展方便。

![img](https://img-blog.csdnimg.cn/20200714172725812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**pom.****xml**

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

![img](https://img-blog.csdnimg.cn/20200714172753670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071417275730.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**web.****xml**

```html
================================= 控制器 ======================================
<servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>
org.springframework.web.servlet.DispatcherServlet
</servlet-class>
//设置初始化参数，指定默认的springmvc的配置文件
//关联springmvc的配置文件：
此配置文件的属性可以不添加，但是需要在WEB-INF的目录下创建   前端控制器名称+servlet.xml文件
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**springmvc.****xml**

```html
<context:component-scan base-package="com.mashibing"></context:component-scan>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
>
    <property name="prefix" value="/WEB-INF/page/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****UserDao****（用户访问对象）**

```java
public class UserDao {
//保存
    public void save(User user) {
        System.out.println("save"); 
    }
//更
public void update(Integer id) {
        System.out.println("update");
    System.out.println(id);
    }
//删
public void delete(Integer id) {
        System.out.println("delete");
    System.out.println(id);
    }
//查
public void query() {
        System.out.println("query");
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**发送四个请求：**

localhost:8080/web_project/save
localhost:8080/web_project/update?id=1
localhost:8080/web_project/delete?id=1
localhost:8080/web_project/query
 
**我们在发送请求的时候有不同的请求方式，能不能通过请求方式来做一次转换**
   POST： 新增      /user
   GET:    获取      /user
   PUT:    更新      /user/1
   DELETE: 删除      /user/1

**java****文件：****UserController****（用户控制器）**

```java
@Controller
public class UserController {

    @Autowired
    private UserDao userDao;
//保存
    @RequestMapping("/user")
    public String save(){
        System.out.println(this.getClass().getName()+"save");
        userDao.save(new User());
        return "success"; //success（成功）
    }
//更
    @RequestMapping("/update")
    public String update(Integer id){
        System.out.println(this.getClass().getName()+"update");
        userDao.update(id);
        return "success"; //success（成功）
    }
//删
    @RequestMapping("/delete")
    public String delete(Integer id){
        System.out.println(this.getClass().getName()+"delete");
        userDao.delete(id);
        return "success"; //success（成功）
    }
//查
    @RequestMapping("/query")
    public String query(){
        System.out.println(this.getClass().getName()+"query");
        userDao.query();
        return "success"; //success（成功）
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172908292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200714172911443.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172915181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200714172918811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071417292676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172930985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172934664.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172938290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714172941757.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**发送四个请求：**

localhost:8080/web_project/save
localhost:8080/web_project/update?id=1
localhost:8080/web_project/delete?id=1
localhost:8080/web_project/query
 
**我们在发送请求的时候有不同的请求方式，能不能通过请求方式来做一次转换**
   POST： 新增      /user
   GET:   获取      /user
   PUT:   更新      /user/1
   DELETE:删除      /user/1





​        **表现层（Representation）**：把资源具体呈现出来的形式，因此叫做表现层。

​        **资源（Resource）**：网络上的一个具体信息，文本，图片，音频，视频都可以称之为资源，如果想要访问到互联网上的某一个资源，那么就必须要使用一个URL来唯一性的获取改资源，也可以这么说，URL是每一个资源的唯一标识符。

​        **状态转化（State Transfer）**：当客户端发出一个请求的时候，就代表客户端跟服务端的一次交互过程，HTTP是一种无状态协议，即所有的状态都保存在服务器端，因此，客户端如果想要操作服务器，必须通过某些手段，让服务器的状态发生转化，而这种转化是建立在表现层的，这就是名字的由来（非人话）

​        人话：我们在获取资源的时候就是进行增删改查的操作，如果是原来的架构风格，需要发送四个请求，分别是：

​        **查询**：localhost:8080/query?id=1

​        **增加**：localhost:8080/insert

​        **删除**：localhost:8080/delete?id=1

​        **更新**：localhost:8080/update?id=1

​        按照此方式发送请求的时候比较麻烦，需要定义多种请求，而在HTTP协议中，有不同的发送请求的方式，分别是GET、POST、PUT、DELETE等，我们如果能让不同的请求方式表示不同的请求类型就可以简化我们的查询



一切看起来都非常美好，但是大家需要注意了，**我们在发送请求的时候只能发送****post****或者****get****，没有办法发送****put****（放置）****和****delete****（删除）****请求**，那么应该如何处理呢？下面开始进入代码环节：



![img](https://img-blog.csdnimg.cn/20200714173005115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714173008630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**success.jsp** ![img](https://img-blog.csdnimg.cn/20200714173016548.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) ![img](https://img-blog.csdnimg.cn/20200714173021984.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**index.jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
================================== 添加 =======================================
<%
  pageContext.setAttribute("ctp",request.getContextPath());
%>
===============================================================================
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
================================== 添加 =======================================
  <form action="${ctp}/user" method（方法）="post">
    <input type="submit（提交）" value="新增">
  </form>
  <a href="${ctp}/user">查询</a>
===============================================================================
  </body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071417310355.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

跳转到：![img](https://img-blog.csdnimg.cn/2020071417310987.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714173111978.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

跳转到：![img](https://img-blog.csdnimg.cn/20200714173119984.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

=====================================================================

**java****文件：****MyFilter****（过滤器）**

```java
public class MyFilter implements Filter {
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("个过滤器开始执行");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**index.jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %> 
<%
  pageContext.setAttribute("ctp",request.getContextPath());
%>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="${ctp}/user" method="post">
    <input type="submit" value="新增">
  </form>
================================== 添加 =======================================
  <form action="${ctp}/user" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="submit" value="更新">
  </form>
  <form action="${ctp}/user" method="post">
    <input type="hidden" name="_method" value="delete">
    <input type="submit" value="删除">
  </form>
===============================================================================
  <a href="${ctp}/user">查询</a>
  </body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**web.****xml**

```html
================================= 过滤器 ======================================
//此过滤器完成请求方式的转换，将post请求转换为put请求或者delete请求
    <filter>
        <filter-name>hidden（隐藏）</filter-name>
        <filter-class>
org.springframework.web.filter.HiddenHttpMethodFilter
        </filter-class>
    </filter>

    <filter-mapping>
        <filter-name>hidden（隐藏）</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202007141732122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//点击更新**

跳转：![img](https://img-blog.csdnimg.cn/2020071417322510.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071417322882.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**========================================================**

**java****文件：****MyFilter****（过滤器）**

```java
public class MyFilter implements Filter {
================================== 添加 =======================================
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init"); //初始化
    }
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException{

        System.out.println(this.getClass().getName()+"------start"); //开始
        servletRequest.setCharacterEncoding("UTF-8");
        servletResponse.setCharacterEncoding("UTF-8");
        filterChain.doFilter(servletRequest,servletResponse);
        System.out.println(this.getClass().getName()+"------stop"); //结束
    }
public void destroy() {
        System.out.println("destroy"); //销毁
    }
===============================================================================
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**web.****xml**

```html
================================= 过滤器 ======================================
    <filter>
        <filter-name>myFilter</filter-name>
        <filter-class>org.mashibing.MyFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>myFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200714173306511.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//下面的是发送、销毁（两次请求）**