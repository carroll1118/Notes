> `动态语言`：程序运行时，可以改变程序结构或变量类型。
* JAVA不是动态语言，JAVA可以称之为“准动态语 言”。但是JAVA有一定的动态性，我们可以利用反射机制、 字节码操作获得类似动态语言的特性。
# 推荐阅读
* [JVM从入门到地狱，你想要的样子它都有（O(∩_∩)O）~~~](https://blog.csdn.net/qq_40722827/article/details/105271939)
* [JVM大厂高频面试题，连这些都不知道，还敢说自己学过JVM?](https://blog.csdn.net/qq_40722827/article/details/105278081)
* [MySQL性能分析神器 Explain,你还不知道它？那你就out了](https://blog.csdn.net/qq_40722827/article/details/105239545)
* [更多精彩文章，请查看我的博客......](https://me.csdn.net/qq_40722827)
#  Java的动态性
* 反射机制 
* 动态编译 
* 动态执行javascript代码 
* 动态字节码操作

> Java 反射机制是一个非常强大的功能，在很多的项目比如 Spring，MyBatis 都都可以看到反射的身影。通过反射机制，我们可以在运行期间获取对象的类型信息。利用这一点我们可以实现工厂模式和代理模式等设计模式，同时也可以解决 Java 泛型擦除等令人苦恼的问题。
#  反射机制：框架设计的灵魂
> 反射机制 : 将类的各个组成部分封装为其他对象，指的是可以于运行时加载、探知、使用编译期间完全未知的类。
* 好处：可以在程序运行过程中，操作这些对象。可以解耦，提高程序的可扩展性。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403180710414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* 程序在运行状态中，可以`动态加载一个只有名称的类`，对于任意一个已加载的类，都能够知道这个类的所有属性和方法；对于任意一个对 象，都能够调用它的任意一个方法和属性；
									 ```Class c = Class.forName("cn.itcast.domain.Person"); ```。
* 加载完类之后，在堆内存中，就产生了一个 Class 类型的对象（一个类只有一个 Class 对象），这个对象就包含了完整的类的结构信息。 我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过 这个镜子看到类的结构，所以，我们形象的称之为：`反射`。

> `加载`：指Java虚拟机查找字节流（查找.class文件），并且根据字节流创建java.lang.Class对象的过程。这个过程，将类的.class文件中的二进制数据读入内存，放在运行时区域的方法区内。然后`在堆中创建java.lang.Class对象，用来封装类在方法区的数据结构`。 
> `类加载阶段：`
> （1）Java虚拟机将.class文件读入内存，通过一个类的全限定名来获取其定义的二进制字节流, 并为之创建一个Class对象。
> （2）任何类被使用时系统都会为其创建一个且仅有一个Class对象。
> （3）这个Class对象描述了这个类创建出来的对象的所有信息，比如有哪些构造方法，都有哪些成员方法，都有哪些成员变量等。
# Class类介绍
* java.lang.Class类十分特殊，用来表示java中类型 (`class`/`interface`/`enum`/`annotation`/`primitive type`/`void`)本 身。
	* Class类的对象包含了某个被加载类的结构。一个被加载的类对应一个 Class对象。 
	* 当一个class被加载，或当类加载器（class loader）的defineClass()被 JVM调用，JVM 便自动产生一个Class 对象。 
* Class类是Reflection的根源。 
	* – 针对任何您想动态加载、运行的类，唯有先获得相应的Class 对象
### 获取Class对象的方式
* `Class.forName("全类名")`：将字节码文件加载进内存，返回Class对象
	* 多用于配置文件，将类名定义在配置文件中。读取文件，加载类
		```java
		Class cls1 = Class.forName("domain.Person");
		```

* `类名.class`：通过类名的属性class获取
	* 多用于参数的传递
		```java
		Class cls2 = Person.class;
		```
* `对象.getClass()`：getClass()方法在Object类中定义着。
	* 多用于对象的获取字节码的方式
		```java
		Person p = new Person();
		Class cls3 = p.getClass();
		```
* 结论：同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。
---------
* 代码实例
* Person .java
```java
package domain;
public class Person {
    private String name;
    private int age;
    public String a;
    protected String b;
    String c;
    private String d;
    public Person() {
    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", a='" + a + '\'' +
                ", b='" + b + '\'' +
                ", c='" + c + '\'' +
                ", d='" + d + '\'' +
                '}';
    }
    public void eat(){
        System.out.println("eat...");
    }
    public void eat(String food){
        System.out.println("eat..."+food);
    }
}
```
* ReflectDemo1 .java
```java
package reflection;
import domain.Person;
/**
 * @Auther Carroll
 * @Date 2020/4/3
 * @e-mail ggq_carroll@163.com
 */
public class ReflectDemo1 {
    public static void main(String[] args) throws Exception {
        //1.Class.forName("类名")
        Class cls1 = Class.forName("domain.Person");
        System.out.println(cls1);
        //2.类名.class
        Class cls2 = Person.class;
        System.out.println(cls2);
        //3.对象.getClass()
        Person p = new Person();
        Class cls3 = p.getClass();
        System.out.println(cls3);

        //== 比较三个对象
        System.out.println(cls1 == cls2);//true
        System.out.println(cls1 == cls3);//true
    }
}
```
* 结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403201839966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
### Class对象功能
##### 获取功能
* **获取成员变量们**
	* `Field[ ] getFields()` ：获取所有public修饰的成员变量
	* `Field getField(String name)` : 获取指定名称的 public修饰的成员变量

	* `Field[ ] getDeclaredFields()` : 获取所有的成员变量，不考虑修饰符
	* `Field getDeclaredField(String name)`  : 获取指定名称的成员变量，不考虑修饰符
	* 代码示例
```java
package reflection;

import domain.Person;
import java.lang.reflect.Field;
/**
 * @Auther Carroll
 * @Date 2020/4/3
 * @e-mail ggq_carroll@163.com
 */
public class ReflectDemo2 {
    public static void main(String[] args) throws Exception {
        //获取Person的Class对象
        Class personClass = Person.class;
        //Class personClass = Class.forName("domain.Person");
        /*Person p = new Person();
        Class personClass = p.getClass();*/

        //Field[] getFields()获取所有public修饰的成员变量
        Field[] fields = personClass.getFields();
        for(Field field:fields){
            System.out.println(field);
        }
        System.out.println("----------------1");
        //Field getField(String name)   获取指定名称的 public修饰的成员变量
        Field a = personClass.getField("a");
        //获取成员变量a的值
        Person p = new Person();
        Object value = a.get(p);
        System.out.println(value);
        //设置a的值
        a.set(p,"Carroll");
        System.out.println(p);
        System.out.println("----------------2");
        //Field[] getDeclaredFields()：获取所有的成员变量，不考虑修饰符
        Field[] declaredFields = personClass.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println(declaredField);
        }
        System.out.println("----------------3");
        //Field getDeclaredField(String name)
        Field d = personClass.getDeclaredField("d");
        //忽略访问权限修饰符的安全检查
        d.setAccessible(true);//暴力反射
        Object value2 = d.get(p);
        System.out.println(value2);
        System.out.println("----------------4");
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200403204048556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
*  **获取构造方法们**
	* `Constructor<?>[ ] getConstructors()`  
	* `Constructor<T> getConstructor(类<?>... parameterTypes)`  

	* `Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)`  
	* `Constructor<?>[ ] getDeclaredConstructors()`  
* **获取成员方法们**
	* `Method[ ] getMethods()`  
	* `Method getMethod(String name, 类<?>... parameterTypes)`  

	* `Method[ ] getDeclaredMethods()`  
	* `Method getDeclaredMethod(String name, 类<?>... parameterTypes)`  
* **获取全类名**	
	* `String getName()`  

	* `Field`：成员变量
		* 操作：
		 	* 设置值：void set(Object obj, Object value)  
			* 获取值：get(Object obj) 
			* 忽略访问权限修饰符的安全检查：setAccessible(true):暴力反射
	* `Constructor`:构造方法
		* 创建对象：
			* T newInstance(Object... initargs)  
			* 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法
	* `Method`：方法对象
		* 执行方法：
			* Object invoke(Object obj, Object... args)  
		* 获取方法名称：
			* String getName:获取方法名
----------------------------
# 案例
* 需求：写一个案例，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
	* 实现：1. 配置文件     2. 反射
	* 步骤：
		* 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
		* 在程序中加载读取配置文件
		* 使用反射技术来加载类文件进内存
		* 创建对象
		* 执行方法
* **代码实现**
* `pro.properties`
```java
className=domain.Student
methodName=sleep
```
* `Student.java`
```java
package domain;
public class Student {
    public void sleep(){
        System.out.println("sleep...");
    }
}
```
* `ReflectTest .java`
```java
package reflection;
import java.io.InputStream;
import java.lang.reflect.Method;
import java.util.Properties;
/**
 * @Auther Carroll
 * @Date 2020/4/3
 * @e-mail ggq_carroll@163.com
 */
public class ReflectTest {
    public static void main(String[] args) throws Exception {
        //可以创建任意类的对象，可以执行任意方法
         /*
          前提：不能改变该类的任何代码。可以创建任意类的对象，可以执行任意方法
         */
        //1.加载配置文件
        //1.1创建Properties对象
        Properties pro = new Properties();
        //1.2加载配置文件，转换为一个集合
        //1.2.1获取class目录下的配置文件
        ClassLoader classLoader = ReflectTest.class.getClassLoader();
        InputStream is = classLoader.getResourceAsStream("pro.properties");
        pro.load(is);

        //2.获取配置文件中定义的数据
        String className = pro.getProperty("className");
        String methodName = pro.getProperty("methodName");

        //3.加载该类进内存
        Class cls = Class.forName(className);
        //4.创建对象
        Object obj = cls.newInstance();
        //5.获取方法对象
        Method method = cls.getMethod(methodName);
        //6.执行方法
        method.invoke(obj);
    }
}
```
* **结果**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040320542297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
#  反射机制的常见作用
* 动态加载类、动态获取类的信息（属性、方法、构造器）
* 动态构造对象
* 动态调用类和对象的任意方法、构造器 
* 动态调用和处理属性 
* 获取泛型信息 
* 操作注解
###  反射机制操作注解
* 可以通过反射API:getAnnotations, getAnnotation获得相关的注解信息
```java
//获得类的所有有效注解 
Annotation[] annotations = clazz.getAnnotations(); 
for (Annotation a : annotations) { 
	System.out.println(a); 
}
//获得类的指定的注解 
SxtTable st = (SxtTable) clazz.getAnnotation(SxtTable.class); 
System.out.println(st.value()); 
//获得类的属性的注解 
Field f = clazz.getDeclaredField("studentName"); 
SxtField sxtField = f.getAnnotation(SxtField.class); 
System.out.println(sxtField.columnName()+"-- "+sxtField.type()+"--"+sxtField.length());
```
### 反射机制性能问题
* `setAccessible` 
	* 启用和禁用访问安全检查的开关,值为 true 则指示反射的对象在使用时应该取消 Java 语 言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查。并不是为true 就能访问为false就不能访问。 – 禁止安全检查，可以提高反射的运行速度

**你知道的越多，你不知道的越多。**
