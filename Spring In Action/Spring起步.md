# Spring起步

## 什么是Spring

Spring的核心是 **提供一个** ***容器(container)***,通常称为**Spring应用上下文(Spring application context)**,负责创建和管理应用组件.组件称为**bean**.

Spring依靠**依赖注入(dependency injection,DI)**装配bean.使用Java注解来进行配置,但一般仅用于Spring不能自动配置的情况.

Spring的**自动配置(autoconfiguration)** 起源于**自动装配(autowiring)**和**组件扫描(component scanning)**.

### Spring项目结构

- 应用源码:”src/main/java”.
- 测试代码:”src/main/test”.
- 非Java资源:”src/main/resources”.
- pom.xml: Maven构建规范.
- application.properties: 配置属性.

#### 引导应用

@SpringBootApplication注解,是一个组合注解,组合了

- @SpringBootConfiguration: 将该类声明为配置类.
- @EnableAutoConfiguration: 启用Spring Boot 自动配置.
- @ComponentScan: 启用组件扫描.

其中的main方法调用静态run方法,run方法创建应用上下文,其中传递的两个参数分别是**配置类**和**命令行参数**.

## 编写Spring应用

Spring 构造型注解

- @Repository
- @Service
- @Controller

三者都是@Component的特殊化,分别在持久性,服务和表示层中.

### 处理Web请求

Spring MVC是Spring自带的Web框架,其核心是**控制器(controller)**.是处理请求并以某种方式进行信息响应的类.

## 俯瞰Spring风景线

### Spring核心框架

- Spring MVC,Spring的Web框架,能够编写控制器类以处理Web请求.也能创建REST API.

### Spring Boot

除了starter依赖和自动配置,Spring Boot还提供

- Actuator能洞察应用运行时的内部工作状况,包括指标,线程dump信息,应用健康状况及应用可用环境属性.
- 灵活的环境属性规范.
- 在核心框架的测试辅助功能之上提供了对测试的额外支持.

### Spring Data

将应用程序的数据repository定义为简单的Java接口.

### Spring Cloud

微服务



## 小结

- Spring旨在简化开发人员所面临的挑战,比如创建Web应用程序,处理数据库,保护应用程序以及实现微服务.
- Spring Boot构建在Spring之上,通过简化依赖管理,自动配置和运行时洞察,使Spring更加易用.
- Spring应用程序可以使用Spring Initializr进行初始化.Spring Initializr是基于Web的应用,并且为大多数Java开发环境提供了原生支持.
- Spring应用上下文中,组件(bean)即可以使用Java或XML显式声明,也可以通过组件扫描发现,还可以使用Spring Boot 自动配置功能实现自动化配置.