<p> <a href="../README.md">返回首页</a></p>

## GO 语言入门
### 基础
- struct
> - 用 `struct` 定义一组不同类型的数据。
> - 用 `type` 关键字定义新的类型，基于基础类型来创建新的定义类型。
```Golang
var myStruct struct {
	cnt  int
	word string
}

func main() {
	//赋值和访问
	myStruct.cnt = 25
	fmt.Println(myStruct.cnt)
	fmt.Println(myStruct.word)
}
```
- 定义方法
> 一旦方法被定义在了某个类型，它就能被该类型的任何值调用。
```go
type MyType string

func (m MyType) sayHi() {
	fmt.Println("hi")
}

func main() {
	value := MyType("a MyType value")
	value.sayHi()
}
```



## 参考
- [go 语言中文文档](https://www.topgoer.cn/docs/golang/chapter03-6)