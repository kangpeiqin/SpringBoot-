## Netty简介
由 `JBOSS` 提供的一个 Java 开源通讯框架,好地封装了 Socket，用于处理网络通信，用以快速开发高性能、高可靠性的网络 IO 程序
## 学习链接
- [Netty-study](https://github.com/xuwujing/Netty-study?utm_source=ld246.com)
## 客户端与服务器端通信
### 服务器端
服务器端的基本信息配置、业务逻辑处理、必要时配置过滤器(编码与解码)
> 被动等待客户端的连接
### 客户端
客户端的主要工作是
1，连接到服务端
2，向服务端发送数据数据
3，处理服务端返回的数据
4，关闭连接
## 客户端与服务器端心跳
提供 `IdleStateHandler` 类，可以实现对三种心跳的检测，分别是 readerIdleTime、writerIdleTime 和 allIdleTime。
> - readerIdleTime：为读超时时间（即测试端一定时间内未接受到被测试端消息）;
> - writerIdleTime：为写超时时间（即测试端一定时间内向被测试端发送消息）
> - allIdleTime：所有类型的超时时间;
