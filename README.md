spring框架概述
spring优点
l方便解耦，简化开发(高内聚低耦合)
•Spring就是一个大工厂(容器)，可以将所有对象创建和依赖关系维护，交给Spring管理
•spring工厂是用于生成bean
lAOP编程的支持
•Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
l声明式事务的支持
•只需要通过配置就可以完成对事务的管理，而无需手动编程
l方便程序的测试
•Spring对Junit4支持，可以通过注解方便的测试Spring程序
l方便集成各种优秀框架
•Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、Hibernate、MyBatis、Quartz等）的直接支持
l降低JavaEE API的使用难度
•Spring 对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低
spring体系结构
Spring框架整体被分为五个模块:核心容器(core container), 面向切面编程(aop), 数据访问(date access), web, test.
Spring4去掉了spring3中的struts,添加了messaging和websocket,其他模块保持不变
Spring的jar包大约20个,每个都有相应的功能,一个jar还可能依赖了若干其他jar,所以搞清楚它们之间的关系,配置maven依赖就可以简洁明了.
core
core部分包含4个模块
spring-core：依赖注入IoC与DI的最基本实现
spring-beans：Bean工厂与bean的装配
spring-context：spring的context上下文即IoC容器
spring-expression：spring表达式语言
它们的完整依赖关系
因为spring-core依赖了commons-logging，而其他模块都依赖了spring-core，所以整个spring框架都依赖了commons-logging，如果有自己的日志实现如log4j，可以排除对commons-logging的依赖，没有日志实现而排除了commons-logging依赖，编译报错
 aop
aop部分包含4个模块
spring-aop：面向切面编程
spring-aspects：集成AspectJ
spring-instrument：提供一些类级的工具支持和ClassLoader级的实现，用于服务器
spring-instrument-tomcat：针对tomcat的instrument实现(包含了spring的tomcat设备代理)
它们的完整依赖关系
 data access
data access部分包含5个模块
spring-jdbc：jdbc的支持
spring-tx：事务控制
spring-orm：对象关系映射，集成orm框架
spring-oxm：对象xml映射
spring-jms：java消息服务
它们的完整依赖关系
web
web包含4个模块
spring-web：基础web功能，如文件上传
spring-webmvc：mvc实现
spring-webmvc-portlet：基于portlet的mvc实现
spring-websocket：为web应用提供的高效通信工具
它们的依赖关系
test
test部分只有一个模块，我将spring-context-support也放在这里
spring-test：spring测试，提供junit与mock测试功能
spring-context-support：spring额外支持包，比如邮件服务、视图解析等
它们的依赖关系
 spring4新增
Spring4去掉了spring3中的struts,添加了messaging和websocket,其他模块保持不变.
spring-websocket：为web应用提供的高效通信工具
spring-messaging：用于构建基于消息的应用程序
它们的依赖关系

spring mvc常用知识点总结
1.spring mvc是靠spring 启动的。通过springjar包的org.springframework.web.servlet.DispatcherServlet这个servlet类具体启动的。
<servlet-name>springmvc</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
这个dispatcherservlet这个类，有个contextConfigLocation参数，指定springmvc的配置文件xml。
2.理清一下思路：每个tomcat下的程序war包都有一个web.xml。web.xml可以启动servlet。
web.xml只能配置servlet相关信息，如servletmapping等等，不能配置bean的注入，扫描包路径，注解驱动等，这
3.二、配置解析
　　1.Dispatcherservlet
　　DispatcherServlet是前置控制器，配置在web.xml文件中的。拦截匹配的请求，Servlet拦截匹配规则要自已定义，把拦截下来的请求，依据相应的规则分发到目标Controller来处理，是配置spring MVC的第一步。
4. <!-- scan the package and the sub package -->
<context:component-scan base-package="test.SpringMVC"/>

<!-- don't handle the static resource -->
<mvc:default-servlet-handler />

<!-- if you use annotation you must configure following setting -->
<mvc:annotation-driven />

<!-- configure the InternalResourceViewResolver -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
id="internalResourceViewResolver">
<!-- 前缀 -->
<property name="prefix" value="/WEB-INF/jsp/" />
<!-- 后缀 -->
<property name="suffix" value=".jsp" />
</bean>
</beans>
5.spring mvc配置文件里的配置标签，必背：
<context:component-scan base-package="test.SpringMVC"/>
<mvc:annotation-driven />
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"

<!-- scan the package and the sub package -->
<context:component-scan base-package="test.SpringMVC"/>

<!-- don't handle the static resource -->
<mvc:default-servlet-handler />

<!-- if you use annotation you must configure following setting -->
<mvc:annotation-driven />

<!-- configure the InternalResourceViewResolver -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
id="internalResourceViewResolver">
<!-- 前缀 -->
<property name="prefix" value="/WEB-INF/jsp/" />
<!-- 后缀 -->
<property name="suffix" value=".jsp" />
</bean>
</beans>
6.spring mvc的那些controller标签，是注入到spring的ioc容器中的。不是spring mvc自己的ioc，spring mvc没有自己的ioc容器，都是spring的
7.@RequestBody
　　该注解用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上 ,再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上
　　@ResponseBody
　　该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区
8.requestParam正确用法@RequestParam(value = "invitCode", required = false) string invitCode
requestParam修饰的东西全在小括号里面（）。
9.四、自动匹配参数 
//match automatically
@RequestMapping("/person")
public String toPerson(String name,double age){
System.out.println(name+" "+age);
return "hello";
}
　五、自动装箱
　　1.编写一个Person实体类 
package test.SpringMVC.model;

public class Person {
public String getName() {
return name;
}
public void setName(String name) {
this.name = name;
}
public int getAge() {
return age;
}
public void setAge(int age) {
this.age = age;
}
private String name;
private int age;

}
　　2.在Controller里编写方法

//boxing automatically
@RequestMapping("/person1")
public String toPerson(Person p){
System.out.println(p.getName()+" "+p.getAge());
return "hello";
}

10.在SpringMVC中，bean中定义了Date，double等类型，如果没有做任何处理的话，日期以及double都无法绑定。

解决的办法就是使用spring mvc提供的@InitBinder标签
11.比较简单的可以直接应用springMVC的注解@initbinder和spring自带的WebDataBinder类和操作
[java] view plain copy
在CODE上查看代码片派生到我的代码片
@InitBinder 
public void initBinder(WebDataBinder binder) { 
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd"); 
dateFormat.setLenient(false); 
binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true)); 
}
还要在springMVC配置文件中加上
[html] view plain copy
在CODE上查看代码片派生到我的代码片
<!-- 解析器注册 --> 
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"> 
<property name="messageConverters"> 
<list> 
<ref bean="stringHttpMessageConverter"/> 
</list> 
</property> 
</bean> 
<!-- String类型解析器，允许直接返回String类型的消息 --> 
<bean id="stringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter"/>
这样就可以直接将上传的日期时间字符串绑定为日期类型的数据了
MyBatis知识点整理
基础：
1、 概念：Java当中的一个持久层框架。
2、 特点、优势：
（1）把java代码和SQL代码做了一个完全分离。
（2）良好支持复杂对象的映射（输入映射、输出映射）
（3）使用动态SQL，可以预防SQL注入。
3、 原理：
（1）创建mybatis-config.xml配置文件
（2）创建sqlSessionFactory
（3）编写数据库表对应的实体类
（4）创建mybatis的sql映射文件，在这个文件中，把实体类的属性和数据库表的列联系起来，并且可以编写sql语句
（5）从sqlSessionFactory的实例获取session
（6）session内部通过Executor执行器执行操作。Executor会用到mapped statement对象，这个对象是对数据库存储的封装，包括：sql语句、输入参数、输出结果类型
（7）关闭session
MyBatis缓存：
1、概念：
（1）一级缓存：一级缓存是SqlSession（会话）级别的缓存。在操作数据库时需要构造sqlSession对象，在对象中有一个数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。
（2）二级缓存：二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。（二级缓存的原理和一级缓存原理一样，第一次查询，会将数据放入缓存中，然后第二次查询则会直接去缓存中取。但是一级缓存是基于 sqlSession 的，而 二级缓存是基于 mapper文件的namespace的，也就是说多个sqlSession可以共享一个mapper中的二级缓存区域，并且如果两个mapper的namespace相同，即使是两个mapper，那么这两个mapper中执行sql查询到的数据也将存在相同的二级缓存区域中）
它们的关系如图：

2、一级缓存的使用：
我们在一个 sqlSession 中，对User表根据id进行两次查询，查看他们发出sql语句的情况：
（1）第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库查询用户信息。得到用户信息，将用户信息存储到一级缓存中。 
（2）如果中间sqlSession去执行commit操作（执行插入、更新、删除），则会清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。
（3）第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中获取用户信息。

3、二级缓存的使用：

4、注意事项：
（1）如果设置了cacheEnabled=true; 那么MyBatis在执行查询的时候，先查看二级缓存（全局缓存）是否有查询的结果，如果有，直接返回缓存的结果；如果没有，再执行真正的查询，把查询的结果放到缓存中，再把结果返回给用户。
（2）二级缓存：mybatis中，每一个mapper都可以有一个二级缓存。使用<cache>节点配置。
（3）二级缓存使用场景：
 对于访问多的查询请求且用户对查询结果实时性要求不高，此时可采用mybatis二级缓存技术降低数据库访问量，提高访问速度，业务场景比如：耗时较高的统计分析sql、电话账单查询sql等。实现方法如下：通过设置刷新间隔时间，由mybatis每隔一段时间自动清空缓存，根据数据变化频率设置缓存刷新间隔flushInterval，比如设置为30分钟、60分钟、24小时等，根据需求而定。
 mybatis二级缓存对细粒度的数据级别的缓存实现不好，比如如下需求：对商品信息进行缓存，由于商品信息查询访问量大，但是要求用户每次都能查询最新的商品信息，此时如果使用mybatis的二级缓存就无法实现当一个商品变化时只刷新该商品的缓存信息而不刷新其它商品的信息，因为mybaits的二级缓存区域以mapper为单位划分的，当一个商品信息变化会将所有商品信息的缓存数据全部清空。解决此类问题可能需要在业务层根据需求对数据有针对性缓存。
语法：
1、#{}和${}
（1）#{}是一个占位符，接收输入参数，参数类型可以是基本类型、POJO、HashMap
（2）${}是一个拼接符号，可能导致sql注入，不建议使用。
2、resultMap标签：
（1）结果集映射的标签
（2）resultMap：
A、type：指定结果集中保存的实体类的类型
B、id：resultMap标签的标识
C、autoMapping：值范围true|false，设置是否启动自动映射功能。自动映射功能就是自动查找与字段名小写同名的属性名，并调用setter方法。设置为false之后，需要在resultMap内明确注明映射关系才会调用对应的setter方法。
（3）id：用于设置主键字段与领域模型属性的映射关系
（4）result：用于设置普通字段与领域模型属性的映射关系
3、sql标签：用来封装sql语句或者sql片段
MyBatis的DAO开发方法：
1、 使用mapper代理方法：
（1） 编写mapper.java 的接口类
（2） 编写mapper.xml
2、 编写接口类，需要遵守一些规范，MyBatis可以自动生成接口类的DAO实现类。规范有：
（1） mapper.xml中namespace需要等于mapper接口类的地址
（2） mapper接口类的方法的名称等于mapper.xml中statement（对应sql语句）的id
（3） mapper接口类的输入参数类型等于mapper.xml中statement的parameterType指定的类型
（4） mapper接口类的返回参数类型等于mapper.xml中statement的resultType指定的类型
3、
MyBatis和Hibernate的本质区别：
1、 Hibernate不需要写sql，通过hql或者Hibernate直接生成sql。Mybatis需要写sql。
2、 Mybatis适合于需求经常改动的项目，因为它的sql由程序员生成，容易改动。Hibernate适合于需求改动较少的项目。
MyBatis和Ibatis的本质区别：
1、MyBatis简化了编码过程，不需要写DAO的实现类。直接写一个DAO的接口和一个配置文件。MyBatis自动生成DAO实现类。
嵌套查询 及 嵌套结果查询：
嵌套查询：使用2次查询，然后在MyBatis（内存）中进行拼装。可能引起N+1问题。
嵌套结果查询：使用1次查询，然后将结果进行拼装。

