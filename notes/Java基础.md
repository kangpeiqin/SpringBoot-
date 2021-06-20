<p> <a href="../README.md">返回首页</a></p>

# 基础
## I/O
I/O本质是将什么样的数据写到什么地方。所以传输数据的格式和传输数据的方式会影响的I/O的效率。Java的I/O操作类在包java.io下
> 传输数据的数据格式
- 基于字节：InputStream和OutputStream
- 基于字符：Writer和Reader
> 传输数据的方式
- 基于磁盘：File
- 基于网络：Socket
### Q&A
- 为什么有操作字符的I/O接口？

以InputStream/OutputStream为基类的流基本都是**以二进制形式处理数据的，不能够方便地处理文本文件**，没有编码的概念，能够方便地按字符处理文本数据的基类是Reader和Writer
### 序列化和反序列化
序列化就是将内存中的Java对象持久保存到一个流中，反序列化就是从流中恢复Java对象到内存。序列化和反序列化主要有两个用处：一是对象状态持久化，二是网络远程调用，用于传递和返回对象。
- Java主要通过接口Serializable和类ObjectInputStream/ObjectOutputStream提供对序列化的支持
- 缺点：列化后的形式比较大、浪费空间，序列化/反序列化的性能也比较低，是Java特有的技术，不能与其他语言交

1、将字段声明为transient，默认序列化机制将忽略该字段，不会进行保存和恢复

2、定义版本号：在序列化时，会将该版本号写入流，在反序列化时，会将流中的值与类定义中的版本号进行比较，如果不匹配，会抛出InvalidClassException

- 其他方式：文本格式：ⅩML和JSON、二进制：ProtoBuf、Thrift、MessagePack
> 更多可以参考<a href="https://www.cyc2018.xyz/Java/Java%20IO.html#%E4%B8%80%E3%80%81%E6%A6%82%E8%A7%88" target="view_blank">Java I/O</a>或者<a href="https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86?id=io-%e6%b5%81" target="view_blank">I/O流</a>
## 泛型
**类型参数化**，处理的数据类型不是固定的，而是可以作为参数传入。代码与它们能够操作的数据类型不再绑定在一起，同一套代码可以用于多种数据类型
- 作用：复用代码，降低耦合，保证类型安全，泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。
- 原理：类型擦除：Java 在编译期间，所有的泛型信息都会被擦掉，替换成Object类型
- 使用：泛型类、泛型接口、泛型方法。
## 多线程
## 类加载机制
## 函数式编程
参考链接：https://github.com/winterbe/java8-tutorial

### 内存模型