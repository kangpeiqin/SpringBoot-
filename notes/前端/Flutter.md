# `Flutter` 学习笔记
## `Dart` 语言
> `Dart` 运行在 `Dart` 虚拟机上，也可以编译为直接在硬件上运行的 `ARM` 代码。
> - 可以省去new、const等关键词
## `Flutter`
### `计数器` 应用分析

### 架构
> 分层架构：框架层、引擎层
### 基本概念
- `Widget` 
> 使用代码来构建 `UI`，而不是 `XML`，Flutter 中几乎所有的东西都是 widget
- 状态管理
- 页面跳转(路由)
### 常用组件
#### 结构化组件
> 使用结构化组件，我们能够轻松实现一些具有固定模板的组件，如抽屉菜单、顶部导航栏等。
- `Scaffold` 组件
> Scaffold组件用于组合使用一些常用组件，如顶部菜单栏、左右抽屉侧边栏、底部导航栏、悬浮按钮。


### 包管理
使用配置文件`pubspec.yaml`来管理第三方依赖包。会从 `Google` 提供的 [Pub 仓库](https://pub.dev/) 去下载对应的依赖包。

## Reference
[《Flutter实战》](https://book.flutterchina.club/)