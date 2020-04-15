[java基础（一）](https://blog.csdn.net/qq_40722827/article/details/104591885)
[java基础（二）](https://blog.csdn.net/qq_40722827/article/details/104607292)
[java基础（三）](https://blog.csdn.net/qq_40722827/article/details/104627933)

@[toc]
###  几大排序算法
* 选择排序
* 冒泡排序
* 快速排序
* 二分排序
* 希尔排序
* 桶排序
* 堆排序

### Arrays工具类
* Arrays.sort(array)   :   排序
* Arrays.toString(array) : 将数组中内容拼接成字符串
* Arrays.binarySearch(array,5) : 使用二分查找法，查询元素的下标
* Arrays.copyOf(array，10) : 从数组中拷贝指定数量的元素，并放到一个新的数组中返回
* Arrays.copyOfRange(array,1,4) ：将数组的[1,4）范围的元素拷贝到一个新数组，并返回
* Arrays.equals(array，array) : 判断两个数组是否相同（比较的不是地址，是数组中 的元素）
* Arrays.fill(array,10)  : 将数组的元素填充为指定的值
###  面向对象
* 类和对象的关系：类是对象的集合，对象是类的个体
```java
//类的定义
[访问权限修饰符] class 类名{
   //类体
   //类的所有对象的共同特征(属性)和行为(方法)
}
//类名命名用大驼峰
```

* ==类是一种自定义的数据类型==
* 类是一种自定义的引用数据类型
* 方法存储于 方法区，没有在堆上开辟空间

* 非静态成员，是属于对象的。在访问的时候需要使用对象访问
* 静态成员，用static修饰的，是属于类的。访问的时候，需要用类来访问。（静态成员也可以用对象访问，但是不推荐）
* 非静态属性，空间的开辟是在对象实例化得时候在堆空间开辟，随着对象的销毁而销毁
* 静态属性，空间的开辟是在类第一次加载到jvm内存（程序第一次使用到这个类的时候）
* 非静态方法中可以直接访问静态成员和非静态成员。
* 静态方法只能直接调用静态成员。

###  构造方法
* 构造方法在实例化的时候被调用
* 构造方法必须与类名相同
* 构造方法不能被显示调用

### this关键字
* this表示对当前对象的引用
* 用在非静态方法中，谁调用方法，this就代表谁
* 构造方法中，this表示刚刚实例化的对象
* this() 带调用当前类的其他构造方法，必须放在第一行。不能循环调用
###  封装
* 面向对象的三大特性：封装、继承、多态

### 继承

### 方法的重写
* 在子类中，对从父类继承的方法进行重新实现
* @Override    :   注解，系统内置的。用于方法前。作用：检测下面的方法是不是重写的方法

### final关键字
* 修饰变量：表示变量的值不可被改变
* 修饰类，不可以被继承
* 修饰方法，不可以被重写

###  Object类
**Object类的几个方法**
* getClass
* equals
* hashCode
* toString
### 多态
### 抽象类
* 抽象类不能实例化对象

### 接口
* 接口中可以写方法，默认public  abstract权限
* 接口中得属性默认 public static  final 
* java1.8新特性，接口中的静态方法，只能由接口自己调用实现
* java1.8新特性， default就是给接口添加一个默认的方法，实现这个接口的时候，可以重写这个方法，可以使用默认的
* 一个接口可以有多个父接口
* 接口之间是有继承的，多继承
* 接口起到了规范的作用

* 函数式接口：接口内有且只有一个必须要实现的方法
* @FunctionalInterface  用来判断一个接口是否是函数式接口
### 包和权限修饰符

|                  | public | protected | 缺省（空的） | private |
| ---------------- | ------ | --------- | ------------ | ------- |
| 同一类中         | √      | √         | √            | √       |
| 同一包中的类     | √      | √         | √            |         |
| 不同包的子类     | √      | √         |              |         |
| 不同包中的无关类 | √      |           |              |         |

* 可见，public具有最大权限。private则是最小权限。

###  lambda表达式
**java8 新特性**

1. lambda表达式本质上是一个匿名函数
2. 一般配合抽象类和接口使用，配合接口居多
3.  “-> ” ,lambda运算符，读作“goes  to”

* 方法引用

* 接口引用

###  包装类
* 自动装箱：可以直接把基本数据类型的变量或者值赋值给对应的包装类对象。
* 自动拆箱：可以把包装类的对象赋值给基本数据类型的变量。
* jdk 1.4 之后，自动实现装箱和拆箱
* 享元原则

| 基本类型 | 对应的包装类（位于java.lang包中） |
| -------- | --------------------------------- |
| byte     | Byte                              |
| short    | Short                             |
| int      | **Integer**                       |
| long     | Long                              |
| float    | Float                             |
| double   | Double                            |
| char     | **Character**                     |
| boolean  | Boolean                           |
**字符串与基本数据类型的转换**
* 基本数据类型转为字符串:String.valueOf(n)
* 基本数据类型转为字符串 : Integer.valueOf(n).toString()
* 字符串转换为基本数据类型：Integer.valueOf(str)
* 字符串转换为基本数据类型：Integer.parseInt(str)
### 数学类：Math
* Random类：在java.util包里
* 常用方法

| 方法名                                       | 说明                |
| -------------------------------------------- | ------------------- |
| public static int abs(int a)                 | 获取参数a的绝对值： |
| public static double ceil(double a)          | 向上取整            |
| public static double floor(double a)         | 向下取整            |
| public static double pow(double a, double b) | 获取a的b次幂        |
| public static long round(double a)           | 四舍五入取整        |

### 日期类
* Date
* SimpleDateFormat
* Calendar
* DateFormat类的常用方法有：
	- `public String format(Date date)`：将Date对象格式化为字符串。
	- `public Date parse(String source)`：将字符串解析为Date对象。
### BigDecimal类
| 构造方法名             | 描述                                            |
| ---------------------- | ----------------------------------------------- |
| BigDecimal(double val) | 将double类型的数据封装为BigDecimal对象          |
| BigDecimal(String val) | 将 BigDecimal 的字符串表示形式转换为 BigDecimal |

注意：推荐使用第二种方式，第一种存在精度问题；

* BigDecimal类中使用最多的还是提供的进行四则运算的方法，如下：

| 方法声明                                     | 描述     |
| -------------------------------------------- | -------- |
| public BigDecimal add(BigDecimal value)      | 加法运算 |
| public BigDecimal subtract(BigDecimal value) | 减法运算 |
| public BigDecimal multiply(BigDecimal value) | 乘法运算 |
| public BigDecimal divide(BigDecimal value)   | 触发运算 |

注意：对于divide方法来说，如果除不尽的话，就会出现java.lang.ArithmeticException异常。此时可以使用divide方法的另一个重载方法；

> BigDecimal divide(BigDecimal divisor, int scale, int roundingMode): divisor：除数对应的BigDecimal对象；scale:精确的位数；roundingMode取舍模式

> 小结：Java中小数运算有可能会有精度问题，如果要解决这种精度问题，可以使用BigDecimal
### 枚举
### [异常](https://blog.csdn.net/qq_40722827/article/details/103177498)
详细信息：https://blog.csdn.net/qq_40722827/article/details/103177498
* throw  写在方法体中，throw 之后的代码不在执行
* throws  写在方法声明部分，表示将异常抛出。谁调用方法，谁处理异常
###  字符串
* 字符串 底层维护了一个字符数组
* 字符串是在常量池开辟的空间，遵循享元原则
* 字符串比较用equals，不要用 ==
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302183329844.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030218345491.png)
### String转换成基本类型 
* 除了Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：
	- `public static byte parseByte(String s)`：将字符串参数转换为对应的byte基本类型。
	- `public static short parseShort(String s)`：将字符串参数转换为对应的short基本类型。
	- **`public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。**
	- **`public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。**
	- `public static float parseFloat(String s)`：将字符串参数转换为对应的float基本类型。
	- `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。
	- `public static boolean parseBoolean(String s)`：将字符串参数转换为对应的boolean基本类型。
	
### [java中String、StringBuffer、StringBuilder](https://blog.csdn.net/qq_40722827/article/details/103154322)
详细信息：https://blog.csdn.net/qq_40722827/article/details/103154322

### 正则表达式
```java
//一位的约束
\d : 匹配所有的数字[0-9]
\D : 匹配所有的非数字[^0-9]
\w : 匹配所有的单词字符[a-zA-Z0-9_]
\W : 匹配所有的非单词字符[^a-zA-Z0-9_]
^ : 匹配一个字符串的开头
$ : 匹配一个字符串的结尾
[] : 匹配字符串中的一位字符
[abc] : 需要匹配一个字符可以是 a,b,c 其中之一
[a-z] : 匹配的字符可以是a到z的任意一位字符
[^abc] : 需要匹配一个字符可以是除了 a,b,c 之外的任意字符

java 正则表达式的转义字符：\\
.  :  代表通配符

//多位的约束
+ : 前面的一位或者一组字符连续出现了1次或多次
? : 前面的一位或者一组字符连续出现了0次或1次
* : 前面的一位或者一组字符连续出现了0次或多次(这个的多次包含1次)
{m} : 前面的一位或者一组字符连续出现了m次
{m,} : 前面的一位或者一组字符连续出现了至少m次
{m,n} : 前面的一位或者一组字符连续出现了至少m次,至多n次
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
