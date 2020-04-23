> IO的方式通常分为几种，同步阻塞的BIO、同步非阻塞的NIO、异步非阻塞的AIO。
### 关键词解释
* **同步与异步（synchronous/asynchronous）**：同步是一种可靠的有序运行机制，当我们进行同步操作时，后续的任务是等待当前调用返回，才会进行下一步；而异步则相反，其他任务不需要等待当前调用返回，通常依靠事件、回调等机制来实现任务间次序关系。
* **阻塞与非阻塞**：在进行阻塞操作时，当前线程会处于阻塞状态，无法从事其他任务，只有当条件就绪才能继续，例如：新连接建立完毕、数据读取、写入操作完成；而非阻塞则是不管IO操作是否结束，直接返回，相应操作在后台继续处理。

**同步和异步的概念：实际的I/O操作**
* 同步是用户线程发起I/O请求后需要等待或者轮询内核I/O操作完成后才能继续执行。
* 异步是用户线程发起I/O请求后仍需要继续执行，当内核I/O操作完成后会通知用户线程，或者调用用户线程注册的回调函数。

**阻塞和非阻塞的概念：发起I/O请求**
* 阻塞是指I/O操作需要彻底完成后才能返回用户空间
* 非阻塞是指I/O操作被调用后立即返回一个状态值，无需等I/O操作彻底完成

------------

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200422201753469.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
###  传统的IO
**传统的 IO 大致可以分为4种类型：**
* InputStream、OutputStream 基于字节操作的 IO
* Writer、Reader 基于字符操作的 IO
* File 基于磁盘操作的 IO
* Socket 基于网络操作的 IO

**java.io 下的类和接口很多，但大部分都是 InputStream、OutputStream、Writer、Reader 的子集。**
###  BIO、NIO、AIO的区别
* BIO 就是传统的 java.io 包，它是基于流模型实现的，交互的方式是同步、阻塞方式，也就是说在读入输入流或者输出流时，在读写动作完成之前，线程会一直阻塞在那里，它们之间的调用时可靠的线性顺序。它的有点就是代码比较简单、直观；缺点就是 IO 的效率和扩展性很低，容易成为应用性能瓶颈。（**BIO是一个连接一个线程**）
* NIO 是 `Java 1.4` 引入的 java.nio 包，提供了 `Channel(通道)`、`Selector(选择器)`、`Buffer(缓冲区)` 等新的抽象，可以构建多路复用的、同步非阻塞 IO 程序，同时提供了`更接近操作系统底层高性能的数据操作方式`。（**NIO是一个请求一个线程**）
* AIO（Asynchronous IO）是 `Java 1.7` 之后引入的包，是 NIO 的升级版本，提供了异步非阻塞的 IO 操作方式。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。（**AIO是一个有效请求一个线程**）
###  传统的 Socket 实现（BIO）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200422214802494.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* 通过线程池实现一个服务端可以同时接收多个客户端的消息。
```java
//自定义线程池
public class HandlerSocketThreadPool {
    private ExecutorService executorService;
    public HandlerSocketThreadPool(int maxPoolSize,int queueSize){
        executorService = new ThreadPoolExecutor(
                maxPoolSize,
                maxPoolSize,
                120L,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<Runnable>(queueSize));
    }

    public void execute(Runnable task){
        this.executorService.execute(task);
    }
}
```

```java
public class Client_Threadpool {
    public static void main(String[] args) throws Exception {
        System.out.println("客户端启动成功~~~");
        //客户端要请求与服务端的scoket管道连接
        Socket socket = new Socket("127.0.0.1",9999);
        //从socket通信管道中得到一个字节输出流
        OutputStream outputStream = socket.getOutputStream();
        //低级的字节输出流包装成高级的打印流
        PrintStream printStream = new PrintStream(outputStream);
        //开始发送消息出去
        //printStream.println("我是客户端，喜欢你很久了，第一次给你发消息，今晚一起吃个饭?");  //单次发送
        while (true){
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入：");
            printStream.println(sc.nextLine());
            printStream.flush();
            System.out.println("客户端本次发送完毕~~~");
        }
    }
}
```

```java
public class Server_Thredpool {
    public static void main(String[] args) throws Exception {
        try {
            System.out.println("TCP服务端启动~~~");
            //注册端口
            ServerSocket serverSocket = new ServerSocket(9999);
            //一个服务端只需要对应一个线程池
            HandlerSocketThreadPool handlerSocketThreadPool =
                    new HandlerSocketThreadPool(3,100);
            while (true){
                //开始等待客户端的Socket管道连接  accept()方法，阻塞等待客户端连接
                Socket socket = serverSocket.accept();

                System.out.println("有人上线了");
                //每次接收到客户端发来的请求，就单独创建一个线程来处理
                handlerSocketThreadPool.execute(new ReaderClientRunnable(socket));
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}

class ReaderClientRunnable implements Runnable{
    private Socket socket;
    public ReaderClientRunnable(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            //读取一行数据
            InputStream inputStream = socket.getInputStream();
            //转成一个缓冲字符流
            Reader fr = new InputStreamReader(inputStream);
            BufferedReader bufferedReader = new BufferedReader(fr);
            //按行读取数据
            String line = null;
            while ((line = bufferedReader.readLine())!=null){
                System.out.println("服务端收到"+socket.getInetAddress()+"发来的数据："+line);
            }
        }catch (Exception e){
            System.out.println(socket.getInetAddress()+"下线了");
        }
    }
}
```

###  NIO 的Socket 实现
> NIO 是利用了单线程轮询事件的机制，通过高效地定位就绪的 Channel，来决定做什么，仅仅 select 阶段是阻塞的，可以有效避免大量客户端连接时，频繁线程切换带来的问题，应用的扩展能力有了非常大的提高。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200422214820752.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* NIO通信的三要素：Channel(通道)，Buffer(缓冲池),Selector(多路复用器)。
--------------
* `缓冲区Buffer`：Buffer是一个对象。它包含一些要写入或者读出的数据。在面向流的I/O中，可以将数据写入或者将数据直接读到Stream对象中，在NIO中，所有的数据都是用缓冲区处理。
* `通道Channel`：Channel是一个通道，可以通过它读取和写入数据，他就像自来水管一样，网络数据通过Channel读取和写入。通道和流不同之处在于通道是双向的，流只是在一个方向移动，而且通道可以用于读，写或者同时用于读写。Channel是全双工的，所以它比流更好地映射底层操作系统的API，特别是在UNIX网络编程中，底层操作系统的通道都是全双工的，同时支持读和写。
	* **Channel有四种实现**：
	* `FileChannel`:从文件中读取数据。
	* `DatagramChannel`:从UDP网络中读取或者写入数据。
	*  `SocketChannel`:从TCP网络中读取或者写入数据。
	* `ServerSocketChannel`:允许你监听来自TCP的连接，就像服务器一样。每一个连接都会有一个SocketChannel产生。
* `多路复用器Selector`：Selector选择器可以监听多个Channel通道感兴趣的事情(read、write、accept(服务端接收)、connect，实现一个线程管理多个Channel，节省线程切换上下文的资源消耗。Selector只能管理非阻塞的通道，FileChannel是阻塞的，无法管理。
```java
// NIO模式 非阻塞式 IO 支持高并发的！！
public class Client {
	public static void main(String[] args) {
		try {
			// 1.创建连接服务端的地址
			InetSocketAddress socketAddress =
					new InetSocketAddress("127.0.0.1", 9999);
			// 2.创建一个与服务端的连接通道 ，注意此时并没有连接服务端！！通道！！
			SocketChannel channel = SocketChannel.open() ;
			// 3.连接通道 : 让通道与服务端的地址接通 
			channel.connect(socketAddress);
			// 4.创建缓冲区，非阻塞，数据是不会直接与服务端通信的，交给缓冲区！
			ByteBuffer buf = ByteBuffer.allocate(1024); // 1kB
			Scanner sc = new Scanner(System.in);
			// 5.循环发送数据给服务端
			while(true){
				// 定义一个字节数组封装用户输入的数据
				// 接收用户的键盘输入，存储到msg中去
				String msg = sc.nextLine();
				byte[] buffer = msg.getBytes();
				// 把数据写给缓冲区
				buf.put(buffer);
				// 复位缓冲区 ,把指针放到第一个位置！！
				buf.flip();
				// 开始写出数据 , 把缓冲区发给了服务端
				channel.write(buf);
				// 清空缓冲区 
				buf.clear();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

```java
/**
 * NIO服务端:
 * 		1.接收无数个客户端的连接。
 * 		2.得到一个多路复用器对象去轮询所有的客户端管道。
 * 		3.发现该管道有数据过来触发读取，没有就继续轮询下一个客户端管道。
 */
public class NIOServer {
	// 1.创建通道的管理器 : 多路复用器（轮询客户端的关键对象！）
	// 管理多个客户端通道
	private Selector selector ;
	private ServerSocketChannel channel; // 负责接收客户端的连接等！
	// 创建好多路复用器对象
	public NIOServer(int port) {
		try {
			// 打开通道管理器，创建一个多路复用器，管理通道 
			// 创建一个多路复用器
			this.selector = Selector.open();
			// 2.创建一个服务器的通道用于接收客户端的连接 
			// ServerSocketChannel 这个对象是接收客户端的
			channel = ServerSocketChannel.open() ;
			// 绑定服务端的通道端口
			channel.bind(new InetSocketAddress(9999));
			// 3.设置通道为非阻塞 
			channel.configureBlocking(false);
			// 4.让多路复用器开始负责使用管道接收客户端的连接请求 -> 然后管理通道
			// 一个线程：多路复用器：selector
			// 一个线程：管道channel
			channel.register(selector,  SelectionKey.OP_ACCEPT);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	private void listen() {
		try {
			// 多路复用器，开始轮询客户端通道，看是否有事件需要处理！
			while(true){
				System.out.println("轮询了一次~~~");
				//1 必须要让多路复用器开始监听
				// 开始轮询
				this.selector.select();
				//2.通过多路选择器轮询所有的客户端事件状态
				// 提取所有通道的状态
				Iterator<SelectionKey> keys = selector.selectedKeys().iterator();
				//3.遍历这些状态   [c1 ,c2 ,c3 ,c4.....]
				while(keys.hasNext()){
					System.out.println("事件处理了一次~~~");
					// 该通道的事件对象！！
					SelectionKey key = keys.next() ;
					// 删除即将处理的key，以防止被重复处理 
					keys.remove();
					// 开始处理状态 
					if(key.isAcceptable()){
						// 客户端发来了连接请求
						// 处理与客户端的连接
						handlerAccept(key);
					}else if(key.isReadable()){
						// 获得了可读事件
						handlerReader(key);
					}
				}
			}
		} catch (Exception e) {
			System.out.println("在轮询是出现异常！");
		}
		
	}

	private void handlerAccept(SelectionKey key) {
		try {
			// 1.处理与客户端通道的连接
			// 获取服务端通道
			ServerSocketChannel ss = (ServerSocketChannel) key.channel();
			// 接收客户端的请求
			SocketChannel channel = ss.accept();
			// 设置非阻塞模式
			channel.configureBlocking(false);
			// 注册到多路通道上去(申明这个管道是可读了！)
			channel.register(selector, SelectionKey.OP_READ);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	private void handlerReader(SelectionKey key) {
		try {
			// 获取通道对象
			SocketChannel channel = (SocketChannel) key.channel();
			// 定义缓冲区读取数据 
			ByteBuffer buffer = ByteBuffer.allocate(1024);
			// 读取通道的数据放置到缓冲区中去 
			int count = channel.read(buffer);
			// 判断读取的结果 
			if(count == -1 ){
				// 说明没有数据可读
				key.channel().close();
				key.cancel();
				System.out.println("通道关闭了一个客户端！！");
				return ;
			}
			//5 有数据则进行读取 读取之前需要进行复位方法(把position 和limit进行复位)
			buffer.flip();
			//6 根据缓冲区的数据长度创建相应大小的byte数组，接收缓冲区的数据
			byte[] bytes = new byte[buffer.remaining()];
			//7 接收缓冲区数据
			buffer.get(bytes);
			//8 打印结果
			String body = new String(bytes).trim();
			System.out.println("Server : " + body);
		} catch (Exception e) {
			try {
				// 从服务端移除该客户端的通道对象
				key.channel().close();
				key.cancel();
			} catch (IOException ex) {
				ex.printStackTrace();
			}
		}
	}

	public static void main(String[] args) {
		// 初始化服务端:只是做好了多路复用器：管理客户端通道的Selector对象！！
		NIOServer server = new NIOServer(9999);
		// 监听轮询客户端！！ 深刻理解NIO
		server.listen();
	}
}
```

###  AIO 的 Socket 实现

> AIO最大的一个特性就是异步能力，这种能力对socket与文件I/O都起作用。AIO其实是一种在读写操作结束之前允许进行其他操作的I/O处理。AIO是对JDK1.4中提出的同步非阻塞I/O(NIO)的进一步增强。

**增加的新的类**

* `AsynchronousChannel`：支持异步通道，包括服务端AsynchronousServerSocketChannel和普通AsynchronousSocketChannel等实现。
* `CompletionHandler`：用户处理器。定义了一个用户处理就绪事件的接口，由用户自己实现，异步io的数据就绪后回调该处理器消费或处理数据。
* `AsynchronousChannelGroup`：一个用于资源共享的异步通道集合。处理IO事件和分配给CompletionHandler

**在java.nio.channels包下增加了下面四个异步通道**
* `AsynchronousSocketChannel`
* `AsynchronousServerSocketChannel`
* `AsynchronousFileChannel`
* `AsynchronousDatagramChannel`
```java
public class Client{
    public static void main(String[] args) throws Exception {
        AsynchronousSocketChannel client = AsynchronousSocketChannel.open();
        client.connect(new InetSocketAddress("localhost", 9999));
        client.write(ByteBuffer.wrap("just a test".getBytes())).get();
    }

}
```

```java
public class Server {
    public final static int PORT = 9999;
    private AsynchronousServerSocketChannel server;

    public AIOServer() throws IOException {
        server = AsynchronousServerSocketChannel.open().bind(
                new InetSocketAddress(PORT)
        );
    }

    public void startWithFuture() throws InterruptedException, ExecutionException,
            TimeoutException {
        System.out.println("Sever listen on " + PORT);
        Future<AsynchronousSocketChannel> future = server.accept();
        AsynchronousSocketChannel socket = future.get();
        ByteBuffer readBuf = ByteBuffer.allocate(1024);
        readBuf.clear();
        socket.read(readBuf).get(100, TimeUnit.SECONDS);
        readBuf.flip();
        System.out.println("received message:" + new String(readBuf.array()));
        System.out.println(Thread.currentThread().getName());
    }

    public void startWithCompletionHandler() throws InterruptedException, ExecutionException,
            TimeoutException {
        System.out.println("Server listen on " + PORT);
        //注册事件和事件完成后的处理器
        server.accept(null, new CompletionHandler<AsynchronousSocketChannel, Object>() {
            final ByteBuffer buffer = ByteBuffer.allocate(1024);

            public void completed(AsynchronousSocketChannel result, Object attachment) {
                System.out.println(Thread.currentThread().getName());
                System.out.println("start");
                try {
                    buffer.clear();
                    result.read(buffer).get(100, TimeUnit.SECONDS);
                    buffer.flip();
                    System.out.println("received message: " + new String(buffer.array()));
                }  catch(InterruptedException | ExecutionException e) {
                    e.printStackTrace();
                } catch (TimeoutException e) {
                    e.printStackTrace();
                } finally {
                    try{
                        result.close();
                        server.accept(null, this);
                    } catch(Exception e) {
                        e.printStackTrace();
                    }
                }
                System.out.println("end");
            }

            @Override
            public void failed(Throwable exc, Object attachment) {
                System.out.println("failed: " + exc);
            }
        });

        // 主线程继续自己的行为
        while(true) {
            System.out.println("main thread");
            Thread.sleep(1000);
        }
    }

    public static void main(String[] args) throws Exception {
        new AIOServer().startWithCompletionHandler();
    }
}
```
###  具体适用场景
* BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。
* NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。
* AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

**你知道的越多，你不知道的越多。**
