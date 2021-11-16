<p> <a href="../README.md">返回首页</a></p>

## Spring

## Spring Boot
### 自动配置的原理

## Spring Security
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
