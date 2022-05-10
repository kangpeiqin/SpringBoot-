<p> <a href="../README.md">返回首页</a></p>

## GO 语言入门
### 示例
```golang
package main

import "fmt"

//函数名，参数列表，返回值列表(无返回值则可以省略)，函数体
func calc(x, y int) (int, int) {
	// 类型相同的相邻参数，参数类型可合并。 多返回值必须用括号。
	sum := x + y
	sub := x - y
	return sum, sub
}

//全局变量得声明
var arr = [...]int{0, 1, 2}

func main() {
	// ----- 1、匿名变量得使用--------------
	x, _ := calc(1, 2)
	_, y := calc(3, 2)
	fmt.Println(x, y)
	//--- 数组类型的定义与使用------
	d := [...]struct {
		name string
		age  uint8
	}{
		{"user1", 10},
		{"user2", 20},
	}
	fmt.Println(d)
	// --- 2、切片(slice)-------------
	var slice []int = arr[0:1]
	fmt.Println("slice", slice)
	//-----切片 end -------------------
	//--- 3、Map 相关：make Map 分配内存: make(map[KeyType(键的类型)]ValueType(值得类型), [cap(map的容量，非必须)])----
	scoreMap := make(map[string]int, 8)
	scoreMap["Jane"] = 90
	scoreMap["Mike"] = 80
	//声明时填充元素
	userInfo := map[string]string{
		"username": "nick",
		"password": "123456",
	}
	fmt.Println(scoreMap, userInfo)
	//判断某键是否再 map 当中存在
	v, exist := scoreMap["jack"]
	if exist {
		fmt.Println(v)
	} else {
		fmt.Println("none")
	}
	//删除元素
	delete(scoreMap, "Jane")
	// map 遍历
	for k, v := range scoreMap {
		fmt.Println(k, v)
	}
	//--------------Map End-----------------
}

```
### 基础
- 匿名变量
> 使用多重赋值时，如果想要忽略某个值，可以使用匿名变量（anonymous variable）。 匿名变量用一个下划线`_`表示。
匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。
- 基本类型
> bool、byte(代表了ASCII码的一个字符)、float、rune(代表一个`UTF-8`字符)
- 切片
> `slice` 并不是数组或数组指针。它通过内部指针和相关属性引用数组片段，以实现变长方案
- 指针
> Go语言中的函数传参都是值拷贝，当我们想要修改某个变量的时候，我们可以创建一个指向该变量地址的指针变量。传递数据使用指针，而无须拷贝数据。
- 结构体
> 可以使用`type`关键字来定义自定义类型。结构体可以用于封装多个基本类型。
- 匿名函数
- 闭包
- 延迟调用(`defer`)
> 延迟调用会直到 return 前才被执。因此，可以用来做资源清理(数据库连接释放，关闭文件句柄)。
- 接口
> 接口（interface）定义了一个对象的行为规范，只定义规范不实现，由具体的对象来实现规范的细节。接口是一组方法的集合
- 异常处理
- 单元测试
### 并发编程
> 在`java/c++`中我们要实现并发编程的时候，我们通常需要自己维护一个线程池，并且需要自己去包装一个又一个的任务，同时需要自己去调度线程执行任务并维护上下文切换。
`goroutine`的概念类似于线程，但 `goroutine`是由Go的运行时（runtime）调度和管理的。Go程序会智能地将 `goroutine` 中的任务合理地分配给每个CPU。


## 参考
- [go 语言中文文档](https://www.topgoer.cn/docs/golang/chapter03-6)