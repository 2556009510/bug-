1. **切面（Aspect）:** 指关注点模块化，这个关注点可能会横切多个对象。事务管理是企业级Java应用中有关横切关注点的例子。 在Spring AOP中，切面可以使用通用类基于模式的方式（schema-based approach）或者在普通类中以`@Aspect`注解（@AspectJ 注解方式）来实现。

2. **连接点（Join point）:** 在程序执行过程中某个特定的点，例如某个方法调用的时间点或者处理异常的时间点。在Spring AOP中，一个连接点总是代表一个方法的执行。                                                                                               ![img](https://img-blog.csdnimg.cn/20200706221511152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTA0MjU2OQ==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) 

3. **通知（Advice）:** 在切面的某个特定的连接点上执行的动作。通知有多种类型，包括“around”, “before” and “after”等等。通知的类型将在后面的章节进行讨论。 许多AOP框架，包括Spring在内，都是以拦截器做通知模型的，并维护着一个以连接点为中心的拦截器链。

4. **切点（Pointcut）:** 匹配连接点的断言。通知和切点表达式相关联，并在满足这个切点的连接点上运行（例如，当执行某个特定名称的方法时）。切点表达式如何和连接点匹配是AOP的核心：Spring默认使用AspectJ切点语义。

5. 引入（Introduction）: 

   声明额外的方法或者某个类型的字段。Spring允许引入新的接口（以及一个对应的实现）到任何被通知的对象上。例如，可以使用引入来使bean实现 

   ```
   IsModified
   ```

   接口， 以便简化缓存机制（在AspectJ社区，引入也被称为内部类型声明（inter））。  

   **=========================** **下面很少用 ==========================**                           

6. **目标对象（Target object）:** 被一个或者多个切面所通知的对象。也被称作被通知（advised）对象。既然Spring AOP是通过运行时代理实现的，那么这个对象永远是一个被代理（proxied）的对象。

7. **AOP代理（AOP proxy）:**AOP框架创建的对象，用来实现切面契约（aspect contract）（包括通知方法执行等功能）。在Spring中，AOP代理可以是JDK动态代理或CGLIB代理

8. **织入（Weaving）: （上面1到5的过程就是织入）** 把切面连接到其它的应用程序类型或者对象上，并创建一个被被通知的对象的**过程**。这个过程可以在编译时（例如使用AspectJ编译器）、类加载时或运行时中完成。 Spring和其他纯Java AOP框架一样，是在运行时完成织入的。

**AOP****的通知类型：**

1. **前置通知（Before advice）:** 在连接点之前运行但无法阻止执行流程进入连接点的通知（除非它引发异常）。
2. **后置返回通知（After returning advice）:**在连接点正常完成后执行的通知（例如，当方法没有抛出任何异常并正常返回时）。
3. **后置异常通知（After throwing advice）:** 在方法抛出异常退出时执行的通知。
4. **后置通知（总会执行）（After (finally) advice）:** 当连接点退出的时候执行的通知（无论是正常返回还是异常退出）。
5. **环绕通知（Around Advice）:**环绕连接点的通知，例如方法调用。这是最强大的一种通知类型，。环绕通知可以在方法调用前后完成自定义的行为。它可以选择是否继续执行连接点或直接返回自定义的返回值又或抛出异常将执行结束。