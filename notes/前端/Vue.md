[返回首页](../README.md)

> `Vue.js`
## 概述
### `MVC` 和 `MVMM` 模式的概念

### 什么是前端工程化(对于前端发展史的了解)

### `Vue` 解决的问题

### `Vue 3.0`的新特性和优化点

## 基础
### 单页应用(Single Page Application(SPA))
指只有一个 `HTML` 页面的应用，在用户与应用程序交互时动态地更新该页面的内容。一般通过 `Vue` 进行开发的都是一个`SPA`应用。
### `Vue` 实例与组件
每一个单页`Vue`应用都需要从一个`Vue`实例(根实例)开始，是由一棵嵌套的、可重用的组件树组成的。
一个`Vue`实例若想和页面上的`DOM`渲染进行挂载，就需要调用`mount`方法，参数传递`id选择器`，挂载之后这个`id选择器`对应的`DOM`会被`Vue`实例接管。
### 模板语法
#### 插值表达式
#### 指令

### 深入理解组件
组件是可被复用的一些代码块。为了保证组件中的数据都是相互隔离、互不影响的，所以子组件中的`data`属性是一个函数。可以使用`methods`为每个组件添加方法，然后可以通过`this.xxx()`来调用。
> 单文件组件(Single File Components(SFC))：文件扩展名为`.vue`的单文件组件。`<style>`标签：`scoped`属性(当前`<style>`中的样式代码只会对当前的单文件组件生效),`lang`属性(可以用来启用`scss`或`less`
#### 全局组件
可以直接在任意组件当中使用
#### 局部组件
只能在当前注册的这个组件中使用
#### 组件的生命周期
指的是组件自身的一些方法(钩子函数)，这些方法在特殊的时间点或遇到一些特殊的框架事件时会被自动触发。
#### 组件之前如何通信
##### 父组件向子组件通信
##### 子组件向父组件通信
##### 非父子关系组件的通信
#### 组件插槽
### 动画
## 状态管理

## 路由管理(`Vue Router`)

## 构建工具(`Vue Cli` 和 `Vite`)
### Vue Cli(`Command Line Interface`)
基于`Node.js`的`npm`包，可以用来生成项目的`脚手架`(项目初期的基本目录结构)
#### 安装和使用
```bash
# 全局按章
npm install -g @vue/cli
# 查看版本
vue --version
# 初始化前端项目
vue create project_name
# 检查当前的代码是否符合规范，可以通过.eslintrc.js文件来设置，基于 `vue-cli-service` 提供的命令
npm run build lint
# 得到项目的 Webpack 配置文件
vue-cli-service inspect
# 使用图形化界面管理项目
vue ui
```
> - CSS预处理工具强调提供一些API，使得编写CSS样式时更具有逻辑性，使CSS更有组织性，例如可以定义变量等
> - Babel是一个开源的npm包，可用于将ES6代码转换成浏览器兼容性更强的ES5
> - E2E Testing：端对端测试，对整体流程进行测试，类似于黑盒测试
> - npx：可以直接执行 node_modules 下的相关模块中的命令
#### 自定义配置
默认情况下`Webpack`的相关配置项是不会暴露给开发者的，可以通过`vue.config.js`来修改和覆盖`Vue Cli`的默认`Webpack`配置
### Vite
在开发环境下基于浏览器原生的`ES6 Modules`提供功能支持，在生产环境下基于`Rollup`打包，脱离`Webpack`依赖。
包括脚手架创建、快速启动、按需编译、热模块替换等特性，大大提升了开发效率。
> Rollup：JavaScript模块打包器，和Webpack有着同样的模块打包功能，最大的特点是基于`ES Modules`进行打包，不需要通过类似`Babel`转化的方案将`ES 6 Modules`的`import`转化成`CommonJS`的`require`方式，极大地利用了浏览器的原生特性。
## 服务端渲染和客户端渲染

## 源码解析

## 参考
- https://cn.vuejs.org/