## 概念
Typed JavaScript at Any Scale：添加了类型系统的 JavaScript，适用于任何规模的项目。
> 以 .ts 为后缀，用 TypeScript 编写 React 时，以 .tsx 为后缀。最终编译成 JS
## 基础
### 数据类型
- 原始数据类型 & 对象类型
- 类型推论
> 没有指定类型，`TS` 会推论出一个类型
- 联合类型
> 表示取值可以为一种或者多种的类型
```typescript
 // 允许 myFavoriteNumber 的类型是 string 或者 number，但是不能是其他类型
let myFavoriteNumber: string | number;
// 属性或者方法访问：只能访问此联合类型的所有类型里共有的属性或方法
```
- 接口
> 定义对象的类型：描述对象行为、属性
```typescript
interface Person {
    readonly id: number; //只读属性，只能在对象创建的时候被赋值
    name: string;
    age?: number; // 可选属性： 
    [propName: string]: string | number; // 任意属性取值，使用联合类型
}
```
- 数组类型
```typescript
let arr: number[] = [1, 1, 2, 3, 5]; //「类型 + 方括号」来表示数组
let array: Array<number> = [1, 1, 2, 3, 5]; //数组泛型
```
- 函数声明&表达式
```typescript
//声明，可选参数
function sum(x: number, y: number,lastName?: string): number {
    return x + y;
}
//表达式
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
- 类型断言
> 手动指定一个值的类型。
- 声明文件
> 当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

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