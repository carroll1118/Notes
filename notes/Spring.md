### BeanFactory 和 ApplicationContext 有什么区别
* BeanFactory 可以理解为含有 bean 集合的工厂类。BeanFactory 包含了 bean 的定义，以便在接收到客户端请求时将对应的 bean 实例化。
* BeanFactory 还能在实例化对象的时生成协作类之间的关系。此举将 bean 自身与 bean 客户端的配置中解放出来。BeanFactory 还包含了 bean 生命周期的控制，调用客户端的初始化方法（initialization methods）和销毁方法（destruction methods）。
* 从表面上看，ApplicationContext 如同 BeanFactory 一样具有 bean 定义、bean 关联关系的设置，根据请求分发 bean 的功能。但 ApplicationContext 在此基础上还提供了其他的功能：
	* 提供了支持国际化的文本消息
	* 统一的资源文件读取方式
	* 已在监听器中注册的 bean 的事件

### Spring Bean 的生命周期
* Spring Bean 的生命周期简单易懂。在一个 bean 实例被初始化时，需要执行一系列的初始化操作以达到可用的状态。同样的，当一个 bean 不在被调用时需要进行相关的析构操作，并从 bean 容器中移除。
* Spring bean factory 负责管理在 spring 容器中被创建的 bean 的生命周期。Bean 的生命周期由两组回调（call back）方法组成。
	* 初始化之后调用的回调方法。
	* 销毁之前调用的回调方法。
* Spring 框架提供了以下四种方式来管理 bean 的生命周期事件：
	* InitializingBean 和 DisposableBean 回调接口
	* 针对特殊行为的其他 Aware 接口
	* Bean 配置文件中的 Custom init() 方法和 destroy() 方法
	* @PostConstruct 和 @PreDestroy 注解方式

### Spring IOC 如何实现
* Spring 中的 `org.springframework.beans` 包和 `org.springframework.context` 包构成了 Spring 框架 IoC 容器的基础。
* BeanFactory 接口提供了一个先进的配置机制，使得任何类型的对象的配置成为可能。ApplicationContext 接口对 BeanFactory（是一个子接口）进行了扩展，在 BeanFactory 的基础上添加了其他功能，比如与 Spring 的 AOP 更容易集成，也提供了处理 message resource 的机制（用于国际化）、事件传播以及应用层的特别配置，比如针对 Web 应用的 WebApplicationContext。
* `org.springframework.beans.factory.BeanFactory` 是 Spring IoC 容器的具体实现，用来包装和管理前面提到的各种 bean。BeanFactory 接口是 Spring IoC 容器的核心接口。
### 说说 Spring AOP
* 面向切面编程，在我们的应用中，经常需要做一些事情，但是这些事情与核心业务无关，比如，要记录所有 update 方法的执行时间时间，操作人等等信息，记录到日志， 通过 Spring 的 AOP 技术，就可以在不修改 update 的代码的情况下完成该需求。
### Spring AOP 实现原理
* Spring AOP 中的动态代理主要有两种方式：`JDK 动态代理` 和 `CGLIB 动态代理`。JDK 动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK 动态代理的核心是 InvocationHandler 接口和 Proxy 类。
* 如果目标类没有实现接口，那么 Spring AOP 会选择使用 CGLIB 来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB 是通过继承的方式做的动态代理，因此如果某个类被标记为 final，那么它是无法使用 CGLIB 做动态代理的。
### 动态代理（CGLIB 与 JDK）
* JDK 动态代理类和委托类需要都实现同一个接口。也就是说只有实现了某个接口的类可以使用 Java 动态代理机制。但是，事实上使用中并不是遇到的所有类都会给你实现一个接口。因此，对于没有实现接口的类，就不能使用该机制。而 CGLIB 则可以实现对类的动态代理。
### Spring 事务实现方式
* 编码方式
	* 所谓编程式事务指的是通过编码方式实现事务，即类似于 JDBC 编程实现事务管理。
* 声明式事务管理方式
	* 声明式事务管理又有两种实现方式：
		* 基于 xml 配置文件的方式；
		* 另一个实在业务方法上进行 `@Transaction` 注解，将事务规则应用到业务逻辑中；
### Spring 事务底层原理
* **划分处理单元 IOC**
	* 由于 Spring 解决的问题是对单个数据库进行局部事务处理的，具体的实现首相用 Spring 中的 IOC 划分了事务处理单元。并且将对事务的各种配置放到了 IOC 容器中（设置事务管理器，设置事务的传播特性及隔离机制）。

* **AOP 拦截需要进行事务处理的类**
	* Spring 事务处理模块是通过 AOP 功能来实现声明式事务处理的，具体操作（比如事务实行的配置和读取，事务对象的抽象），用 TransactionProxyFactoryBean 接口来使用 AOP 功能，生成 proxy 代理对象，通过 TransactionInterceptor 完成对代理方法的拦截，将事务处理的功能编织到拦截的方法中。读取 IOC 容器事务配置属性，转化为 Spring 事务处理需要的内部数据结构（TransactionAttributeSourceAdvisor），转化为 TransactionAttribute 表示的数据对象。
* **对事物处理实现（事务的生成、提交、回滚、挂起）**
	* Spring 委托给具体的事务处理器实现。实现了一个抽象和适配。适配的具体事务处理器：DataSource 数据源支持、Hibernate 数据源事务处理支持、JDO 数据源事务处理支持，JPA、JTA 数据源事务处理支持。这些支持都是通过设计 PlatformTransactionManager、AbstractPlatforTransaction 一系列事务处理的支持。 为常用数据源支持提供了一系列的 TransactionManager。
* **结合**
	* PlatformTransactionManager 实现了 `TransactionInterception` 接口，让其与 TransactionProxyFactoryBean 结合起来，形成一个 Spring 声明式事务处理的设计体系。
### 如何自定义注解实现功能
1. 创建自定义注解和创建一个接口相似，但是注解的 interface 关键字需要以 @ 符号开头。
2. 注解方法不能带有参数；
3. 注解方法返回值类型限定为：基本类型、String、Enums、Annotation 或者是这些类型的数组；
4. 注解方法可以有默认值；
5. 注解本身能够包含元注解，元注解被用来注解其它注解。
### Spring MVC 运行流程
* Spring MVC 将所有的请求都提交给 DispatcherServlet，它会委托应用系统的其他模块负责对请求进行真正的处理工作。
* DispatcherServlet 查询一个或多个 HandlerMapping，找到处理请求的 Controller.
* DispatcherServlet 请求提交到目标 Controller
* Controller 进行业务逻辑处理后，会返回一个 ModelAndView
* Dispatcher 查询一个或多个 ViewResolver 视图解析器,找到 ModelAndView 对象指定的视图对象
* 视图对象负责渲染返回给客户端
### Spring MVC 启动流程
* 在 web.xml 文件中给 Spring MVC 的 Servlet 配置了 load-on-startup，所以程序启动的时候会初始化 Spring MVC，在 HttpServletBean 中将配置的 contextConfigLocation 属性设置到 Servlet 中，然后在 FrameworkServlet 中创建了 WebApplicationContext，DispatcherServlet 根据 contextConfigLocation 配置的 classpath 下的 xml 文件初始化了 Spring MVC 总的组件。
### Spring 的单例实现原理
* Spring 对 Bean 实例的创建是采用单例注册表的方式进行实现的，而这个注册表的缓存是 `ConcurrentHashMap` 对象。
### Spring 框架中用到了哪些设计模式
* `代理模式`：在 AOP 和 Remoting 中被用的比较多。
* `单例模式`：在 Spring 配置文件中定义的 Bean 默认为单例模式。
* `模板方法`：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
* `前端控制器`：Spring 提供了 DispatcherServlet 来对请求进行分发。
* `视图帮助`(View Helper )：Spring 提供了一系列的 JSP 标签，高效宏来辅助将分散的代码整合在视图里。
* `依赖注入`：贯穿于 BeanFactory / ApplicationContext 接口的核心理念。
* `工厂模式`：BeanFactory 用来创建对象的实例。

**你知道的越多，你不知道的越多。**
