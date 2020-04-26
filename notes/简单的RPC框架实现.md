> RPC(Remote Proceduce Call 远程过程调用) 一般用来实现部署在不同机器上的系统之间的方法调用，使程序能够像访问本地系统资源一样，通过网络传输过去访问远端系统资源。
###  基础概念
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424181109362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)


**远程调用分为`本地调用端`与`远程服务端`**

* 调用者根据服务接口获得对应的代理对象，然后直接调用接口的方法即可获得返回结果，可以实现像调用本地服务一样调用远程服务；
* 本地调用端主要通过动态代理的方式来实现上述功能，调用接口方法的时候，其代理对象实现了具体的网络通讯细节，将接口名、方法名、方法参数等请求信息发送给远程服务端并等待远程服务端的返回信息；
* 远程服务端根据请求信息通过反射获得具体的服务实现类，执行实现类的相应方法后并将调用结果返回给调用端；调用端接收到返回值，代理对象将其封装为返回结果给调用者， 整个远程调用即结束。
###  代码实现
* 远程服务接口
```java
public interface HelloService {
    public String sayHi(String name);
}
```
* 远程服务接口实现类
```java
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHi(String name) {
       return "hi,"+name;
    }
}
```
* 服务端发布服务

```java
public class RPCServerTest {
    public static void main(String[] args) throws Exception {
        Server server = new ServerCenter(8888);
        //将服务端的接口信息注册到注册中心
        server.register(HelloService.class, HelloServiceImpl.class);
        server.start();
    }
}
```
* 服务注册中心接口

```java
public interface Server {
    public void start();
    public void stop();
    //注册服务
    public void register(Class<?> service,Class serviceImpl);
}
```

* 服务注册中心实现类

```java
public class ServerCenter implements Server {
    //serviceRegiser 存储了服务名称和服务对象的关系。
    private static HashMap<String,Object> serviceRegiser = new HashMap<>();
    private static int port;
    private static ExecutorService executor = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

    private static boolean isRunning = false;

    public ServerCenter(int port){
        this.port = port;
    }
    @Override
    public void start(){
        ServerSocket server = null;
        try {
            server = new ServerSocket();
            server.bind(new InetSocketAddress(port));
        }catch (Exception e){
            e.printStackTrace();
        }

        isRunning = true;

        while (true){
                System.out.println("sart server....");
                Socket socket = null;
                try {
                    //等待客户端连接
                    socket = server.accept();
                    executor.execute(new ServiceTask(socket));
                }catch (Exception e){
                    e.printStackTrace();
                }
        }

    }

    @Override
    public void stop() {
        isRunning = false;
        executor.shutdown();
    }

    @Override
    public void register(Class<?> service, Class serviceImpl) {
        serviceRegiser.put(service.getName(),serviceImpl);
    }


    static class ServiceTask implements Runnable{
        private Socket socket;
        public ServiceTask(Socket socket) {
            this.socket = socket;
        }
        @Override
        public void run() {
            ObjectInputStream input = null;
            ObjectOutputStream output = null;
            try {
                System.out.println("收到客户端连接请求，处理该请求---------");
                //收到客户端连接请求，处理该请求
                input = new ObjectInputStream(socket.getInputStream());
                String serviceName  = input.readUTF();
                String methodName = input.readUTF();
                Class[] parameterTypes = (Class[])input.readObject();
                Object[] arguments = (Object[])input.readObject();

                Class ServiceClass  = (Class) serviceRegiser.get(serviceName);
                Method method = ServiceClass.getMethod(methodName,parameterTypes);
                Object result = method.invoke(ServiceClass.newInstance(),arguments);
                output = new ObjectOutputStream(socket.getOutputStream());
                output.writeObject(result);
            } catch (Exception e){
                e.printStackTrace();
            }finally {
                try {
                    if(output != null) output.close();
                    if (input != null) input.close();
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }
    }
}
```

*  客户端代理实现

```java
public class Client {
    public static <T>  T getRemoteProxyObj(Class serviceInterface, InetSocketAddress address){
        return (T)Proxy.newProxyInstance(serviceInterface.getClassLoader(), new Class<?>[]{serviceInterface}, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) {

                ObjectInputStream input = null;
                ObjectOutputStream output = null;
                Socket socket = null;
                try {
                    //客户端向服务端发送请求，请求具体的某一个端口
                    socket = new Socket();
                    socket.connect(address);

                    output = new ObjectOutputStream(socket.getOutputStream()); //发送序列流
                    output.writeUTF(serviceInterface.getName());
                    output.writeUTF(method.getName());
                    output.writeObject(method.getParameterTypes());
                    output.writeObject(args);

                    //等待服务端处理...
                    input = new ObjectInputStream(socket.getInputStream());

                    return input.readObject();
                }catch (Exception e){
                    e.printStackTrace();
                    return null;
                }finally {
                    try {
                        if(output != null) output.close();
                        if (input != null) input.close();
                    }catch (Exception e){
                        e.printStackTrace();
                    }
                }
            }
        });
    }
}
```

* 构建一个Socket，连接远程服务。
* 向远程服务发送数据。（方法名和方法参数）
* 接收远程服务响应的数据。
--------------

*  客户端调用

```java
public class RPCClientTest {
    public static void main(String[] args) throws ClassNotFoundException {
        HelloService service1 = Client.getRemoteProxyObj(Class.forName("network.rpc.server.HelloService"),new InetSocketAddress("127.0.0.1",8888));
        System.out.println(service1.sayHi("Carroll"));
    }
}
```

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
