[返回首页](../README.md)

## 声明式服务调用`Feign`
由于各个服务提供者都是以`HTTP`接口的形式对外提供服务，因此在服务消费者调用服务提供者时，
我们可以使用JDK原生的`URLConnection`、`Apache`的`HTTP Client`、`Spring`的`RestTemplate`等方式去实现服务间的调用。
但一般情况下，我们会使用`Feign`去进行服用间的调用。
> `Spring Cloud`对`Feign`进行了增强，使`Feign`支持`Spring MVC`的注解，并整合了`Ribbon`(负载均衡)
