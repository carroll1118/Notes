###  泛型
* 泛型：可以将类型在类、方法、接口之间进行传递
* 泛型遵循大驼峰命名，一般只一个大写字母表示
* 泛型，只能在当前类中使用。子类不能继承使用
* 定义格式：
~~~
修饰符 class 类名<代表泛型的变量> {  }
~~~
* 含有泛型的方法的定义格式：
~~~
修饰符 <代表泛型的变量> 返回值类型 方法名(参数){  }
~~~
**泛型的上限**：

* **格式**： `类型名称 <? extends 类 > 对象名称`
* **意义**： `只能接收该类型及其子类`

**泛型的下限**：

- **格式**： `类型名称 <? super 类 > 对象名称`
- **意义**： `只能接收该类型及其父类型`

```java
// 泛型的上限：此时的泛型?，必须是Number类型或者Number类型的子类
public static void getElement1(Collection<? extends Number> coll){}

// 泛型的下限：此时的泛型?，必须是Number类型或者Number类型的父类
public static void getElement2(Collection<? super Number> coll){}
```
###  集合
* 存储数据的容器
* 数组长度不可变；集合长度可变，动态进行调整
* 数组可以存储任意数据类型的元素，集合只能存储引用类型的元素
* Collection 常用API
	* Collection是所有单列集合的父接口，因此在Collection中定义了单列集合(List和Set)通用的一些方法，这些方法可用于操作所有的单列集合。方法如下：
	* `public boolean add(E e)`：  把给定的对象添加到当前集合中 。
	* `public void clear()` :清空集合中所有的元素。
	* `public boolean remove(E e)`: 把给定的对象在当前集合中删除。
	* `public boolean contains(Object obj)`: 判断当前集合中是否包含给定的对象。
	* `public boolean isEmpty()`: 判断当前集合是否为空。
	* `public int size()`: 返回集合中元素的个数。
	* `public Object[] toArray()`: 把集合中的元素，存储到数组中
#####  [collection接口](https://blog.csdn.net/qq_40722827/article/details/103260697)
详细请戳：https://blog.csdn.net/qq_40722827/article/details/103260697
* 向上转型后的方法，只能访问接口的方法
* removeIf()   按条件删除元素
1. [List接口](https://blog.csdn.net/qq_40722827/article/details/103242486)
* List集合中的元素是有序的，不去重的

2. [Set接口](https://blog.csdn.net/qq_40722827/article/details/103249949)
* TreeSet  是Set的接口的实现类，同时也是SortedSet接口的实现类。



#####  Map接口

###  File类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200303140834757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030314113526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200303141635633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)

### Io流
四个父类

###  NIO
* JDK1.4  出现用来替代传统IO的一套新的API.
* NIO是非阻塞型，IO是阻塞型
* NIO面向缓冲区，基于通道的
---------------------------------
**files工具类**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200303152218963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)

###  缓冲区
###  线程与进程
###  反射

**你知道的越多，你不知道的越多。**
