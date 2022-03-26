# `Flutter` 学习笔记
## `Dart` 语言
> 符合构建用户界面的方式。
### 特点
- 面向对象：有的对象都继承自内置的`Object`类。
- 指定数据类型和编译时的常量，可以提高运行速度。
- 没有`public`、`protected`和`private`的概念。
**私有特性通过变量或函数加上下划线来表示**。
- 常用开发库
> - `dart:core`：核心库，包括strings、numbers、collections、errors、dates、URIs等。
> - `dart:html`：网页开发中DOM相关的一些库。
> - `dart.io`：I/O命令行使用的I/O库。
> - `dart:async`：异步编程支持
### 基础
- 变量声明：var，常量：final、const
- 基本数据类型：Number、String、Boolean、List、Map
- 函数：函数也是对象，属于Function对象。将参数使用中括号"[]"括起来，用来表明是可选位置参数。
可以为参数设置默认值。
- 异步支持：进行耗费时间的操作，比如文件读写、网络请求。该功能返回Future或Stream对象。
- 元数据
## `Flutter`
### 基本概念
- `Widget` 
> 使用代码来构建 `UI`，而不是 `XML`，Flutter 中几乎所有的东西都是 widget。
Flutter里Key是每一个Widget的唯一身份标识，它是在组件创建及渲染时生成的。Widget会根据指定的名字生成Key，Key是一个可选参数。
- 状态管理(X)
> 状态由谁进行管理
- 页面跳转(路由)
- 包管理
> 使用配置文件`pubspec.yaml`来管理第三方依赖包。会从 `Google` 提供的 [Pub 仓库](https://pub.dev/) 去下载对应的依赖包。
### 基础组件
> 基础组件用于处理文本和图片的基础操作，如文本展示、文本输入、图片加载、按钮设置、容器控制等。
- 容器组件
> 容器（Container）组件包含一个子Widget，自身具备如alignment、padding等基础属性，方便布局过程中摆放child（子组件）
### `Material`风格组件
一套UI规范，它将经典的设计原理与科技创新相结合，给用户更强的融入感，Flutter已经内置了Material风格组件。
- MaterialApp
> 作为顶级容器表示当前App是Material风格的，设置的样式属性都是全局的。
- Scaffold
> 是Material组件的布局容器，可用于展示抽屉（Drawer）、通知（Snack Bar）及底部导航的效果。

## Reference
[《Flutter实战》](https://book.flutterchina.club/)