<p> <a href="../源码阅读.md">返回</a></p>

# JDK 1.8 源码阅读
[String](#String) &nbsp;
[ArrayList](#ArrayList) &nbsp;[LinkedList](#LinkedList) &nbsp; [HashMap](#HashMap)
### String
> String类也是不可变类，即对象一旦创建，就没有办法修改了。String类也声明为了final，不能被继承，内部char数组value也是final的，初始化后就不能再变了。
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
### ArrayList
封装复杂操作，简化接口调用。
```test
transient Object[] elementData; 
private int size;
```
> 底层为`Object`数组。内部操作的基本都是这个数组和`size`这个整数，
数组会随着实际元素个数的增多而进行扩容，而size则始终记录实际的元素个数。
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
System.arraycopy(elementData, index + 1, elementData, index,
                    numMoved);
```
### LinkedList
`ArrayList`随机访问效率很高，但插入和删除性能比较低，`LinkedList`正好相反
> 实现了`List`接口，而`List`接口扩展了`Collection`接口，`Collection`又扩展了`Iterable`接口，还实现了队列`Queue`接口
- `Queue`接口每种操作都有两种形式
> 区别在于，对于特殊情况的处理不同。特殊情况是指队列为空或者队列为满(指队列有长度大小限制，而且已经占满了)。
LinkedList的实现中，队列长度没有限制，但别的Queue的实现可能有。在队列为空时，element和remove会抛出异常NoSuchElementException，而peek和poll返回特殊值null；在队列为满时，add会抛出异常IllegalStateException，而offer只是返回false
### HashMap

