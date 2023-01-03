## 基本概念

## 安装
## 使用
RabbitMQ Java 客户端使用 com.rabbitmq.client 作为顶级包名，关键的一些类：Connection、Channel、ConnectionFactory、Consumer 等。
### 引入依赖
```xml
 <dependencies>
    <dependency>
        <groupId>com.rabbitmq</groupId>
        <artifactId>amqp-client</artifactId>
        <version>5.8.0</version>
    </dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.11.0</version>
    </dependency>
 </dependencies>
```
### 连接 RabbitMQ，发送消息，消费消息
```java
public class Producer {
    public static void main(String[] args) throws IOException, TimeoutException {
        //连接 RabbitMQ
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("127.0.0.1");
        factory.setVirtualHost("/test");
        factory.setUsername("test");
        factory.setPassword("test");
        //建立连接
        Connection connection = factory.newConnection();
        //创建信道(可以使用信道发送或者接收消息)
        Channel channel = connection.createChannel();
        //声明交换器：创建一个持久化、非自动删除、绑定类型为 direct 的交换器
        channel.exchangeDeclare("exchangeName", "direct", true);
        //声明队列
        channel.queueDeclare("queueName", true, false, false, null);
        //队列绑定交换器
        channel.queueBind("queueName", "exchangeName", "routingKey");
        //发送消息
        channel.basicPublish("exchangeName", "routingKey", null, "hello".getBytes());
        channel.close();
        connection.close();
    }
}
```