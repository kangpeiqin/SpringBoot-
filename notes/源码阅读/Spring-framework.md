<p> <a href="../源码阅读.md">返回</a></p>

# Spring-framework 源码阅读
## 源码编译
> - 下载源码并导入到`IDEA`中
> - 创建并切换分支：`git checkout -b v5.2.13.RELEASE` 
> - 在IDEA中的Terminal窗口中执行命令：`gradlew :spring-oxm:compileTestJava`
- 运行单元测试报错问题
```text
Error:(350, 51) java: 找不到符号   符号:   变量 CoroutinesUtils
```
<a href="https://www.cnblogs.com/bruceChan0018/p/14214856.html">解决方案</a>：导入相应的依赖模块，如下图所示
![dependency](https://s3.bmp.ovh/imgs/2021/09/c95b9d18b9504ff7.jpg)
## 加载流程
> 加载配置文件 —> 读取类定义 —> 类实例化并加载进`IoC`容器  
### 容器的实现
- DefaultListableBeanFactory
> Spring注册及加载bean的默认实现
- AliasRegistry
> 别名注册接口，定义对别名的简单增删等操作