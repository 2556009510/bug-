## **7****、使用@ModelAttribute（模型属性）来获取请求中的数据**

![img](https://img-blog.csdnimg.cn/2020071613121586.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716131217155.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****User****（用户）**

```java
public class User {

    private Integer id;      //id
    private String name;     //名字
    private String password; //密码
    private Integer age;     //年龄

    public User() {}

    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}

    public String getName() {return name;}
    public void setName(String name) {this.name = name;}

    public String getPassword() {return password;}
    public void setPassword(String password) {this.password = password;}

    public Integer getAge() {return age;}
    public void setAge(Integer age) {this.age = age;}

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", password='" + password + '\'' +
                ", age=" + age +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131241983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**update****更新.****jsp**

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
================================== 添加 =======================================
<%
    pageContext.setAttribute("ctp",request.getContextPath());
%>
<body>
<form action="${ctp}/update">
    <input type="hidden" value="1" name="id"><br>
    name:张三<br> //直接显示名字，不显示输入框
    password:<input type="password" name="password"><br>
    age:<input type="text" name="age"><br>
    <input type="submit" value="更新"><br>
===============================================================================
</form>
</body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**java****文件：****UserController****（用户控制器）**

```java
@Controller
public class UserController {

    @RequestMapping("/update")
    public String update(User user){
        System.out.println(user);
        return "success";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131320647.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//****里面张三直接显示**

**跳转到：**![img](https://img-blog.csdnimg.cn/20200716131333333.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131335714.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//idea里面张三**

**没显示**

**（先查询，再取出）**

**======================** **解决方式 ===========================**

**java****文件：****UserController****（用户控制器）**

```java
@Controller
public class UserController {
============ 添加@ModelAttribute("user")，就是每次执行都创建User =============
    @RequestMapping("/update")
    public String update(@ModelAttribute("user") User user){
        System.out.println(user);
        return "success";
    }
================================== 添加 =======================================
@ModelAttribute
    public void testModelAttribute(Model model){
        User user = new User();
        user.setId(1);
        user.setName("李四");
        user.setAge(11);
        user.setPassword("1234");
        model.addAttribute("user",user); //增加属性
===============================================================================
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131402103.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**跳转到：**![img](https://img-blog.csdnimg.cn/20200716131408605.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131414687.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//成功**

![img](https://img-blog.csdnimg.cn/20200716131421436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131428237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//比较地址空间**

**如果是true两个就相同：**![img](https://img-blog.csdnimg.cn/20200716131439450.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

![img](https://img-blog.csdnimg.cn/20200716131444368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131447756.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//改成2都一样？**

**结论：先执行第二个属性值方法，再执行第一个方法**

![img](https://img-blog.csdnimg.cn/2020071613150181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131505638.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//一样**

**java****文件：****UserController****（用户控制器）**

```java
@Controller
public class UserController {

Object o1 = null;
    Object o2 = null;
    Model m1 = null;

    @RequestMapping("/update")
    public String update(@ModelAttribute("user2") User user,Model model){
        System.out.println("update--------------------");
        o2 = user;
        System.out.println(user);
        System.out.println(o1 == o2);
        System.out.println(m1 == model);
        return "success";
    }
@ModelAttribute
    public void testModelAttribute(Model model){
        System.out.println("testModelAttribute---------------+");
        User user = new User();
        user.setId(1);
        user.setName("李四");
        user.setAge(11);
        user.setPassword("1234");
        model.addAttribute("user2",user); //增加属性
        o1 = user;
        m1 = model;
    }
================================== 添加 =======================================
    @ModelAttribute
    public void testModelAttribute2(Model model){
        System.out.println("testModelAttribute2---------------+");
    }
===============================================================================
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131530279.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****从下往上执行**

**java****文件：****UserController****（用户控制器）**

```java
@Controller
public class UserController {

Object o1 = null;
    Object o2 = null;
    Model m1 = null;

    @RequestMapping("/update")
    public String update(@ModelAttribute("user2") User user,Model model){
        System.out.println("update--------------------");
        o2 = user;
        System.out.println(user);
        System.out.println(o1 == o2);
        System.out.println(m1 == model);
        return "success";
    }
================================== 添加 =======================================
    @RequestMapping("/update2")
    public String update2(){ //这里不添加@ModelAttribute（模型属性）
        System.out.println("update2----------");
        return "success";
    }
===============================================================================
@ModelAttribute
    public void testModelAttribute(Model model){
        System.out.println("testModelAttribute---------------+");
        User user = new User();
        user.setId(1);
        user.setName("李四");
        user.setAge(11);
        user.setPassword("1234");
        model.addAttribute("user2",user); //增加属性
        o1 = user;
        m1 = model;
    }
    @ModelAttribute
    public void testModelAttribute2(Model model){
        System.out.println("testModelAttribute2---------------+");
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

其实在使用的时候可以简化写法，也就是说，在方法的参数上不加@ModelAttribute也不会有问题

![img](https://img-blog.csdnimg.cn/20200716131642363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613164513.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****那么问题来了，其中第一二个方法会执行吗？**

![img](https://img-blog.csdnimg.cn/20200716131657241.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**跳转到：**![img](https://img-blog.csdnimg.cn/20200716131704142.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/202007161317078.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//成功**

![img](https://img-blog.csdnimg.cn/202007161317158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131720911.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//****说白了就是刚创建的方法与第一个方法不相同**

**========================================================**

![img](https://img-blog.csdnimg.cn/20200716131751679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果添加的@ModelAttribute模型属性（" "）属性的值不对，那么也是获取不到值的。同时可以添加@SessionAttributes会话属性，但是注意，如果没有设置值的话，会报错

![img](https://img-blog.csdnimg.cn/20200716131806673.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**跳转到：**![img](https://img-blog.csdnimg.cn/20200716131814837.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)**//****报错：没有该属性值**

![img](https://img-blog.csdnimg.cn/20200716131823608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613182872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131834347.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

@SessionAttributes（会话属性）要注意，在使用的时候如果没有对应的值，可能会报异常

![img](https://img-blog.csdnimg.cn/20200716131843281.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

@ModelAttribute（模型属性）注解用于将方法的参数或者方法的返回值绑定到指定的模型属性上，并返回给web视图。首先来介绍一个业务场景，来帮助大家做理解，在实际工作中，有些时候我们在修改数据的时候可能只需要修改其中几个字段，而不是全部的属性字段都获取，那么当提交属性的时候，从form表单中获取的数据就有可能只包含了部分属性，此时再向数据库更新的时候，肯定会丢失属性，因为对象的封装是springmvc自动帮我们new的，所以此时需要先将从数据库获取的对象保存下来，当提交的时候不是new新的对象，而是在原来的对象上进行属性覆盖，此时就需要使用@ModelAttribute（模型属性）



**注意：**@ModelAttribute除了可以使用设置值到model中之外，还可以利用返回值

![img](https://img-blog.csdnimg.cn/20200716131854411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131901826.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//李四拿到这个值**

![img](https://img-blog.csdnimg.cn/20200716131908792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131912990.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131916351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716131920508.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**在使用****@ModelAttribute****的时候，需要注意：**
 1、如果参数中的注解没有写名字的话，默认是用参数名称的首字母小写来匹配
 2、如果有值，直接返回，如果没有值，会去session中进行查找，也就是说会判断当前类上是否添加过@SessionAttributes（会话属性）

**推荐：**注解中最好添加参数，来作为标识，进行对象属性的赋值



**总结：**通过刚刚的给参数赋值，大家应该能够发现，当给方法中的参数设置值的时候，如果添加了@ModelAttribute（模型属性），那么在查找值的时候，是遵循以下方式：

1、方法的参数使用参数的类型首字母小写，或者使用@ModelAttribute("")的值

2、先看之前是否在model中设置过该属性值，如果设置过就直接获取

3、看@SessionAttributes（会话属性）注解标注类中的方法是否给session中赋值，如果有的话，也是直接获取，没有报异常

## **8****、使用forward（转发）实现页面转发**

在发送请求的时候，可以通过forward（转发）:来实现转发的功能：

**index.jsp** ![img](https://img-blog.csdnimg.cn/20200716133740711.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)  ![img](https://img-blog.csdnimg.cn/20200716133740775.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

**java****文件：****ForWardController****（转发控制器）**

```java
@Controller
public class ForWardController {

    @RequestMapping("/forward")
    public String forward(){
        System.out.println("forward");
        return "forward:/index.jsp";
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716133807636.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
    @RequestMapping("/forward2")
    public String forward2(){
        System.out.println("forward2");
        return "forward:/forward";
//"forward:/forward"意思是：返回到public String forward(){..｝的方法里面
 
（转发的除了可以转发回到页面之外，还可以转发到其他请求中）
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613382560.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



在使用转发的时候前面要添加forward**:**前缀，同时通过写的字符串能够看到forward请求，而不经过视图处理器

![img](https://img-blog.csdnimg.cn/20200716133833692.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716133837781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## **9****、使用redirect（重定向）来实现重定向**

**java****文件：****RedirectController****（重定向控制器）**

```java
@Controller
public class RedirectController {
//使用重定向的时候需要注意，添加redirect：前缀
//重定向操作也不会经过视图处理器（视图解析器？）

    @RequestMapping("/redirect")
    public String redirect(){
        System.out.println("redirect");
        return "redirect:/index.jsp";
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716133914985.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716133859913.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716133911144.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
    @RequestMapping("/redirect2")
    public String redirect2(){
        System.out.println("redirect2");
        return "redirect:/redirect";
    }
 
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/2020071613394085.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716133942857.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**在javaweb的时候大家应该都接触过重定向和转发的区别：**

**转发：**

由服务器的页面进行跳转，不需要客户端重新发送请求，特点如下：

​        1、地址栏的请求不会发生变化，显示的还是第一次请求的地址

​        2、请求的次数，有且仅有一次请求

​        3、请求域中的数据不会丢失

​        4、根目录：localhost:8080/项目地址/,包含了项目的访问地址

![img](https://img-blog.csdnimg.cn/20200716133951330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**重定向：**

在浏览器端进行页面的跳转，需要发送两次请求（第一次是人为的，第二次是自动的）

特点如下：

​        1、地址栏的地址发生变化，显示最新发送请求的地址

​        2、请求次数：2次

​        3、请求域中的数据会丢失，因为是不同的请求

​        4、根目录：localhost:8080/ 不包含项目的名称

![img](https://img-blog.csdnimg.cn/20200716134004389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**对比：**

| **区别**         | **转发forward()**  | **重定向sendRedirect()** |
| ---------------- | ------------------ | ------------------------ |
| **根目录**       | 包含项目访问地址   | 没有项目访问地址         |
| **地址栏**       | 不会发生变化       | 会发生变化               |
| **哪里跳转**     | 服务器端进行的跳转 | 浏览器端进行的跳转       |
| **请求域中数据** | 不会丢失           | 会丢失                   |

## **10****、静态资源的访问**

**java****文件：****StaticController****（静态控制器）**

```java
@Controller
public class StaticController {

    @RequestMapping("/static")
    public String testStatic(){
        System.out.println("static");
        return "forwrd:/index.jsp";
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**添加图片：** ![img](https://img-blog.csdnimg.cn/20200716134439288.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**index.****jsp**    ![img](https://img-blog.csdnimg.cn/20200716134348483.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)![img](https://img-blog.csdnimg.cn/20200716134440934.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
================================== 添加 =======================================
  <%
    pageContext.setAttribute("ctp",request.getContextPath());
  %>
  <body>
666666666<br>
  <img src="${ctp}/images/timg.jpg">
===============================================================================
  </body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716134446946.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716134449627.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) **//（图片在imagse文件夹下）**

![img](https://img-blog.csdnimg.cn/20200716134457861.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



此时大家发现我们请求的图片根本访问不到，根据查看发现路径是没有问题的，那么为什么会找不到静态资源呢？

​    大家发现此时是找不到对应的mapping（映射），此时是因为DispatcherServlet（前端控制器）会拦截所有的请求，而此时我们没有对应图片的请求处理方法，所以此时报错了，想要解决的话非常简单，只需要添加一个配置即可

**===================** **解决图片（静态资源）问题 ====================**

**springmvc.****xml**

![img](https://img-blog.csdnimg.cn/20200716134510405.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



**上面第一行代码：默认-控制器-处理**

```
此配置表示  我们自己配置的请求由controller（控制器）来处理，但是不能请求的处理交由tomcat来处理
静态资源可以访问，但是动态请求无法访问
```

**上面第二行代码：注解-被动**

第二个也可以简化这样写：<mvc:annotation-driven>

​       但是加上此配置之后，大家又发现此时除了静态资源无法访问之外，我们正常的请求也无法获取了，因此还需要再添加另外的配置：

```
保证静态资源和动态请求都能够访问
```



使用默认的servlet-handler来处理静态资源，原来请求不到原因在于所有的请求都交由DispatcherServlet来处理
但是DispatcherServlet中没有对应静态资源的处理逻辑，所以访问不到，添加默认之后就可以了，但是会发现此时动态请求无法完成
所以需要配合另外一个标签类配合使用

**//****也可以在web.****xml****内添加：**

**//**![img](https://img-blog.csdnimg.cn/20200716134526175.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 



===============================================================

**如果出现这种情况，就刷新整个项目试试（不用重启idea）：**

![img](https://img-blog.csdnimg.cn/20200716134538935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716134541239.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

============================================================

![img](https://img-blog.csdnimg.cn/202007161345454.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://img-blog.csdnimg.cn/20200716134548450.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)