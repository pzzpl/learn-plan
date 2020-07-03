# spring boot 学习笔记

（2020-7-3）本笔记写于springboot一些基础知识学习完之后，根据掌握的程度写的总结。

-----



## 目录

[TOC]



1. [springboot背景知识](1.springboot背景)
2. [springboot的场景stater]()
3. <a href="#jump">创建springboot application </a>
4. [全局配置文件yaml\properties]()
5. [配置第三方组件]()
6. [Profile的使用]()
7. [※自动配置原理]()
8. [日志]()
9. [springboot中的web开发]()
10. [关于内置web容器]()
11. [整合持久层框架、数据库]()
12. [springboot与redis]()
13. [springboot与消息队列]()
14. [springboot与zookeeper、dubbo]()
15. [spring security 、shrio]()
16. [docker]()





## 1.springboot背景

以往使用spring框架开发，配置文件都要从头开始一步一步配置，效率太慢。于是有人把所有可能用到的技术栈需要的配置，都用类配置方式根据条件自动配置。



## 2.场景stater

因为springboot的开发团队把所有可能用到的技术栈的配置都预先给你配置了，但是我们开发的时候不是全部配置都用到。所以就有一个个场景，譬如我们要开发用到ssm+redis+mysql+themyleaf，就可以使用对应的starter。这些stater包含一些自动配置类，譬如springMVC就会被WebAutoConfiguration配置类自动配置（其实就是把一些组件加入到容器中）。



## 3.创建springboot项目

使用项目构建工具maven或者gradle

### 使用maven

1. **在pom中配置父项目(父项目列举了所有可能用到的依赖及其版本和一些插件)**

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>

点进去父项可以看到其父项目管理所有dependency
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.1.RELEASE</version>
 </parent>
```

2. **导入场景stater**

```xml
<dependency>    
    <groupId>org.springframework.boot</groupId>    
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

3.**创建springboot入口类**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringDemo {
    public static void main(String[] args) {
        SpringApplication.run(SpringDemo.class);
    }
}
```

然后启动运行main方法就可以运行springboot应用。

可以看到web场景下，内置了tomcat在8080端口。

![](E:\gitrepo\learn-plan\content\springboot\images\微信截图_20200703102959.png)

### 使用gradle

to be continued...

### 使用spring initializer快速创建springboot项目

IDEA或者spring版eclipse都有 spring initializer。通过这个工具可以快速选定项目所需要的场景。

![](E:\gitrepo\learn-plan\content\springboot\images\微信截图_20200703104548.png)

![](E:\gitrepo\learn-plan\content\springboot\images\微信截图_20200703104722.png)

各种场景都可以选择。

## 4.自动配置原理

问题：为什么springboot能够通过stater快速帮我们配置呢？它是怎样帮我们配置的。

盲猜：它把所有有可能用到的配置写在xml或者其他文件中，再用读到一个对象中，再加载组件。



### 几个重要spring注解

@Configuration ：被其修饰的类为一个配置类（相当于一个以前spring配置的xml文件），里面可以配置各种bean（组件）



@ComponentScan ： 扫描被@Controller @Service @Repository @Component @Configuration 修饰的类，并且用反射创建对象，放入IOC容器



@Import ： 可以快速IOC中注册 一个组件。实现ImportSelector可以同时注册多个组件。实现ImportBeanDefinitionRegistor的



@Bean



@EnableXXX ：开启某种功能



@Conditional ：根据情况决定是否创建该类的对象，加入IOC

### 自动配置过程

```java
SpringApplication.run(Springbootdemo03Application.class, args);

@SpringBootApplication

```

1. main运行SpringApplication.run(Springbootdemo03Application.class, args);

2. 找到@SpringBootApplication，这个注解。

3. 这个注解是一个复合注解

   ![](E:\gitrepo\learn-plan\content\springboot\images\微信截图_20200703110242.png)

   开启了自动扫描，和自动配置

   --to be continued...

   
