## `Webpack`
开源的`JS`模块打包工具，其最核心的功能是解决模块之间的依赖，
把各个模块按照特定的规则和顺序组织在一起，最终合并为一个`JS`文件（有时会有多个）。
这个过程就叫作模块打包。其次还可以处理样式、图片等类型的资源，
我们把源代码交给`Webpack`，由它去进行加工、拼装处理，产出最终的资源文件，等待送往用户。
### 模块的概念
> 比如在工程中引入一个日期如理的`npm`包，或者编写一个提供工具方法的`JS`文件，这些都可以称作模块。
在过去的很长一段时间里，`JS`语言并没有模块这一概念，如果工程中有多个`JS`文件，我们只能通过`script`标签
将其插入到页面当中。
> - 这样做的缺点：(1)需要手动维护`JS`的加载顺序。(2)每个`script`标签都意味着需要向服务器请求一次静态资源，建立连接成本高。
> - 模块化解决的问题：可以清晰看到模块间的依赖关系，只需要加载合并后的资源文件，减少网络开销。多个模块之间的作用域是隔离的，彼此不会有命名冲突。
> - 解决方案：`AMD`、`CommonJS`、`ES6标准模块`。大多数`npm`模块还是`CommonJS`的形式，而浏览器并不支持其语法，因此使用打包工具让我们
在工程中使用模块化的同时也能使我们的代码正常运行在浏览器。
### 工作方式
- 将存在依赖关系的模块按照特定规则合并为单个JS文件，一次全部加载进页面中。
- 在页面初始时加载一个入口模块，其他模块异步地进行加载。
### `npm` 安装模块的方式
- 全局安装
> 会帮我们绑定一个命令行环境变量，一次安装、处处运行。
- 本地安装
> 添加其成为项目中的依赖，只能在项目内部使用。
```bash
# webpack 是核心模块，webpack-cli则是命令行模块
npm install webpack webpack-cli --save-dev 
```
### 模块打包
```bash
#  npx 会自动查找当前依赖包中的可执行文件，如果找不到，就会去 PATH 里找。如果依然找不到，就会帮你安装
# entry：资源打包的入口，webpack 从这里开始进行模块依赖的查找，得到项目中包含的js模块，并通过它们来生成最终的产物。
# output-filename：打包输出资源名，boundle.js 就是 Webpack 的打包结果
# mode：打包模式(developement\production\none)，会自动添加适合当前模式的一系列配置
npx webpack --entry=./index.js --output-filename=bundle.js --mode=development
```
- 使用 `npm scripts`
> 可以在 `package.json` 中添加脚本命令，简化命令
```bash
# scripts 是 npm 提供的脚本功能，我们可以直接使用由模块所添加的指令。
  "scripts": {
    "build": "webpack --entry=./index.js --output-filename=bundle.js --mode=development"
  }
# 执行 yarn run build 便可以进行构建
# webpack 默认的源代码入口是`src/index.js`,因此可以省略 `entry` 的配置
# 命令行查看帮助文档
npx webpack -h
```
- 使用配置文件
> 当项目需要越来越多的配置时，就要往命令中添加更多的参数，到后期维护就相当困难，所以可以把这些参数改为对象的形式专门放在一个
配置文件当中，在`Webpack`每次打包的时候读取该配置文件即可。`Webpack`的默认配置文件为`webpack.config.js`,当然，也可以使用其他
文件名，需要使用命令行参数制定。如下所示：
```
//webpack.config.js 文件
//使用 module.exports 导出了一个对象，打包时被 webpack 接收的对象
module.exports = {
    //资源输入属性
    entry: './src/index.js',
    //资源输出属性
    output:{
        filename: 'bundle.js'
    },
    mode: 'development'
}
//package.json 中的配置
"scripts": {
    "build": "webpack"
}
```
#### 模块标准
- `CommonJS`
> 09 年提出的包含模块、文件、IO、控制台在内的一些列标准。进行变量及函数声明时不会污染全局环境。
会形成一个属于模块自身的作用域。
- 通过 `module.exports` 可以导出模块中的内容
> `CommonJS` 模块内部会有一个`module`对象用于存放当前模块的信息
- 通过 `require` 函数进行模块导入
```
模块时第一次被加载，则会首先执行该模块，然后导出内容。


```
- `ES6 Module`

### Q&A
- 为什么需要模块化？
> 应用功能和规模大了之后，需要按照更好的组织方式，将特定的功能拆分多个文件，方便后续的维护。

- 为什么不直接将工程中的源文件发布到服务器或CDN，要进行打包？