<p> <a href="../源码阅读.md">返回</a></p>

# `JDK-1.8`源码阅读
[String](#String) &nbsp; [Integer](#Integer) &nbsp;[StringBuilder](#StringBuilder)
[ArrayList](#ArrayList) &nbsp;[LinkedList](#LinkedList) &nbsp; [HashMap](#HashMap)
## 总结
> 封装复杂操作，简化接口调用。实现什么样的功能：数据结构(用什么结构存储数据(基本的数据类型)) + 一组操作
## 基础类
基本都实现了`Comparable`接口，用于比较两个元素。
### String
不可变类，即对象一旦创建，就没有办法修改了。String声明为了final，不能被继承，内部char数组value也是final的，初始化后就不能再变了。
- 用字符数组表示字符串，String中的大部分方法内部也都是操作的这个字符数组
```text
//使用 final 进行修饰，表示常量，不可变，在对象初始化(构造函数里面)时进行赋值
private final char value[];
//构造函数
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```
> `length()`方法返回的是这个数组的长度，`indexOf()`查找字符串是在这个数组中进行查找。
- 常量字符串
> String类型的对象，在内存中，它们被放在一个共享的地方：字符串常量池，
它保存所有的常量字符串，每个常量只会保存一份，被所有使用者共享
### `StringBuilder` & `StringBuffer`
StringBuffer类是线程安全的，而StringBuilder类不是。
> 没有采用 `final` 进行修饰，可以进行修改，同，字符数组中不一定所有位置都已经被使用。
```text
//The value is used for character storage.
char[] value;
// The count is the number of characters used.
int count;
```
### Integer
> 缓存：IntegerCache
## 集合框架
<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d20706cc84b7421daa747c1566bde4a5~tplv-k3u1fbpfcp-watermark.image">

### ArrayList
- 底层为`Object`数组
> 内部操作的基本都是这个数组和`size`这个整数，
数组会随着实际元素个数的增多而进行扩容，而size则始终记录实际的元素个数。
```test
transient Object[] elementData; 
private int size;
```
- 数组扩容(1.5倍)
```text
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        //新的数组容量，右移一位，相当与除以2
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
}
```
- 删除元素
```text
//将size-1,同时释放引用以便原对象能被垃圾收集器回收
elementData[--size] = null; // clear to let GC do its work
```
- 数组的拷贝
```text
//native 方法
System.arraycopy(elementData, index + 1, elementData, index,numMoved);
```
### LinkedList
`ArrayList`随机访问效率很高，但插入和删除性能比较低，`LinkedList`正好相反
> 实现了`List`接口，而`List`接口扩展了`Collection`接口，`Collection`又扩展了`Iterable`接口，还实现了队列`Queue`接口
- `Queue`接口每种操作都有两种形式
> 区别在于，对于特殊情况的处理不同。特殊情况是指队列为空或者队列为满(指队列有长度大小限制，而且已经占满了)。
LinkedList的实现中，队列长度没有限制，但别的Queue的实现可能有。在队列为空时，element和remove会抛出异常NoSuchElementException，而peek和poll返回特殊值null；在队列为满时，add会抛出异常IllegalStateException，而offer只是返回false
### HashMap
- 存储结构
> 数组+链表+红黑树
- 元素要存入数组当中的哪个位置(哈希)？如果该位置有元素了(哈希冲突)，应该如何处理(链地址法)？
> 直接利用哈希算法定位到存储的位置。
## 多线程
> 主要位于`util.concurrent`包中
### 原子类
保证原子更新操作，一个操作不可被中断。如：i作为某对象的成员变量，`i++`这个操作不是原子操作，
它分为三个步骤：
```text
1、取i的当前值
2、在当前值基础上加1
3、将新值重新赋值给i
```
- 使用`synchronized`的缺点
> 成本较高，需要先获取锁，最后需要释放锁，获取不到锁还需要等待，会有线程的上下文切换。
- [CAS(Compare And Swap)](https://www.jianshu.com/p/ae25eb3cfb5d)和ABA问题
> CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。
### 线程池
![UML类图](https://i.bmp.ovh/imgs/2022/01/8e5743583c4ddeff.png)
- `Executor` & `ExecutorService`
> 任务的提交与执行分离
- 工具类`Executors`
> 提供了一些静态方法，为开发者提供各种封装好的具有各自特性的线程池
> - newFixedThreadPool: 创建一个固定线程数量的线程池
> - newSingleThreadExecutor: 创建一个单线程线程池
> - newCachedThreadPool: 当需要时才创建新的线程，会对之前创建的线程进行复用，当有很多短暂的异步任务，使用 newCachedThreadPool 将会极大的改善程序的性能

### [AQS(抽象队列同步器)](http://concurrent.redspider.group/article/02/11.html)
- 作用：用来构建锁和同步器的框架(ReentrantLock、FutureTask都是基于此)
> * 抽象：抽象类，只实现一些主要逻辑，有些方法由子类实现；
> * 队列：使用先进先出（FIFO）队列存储数据；
> * 同步：实现了同步的功能。
#### 数据结构
```
                            state(资源)
          +------+  prev +-----+       +-----+
      head |      | <---- |     | <---- |     |  tail
           +------+       +-----+       +-----+
```
#### 资源的共享模式
- 独占模式
> 资源是独占的，一次只能一个线程获取。如`ReentrantLock`。
- 共享模式
> 同时可以被多个线程获取，具体的资源个数可以通过参数指定。如Semaphore/CountDownLatch。
#### 一般的设计思路
1、定义一个变量`int state = 0`，使用这个变量表示被获取的资源的数量。
2、线程在获取资源前要先检查`state`的状态，如果为0，则修改为1，表示获取资源成功，否则表示资源已经被其他线程占用，此时线程要堵塞以等待其他线程释放资源。
3、为了能使得资源释放后找到那些为了等待资源而堵塞的线程，我们把这些线程保存在`FIFO`队列中。
4、当占有资源的线程释放掉资源后，可以从队列中唤醒一个堵塞的线程，由于此时资源已经释放，因此这个被唤醒的线程可以获取资源并且执行。

参考文章：[JUC解析-AQS(1)](https://juejin.cn/post/6844903583234654221)

