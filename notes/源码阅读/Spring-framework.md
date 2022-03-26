<p> <a href="../源码阅读.md">返回</a></p>

# Spring-framework 源码阅读
## 源码编译
> - 下载源码并导入到`IDEA`中
> - 切换分支：`git checkout -b v5.2.13.RELEASE` 
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

## `SpringMVC` 源码阅读

## SpringBoot
### 自动配置原理解析
```
//在启动时，使用`@EnableXXX`与`@Import`注册bean。
// EnableAutoConfiguration 注解，使用 @Import 注解往容器中注册相应的bean
@Import(AutoConfigurationImportSelector.class)
@Import(AutoConfigurationPackages.Registrar.class)
// AutoConfigurationImportSelector 类实现 ImportSelector 接口
//大批量的bean注册：这里主要会从 spring.factories 文件中读取配置好的一些类
@Override
public String[] selectImports(AnnotationMetadata annotationMetadata) {
	if (!isEnabled(annotationMetadata)) {
		return NO_IMPORTS;
	}
	AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
	return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
}
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
	if (!isEnabled(annotationMetadata)) {
		return EMPTY_ENTRY;
	}
	AnnotationAttributes attributes = getAttributes(annotationMetadata);
	List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
	configurations = removeDuplicates(configurations);
	Set<String> exclusions = getExclusions(annotationMetadata, attributes);
	checkExcludedClasses(configurations, exclusions);
	configurations.removeAll(exclusions);
	configurations = getConfigurationClassFilter().filter(configurations);
	fireAutoConfigurationImportEvents(configurations, exclusions);
	return new AutoConfigurationEntry(configurations, exclusions);
}
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
	List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
			getBeanClassLoader());
	Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
			+ "are using a custom packaging, make sure that file is correct.");
	return configurations;
}
```
### 声明式事务的原理
### Spring Cache
```text
注解：EnableCaching 默认适用 JDK 动态代理
通过：@Import(CachingConfigurationSelector.class) 将 AutoProxyRegistrar 和 ProxyCachingConfiguration 加载进 IoC 容器当中

```