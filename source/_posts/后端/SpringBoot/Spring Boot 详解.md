---
title: SpringBoot详解
tags: SpringBoot
categories:
- 后端
- SpringBoot
cover: https://tva1.sinaimg.cn/large/e6c9d24ely1h0ocwlevtgj20p00angms.jpg


---

#  第1章 Spring 基础

## Spring 概述

​       Spring是一个轻量级Java开发框架，为开发Java应用程序提供全面的基础架构支持。Spring负责基础架构，因此Java开发者可以专注于应用程序的开发。

​       Spring的功能模块被有组织地分散到约20个模块中，这些模块分布在核心容器，数据访问/集成（Data Access/Integration），Web，AOP（Aspect Oriented Programming，面向切面的编程），植入（Instrumentation），消息传输（Messaging）和测试（Test）中。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv189rj3jvj60qg0knt9j02.jpg)

## Spring IoC

### Spring IoC 的基本概念

​       控制反转（Inversion of Control，IoC）是Spring框架的核心，用来消减计算机程序的耦合问题。当Spring框架出现后，对象的实例不再由调用者来创建，而是由Spring容器来创建。这样，控制权由调用者转移到Spring容器，控制权发生了反转，这就是Spring的控制反转。

​       从Spring容器角度来看，Spring容器负责将被依赖对象赋值给调用者的成员变量，相当于为调用者注入它所依赖的实例，这就是Spring的依赖注入，主要目的是为了解耦，体现一种“组合”的理念。

​       综上所述，控制反转是一种通过描述（在Spring中可以是XML或注解）并通过第三方去产生或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入。

### Spring 的常用注解

1、声明Bean的注解

- @Component

​        该注解是一个泛化的概念，仅仅表示一个组件对象（Bean），可以作用在任何层次上，没有明确的角色。

- @Repository

​        该注解用于将数据访问层（DAO）的类标识为Bean，即注解数据访问层Bean，其功能与@Component相同。

- @Service

​        该注解用于标识一个业务逻辑组件类（Service层），其功能与@Component相同。

- @Controller

​       该注解用于标识一个控制器组件类（Spring MVC的Controller），其功能与@Component相同。

2、注入Bean的注解

- @Autowired

​       该注解可以对类成员变量、方法及构造方法进行标注，完成自动装配的工作。通过@Autowired的使用来消除setter和getter方法。默认按照Bean的类型进行装配。

- @Resource

​       该注解与@Autowired的功能一样。区别在于，该注解默认是按照名称来装配注入的，只有当找不到与名称匹配的Bean才会按照类型来装配注入；而@Autowired默认按照Bean的类型进行装配，如果想按照名称来装配注入，则需要结合@Qualifier注解一起使用。

​       @Resource注解有两个属性：name和type。name属性指定Bean实例名称，即按照名称来装配注入；type属性指定Bean类型，即按照Bean的类型来装配。

- @Qualifier

​       该注解与@Autowired注解配合使用。当@Autowired注解需要按照名称来装配注入，则需要结合该注解一起使用，Bean的实例名称由@Qualifier注解的参数指定。

### Java 配置

​       Java配置是Spring4.x推荐的配置方式，它是通过@Configuration和@Bean来实现的。@Configuration声明当前类是一个配置类，相当于一个Spring配置的XML文件。@Bean注解在方法上，声明当前方法的返回值为一个Bean。

​       **全局配置尽量使用Java配置，例如数据库相关的配置；业务Bean的配置尽量使用注解配置，例如数据访问层、业务逻辑层、控制器层等相关的配置。**

## Spring AOP

### Spring AOP 的基本概念

​       Spring AOP是Spring框架体系结构中非常重要的功能模块之一，该模块提供了面向切面编程实现，在 **事务处理、日志记录、安全控制 ** 等操作中被广泛使用。

​       AOP（Aspect-Oriented Programming），即面向切面编程。它与OOP（Object-Oriented Programming，面向对象编程）相辅相成，提供了与OOP不同的抽象软件结构的视角。在OOP中，以类作为程序的基本单元，而AOP中的基本单元是Aspect（切面）。Struts 2 的拦截器设计就是基于AOP的思想，是个比较经典的应用。

​       尽管使用OOP可以通过封装或者继承的方式达到代码的重用，但仍然存在同样的代码分散到各个方法中。因此，如果采用OOP来处理日志操作、安全控制等操作，不仅增加了开发者的工作量，而且提高了升级维护的难度。为了解决此类问题，AOP思想应运而生。

​       AOP采取横向抽取机制，即将分散在各个方法中的重复代码提取出来，然后在程序编译或运行阶段，再将这些抽取出来的代码应用到需要执行的地方。这种横向的抽取机制，采用传统的OOP是无法办到的，因为OOP实现的是父子关系的纵向重用。但是AOP不是OOP的替代品，而是OOP的补充，两者相辅相成。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gv1wxage22j60wq0rzaao02.jpg" style="zoom:80%;" />

### 基于注解开发 AspectJ

1、Spring的通知类型

- 环绕通知

​       在目标方法执行前和执行后实施增强，可以应用于日志记录、事务处理等功能。

- 前置通知

​       在目标方法执行前实施增强，可应用于权限管理等功能。

- 后置返回通知

​       在目标方法成功执行后实施增强，可以应用于关闭流、删除临时文件等功能。

- 后置（最终）通知

​       在目标方法执行后实施增强，与后置返回通知不同的是，不管是否发生异常都要执行该通知，可以应用于释放资源。

- 异常通知

​       在方法抛出异常后实施增强，可以应用于处理异常、记录日志等功能。

- 引入通知

​       在目标类中添加一些新的方法和属性，可以应用于修改目标类（增强类）。  

2、AspectJ注解

![](https://tva1.sinaimg.cn/large/008i3skNly1gv1xsskkuqj61710s90we02.jpg)

**注意：配置类中要使用@EnableAspectJAutoProxy注解开启Spring对AspectJ的支持。**

## Spring Bean

​       在Spring的应用中，Spring IoC容器可以创建、装配和配置应用组件对象，这里的组件对象称为Bean。

### Bean的实例化

​       在面向对象编程中，想使用某个对象时，需要事先实例化该对象。同样，在Spring框架中，想使用Spring容器中的Bean，也需要实例化Bean。

​       Spring框架实例化Bean有3种方式：构造方法实例化、静态工厂实例化、实例工厂实例化，其中最常用的是构造方法实例化。

### Bean的作用域

​       在Spring中，不仅可以完成Bean的实例化，还可以为Bean指定作用域。在Spring中为Bean的实例定义如下的作用域，通过@Scope注解来实现。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv1z7i6hf3j60tk0gnwfv02.jpg)

​       其中，singleton和prototype是最常用的两种，后面4种作用域仅用在Web Spring应用程序上下文中。

### Bean的初始化和销毁

​       在实际工程应用中，经常需要在Bean使用之前或之后做些必要的操作，Spring对Bean的生命周期的操作提供了支持。可以使用@Bean注解的initMethod和destroyMethod属性（相当于XML配置的init-Method和destroy-Method）对Bean进行初始化和销毁。

## Spring 的数据库编程

​       数据库编程是互联网编程的基础，Spring框架为开发者提供了JDBC模版模式，即jdbcTemplate，它可以简化许多代码，但在实际应用中jdbcTemplate并不常用。我们更多地会使用Hibernate框架或者MyBatis框架进行数据库编程。

### Spring JDBC 的XML配置

​       主要使用Spring JDBC模块的core和dataSource包进行Spring数据库编程。core包是JDBC的核心功能包，包括常用的JdbcTemplate类；dataSource包是访问数据源的工具类包。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv1zwwub0wj60sf0gi75d02.jpg)

### Spring JDBC 的Java配置

![](https://tva1.sinaimg.cn/large/008i3skNly1gv24oywsy4j613s0ik75y02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1gv24s3kb67j613l0t7gnb02.jpg)

### Spring Jdbc Template 的常用方法

​       获取JDBC模版后，要如何使用它呢？首先，需要了解Jdbc Template类的常用方法：update( )和query( )方法。

- `public int update(String sql,Object args[])`

​       该方法可以对数据表进行增加、修改、删除等操作。使用args[ ]设置SQL语句中的参数，并返回更新的行数。

- `public List<T> query(String sql,RowMapper<T>rowMapper,Object args[])`

​       该方法可以对数据表进行查询操作。rowMapper将结果集映射到用户自定义的类中（前提是自定义的类中的属性要与数据表的字段相对应）。

### 基于@Transactional注解的声明式事务管理

​       Spring的声明式事务管理，是通过AOP技术实现的事务管理，其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，在执行完目标方法之后根据执行的情况提交或者回滚事务。

​       @Transactional注解可以作用于接口、接口方法、类以及类方法上。

​       当作用于类上时，该类所有的public方法都将具有该类型的事务属性，同时也可以在方法级别使用该注解来覆盖类级别的定义。

​       虽然@Transactional注解可以作用于接口、接口方法、类以及类方法上，但是Spring小组建议不要在接口或者接口方法上使用该注解，因为这只有在使用基于接口的代理时它才会生效。

### 如何在事务处理中捕获异常

​       声明式事务处理的流程是：

（1）Spring根据配置完成事务定义，设置事务属性。

（2）执行开发者的代码逻辑。

（3）如果开发者的代码产生异常（如主键重复）并且满足事务回滚的配置条件，则事务回滚；否则，事务提交。

（4）事务资源释放。

​       如果在代码逻辑中添加try···catch···语句，主键虽然重复，但事务并没有回滚。这是因为在默认的情况下，Spring只在发生未被捕获的RuntimeException时才会回滚事务。**在实际工程应用中，在catch语句中添加`TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();`语句即可。**

# 第2章 Spring MVC 基础

​       MVC思想将一个应用分成3个基本部分：Model（模型）、View（视图）和Controller（控制器），这3个部分以最低的耦合进行协同工作，从而提高应用的可扩展性及可维护性。Spring MVC是一款优秀的基于MVC思想的应用框架，它是Spring提供的一个实现了Web MVC设计模式的轻量级Web框架。

## Spring MVC 的工作原理

​       Spring MVC框架主要由DispatcherServlet、处理器映射、控制器、视图解析器、视图组成，工作原理如下图所示。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv26gqanquj614q0pnmy202.jpg)

**总结：Spring MVC的工作流程如下：**

（1）客户端请求提交到DispatcherServlet。

（2）由DispatcherServlet控制器寻找一个或多个HandlerMapping，找到处理请求的Controller。

（3）DispatcherServlet将请求提交到Controller。

（4）Controller调用业务逻辑处理后，返回ModelAndView。

（5）DispatcherServlet寻找一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图。

（6）视图负责将结果显示到客户端。

## Spring MVC 的工作环境

## 基于注解的控制器

​       在使用Spring MVC进行Web应用开发时，Controller是Web应用的核心。Controller实现类包含了对用户请求的处理逻辑，是用户请求和业务逻辑之间的“桥梁”，是Spring MVC框架的核心部分，负责具体的业务逻辑处理。

### Controller注解类型

​       在Spring MVC中，使用org.springframework.stereotype.Controller注解类型声明某类的实例是一个控制器，同时别忘了在Spring MVC的配置文件中使用<context:component-scan/>元素或在Spring MVC配置类中使用@ComponentScan指定控制器类的基本包，进而扫描所有注解的控制器类。

### RequestMapping注解类型

​       在基于注解的控制器类中，可以为每个请求编写对应的处理方法。如何将请求与处理方法一一对应呢？需要使用org.springframework.web.bind.annotation.RequestMapping注解类型。

- （1）方法级别注解

`@RequestMapping (value = "/index/login")`

`public String login( ){ }`

- （2）类级别注解

`@RequestMapping ("/index")`

`public class IndexController{ }`

### 编写请求处理方法

​       在控制类中每个请求处理方法可以有多个不同类型的参数，以及一个多种类型的返回结果。

（1）请求处理方法中常出现的参数类型

​       Servlet API、输入输出流、表单实体类、注解类型、Model等Java类型。

（2）请求处理方法常见的返回类型

​       最常见的返回类型，就是代表逻辑视图名称的String类型，除了String类型以外，还有Model、View以及其他任意的Java类型。

### Controller接收请求参数的常见方式

​       Controller接收请求参数的方式有很多种，有的适合get请求方式，有的适合post请求方式，有的二者都适合。

- 通过实体bean接收请求参数

​       通过一个实体bean来接收请求参数，适用于get和post提交请求方式。需要注意的是，bean的属性名称必须与请求参数名称相同。

- 通过处理方法的形参接收请求参数

​       通过处理方法的形参接收请求参数，也就是直接把表单参数写在控制器类相应方法的形参中，即形参名称与请求参数名称完全相同。该接收参数方式适用于get和post提交请求方式。

- 通过@RequestParam接收请求参数

​       通过@RequestParam接收请求参数，适用于get和post提交请求方式。与形参方式的区别在于：当请求参数与接收参数名不一致时，“通过处理方法的形参接收请求参数”不会报400错误，而“通过@RequestParam接收请求参数”会报400错误。

- 通过@ModelAttribute接收请求参数

​       @ModelAttribue注解放在处理方法的形参上，用于将多个请求参数封装到一个实体对象，从而简化数据绑定的流程，而且自动暴露为模型数据用于视图页面展示时使用。而“通过实体bean接收请求参数”只是将多个请求参数封装到一个实体对象，并不能暴露为模型数据（需要使用model.addAttribute语句才能暴露为模型数据）。通过@ModelAttribute接收请求参数适用于get和post提交请求方式。

### 重定向与转发

​       重定向：将用户从当前处理请求定向到另一个视图（如JSP）或处理请求，以前的请求（request）中存放的信息全部失效，并进入一个新的request作用域。

​       转发：将用户对当前处理的请求转发给另一个视图或处理请求，以前的request中存放的信息不会失效。

​       转发是服务器的行为，重定向是客户端的行为。

- 转发到一个请求方法（同一个控制器类里，可省略/index/）

​      `return "forward:/index/isLogin";`

- 重定向到一个请求方法

​     `return "redirect:/index/isRegister";`

- 转发到一个视图

​     `return "register";`

### 应用@Autowired进行依赖注入

​       可以通过org.springframework.beans.factory.annotation.Autowired注解类型将依赖注入到一个属性（成员变量）或方法，例如：

​       `@Autowired`

​      `public UserService userService`

### @ModelAttribute

​       通过org.springframework.web.bind.annotation.ModelAttribute注解类型可以实现以下两个功能：

（1）绑定请求参数到实体对象（表单的命令对象）

（2）注解一个非请求处理方法

​       被@ModelAttribute注解的控制器的一个非请求处理方法，将在每次调用该控制器类的请求处理方法前被调用。这种特性可以用来控制登录权限，当然，控制登录权限的方法有很多，如拦截器、过滤器等。

## 表单标签库与数据绑定

​       数据绑定：将用户参数输入值绑定到领域模型的一种特性。在Spring MVC的Controller和View参数数据传递中，所有HTTP请求参数的类型均为字符串。如果模型需要绑定的类型为double或int，则需要手动进行类型转化。而有了数据绑定后，就不再需要手动将HTTP请求中的String类型转化为模型需要的类型。数据绑定的另一个好处是，当输入验证失败时，会重新生成一个HTML表单，无需重新填写输入字段。在Spring MVC中，为了方便、高效地使用数据绑定，还需要学习表单标签库。

### 表单标签库

​       表单标签库中包含可以用在JSP页面中渲染HTML元素的标签。JSP页面使用Spring表单标签库时，必须在JSP页面开头处声明taglib指令：

`<% @ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>`

​       表单标签库中有form、input、password、hidden、textarea、checkbox、checkboxes、radiobutton、radiobuttons、select、option、options、errors等元素。

- 表单标签

<form:form modelAttribute = "xxx" method = "post" action = "xxx"></form:form >

- input标签

<form: input path = "xxx" />

- password标签

<form: password path = "xxx" />

- hidden标签

<form: hidden path = "xxx" />

- textarea标签

<form: textarea path = "xxx" />

- checkbox标签

<form: checkbox path = "xxx" value = "xxx" />

- checkboxes标签

<form: checkboxes items = "xxx" path = "xxx" />

​       checkboxes渲染多个复选框，是一个选项组，等价于多个path相同的checkbox标签。它有3个非常重要的属性：items、itemLabel和itemValue。

​       items：用于生成input元素的Collection、Map或Array。

​       itemLabel：items属性中指定的集合对象的属性，为每一个input元素提供label。

​       itemValue：items属性中指定的集合对象的属性，为每一个input元素提供value。

- radiobutton标签

<form: radiobutton path = "xxx" value = "xxx" />

- radiobuttons标签

<form: radiobuttons path = "xxx" items = "xxx" />

​       该标签的 itemLabel和 itemValue属性与 checkboxes标签的 itemLabel和 itemValue属性完全一样，但只允许单选。

- select标签

​       select标签的选项可能来自其属性items指定的集合，或者来自于一个嵌套的option标签或options标签。

<form: select path = "xxx" items = "xxx" />

或

<form: select path = "xxx" items = "xxx" >

​        < option value = "xxx"> xxx < /option>

< / form:select >

或

<form: select path = "xxx" >

​        < form: options items = "xxx" />

< / form:select >

​       该标签的 itemLabel和 itemValue属性与 checkboxes标签的 itemLabel和 itemValue属性完全一样。

- options标签

​       options标签生成一个select标签的选项列表。因此，需要和select标签一同使用，具体用法见select标签。

- errors标签

​       errors标签渲染一个或多个span元素，每个span元素包含一个错误消息。它可以用于显示一个特定的错误消息，也可以显示所有的错误消息。

<form: errors path = " * " />

或

<form: errors path = " xxx " />

其中，“ * ”表示显示所有的错误消息；“xxx”表示显示由“xxx”指定的特定错误消息。

### 数据绑定

优点：表单提交失败后，输入过的信息不用再输入，会自动回填（必须结合form标签）。

## JSON数据交互

​       Spring MVC在数据绑定的过程中，需要对传递数据的格式和类型进行转换，它既可以转换String等类型的数据，也可以转换JSON等其他类型的数据。

### JSON数据结构

​       JSON（JavaScript Object Notation，JS对象标记）是一种轻量级的数据交换格式。与XML一样，JSON也是基于纯文本的数据格式，具有以下两种数据结构：

- 对象结构：以 { 开始，以 } 结束。中间部分由0个或多个以英文“ , ”分隔的key/value键值对构成，key和value以英文“ : ” 分隔。

​      {

​              key1 : value1,

​              key2 : value2,  

​              ···

​      }

​       其中，可以、必须是String类型，value可以是String、Number、Object、Array等数据类型。

- 数据结构：以 [ 开始，以 ] 结束。中间部分由0个或多个以英文“ , ”分隔的值的列表构成。

​      [

​              value1,

​              value2,  

​              ···

​       ]

**重点：上述两种（对象和数组）数据结构可以分别组合构成更为复杂的数据结构。**

### JSON数据转换

​       为实现浏览器与控制器类之间的JSON数据交互，Spring MVC提供了MappingJackson2HttpMessageConverter实现类默认处理JSON格式请求响应。该实现类利用Jackson开源包读写JSON数据，将Java对象转换为JSON对象和XML文档，同时也可以将JSON对象和XML文档转换为Java对象。

​       在使用注解开发时，需要用到两个重要的JSON格式转换注解，分别是@RequestBody和@ResponseBody。

- RequestBody：用于将请求体中的数据绑定到方法的形参中，该注解应用在方法的形参上。
- @ResponseBody：用于直接返回JSON对象，该注解应用在方法上。

## Spring MVC 的基本配置

​       Spring MVC的定制配置需要配置类实现WebMvcConfigurer接口，并在配置类使用@EnableWebMvc注解来开启对Spring MVC的配置支持，这样开发者就可以重写接口方法完成常用的配置。

### 静态资源配置

​       应用程序的静态资源（CSS、JS、图片等）需要直接访问，这时需要开发者在配置类重写`public void addResourceHandlers(ResourceHandlerRegistry registry)`接口方法来实现。

`public void addResourceHandlers(ResourceHandlerRegistry registry)`{

​        `registry.addResourceHandler("/html/**").addResourceLocations("/html/");`

}

​       addResourceHandler：指的是对外暴露的访问路径

​       addResourceLocations：指的是静态资源的存放位置

### 拦截器配置

​       Spring的拦截器（Interceptor）实现对每一个请求处理前后进行相关的业务处理，类似于Servlet的过滤器（Filter）。开发者如果需要自定义Spring的拦截器，可以通过下面两个步骤完成：

- 创建自定义拦截器类
- 配置拦截器

### 文件上传配置

​       文件上传是一个应用中经常使用的功能，Spring MVC通过配置一个MultipartResolver来上传文件。在Spring MVC的控制器中，可以通过MultipartFile myfile来接收单个文件上传，通过List < ultipartFile > myfiles来接收多个文件上传。

# 第3章 Spring Boot 入门

​       Spring框架非常优秀，但问题在于“配置过多”，造成开发效率低、部署流程复杂以及集成难度大等问题。为了解决上述问题，Spring Boot应运而生。

## Spring Boot 概述

### 什么是Spring Boot

​       Spring Boot设计目的是用来**简化**新Spring应用的初始搭建以及开发过程。在Spring Boot框架中，使用“约定优于配置（Convention Over Configuration，COC）”的理念。针对企业应用开发，提供了符合各种场景的**spring-boot-starter自动配置依赖模块**，这些模块都是“开箱即用”。可以说，Spring Boot是开发者和Spring框架的中间层，目的是帮助开发者管理应用的配置。

### Spring Boot的优点

- 使编码变得简单：推荐使用注解
- 使配置变得快捷：具有自动配置、快速构建项目、快速集成第三方技术的能力
- 使部署变得简便：内嵌Tomcat、Jetty等Web容器
- 使监控变得容易：自带项目监控

### Spring Boot的主要特性

- 约定优于配置：大多数情况直接使用默认配置即可，只需要很少的配置。
- 独立运行的Spring应用：可以jar包的形式独立运行，使用java -jar命令或执行main方法
- 内嵌Web容器：内嵌Servlet容器，可内嵌Tomcat、Jetty，无须以war包形式部署应用
- 提供starter简化Maven配置：提供了一系列starter pom简化Maven的依赖加载，基本可以做到自动化配置、高度封装、开箱即用
- 自动配置Spring：Spring Boot根据项目依赖，自动配置Spring框架，极大减少项目配置
- 提供准生产的应用监控：提供基于HTTP、SSH、TELNET对运行的项目进行跟踪监控
- 无代码生成和XML配置：Spring Boot不是借助代码生成来实现的，而是通过条件注解来实现的。提倡使用Java配置和注解配置相结合的配置方式，方便快捷。

## 第一个Spring Boot应用

### Maven简介

​       Apache Maven是一个软件项目管理工具。基于项目对象模型（Project Object Model，POM）的理念，通过一段核心描述信息来管理项目构建、报告和文档信息。在Java项目中，Maven主要完成两件工作：

- 统一开发规范与工具
- 统一管理jar包

​        Maven统一管理项目开发过程所需要的jar包，但这些jar包将不再包含在项目内（即不在lib目录下），而是存放在仓库中。

### Maven的pom.xml

​       Maven是基于项目对象模型的理念来管理项目的，所有的Maven项目都有一个pom.xml配置文件来管理项目的依赖以及项目的编译等功能。

（1）properties元素

​       在< properties >< /properties >之间可以定义变量，以便在< dependency >< /dependency >中引用。

（2）dependencies元素

​       < dependencies >< /dependencies >，包含多个项目依赖需要使用的< dependency >< /dependency >元素。

（3）dependency元素

​       < dependency >< /dependency >元素内部通过< groupId >< /groupId >、< artifactId >< /artifactId >、< version >< /version >3个子元素确定唯一的依赖，也可以称为3个坐标。

### 在Eclipse中创建Maven Web项目

### Maven手工构建第一个Spring Boot应用

## Spring Boot快速构建

### https://start.spring.io/

### Spring Tool Suite

​       下载网址：https://spring.io/tools

# 第4章 Spring Boot 核心

​       在Spring Boot产生之前，Spring项目会有很多个配置文件，例如web.xml、application.xml，应用程序自身也需要多个配置文件，同时需要编写程序读取这些配置文件。现在Spring Boot简化了Spring项目配置的管理和读取，仅需要一个application.properties文件，并提供了多种读取配置文件的方式。

## Spring Boot 的基本配置

### 启动类和核心注解@SpringBootApplication

​       Spring Boot应用通常都有一个名为 *Application的程序入口类，该入口类需要使用Spring Boot的核心注解@SpringBootApplication标注为应用的启动类，该入口类有一个标准的Java main方法，在main方法中通过“`SpringApplication.run(*Application.class,args);`”启动Spring Boot应用。

​       Spring Boot 的核心注解@SpringBootApplication是一个组合注解，主要组合了@SpringBootConfiguration、@EnableAutoConfiguration和@ComponentScan注解。

- @SpringBootConfiguration

​       @SpringBootConfiguration是Spring Boot应用的配置注解，也是一个组合注解。在Spring Boot应用中推荐使用@SpringBootConfiguration代替@Configuration。

- @EnableAutoConfiguration

​       @EnableAutoConfiguration注解可以让Spring Boot根据当前应用项目所依赖的jar包自动配置项目的相关配置。

- @ComponentScan

​       该注解的功能是让Spring Boot自动扫描@SpringBootApplication所在类的同级包以及它的子包中的配置。

### 关闭某个特定的自动配置

​       使用@SpringBootApplication注解的exclude参数关闭特定的自动配置。以关闭neo4j自动配置为例，代码如下：

​      `@SpringBootApplication(exclude={Neo4jDataAutoConfiguration.class})`

### 定制 Banner

​       打开网页http://patorjk.com/software/taag，输入自定义字符串，生成后复制到/src/main/resources目录下的banner.txt文件中。

### 关闭 Banner

​       在/src/main/resources目录下的application.properties文件中添加如下配置：

​       `spring.main.banner-mode = off`

### Spring Boot 的全局配置文件

​       /src/main/resources目录下的application.properties即为Spring Boot的全局配置文件，可以修改或者配置多个参数，具体可以参考官方说明文档。

- 设置端口号：`server.port = 8888`
- 设置Web应用的上下文路径：`server.servlet.context-path = /xxx`

### Spring Boot 的 Starters

​       例如：

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 读取应用配置

​       Spring Boot提供了3种方式读取项目的application.properties配置文件的内容，这3种方式分别为Environment类、@Value注解以及@ConfigurationProperties注解。

### Environment

​       Environment是一个通用的读取应用程序运行时的环境变量的类，可以通过key-value的方式读取application.properties、命令行输入参数、系统属性、操作系统环境变量等。

### @Value

`@Value("${test.msg}")`     //test.msg为配置文件application.properties中的key

`private String msg;`          //通过@Value注解将配置文件中的key对应的value赋值给msg

### @ConfigurationProperties

​       使用@ConfigurationProperties首先建立配置文件与对象的映射关系，然后在控制器方法中使用@Autowired注解将对象注入。

### @PropertySource

​       开发者希望读取项目的其他配置文件，而不是全局配置文件application.properties，那么可以使用@PropertySource注解找到项目的其他配置文件，然后结合前3种方法任意一种读取即可。

## 日志配置

​       默认情况下，Spring Boot 项目使用LogBack实现日志，使用apache Commons Logging作为日志接口。

​       日志级别有ERROR、WARN、INFO、DEBUG和TRACE。Spring Boot默认的日志级别为INFO，日志信息可以打印到控制台。

- 可以设定日志的输出级别
- 可以指定日志输出到文件
- 可以指定日志输出文件的目录
- 指定日志输出和文件输出的格式

## Spring Boot 的自动配置原理

## Spring Boot 的条件注解

### 条件注解

​       所谓Spring Boot的条件注解，就是应用程序的配置类的配置项，在满足某些特定条件后才会被自动启用。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv3hwt3ax8j612x0cdmyc02.jpg)

### 自定义条件

​       Spring的@Conditional注解根据满足某特定条件创建一个特定的Bean。

### 自定义 Starters

# 第5章 Spring Boot 的 Web 开发

​        Web开发是一种基于B/S架构（浏览器/服务器）的应用软件开发技术，分为前端（用户接口）和后端（业务逻辑和数据），前端的可视化及用户交互由浏览器实现，即以浏览器作为客户端，实现客户与服务器远程的数据交互。Spring Boot的Web开发内容主要包括内嵌Servlet容器和Spring MVC。

## Spring Boot 的 Web 开发支持

## Thymeleaf模版引擎

## Spring Boot 处理 JSON 数据

​       在Spring Boot的Web应用中，内置了JSON数据的解析功能，默认使用Jackson自动完成解析（不需要加载Jackson依赖包）。当控制器返回一个Java对象或者集合数据时，Spring Boot自动将其转换成JSON数据，使用起来非常方便简洁。

​       Spring Boot处理JSON数据时，需要用到两个重要的JSON格式转换注解，分别是@RequestBody和@ResponseBody。

- @RequestBody：用于将请求体中的数据绑定到方法的形参上，该注解应用在方法的形参上。
- @ResponseBody：用于直接返回JSON对象，该注解应用在方法上。

## Spring Boot文件上传和下载

​       文件上传和下载是Web应用开发中常用的功能之一。在实际的Web应用开发中，为了成功上传文件，必须将表单的method设置为post，并将enctype设置为multipart/form-data。只有这样设置，浏览器才能将所选文件的二进制数据发送给服务器。

​       从Servlet3.0开始，就提供了处理文件上传的方法，但这种文件上传需要在Java Servlet中完成，而Spring MVC提供了更简单的封装。Spring MVC是通过Apache Commons FileUpload技术实现一个MultipartResolver的实现类CommonsMultipartResolver完成文件的上传。因此，Spring MVC的文件上传需要依赖Apache Commons FileUpload组件。

​       Apache Commons FileUpload将上传文件自动绑定到MultipartFile对象中，MultipartFile提供了获取上传文件内容、文件名等方法，并通过transferTo方法将文件上传到服务器的磁盘中。MultipartFile的常用方法如下：

- `byte[] getBytes()`:获取文件数据
- `String getContentType()`:获取文件MIME类型，如image/jpeg等
- `InputStream getInputStream()`:获取文件流
- `String getName()`:获取表单中文件组件的名字
- `String getOriginalFilename()`:获取上传文件的原名
- `long getSize()`:获取文件的字节大小，单位为b（byte）
- `boolean isEmpty()`:是否有（选择）上传文件
- `void transferTo(File dest)`:将上传文件保存到一个目标文件中

​       Spring Boot的spring-boot-starter-web已经集成了Spring MVC，所以使用Spring Boot上传文件更加便捷，只需引入Apache Commons FileUpload组件依赖即可。

## Spring Boot 的异常统一处理

​       在Spring Boot应用的开发中，不管是对底层数据库操作，对业务层操作，还是对控制层操作，都会不可避免地遇到各种可预知的、不可预知的异常需要处理。如果每个过程都单独处理异常，那么系统的代码耦合度高、工作量大且不好统一，以后维护的工作量也大。

​       如果能将所有类型的异常处理从各层中解耦出来，则既保证了相关处理过程的功能较单一，也实现了异常信息的统一处理和维护。幸运的是，Spring Boot框架支持这样的实现。

### 自定义error页面

​       在 Spring Boot Web应用的src/main/resources/templates目录下添加error.html页面，访问发生错误或异常时，Spring Boot将自动找到该页面作为错误页面。Spring Boot为错误页面提供了以下属性：

- timestamp：错误发生时间
- status：HTTP状态码
- error：错误原因
- exception：异常的类名
- message：异常消息（如果这个错误是由异常引起的）
- errors：BindingResult异常里的各种错误（如果这个错误是由异常引起的）
- trace：异常跟踪信息（如果这个错误是由异常引起的）
- path：错误发生时请求的URL路径

### @ExceptionHandler 注解

​       使用自定义error页面并没有真正处理异常，我们可以使用@ExceptionHandler注解处理异常。如果在Controller中有一个使用@ExceptionHandler注解修饰的方法，那么当Controller的任何方法抛出异常时，都会由该方法处理异常。

​       而一个Spring Boot应用往往存在多个控制器，不太适合在每个控制器里添加使用@ExceptionHandler注解修饰的方法进行异常处理。可以将使用@ExceptionHandler注解修饰的方法放到一个父类中，然后所有需要处理异常的控制器集成该类即可。

### @ControllerAdvice 注解

​       使用上述父类Controller进行异常处理，也有其自身的缺点，那就是代码耦合性太高。我们可以使用@ControllerAdvice 注解降低这种父子耦合关系。

​       @ControllerAdvice 注解，顾名思义，是一个增强的Controller，使用该Controller，可以实现3个方面的功能：

- 全局异常处理
- 全局数据绑定
- 全局数据预处理

​       使用@ControllerAdvice注解的类是当前Spring Boot应用中所有类的统一异常处理类，该类中使用@ExceptionHandler注解的方法统一处理异常，不需要在每个Controller中逐一定义异常处理方法，这是因为对所有注解了@RequestMapping的控制器有效。

## Spring Boot 对 JSP 的支持

# 第6章 Spring Boot 的数据访问

​       Spring Data 是Spring访问数据库的一揽子解决方法，是一个伞形项目，包含大量关系型数据库及非关系型数据库的数据访问解决方案。

## Spring Data JPA

​       Spring Data JPA 是Spring Data的子项目，首先需要了解一下Hibernate，因为Spring Data JPA是由Hibernate默认实现的。

- Hibernate：是一个开源的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，它将POJO（Plain Ordinary Java Object）简单的Java对象与数据库表建立映射关系，是一个全自动的ORM（Object Relational Mapping）框架。Hibernate可以自动生成SQL语句、自动执行，使得Java开发人员可以随心所欲地使用对象编程思维来操纵数据库。
- JPA：（Java Persistence API）是官方提出来的Java持久化规范，通过注解或XML描述对象—关系（表）的映射关系，并将内存中的实体对象持久化到数据库。

​       Spring Data JPA通过提供基于JPA的Repository极大地简化了JPA的写法，在几乎不写实现的情况下，实现数据库的访问和操作。使用Spring Data JPA建立数据访问层十分方便，只需要定义一个继承JpaRepository接口的接口即可。

​       继承了JpaRepository接口的自定义数据访问接口，具有JpaRepository接口的所有数据访问操作方法。JpaRepository接口提供的常用方法如下：

- `List<T>findAll()`:查询所有实体对象数据，返回一个List集合
- `List<T>findAll(Sort sort)`:按照指定的排序规则查询所有实体对象数据，返回一个List集合
- `List<T>findAllById(Iterable<ID>ids)`:根据所提供的实体对象id（多个），将对应的实体全部查询出来，返回一个List集合
- `<S extends T> List <S> saveAll(Iterable <S> entities)`:将提供的集合中的实体对象数据保存到数据库
- `void flush()`:将缓存的对象数据操作更新到数据库
- `<S extends T> S saveAndFlush(S entity) :`保存对象的同时立即更新到数据库
- `void deleteInBatch(Iterable<T> entities)`:批量删除提供的实体对象
- `void deleteAllInBatch()`:批量删除所有实体对象
- `T getOne(ID id)`:根据id获得对应的实体对象
- `<S extends T> List <S> findAll(Example <S> example)`:根据提供的example实例查询实体对象数据
- `<S extends T> List <S> findAll(Example <S> example,Sort sort)`:根据提供的example实例，并按照指定规则，查询实体对象数据

### Spring Boot 的支持

​       创建项目的时候需要选择Spring Data JPA 模块依赖。

### 简单条件查询

​       只需要定义一个继承JpaRepository接口的接口即可使用Spring Data JPA建立数据访问层。因此，自定义的数据访问接口完全继承了JpaRepository的接口方法。**但更重要的是**，在自定义的数据访问接口中可以根据查询关键字定义查询方法，这些查询方法需要符合它的命名规则，一般是根据持久化实体类的属性名来确定的。 

1、查询关键字

​       目前，Spring Data JPA 支持的查询关键字如下图所示：

![](https://tva1.sinaimg.cn/large/008i3skNly1gv4hnagw0oj611g0px0uq02.jpg)

2、限制查询结果数量

​       在Spring Data JPA中，使用Top和First关键字限制查询结果数量。

### 关联查询

​       在Spring Data JPA中，有一对一、一对多、多对多等关系映射。

1、@OneToOne

​       在Spring Data JPA中，可以用两种方式描述一对一关系映射：

- 通过外键的方式：一个实体通过外键关联到另一个实体的主键
- 通过一张关联表来保存两个实体一对一的关系

​       @OneToOne注解有5个属性：targetEntity、cascade、fetch、optional和mappedBy。

- targetEntity属性：class类型属性。定义关系类的类型，默认是该成员属性对应的类类型，所以通常不需要提供定义。
- cascade属性：CascadeType[ ]类型。该属性定义类与类之间的级联关系。定义的级联关系将被容器视为对当前类对象及其关联类对象采取相同的操作，而且这种关系是递归调用的。cascade的值只能从`CascadeType.PERSIST`（级联新建）、`CascadeType.REMOVE`（级联删除）、`CascadeType.REFRESH`（级联刷新）、`CascadeType.MERGE`（级联更新）中选择一个或多个。还有一个选择是使用`CascadeType.ALL`表示选择全部4项。
- fetch属性：分为两种。`FetchType.LAZY`：懒加载，加载一个实体时，定义懒加载的属性不会立即从数据库中加载。`FetchType.EAGER`：急加载，加载一个实体时，定义急加载的属性会立即从数据库中加载。
- optional属性：optional = true，表示idCard属性可以设置为null，也就是允许没有身份证。
- mappedBy属性：mappedBy标签一定是定义在关系的被维护端，它指向关系的维护端；只有@OneToOne、@OneToMany、@ManyToMany才有mappedBy属性，@ManyToOne不存在该属性。拥有mappedBy注解的实体类为关系的被维护端。

2、@OneToMany和@ManyToOne

​       在JPA规范中，一对多的双向关系由**多端**（如Article）来维护。就是说多端为关系的维护端，负责关系的增删改查；一端则为关系的被维护端，不能维护关系。

​       一端（Author）使用@OneToMany注解的mappedBy=“author”属性表明一端（Author）是关系的被维护端。多端（Article）使用@ManyToOne和@JoinColumn来注解属性author，@ManyToOne表明Article是多端，@JoinColumn设置在article表的关联字段（外键）上。

3、@ManyToMany

​       在Spring Data JPA中，使用@ManyToMany来注解多对多的映射关系，由一个关联表来维护。关联表的表名默认为：主表名+下划线+从表名（主表是指关系维护端对应的表，从表是指关系被维护端对应的表）。关联表只有两个外键字段，分别指向主表ID和从表ID。字段的名称默认为：主表名+下划线+主表中的主键列名，从表名+下划线+从表中的主键列名。需要注意的是**多对多关系中一般不设置级联保存、级联删除、级联更新等操作**。

### @Query和@Modifying注解

1、@Query

​       使用@Query注解可以将JPQL语句直接定义在数据访问接口方法上，并且接口方法名不受查询关键字和关联查询命名规范约束。

2、@Modifying

​       可以使用@Modifying和@Query注解组合定义在数据访问接口方法上，进行更新查询操作。

### 排序和分页查询

​       在实际应用开发中，排序和分页查询是必须的。幸运的是，Spring Data JPA为我们提供了Sort类、Page接口以及Pageable接口。

## Spring Boot 使用 JdbcTemplate

​       JDBC模版（JdbcTemplate）是 Spring 对数据库的操作在JDBC基础上做了封装，建立了一个JDBC存取框架。在Spring Boot应用中，如果使用了JdbcTemplate操作数据库，那么只需要在pom.xml中添加spring-boot-starter-jdbc模块，即可通过@Autowired注解依赖注入JdbcTemplate对象，然后调用JdbcTemplate提供的方法操作数据库。

## Spring Boot 整合 MyBatis

​       MyBatis是一个基于Java的持久层框架。MyBatis提供的持久层框架包括SQL Maps和Data Access Objects（DAO），它消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plain Old Java Objects，普通的java对象）映射成数据库中的记录。

## Spring Boot 的事务管理

### Spring Data JPA 的事务支持

​       Spring Data JPA 对所有的默认接口方法都开启了事务支持，并且对查询类事务默认启用了readOnly。

### Spring Boot 的事务支持

​       在Spring Boot中，自动配置了事务管理器，并自动开启了注解事务的支持。

1、自动配置的事务管理器

​       在使用JDBC访问数据库时，Spring Boot定义了DataSourceTransactionManager的Bean。

​       在使用JPA访问数据库时，Spring Boot定义了PlatformTransactionManager的实现JpaTransactionManager的Bean。

2、自动开启注解事务的支持

​       Spring Boot使用org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration类配置事务的支持，并在该类中自动开启注解事务的支持。

​       不过自动启用该注解有两个前提条件：

- @ConditionalOnBean（PlatformTransactionManager.class）
- @ConditionalOnMissingBean（AbstractTransactionManagementConfiguration.class）

​       而一般情况下，这两个条件都是满足的，所以在启动类上写不写@EnableTransactionManagement注解都可以开启事务支持。

## REST

### REST简介

​       REST（Representational State Transfer，表现层状态转换）是一种软件架构风格，是一种针对网络应用的设计和开发方式，可以降低开发的复杂性，提高系统的可伸缩性。目前在3种主流的Web服务实现方案中，因为REST模式的Web服务与复杂的SOAP和XML-RPC对比来讲更加简洁，所以越来越多的Web服务开始采用REST风格设计和实现。

​       REST是一组架构约束条件和原则，这些约束有：

- 使用客户/服务器模式：客户和服务器之间通过一个统一的接口来互相通信。
- 层次化的系统：在一个REST系统中，客户端并不会固定地与一个服务器打交道。
- 无状态：在一个REST系统中，服务端并不会保存有关客户的任何状态。也就是说，客户端自身负责用户状态的维持，并在每次发送请求时都需要提供足够的信息。
- 可缓存：REST系统需要恰当地缓存请求，以尽量减少服务端和客户端之间的信息传输，以提高性能。
- 统一的接口：一个REST系统需要使用一个统一的接口来完成子系统之间以及服务与用户之间的交互。这使得REST系统中的各个子系统可以独自完成演化。

​       满足这些约束条件和原则的应用程序或设计就是RESTful。**RESTful是设计风格而不是标准**，REST通常基于HTTP、URI、XML以及HTML这些现有的广泛流行的协议和标准。

​       理解RESTful架构，应该先理解Representational State Transfer这个词组到底是什么意思，它的每一个词表达了什么含义。

- 资源（Resources）

​       “表现层状态转化”中的“表现层”其实指的是“资源”的“表现层”。

​       “资源”就是网络上的一个实体，或者说是网络上的一个具体信息。“资源”可以是一段文本、一张图片、一段视频、总之就是一个具体的实体。可以使用一个URI（统一资源定位符）指向资源，每种资源对应一个特定的URI。我们需要获取资源时，访问它的URI即可，因此URI是每个资源的地址或独一无二的标识符。REST风格的Web服务，是通过一个简洁清晰的URI来提供资源链接，客户端通过对URI发送HTTP请求获得这些资源，而获取和处理资源的过程让客户端应用的状态发生改变。

- 表现层（Representation）

​       “资源”是一种信息实体，可以有多种外在的表现形式。将“资源”呈现出来的形式称为它的“表现层”。例如，文本可以使用txt格式表现，也可以使用XML或JSON格式表现。

- 状态转化（State Transfer）

​       客户端访问一个网站，就代表了它和服务器的一个互动过程。在这个互动过程中，将涉及到数据和状态的变化。我们知道HTTP协议是一个无状态的通信协议，这意味着所有状态都保存在服务器端。因此，如果客户端操作服务器，需要通过某种手段（如HTTP协议）让服务器端发生“状态变化”。而这种状态是建立在表现层之上的，所以就是“表现层状态转化”。

​       在流行的各种Web框架中，包括Spring Boot都支持REST开发。REST并不是一种技术或者规范，而是一种架构风格，包括如何标识资源，如何标识操作接口及操作的版本、如何标识操作的结果等，主要内容如下：

**1、使用“api”作为上下文**

​       在REST架构中，建议使用“api”作为上下文，例如：`http://localhost:8080/api`。

**2、增加一个版本标识**

​       在REST架构中，可以通过URL标识版本信息，例如：`http://localhost:8080/api/v1.0`。

**3、标识资源**

​       在REST架构中，可以将资源名称放到URL中，例如：`http://localhost:8080/api/v1.0/user`。

**4、确定HTTP Method**

​       HTTP协议有5个常用的表示操作方式的动词：GET、POST、PUT、DELETE和PATCH，它们分别对应种基本操作：

​       GET用来获取资源；

​       POST用来增加资源（也可以用于更新资源）；

​       PUT用来更新资源；

​       DELETE用来删除资源；

​       PATCH用来更新资源的部分属性。

**5、确定 HTTP Status **

​       服务器向用户返回的状态码和提示信息，常用的如下：

（1）200 OK - [ GET ]：服务器成功返回用户请求的数据

（2）201 CREATED - [ POST/PUT/PATCH]：用户新建或修改数据成功

（3）202 Accepted - [ * ]：表示一个请求已经进入后台排队（异步任务）

（4）204 NO CONTENT - [ DELETE ]：用户删除数据成功

（5）**400** INVALID REQUEST - [ POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作

（6）401 Unauthorized - [ * ]：表示用户没有权限（令牌、用户名、密码错误）

（7）403 Forbidden - [ * ]：表示用户得到授权（与401相对），但是访问被禁止

（8）**404** NOT FOUND - [ * ]：用户发出的请求针对的是不存在的记录，服务器没有进行操作

（9）406 Not Acceptable - [ GET ]：用户请求的格式不可得（例如用户请求JSON格式，但只有XML格式）

（10）410 Gone - [ GET ]：用户请求的资源被永久删除，且不会再得到

（11）422 Unprocesable entity - [ POST/PUT/PATCH ]：当创建一个对象时，发生一个验证错误

（12）**500** INTERNAL SERVER ERROR - [ * ]：服务器发生错误，用户将无法判断发出的请求是否成功

### Spring Boot 整合 REST

​       在Spring Boot的Web应用中，自动支持REST。

### Spring Data REST

​       Spring Data JPA基于Spring Data的repository之上，可以将repository自动输出为REST资源。目前，Spring Data REST支持将Spring Data JPA、Spring Data MongoDB、Spring Data Neo4j、Spring Data GemFire以及Spring Data Cassandra 的repository自动转换成REST服务。（只需要引入spring-boot-starter-data-rest的依赖即可）

### REST服务测试

​       在Web和移动端开发时，常常会调用服务器端RESTful接口进行数据请求，为了调试，一般会先用工具进行测试，通过测试后才开始在开发中使用。例：Wisdom REST Client，是用Java语言编写的REST客户端，是GitHub上的开源项目，可以直接下载。

## MongoDB

​       MongoDB是一个基于分布式文件存储的NoSQL数据库，由C++语言编写，旨在为Web应用提供可扩展的高性能数据存储解决方案。

​       MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库中功能最丰富、最像关系数据库的。它支持的数据结构十分松散，是类似JSON的BSON（Binary JSON，二进制JSON）格式，因此可以存储比较复杂的数据类型。Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。

### 安装 MongoDB

### Spring Boot 整合 MongoDB

1、Spring 对 MongoDB 的支持

​       Spring 对 MongoDB 的支持主要通过 Spring Data MongoDB 实现的，Spring Data MongoDB 提供了如下功能：

- 对象/文档映射注解
- MongoTemplate
- Repository

2、Spring Boot 对 MongoDB 的支持

​       Spring Boot 对 MongoDB 可以实现自动配置，主要配置了数据库连接、MongoTemplate，可以在配置文件中使用spring.data.mongodb为前缀的属性来配置MongoDB的相关信息。

​       Spring Boot 对 MongoDB 提供了一些默认属性，例如，默认端口号27017，默认服务器localhost，默认数据库test，默认无用户名无密码的访问，并默认开启了对Repository的支持。（只需引入spring-boot-starter-data-mongodb依赖）

### 增删查改

## Redis

​       Redis是一个开源的使用ANSI C 语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。它支持字符串、哈希表、列表、集合、有序集合、位图、地理空间信息等数据类型，同时也可以作为高速缓存和消息队列代理。但是，Redis在内存中存储数据，因此，存放在Redis中的数据不应该大于内存容量，否则会导致操作系统性能降低。

### 安装 Redis

### Spring Boot 整合 Redis

1、 Spring Data Redis

​       Spring 对 Redis 的支持是通过Spring Data Redis来实现的。Spring Data Redis提供了RedisTemplate 和 StringRedisTemplate两个模版来进行数据操作，其中StringRedisTemplate只针对键值都是字符串类型的数据进行操作。

​       RedisTemplate 和 StringRedisTemplate两个模版提供的主要数据访问方法如下图所示：

![](https://tva1.sinaimg.cn/large/008i3skNly1gv5wkjeu7aj30xe053jro.jpg)

2、Serializer

​       当数据存储到 Redis 时，键和值都是通过Spring提供的Serializer序列化到数据的，RedisTemplate默认使用JdkSerializationRedisSerializer序列化，StringRedisTemplate默认使用StringRedisSerializer序列化。

3、Spring Boot 的支持

​       在RedisAutoConfiguration配置类中，默认配置了RedisTemplate 和 StringRedisTemplate，可以直接使用Redis存储数据。（只需要引入spring-boot-starter-data-redis依赖）

### 使用StringRedisTemplate和RedisTemplate

## 数据缓存Cache

​       我们知道内存的读取速度远远大于硬盘的读取速度。当需要重复地获取相同数据时，一次一次地请求数据库或者远程服务，导致在数据库查询或者远程方法调用上消耗大量的时间，最终导致程序性能降低，这就是数据缓存要解决的问题。

​       Spring Boot中，Spring Cache 对 Cache进行了抽象，提供了CacheManager和Cache接口，并提供了@Cacheable、@CachePut、@CacheEvict、@Caching、@CacheConfig等注解。Spring Boot应用基于Spring Cache，即提供了基于内存实现的缓存管理器用于单体应用系统，也集成了Redis、EhCache等缓存服务器用于大型系统或分布式系统。

### Spring 缓存支持

​       Spring框架定义了org.springframework.cache.CacheManager和org.springframework.cache.Cache接口来统一不同的缓存技术。针对不同的缓存技术，需要实现不同的CacheManager。例如，使用EhCache作为缓存技术时，需要注册实现CacheManager的Bean。

![](https://tva1.sinaimg.cn/large/008i3skNly1gv6gkwqo95j61av0ay75r02.jpg)

​       一旦配置好Spring缓存支持，就可以在Spring容器管理的Bean中使用缓存注解（基于AOP原理），一般情况下，都是在业务层（Service类）使用这些注解。

- **@Cacheable**

​       @Cacheable可以标记在一个方法上，也可以标记在一个类上。当标记在一个方法上时，表示该方法是支持缓存的；当标记在一个类上时，则表示该类所有的方法都是支持缓存的。对于一个支持缓存的方法，在方法执行前，Spring先检查缓存中是否存在方法返回的数据，如果存在，则直接返回缓存数据；如果不存在，则调用方法并将方法返回值存入缓存。       

​       @Cacheable注解经常使用 value、key、condition等属性：

​       （1）value：缓存的名称，指定一个或多个缓存名称。如@Cacheable(value="mycache")或者@Cacheable(value={" cache1","cache2"})。该属性与cacheNames属性意义相同。

​       （2）key：缓存的key，可以为空。如果指定，需要按照SpEL表达式编写；如果不指定，则默认按照方法的所有参数进行组合。如@Cacheable(value=" testcache"，key="#student.id")。

​       （3）condition：缓存的条件，可以为空。如果指定，需要按照SpEL编写，返回true或者false，只有为true才进行缓存。如@Cacheable(value=" testcache", condition="#student.id>2")。该属性与unless相反，条件成立时，不进行缓存。

- **@CacheEvict**

​       @CacheEvict是用来标注在需要清除缓存元素的方法或类上的。当标记在一个类上时，表示其中所有方法的执行都会触发缓存的清除操作。@CacheEvict可以指定的属性有value、key、 condition、allEntries和beforelnvocation。其中，value、key和condition的语义与@Cacheable对应的属性类似。

​       （1）allEntries：是否清空所有缓存内容，默认为false，如果指定为true，则方法调用后将立即清空所有缓存。如@ CacheEvict(value="testcache", allEntries=true)。

​       （2）beforeInvocation：是否在方法执行前就清空，默认为false，如果指定为true，则在方法还没有执行时就清空缓存。默认情况下，如果方法执行抛出异常，则不会清空缓存。

- **@CachePut**

​       @CachePut也可以声明一个方法支持缓存功能，与@Cacheable不同的是使用@CachePut标注的方法在执行前不会去检查缓存中是否存在之前执行过的结果，而是每次都会执行该方法，并将执行结果以键值对的形式存入指定的缓存中。

​       @CachePut也可以标注在类上和方法上。@CachePut的属性与@Cacheable的属性一样。

- **@Caching**

​       @Caching注解可以在一个方法或者类上同时指定多个 Spring Cache 相关的注解。其拥有3个属性： cacheable、put和evict，分别用于指定@Cacheable、@CachePut和@CacheEvict。

- **@CacheConfig**

​       所有的Cache注解都需要提供Cache名称，如果每个Service方法上都包含相同的Cache名称，可能写起来重复。此时，可以使用@CacheConfig注解作用在类上，设置当前缓存的一些公共设置。

### Spring Boot 缓存支持

​       在Spring中使用缓存技术的关键时配置缓存管理器CacheManager，而Spring Boot为我们自动配置了多个CacheManager的实现。在Spring Boot应用中，使用缓存技术只需引入相关的缓存技术依赖，并在配置类中使用@Enable Caching注解开启缓存支持即可。

### 使用 Redis Cache

​       在 Spring Boot 中使用 Redis Cache ，只需要添加spring-boot-starter-data-redis依赖即可。

 