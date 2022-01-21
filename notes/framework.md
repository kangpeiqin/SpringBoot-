<p> <a href="../README.md">返回首页</a></p>

## Spring
### Bean 的作用域
- singleton: 单例模式
> 当`spring`创建`IoC`容器的时，会欲初始化所有的该作用域实例，加上`lazy-init`就可以避免预处理
- prototype：原型模式(多例)
> 每次通过getBean获取该bean就会新产生一个实例，创建后spring将不再对其管理
- request
> 每次请求都新产生一个新的实例
- session: 
> 每次会话都会产生一个新的实例
- global session
### 循环依赖
BeanA对象的创建依赖于BeanB，BeanB对象的创建也依赖于BeanA，这就造成了死循环，如果不做处理的话会造成栈溢出。
Spring通过提前曝光机制，利用三级缓存解决循环依赖问题。

### Q&A
- `Controller`是单例还是多例？
> 默认是单例的，不要使用非静态的成员变量，否则会发生数据逻辑混乱。
使用`@Scope("prototype")`可声明为多例。

## Spring Boot
> `SpringBoot`中推荐基于`Java Config`的方式来代替传统的XML方式去引入`Bean`
### 基础
- 组件装配(类的实例化)的几种方式
> - 使用`@Component`注解
> - 配置类`@Configuration`与`@Bean`
> - 使用模块装配`@EnableXXX`与`@Import`(要注册较多的Bean可使用这个方式)
> - `@ComponentScan`：
> 从定义的扫描包路径(默认是SpringBoot主配置所在的包及其子包)扫描标识了@Controller、@Service、@Repository、@Component注解的类到Spring容器中
- 其他注解
> `@Conditional`: 根据条件进行注入
```text
若容器中没有这个`Bean`则容器会给你注入一个这样的视图解析器，若容器中有就不注入了
@Bean
@ConditionalOnMissingBean
```
### 自动装配
> 是什么？主要的原理是什么？如何实现？
- 概念和原理
> SpringBoot提供了一种机制，它通过读取`META-INF/spring.factories`文件（这些文件可能存在于类路径中的多个jar包中）来加载一些预先配置的类，而这个核心机制来源于`SpringFactoriesLoader`。
### 启动流程
- SpringApplication实例的初始化
### 日志
Spring Boot在所有**内部日志**中使用Commons Logging，但是默认配置也提供了对常用日志的支持。
> 默认情况下，Spring Boot会用Logback来记录日志，并用INFO级别输出到控制台。
> - SLF4J(Simple Logging Facade For Java)，它是一个针对于各类Java日志框架的统一Facade抽象。而真正的日志实现则是在运行时决定的——它提供了各类日志框架的绑定。
> - Logback是log4j框架的作者开发的新一代日志框架，它效率更高、能够适应诸多的运行环境
- 日志级别
> `ERROR、WARN、INFO、DEBUG、TRACE`。日志级别从低到高分为`TRACE < DEBUG < INFO < WARN < ERROR < FATAL`，如果设置为`WARN`，则低于WARN的信息都不会输出。
- 将日志信息存储到文件
> 在本机环境，我们习惯在控制台看日志，但是线上我们还是要通过将日志信息保存到日志文件中，查询日志文件即可。
> - 设置logging.file或logging.path(文件夹生成一个日志文件)属性。
- 自定义日志配置
> 命名规则：Logback：logback-spring.xml, logback.xml, logback.groovy。官方推荐优先使用带有-spring的文件名作为你的日志配置。
- 针对不同运行时`Profile`使用不同的日志配置
> logging.config=classpath:logging-config.xml
### 异步调用
通常我们开发的程序都是同步调用的，即程序按照代码的顺序一行一行的逐步往下执行，每一行代码都必须等待上一行代码执行完毕才能开始执行。而异步编程则没有这个限制，代码的调用不再是阻塞的。
> 默认情况下的异步线程池配置使得线程不能被重用，每次调用异步方法都会新建一个线程，我们可以自己定义异步线程池来优化。
## Security
### Spring Security
- [基础原理](https://blog.csdn.net/u012702547/article/details/89629415)
### OAuth 2.0
- 授权服务器的配置
> 使用`@EnableAuthorizationServer`表示开启授权服务器。
> - 继承`AuthorizationServerConfigurerAdapter`，并重写方法。主要设置授权服务器如何读取客户端、用户信息和一些端点配置。
- 获取令牌请求
> 由 Spring MVC 的控制器端点来处理：
> - `AuthorizationEndpoint`：用于授权服务端点，默认路径：`/oauth/authorize`
> - `TokenEndpoint`：用于获取令牌的端点，默认路径：`/oauth/token`
- 受保护资源
> 通过过滤器链来进行处理



## Spring Cloud



## 文章推荐
- [SpringBoot 配置类解析](https://mp.weixin.qq.com/s/NvPO5-FWLiOlrsOf4wLaJA)