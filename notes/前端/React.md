# React
## 创建 React 应用
> 使用 create-react-app 创建
```shell
npx create-react-app part1
```
![目录结构](https://i.bmp.ovh/imgs/2021/12/145a7baffaf7c04e.png)
## 组件
在用 React 开发时，需要渲染的内容通常需要定义为 React 组件。
使用 React 编写组件很容易，通过组合组件，甚至可以使相当复杂的应用保持很好的可维护性。 实际上，React 的核心理念，就是将许多定制化的、可重用的组件组合成应用。
> 注意点：1、组件名称的首字母必须要大写 2、 React 组件的内容(通常)需要包含一个根元素(可以直接使用<></>)
### 组件定义与使用：
> 定义
```jsx harmony
const App = () => {
    const now = new Date() //组件内动态内容的渲染
    return (
        <div>
            <p>Hello world，{now.toString()}</p>
        </div>
    )
}
export default App; //使组件让外部可使用
```
> 渲染 root 节点
```jsx harmony
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App'; //导入组件

ReactDOM.render(<App />,document.getElementById('root'))
```
> DOM 节点 root 是 public/index.html 文件中的一个元素
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```
### 向组件中传递数据
```jsx harmony
const Hello = (props) => {
  return (
    <div>
      <p>Hello {props.name}</p>
    </div>
  )
}

const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <div>
      <Hello name={name} age={age} />
    </div>
  )
}
```
### 组件辅助函数
> 在一个函数中定义另一个函数
### 解构
> 在 ES6 中赋值时从对象和数组中解构出值
```text
const Hello = (props) => {
  const { name, age } = props
------------------------------------
const Hello = ({ name, age }) => {
```
### 有状态组件
> 向应用的组件中添加状态
### 事件处理
（被注册为）在特定事件发生时进行调用。 例如，用户与一个网页的不同元素的交互可能会触发一系列不同类型的事件。
### 状态传递给子组件
> 复杂状态
## JSX
在底层，React 组件实际上返回的 JSX 会被编译成 JavaScript。
# React Native
React Native 是一种利用JavaScript 和React 开发原生Android 和iOS应用的框架。它提供了一系列跨平台的组件，从而解耦了特定平台的原生组件。
## 创建应用
> Expo 是一个简化了React Native 应用的安装、开发、构建以及部署的平台

## 核心组件
一系列由React Native 提供的组件，底层是利用平台的原生组件
> Text、View、TextInput、Pressable
