@[toc]
# 推荐阅读
* [一文深入理解Zookeeper核心知识，2020年你值得拥有](https://blog.csdn.net/qq_40722827/article/details/105182718)
* [深入理解Dubbo核心概念,这篇文章你绝对不能错过](https://blog.csdn.net/qq_40722827/article/details/105177384)
* [微服务入门，有这一篇就够了](https://blog.csdn.net/qq_40722827/article/details/105146582)
* [分布式微服务架构（初级篇）](https://blog.csdn.net/qq_40722827/article/details/105159725)
* [一文总结Spring 注解及作用详解](https://blog.csdn.net/qq_40722827/article/details/104950902)
* [更多请关注我的博客...](https://blog.csdn.net/qq_40722827)
# 什么是Nginx?
> * Nginx 是一款轻量级的 Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，其特点是占有内存少，并发能力强。事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。
> * Nginx 以事件驱动的方式编写，所以有非常好的性能，同时也是一个非常高效的反向代理、负载平衡。
# web 服务器
* Nginx 可以作为静态页面的 web 服务器，同时还支持 **CGI 协议**的动态语言，比如 perl、php等。但是不支持 java。Java 程序只能通过与 tomcat 配合完成。Nginx 专为性能优化而开发，性能是其最重要的考量,实现上非常注重效率 ，能经受高负载的考验,有报告表明能支持高
达 50,000 个并发连接数。
* CGI 协议：CGI即通用网关接口(Common Gateway Interface)，是外部应用程序（CGI程序）与Web服务器之间的接口标准，是在CGI程序和Web服务器之间传递信息的规程。
# 正向代理
* Nginx 不仅可以做反向代理，实现负载均衡。还能用作正向代理来进行访问等功能。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330204242446.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330204254716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* 正向代理：如果把局域网外的 Internet 想象成一个巨大的资源库，则局域网中的客户端要访问 Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理。
* 正向代理最大的特点：客户端非常明确要访问的服务器地址；服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端；正向代理模式屏蔽或者隐藏了真实客户端信息。
# 反向代理
* 反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器 IP 地址。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330171351700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* 多个客户端给服务器发送的请求，nginx服务器接收到之后，按照一定的规则分发给了后端的业务处理服务器进行处理了。此时，请求的来源也就是客户端是明确的，但是请求具体由哪台服务器处理的并不明确了，nginx扮演的就是一个反向代理角色
* 反向代理，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息！
# 负载均衡
> * 明确了所谓代理服务器的概念，那么接下来，nginx扮演了反向代理服务器的角色，它是以依据什么样的规则进行请求分发的呢？分发的规则是否可以控制呢？

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330172344376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)

* 随着信息数量的不断增长，访问量和数据量的飞速增长，以及系统业务的复杂度增加，传统的`用户访问<-->Tomcat服务器`架构会造成服务器相应客户端的请求日益缓慢，并发量特别大的时候，还容易造成服务器直接崩溃。很明显这是由于服务器性能的瓶颈造成的问题，那么如何解决这种情况呢？
* 这时候集群的概念产生了，单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。
 * 这里提到的客户端发送的、nginx反向代理服务器接收到的请求数量，就是我们说的负载量；请求数量按照一定的规则进行分发到不同的服务器处理的规则，就是一种均衡规则；所以将服务器接收到的请求按照规则分发的过程，称为负载均衡。
## 负载均衡调度算法
* weight轮询（默认）：接收到的请求按照顺序逐一分配到不同的后端服务器，即使在使用过程中，某一台后端服务器宕机，nginx会自动将该服务器剔除出队列，请求受理情况不会受到任何影响。 这种方式下，可以给不同的后端服务器设置一个权重值（weight），用于调整不同的服务器上请求的分配率；权重数据越大，被分配到请求的几率越大；该权重值，主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的。
* ip_hash：每个请求按照发起客户端的ip的hash结果进行匹配，这样的算法下一个固定ip地址的客户端总会访问到同一个后端服务器，这也在一定程度上解决了集群部署环境下session共享的问题。
* fair：智能调整调度算法，动态的根据后端服务器的请求处理到响应的时间进行均衡分配，响应时间短处理效率高的服务器分配到请求的概率高，响应时间长处理效率低的服务器分配到的请求少；结合了前两者的优点的一种调度算法。但是需要注意的是nginx默认不支持fair算法，如果要使用这种调度算法，请安装upstream_fair模块
* url_hash：按照访问的url的hash结果分配请求，每个请求的url会指向后端固定的某个服务器，可以在nginx作为静态服务器的情况下提高缓存效率。同样要注意nginx默认不支持这种调度算法，要使用的话需要安装nginx的hash软件包
# 动静分离
* 为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速
度。降低原来单个服务器的压力。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330173737168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
# Nginx常用命令
* `start nginx`  启动nginx 
* `nginx -s stop` 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。 
* `nginx -s quit` 平稳关闭Nginx，保存相关信息，有安排的结束web服务。 
* `nginx -s reload` 因改变了Nginx相关配置，需要重新加载配置而重载。 
* `nginx -s reopen` 重新打开日志文件。 
* `nginx -c filename` 为 Nginx 指定一个配置文件，来代替缺省的。 
* `nginx -t` 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文 件。
* `nginx -v` 显示 nginx 的版本。 
* `nginx -V` 显示 nginx 的版本，编译器版本和配置参数
#  nginx 原理与优化参数配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330174255699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330174306660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
### master-workers 的机制的好处 
* 首先，对于每个 worker 进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，同时在编程以及问题查找时，也会方便很多。其次，采用独立的进程，可以让互相之间不会影响，一个进程退出后，其它进程还在工作，服务不会中断，master 进程则很快启动新的worker 进程。当然，worker 进程的异常退出，肯定是程序有 bug 了，异常退出，会导致当前 worker 上的所有请求失败，不过不会影响到所有请求，所以降低了风险。
### 需要设置多少个 worker
* Nginx 同 redis 类似都采用了 io 多路复用机制，每个 worker 都是一个独立的进程，但每个进程里只有一个主线程，通过异步非阻塞的方式来处理请求， 即使是千上万个请求也不在话下。每个 worker 的线程可以把一个 cpu 的性能发挥到极致。所以 worker 数和服务器的 cpu数相等是最为适宜的。设少了会浪费 cpu，设多了会造成 cpu 频繁切换上下文带来的损耗。
### 设置 worker 数量。
```java
worker_processes 4
#work 绑定 cpu(4 work 绑定 4cpu)。
worker_cpu_affinity 0001 0010 0100 1000
#work 绑定 cpu (4 work 绑定 8cpu 中的 4 个) 。
worker_cpu_affinity 0000001 00000010 00000100 00001000
```
### 连接数 worker_connection
* 这个值是表示每个 worker 进程所能建立连接的最大值，所以，一个 nginx 能建立的最大连接数，应该是 `worker_connections* worker_processes`。
* 当然，这里说的是最大连接数，对于HTTP 请 求 本 地 资 源 来 说 ， 能 够 支 持 的 最 大 并 发 数 量 是 `worker_connections* worker_processes`
* 如果是支持 http1.1 的浏览器每次访问要占两个连接，所以普通的静态访问最大并发数是： `worker_connections * worker_processes /2`
* 如果是 HTTP 作 为反向代理来说，最大并发数量应该是 `worker_connections * worker_processes/4`。因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服务的连接，会占用两个连接。
# Nginx安装(Linux)
* 官网地址：https://nginx.org/en/download.html
* 安装Nginx源
```java
rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7- 0.el7.ngx.noarch.rpm
```
* 安装该rpm后，我们就能在/etc/yum.repos.d/ 目录中看到一个名为nginx.repo 的文件。
* 安装完Nginx源后，就可以正式安装Nginx了。
```java
yum install -y nginx
```
#  配置文件描述
* nginx.conf配置文件描述：
### 基本配置
* 全局块配置：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路
径，配置文件引入，允许生成worker process数等。

```java
#user nobody; #配置用户或者组，默认为nobody 
worker_processes 1; #允许生成的进程数，默认为1 
#error_log logs/error.log; #制定日志路径，级别。这个设置可以放入全局块， 
				#http块，server块，级别以此为:debug|info|notice|warn|error|crit|alert|emerg 
#error_log logs/error.log notice; 
#error_log logs/error.log info; 
#pid logs/nginx.pid; #指定nginx进程运行文件存放地址
```
### events块配置
* events块：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接
请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
```java
accept_mutex on; #设置网路连接序列化，防止惊群现象发生，默认为on 
multi_accept on; #设置一个进程是否同时接受多个网络连接，默认为off 
#use epoll; #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport 
worker_connections 1024; #最大连接数，默认为1024(早期是512)
```
**max_client**
* nginx作为http服务器的时候：
```java
max_clients = worker_processes * worker_connections 

由HTTP客户端发起一个请求，创建一个到服务器指定端口（默认是80端口）的TCP连接。HTTP服务器则在那个端口监听客户端的请求。一旦收到请求，
服务器会向客户端返回一个状态，比如"HTTP/1.1 200 OK"，以及返回的内容，如请求的文 件、错误消息、或者其它信息。同一时刻nginx在处理客
户端发送的http请求应该只是一个connection，由此可知理论上作 为http web服务器角色的nginx能够处理的最大连接数就是最大客户端连接数。
```
* nginx作为反向代理服务器的时候：

```java
max_clients = worker_processes * worker_connections/4 

如果作为反向代理，因为浏览器默认会开启2个连接到server，而且Nginx还会使用fds（file descriptor）从同一个连接池建立连接到upstream后
端。则最大连接数的计算公式需要除4
```
### http块配置
* 可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。

```java
#配置nginx支持哪些文件扩展名与文件类型映射表。在conf/mime.types查看支持哪些类型 
include      mime.types; 
#默认文件类型(流)类型，支持很多文件、图片、js/css等 
default_type application/octet-stream; 

#自定义格式 
log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for';
access_log log/access.log myFormat; #combined为日志格式的默认值 
	#优化参数 允许sendfile方式传输文件，开启高校效传输模式 
sendfile on; #tcp_nopush on; #防止网络阻塞 

	#keepalive_timeout 0; 
	keepalive_timeout 65; #长连接超时时间（单位秒） 
	#gzip on; #开启gzip压缩
```
##### server块配置
* 配置虚拟主机的相关参数，一个http中可以有多个server。

```java
#配置虚拟主机 
server {
		listen 80; #配置监听端口 
		server_name localhost; #配置服务器名 

		#charset koi8-r; #编码格式 

		#access_log logs/host.access.log main; //主机的访问日志（如没有，全局为准） 
		#默认的匹配/请求，当访问路径中有/，会被该location匹配处理 
		location / { 
			root html; #root是配置服务器的默认网站根目录位置，在nginx目录下html 
			index index.html index.htm; 
		}

		#error_page 404         /404.html; #配置404页面 
		# redirect server error pages to the static page /50x.html 
		#
		error_page   	500 502 503 504 	/50x.html; #配置50x页面 
		location = /50x.html { #精确匹配 
				root html; 
		}

		#禁止(外网）访问 .htaccess文件 
		# deny access to .htaccess files, if Apache's document root 
		# concurs with nginx's one 
		#
		#location ~ /\.ht {
	    #	 deny all; 
	    #} 
	  }
```
##### server2块配置
* 和上方很类似，主要是配置另一个虚拟机信息

```java
# another virtual host using mix of IP-, name-, and port-based configuration 
#
#server { 
# 		listen 			8000; 
# 		listen 			somename:8080; 
# 		server_name 	somename alias another.alias; 

# 		location / { 
# 				root html; 
# 				index index.html index.htm; 
# 		} 
#	}
```
##### server3块配置
* 配置https服务

> * HTTP：是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准（TCP），用于从WWW服务器传 输超文本到本地浏览器的传输协议，它可以使浏览器更加高效，使网络传输减少。 
> * HTTPS：是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的 详细内容就需要SSL。(加密)

```java
# HTTPS server 
	#
	#server { 
	# 	   listen 		443 ssl; 
	# 		server_name localhost; 
	
	# 		ssl_certificate cert.pem; 
	# 		ssl_certificate_key cert.key; 
	
	# 		ssl_session_cache shared:SSL:1m; 
	# 		ssl_session_timeout 5m; 
	
	#		ssl_ciphers HIGH:!aNULL:!MD5; 
	# 		ssl_prefer_server_ciphers on; 
	
	# 		location / { # root html; 
	# 			index index.html index.htm; 
	#		 } 
	#	}
```
# 应用场景
### 需求1: 静态资源
**静态配置文件处理**
* 由于Nginx性能很高，对于常用的静态资源，可直接交由Nginx进行访问处理
* 示例：
```java
location / { 
	root D:/nginx-tomcat/exam; # /opt/static/exam 
	index index.html index.htm; 
}
```
### 需求2：反向代理
**让nginx进行转发，即所 谓的反向代理 访问localhost时转到tomcat**
* 修改nginx.conf文件，查看server 节点，相当于一个代理服务器，可以配置多个。
```java
listen：表示当前的代理服务器监听的端口，默认的是监听80端口。 
server_name：表示服务名称。 
location：表示匹配的路径，这时配置了/表示所有请求都被匹配到这里 
root：里面配置了root这时表示当匹配这个请求的路径时，将会在这个文件夹内寻找相应的文件。 
index：当没有指定主页时，默认会选择这个指定的文件，它可以有多个，并按顺序来加载，如果第一个不存在，则找第二 个，依此类推 
下面的error_page是代表错误的页面，
```

```java
server {
	listen 80; 
	server_name localhost; 
	#charset koi8-r; 
	#access_log logs/host.access.log main; 
	location / { 
		root html; 
		index index.html index.htm; 
		}
	#error_page 404
```
**localhost时转到tomcat时。修改两个地方：**
```java
server_name exam_qf; 
location / { 
	proxy_pass http://127.0.0.1:8080; 
}
```
* proxy_pass，它表示代理路径，相当于转发，而不像之前说的root必须指定一个文件夹。

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
