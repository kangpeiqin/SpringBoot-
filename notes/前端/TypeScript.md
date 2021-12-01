## 概念
Typed JavaScript at Any Scale：添加了类型系统的 JavaScript，适用于任何规模的项目。
> 以 .ts 为后缀，用 TypeScript 编写 React 时，以 .tsx 为后缀。最终编译成 JS
## 基础
### 数据类型
- 原始数据类型 & 对象类型


### 工程
#### 代码检查(使用 ESLint)
> 用来发现代码错误、统一代码风格。 
- 使用步骤
```text
1、安装 ESLint：npm install --save-dev eslint
2、安装 @typescript-eslint/parser，替代掉默认的解析器：npm install --save-dev typescript @typescript-eslint/parser
3、安装插件，默认规则的补充：npm install --save-dev @typescript-eslint/eslint-plugin
4、创建配置文件
- 需要一个配置文件来决定对哪些规则进行检查(.eslintrc.js 或 .eslintrc.json)
- 原理：当运行 ESLint 的时候检查一个文件的时候，它会首先尝试读取该文件的目录下的配置文件，然后再一级一级往上查找，将所找到的配置合并起来，作为当前被检查文件的配置。
5、检查单个或者全部 ts 文件
6、在 VSCode 集成 ESLint 检查
```



### Q&A
- tsc : 无法加载文件
> 运行 set-ExecutionPolicy RemoteSigned 命令