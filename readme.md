[toc]


  
  
  
---

# 0. 综述
![前提知识图](https://raw.githubusercontent.com/LaneRuan/SSM/master/%E5%89%8D%E6%8F%90%E7%9F%A5%E8%AF%86.jpg)

相关链接：  
1. [Spring教程](http://www.yiibai.com/spring/spring-tutorial-for-beginners.html)
2. [SSM框架整合](http://www.cnblogs.com/zyw-205520/p/4771253.html)  
3. [Maven教程](http://www.yiibai.com/maven/)  
4. [mybatis-generator使用方法](http://blog.csdn.net/qr719169236/article/details/51086997)
5. 各种官方文档



---

# 1. 什么是Maven 
> ==Maven 是专门用于构建和管理Java相关项目的工具==。

## 1.1 Maven 主要用处一：相同的项目结构  
使用Maven管理的Java 项目都有着相同的项目结构
1. 有一个pom.xml 用于维护当前项目都用了哪些jar包

2. 所有的java代码都放在 src/main/java 下面

3. 所有的测试代码都放在 test/main/java 下面

![项目结构图](https://raw.githubusercontent.com/LaneRuan/SSM/master/%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84%E5%9B%BE.png)


## 1.2 Maven 主要用处二：统一维护jar包 
比如说有3个Java 项目，这些项目都不是maven风格。那么这3个项目，就会各自维护一套jar包。 而其中有些jar包是相同的。

而maven风格的项目，首先把所有的jar包都放在"仓库“ 里，然后哪个项目需要用到这个jar包，只需要给出jar包的名称和版本号就行了。 这样jar包就实现了共享。

如图所示，在pom.xml里，表示用到了mysql 的jar包，版本号是5.1.29，等等。  

![pom](https://raw.githubusercontent.com/LaneRuan/SSM/master/pom.xml.png)

## 1.3 Maven的安装与配置

eclipse for javaEE版本自带maven插件，可以直接使用。  
但是对本地的仓库进行管理时需要自己安装maven，（安装方法与Tomcat类似）。

1. 将maven压缩包解压，并将maven文件夹放在C盘根目录下（或者其他地方，以此为例）

2.	右键“计算机”，选择“属性”，之后点击“高级系统设置”，点击“环境变量”，来设置环境变量，有以下系统变量需要配置，配置完成后，在cmd上运行mvn –version,若成功显示信息则成功。  
新建系统变量  MAVEN_HOME  变量值：C:\maven\apache-maven-3.3.9  
编辑系统变量  Path         添加变量值： ;%MAVEN_HOME%\bin  
![maven1](https://raw.githubusercontent.com/LaneRuan/SSM/master/maven1.png)  
![maven2](https://raw.githubusercontent.com/LaneRuan/SSM/master/maven2.png)


---

# 2. SSM框架简介

## 2.1 基本概念
1. **Spring**   
        Spring是一个开源框架，Spring是于2003 年兴起的一个轻量级的Java 开发框架，它是为了解决企业应用开发的复杂性而创建的。Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。  
        然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。   
        简单来说，Spring是一个轻量级的==控制反转（IoC==）和==面向切面（AOP）的容器框架==。
        ![Spring框架结构](http://www.yiibai.com/uploads/tutorial/20160117/1-16011F95Uc02.png)
 

2. **Spring MVC**     
        Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。==主要作用是管理Web应用页面流程。==  
        Spring MVC 分离了==控制器==、==模型对象==、==分派器==以及==处理程序对象==的角色，这种分离让它们更容易进行定制。

 

3. **MyBatis**  
        MyBatis 本是Apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis。  
        MyBatis是一个基于Java的持久层框架，也是个ORM（Object Relational Mapping 对象关系映射框架）。  
        Mybatis提供的持久层框架包括SQL Maps和Data Access Objects（==DAO==）  
 

---

## 2.2 Spring


### 2.2.1. IoC( Inversion of Control反转控制) 和DI(Dependency Injection依赖注入)
- ==控制反转==：即依赖关系的获取方式反过来了，何为依赖，从程序的角度看，就是比如A要调用B的方法，那么A就依赖于B。如果不反转，意味着A要主动获取B，才能使用B；控制反转就是A要调用B的话，A并不需要主动获取B，而是由其它人（Spring容器）（所以需要依赖注入）。  
    
- ==依赖注入==：对象的依赖关系将由系统中负责协调各对象的第三方组件（IoC容器）在创建对象的时候进行设定。对象无需自行创建或管理它们的依赖关系。
    
- ==IoC容器==：基于Spring的应用中，应用对象生存于Spring容器中，负责创建对象，装配它们，配置并管理它们的整个生存周期，从生存到死亡。
    
### 2.2.2. AOP(Aspect-Oriented Programming 面向切面编程)  
**在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程**。 
> ==AOP==把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如==权限认证、日志、事物==。==AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性==。  
>Spring中AOP代理由IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理。 

### 2.2.3. 注解 Spring Annotation 
传统的Spring做法是使用.xml文件来对bean进行注入或者是配置aop、事物，这么做有两个缺点：  
1. 如果所有的内容都配置在.xml文件中，那么.xml文件将会十分庞大；如果按需求分开.xml文件，那么.xml文件又会非常多。总之这将导致配置文件的可读性与可维护性变得很低。  
2. 在开发中在.java文件和.xml文件之间不断切换，是一件麻烦的事，同时这种思维上的不连贯也会降低开发的效率。  
为了解决这两个问题，Spring引入了注解，通过"@XXX"的方式，让注解与Java Bean紧密结合，既大大减少了配置文件的体积，又增加了Java Bean的可读性与内聚性。  
常用的注解：==@Autowired @Resource @Service @Controller==


---

## 2.3 Spring MVC
### 2.3.1 MVC设计模式
**MVC 设计模型是一种使用 Model View Controller（ 模型-视图-控制器）设计创建 Web 应用程序的模式**  最典型的：JSP + servlet + javabean（已过时）
- Model（模型）：是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据。
- View（视图）：是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的。
- Controller（控制器）：是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。

### 2.3.2 Spring MVC的优点
- 进行更简洁的Web层的开发；
- 天生与Spring框架集成（如IoC容器、AOP等）；
- 提供强大的约定大于配置的契约式编程支持；  
- 能简单的进行Web层的单元测试；  
- 支持灵活的URL到页面控制器的映射； 
- 非常容易与其他视图技术集成，如React前端框架 因为模型数据不放在特定的API里，而是放在一个Model里（Map数据结构实现，因此很容易被其他框架使用）  
- 非常灵活的数据验证、格式化和数据绑定机制，能使用任何对象进行数据绑定，不必实现特定框架的API；  
- 支持灵活的本地化、主题等解析；  
- 更加简单的异常处理；  
- 对静态资源的支持。 

### 2.3.3 Spring MVC操作流程
![流程视图](http://sishuok.com/forum/upload/2012/7/14/529024df9d2b0d1e62d8054a86d866c9__1.JPG)

---

## 2.4 Mybatis
### 2.4.1 Mabatis简介
MyBatis 是支持定制化 SQL、存储过程以及高级映射的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手工设置参数以及抽取结果集。MyBatis 使用简单的 XML 或注解来配置和映射基本体，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。

### 2.4.2 Mabatis Generator
利用Mabatis Generator自动生成代码可以大大降低工作量，有三种用法：命令行、eclipse插件、maven插件。[http://www.mybatis.org/generator/](http://www.mybatis.org/generator/)

### 2.4.3 Mybatis执行流程
![image](http://img.blog.csdn.net/20150427151555111?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaml1cWl5dWxpYW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 2.4.4 Mybatis和Hibernate的对比
- Mybatis的优势
    - MyBatis可以进行更为细致的SQL优化，可以减少查询字段。 
    - MyBatis容易掌握，而Hibernate门槛较高。
    - Hibernate和MyBatis都有相应的代码生成工具。可以生成简单基本的DAO层方法

- Hibernate的优势
    - Hibernate的DAO层开发比MyBatis简单，Mybatis需要维护SQL和结果映射。（针对高级查询，Mybatis需要手动编写SQL语句，以及ResultMap。而Hibernate有良好的映射机制，开发者无需关心SQL的生成与结果映射，可以更专注于业务流程。）
    - Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
    - Hibernate数据库移植性很好，MyBatis的数据库移植性不好，不同的数据库需要写不同SQL。
    - Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。

---

# 3. 用eclipse导入maven SSM项目
环境需求
1. Jdk 1.8   
2. Eclipse for javaEE
3. Mysql数据库
4. Tomcat

