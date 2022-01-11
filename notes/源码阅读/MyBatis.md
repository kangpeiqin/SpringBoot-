<p> <a href="../源码阅读.md">返回</a></p>

# MyBatis 源码阅读
## 概述
MyBatis属于半自动化的ORM(Object Relational Mapping：对象关系映射)框架，能够将数据库中的记录转换为Java实体。之所以称为半自动框架是因为它需要手工匹配Java类，SQL和映射关系。
```text
数据库的表（table） --> 类（class）
记录（record，行数据）--> 对象（object）
字段（field）--> 对象的属性（attribute）
```
## 整体架构
功能架构主要分为三层：接口层(提供API操作数据库)、数据处理层(参数映射、SQL解析、执行、结果映射)、基础支撑层(提供了连接管理、事务管理、配置加载和缓存处理等共用的基础功能)
<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23baa2bdb0c14ea0872610c1861120dc~tplv-k3u1fbpfcp-watermark.image" style="width: auto;
    max-width: 100%;
    border-radius: 12px;
    display: block;
    margin: 20px auto;
    object-fit: contain;
    box-shadow: 2px 4px 7px #999;">
## 执行流程
<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/80807c63e2634462a813647419b44328~tplv-k3u1fbpfcp-watermark.image" style="width: auto;
    max-width: 100%;
    border-radius: 12px;
    display: block;
    margin: 20px auto;
    object-fit: contain;
    box-shadow: 2px 4px 7px #999;">
<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ddc80b4046804e7facbffb3bb0a61fb6~tplv-k3u1fbpfcp-watermark.image" style="width: auto;
    max-width: 100%;
    border-radius: 12px;
    display: block;
    margin: 20px auto;
    object-fit: contain;
    box-shadow: 2px 4px 7px #999;">
- 主要流程：加载配置文件(主要有两类：注解和XML文件)-->SQL解析-->SQL执行-->结果映射
## 从单元测试看具体的执行流程
配置文件的解析:
![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b60d3381de35475cb2bb3fd3e5d3e500~tplv-k3u1fbpfcp-watermark.image)
> 1、读取主配置文件，将XML文件解析成Configuration对象，利用Configuration对象构建sqlSessionFactory(SqlSession工厂，SqlSession定义了操作数据库的API接口)
```
//读取主配置文件
try (Reader reader = Resources.getResourceAsReader("org/apache/ibatis/autoconstructor/mybatis-config.xml")) {
    sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
}
//执行SQL脚本
BaseDataTest.runScript(sqlSessionFactory.getConfiguration().getEnvironment().getDataSource(),"org/apache/ibatis/autoconstructor/CreateDB.sql");
// ===> XML文件具体的解析流程：SqlSessionFactoryBuilder.java
XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);
SqlSessionFactory sqlSessionFactory =  build(parser.parse());
public Configuration parse() {
    //部分省略，读取configuration标签
   parseConfiguration(parser.evalNode("/configuration"));
   return configuration;
}
//解析各种定义标签当中的内容
private void parseConfiguration(XNode root) {
    try {
      // issue #117 read properties first
      propertiesElement(root.evalNode("properties"));
      Properties settings = settingsAsProperties(root.evalNode("settings"));
      loadCustomVfs(settings);
      loadCustomLogImpl(settings);
    //...省略
}
//类型别名注册

```
## JDBC规范
## 工具类
- MetaObject
> 用于获取和设置对象的属性值
- MetaClass
> 反射工具类，用于获取类相关的信息。例如，可以使用MetaClass判断某个类是否有默认构造方法，还可以判断类的属性是否有对应的Getter/Setter方法
- ObjectFactory
> MyBatis中的对象工厂，只有一个默认的实现，即DefaultObjectFactory
- ProxyFactory
> 代理工厂，主要用于创建动态代理对象，两个不同的实现类，分别为CglibProxyFactory和JavassistProxyFactory
## MyBatis核心组件
<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b673bde53182492daf96150475fef30b~tplv-k3u1fbpfcp-watermark.image" style="width: auto;
    max-width: 100%;
    border-radius: 12px;
    display: block;
    margin: 20px auto;
    object-fit: contain;
    box-shadow: 2px 4px 7px #999;"/>
- Configuration：用于描述MyBatis的主配置信息，其他组件需要获取配置信息时，直接通过Configuration对象获取。此外，MyBatis在应用启动时，将Mapper配置信息、类型别名、TypeHandler等注册到Configuration组件中，其他组件需要这些信息时，也可以从Configuration对象中获取
- MappedStatement：用于描述Mapper中的SQL配置信息，是对Mapper XML配置文件中<select|update|delete|insert>等标签或者@Select/@Update等注解配置信息的封装。
- SqlSession：是MyBatis提供的面向用户的API，表示和数据库交互时的会话对象，用于完成数据库的增删改查功能。SqlSession是Executor组件的外观，目的是对外提供易于理解和使用的数据库操作接口。
- Executor：MyBatis的SQL执行器，对数据库所有的增删改查操作都是由Executor组件完成的。
- StatementHandler：封装了对JDBCStatement对象的操作，比如为Statement对象设置参数，调用Statement接口提供的方法与数据库交互。
- ParameterHandler：当MyBatis框架使用的Statement类型为CallableStatement和PreparedStatement时，ParameterHandler用于为Statement对象参数占位符设置值。  
- ResultSetHandler：封装了对JDBC中的ResultSet对象操作，当执行SQL类型为SELECT语句时，ResultSetHandler用于将查询结果转换成Java对象
- TypeHandler：MyBatis中的类型处理器，用于处理Java类型与JDBC类型之间的映射
### Configuration
- 配置信息有两种：1、配置MyBatis框架属性的主配置文件。2、配置执行SQL语句的Mapper配置文件。
> Configuration的作用是描述MyBatis主配置文件的信息。mapperRegistry：用于注册Mapper接口信息，建立Mapper接口的Class对象和MapperProxyFactory对象之间的关系，其中MapperProxyFactory对象用于创建Mapper动态代理对象
### Executor
SqlSession是MyBatis提供的操作数据库的API，但是真正执行SQL的是Executor组件
### MappedStatement
MyBatis通过MappedStatement描述<select|update|insert|delete>或者@Select、@Update等注解配置的SQL信息
### SqlSession执行Mapper的过程
> Mapper由两部分组成，分别为Mapper接口和通过注解或者XML文件配置的SQL语句
### Mapper接口中定义方法是如何被执行的？
Mapper接口用于定义执行SQL语句相关的方法，方法名一般和Mapper XML配置文件中<select|update|delete|insert>标签的id属性相同，接口的完全限定名一般对应Mapper XML配置文件的命名空间。
## Q&A
- 所有的`XML`在程序启动时是如何被加载和解析进内存的？
> 读取配置文件信息-->将配置文件信息解析进内存
