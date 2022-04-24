# `Flutter` 学习笔记
## `Dart` 语言
> `Dart` 运行在 `Dart` 虚拟机上，也可以编译为直接在硬件上运行的 `ARM` 代码。
> - 可以省去`new`、`const`等关键词
> - 字符串插值:$variableName (或 ${expression})
### 变量
- 默认值
> 未初始化以及可空类型的变量拥有一个默认的初始值 null。用 `?` 制定变量可为空还是不可为空。
```text
int? lineCount;
```
- late 关键字
- Final、Const
> 一个 final 变量只可以被赋值一次；一个 const 变量是一个编译时常量。
- 内置类型()
### 函数
函数也是对象并且类型为 `Function`，可以有两种形式的参数：必要参数、可选参数。可以使用箭头函数(`=>`)
- 命名参数
> 命名参数默认为可选参数，除非他们被特别标记为 `required`。
```text
// 使用 {参数1, 参数2, …} 来指定命名参数
void enableFlags({bool? bold, bool? hidden}) {...}  
```
- 可选参数
> 将参数使用中括号`[]`括起来，用来表明是可选位置参数
```dart
//[] 代表可选参数，函数被调用时，可以选择不传值
String getUserInfo(String name, String gender, [String? from]) {
  var info = '$name\'s gender $gender';
  if (from != null) {
    info = '$info come from $from';
  }
  return info;
}
//调用
getUserInfo('user','M')

```
### 类
- 扩展类(继承)
> Dart 支持单继承。
### 空安全
选择使用空安全时，代码中的类型将默认是非空的，除非你声明它们可空，它们的值都不能为空。
有了空安全，原本处于运行时的空值引用错误将变为编译时的分析错误。
- 若想让变量可以为 null，只需要在类型声明后加上 ?
```dart
int? aNullableInt = null;
```
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