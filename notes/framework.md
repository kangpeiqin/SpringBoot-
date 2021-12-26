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

### Q&A
- `Controller`是单例还是多例？
> 默认是单例的，不要使用非静态的成员变量，否则会发生数据逻辑混乱。
使用`@Scope("prototype")`可声明为多例。

## Spring Boot
### 基础
- 组件装配(类的实例化)的几种方式
> - 使用@Component注解
> - 配置类@Configuration与@Bean
> - 使用模块装配@EnableXXX与@Import(要注册较多的Bean可使用这个方式)
> - @ComponentScan：
> 从定义的扫描包路径(默认是SpringBoot主配置所在的包及其子包)扫描标识了@Controller、@Service、@Repository、@Component注解的类到Spring容器中
- 其他注解
> @Conditional
```text
若容器中没有这个则容器会给你注入一个这样的视图解析器，若容器中有就不注入了
@Bean
@ConditionalOnMissingBean
```
### 自动装配
类似Java的SPI、`Dubbo`的SPI机制，SpringBoot也提供了一种机制，它通过读取`META-INF/spring.factories`文件（这些文件可能存在于类路径中的多个jar包中）来加载一些预先配置的类，而这个核心机制来源于`SpringFactoriesLoader`。
### 启动流程
- SpringApplication实例的初始化

## Security
### Spring Security
- 基础原理：https://blog.csdn.net/u012702547/article/details/89629415
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
