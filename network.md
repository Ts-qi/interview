### 前言：

######  

### HTTP 与 HTTPS

####  1 概念：

###### 		http:(Hypertext Transfer Protocol Vertion) 超文本传输协议，进行网络连接传输文本信息

###### 		https:（Secure Hypertext Transfer Protocol）安全超文本传输协议， 它是一个安全通信通道；它基於HTTP开发，用于在客戶計算機和服務器之間交換信息；

​	 	HTTPS是 HTTP over SSL/TLS，HTTP是应用层协议，TCP是传输层协议，在应用层和传输层之间，增加了一个安全套接层SSL/TLS：

> SSL (Secure Socket Layer，安全套接字层)；

> TLS (Transport Layer Security，传输层安全协议)；

> SSL使用40 位关键字作为RC4流加密算法；（RC4流 ？？？  ？？？）

#### 2 HTTPS 作用

> 数据完整性， 防止内容被第三方冒充或篡改；

> 内容加密，建立一个信息安全通道，来保证数据传输的安全；

> 身份认证，确认网站的真实性；

#### 3 HTTPS 和 HTTP 的区别；

> HTTPS 需要到 CA申请证书（缴费）；

> HTTP 是超文本传输协议，信息是明文传输；HTTPS 则是具有安全性的 ssl加密传输协议；

> 二者使用的是完全不同的连接方式，用的端口也不一样。HTTP是默认使用 80 端口；HTTPS 默认使用 443 端口；

> HTTP连接很简单，是无状态的；HTTPs 协议是由 SSL+HTTP 协议构建的可进行加密传输，身份认证的网络协议，比 HTTP协议安全； 

#### 4 HTTP 协议的传输特点；

> 优点：1 基于应用级的接口使用方便；2 要求不高，容错性强；

> 缺点：1  传输速度慢，数据包大（HTTP 协议中包含辅助应用信息）；
>
> ​			2 如实交互，服务器性能压力大；
>
> ​			3 数据交互安全性差；	

####  5 OSI七层网络模型；

![img](https://upload-images.jianshu.io/upload_images/6186031-106bc19607c54927.png?imageMogr2/auto-orient/strip|imageView2/2/w/960/format/webp)

https://www.jianshu.com/p/534a2b1a9296



####  6 TCP/IP--传输控制协议/网络协议- 四层；

![image-20191023185220735](/Users/tangqi/Library/Application Support/typora-user-images/image-20191023185220735.png)

####  7 TCP/IP 与 OSI网络模型的对应关系；

![这里写图片描述](https://img-blog.csdn.net/20180825195520935?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjg2Nzk3Mg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



#### 8 TCP 连接为什么是 3 次握手？而不是 2 次或者 4 次？

> TCP 概念： 传送控制协议（ Transmission Control Protocol）;
>
> ​	如果是 2 次握手建立连接，假设 有这样的一种场景，客户端发送了第一个请求连接并且没有丢失，只是因为网络节点中滞留的时间太长了，由于 TCP的客户端迟迟没有收到确认的报文，以为服务器没有收到，此时重新像服务器发送报文，此后客户端和服务端经过两次握手完成连接，传输数据，然后关闭连接。此时此前滞留的那一次请求连接，网络通畅到达了服务器，这个报文是失效的，但是两次握手的机制将会让客户端和服务器再次建立连接可能出现 4 次握手，5 次握手，浪费很多不必要的资源，造成不必要的错误；

####  9 如何复用TCP 连接？

>  1 、复用TCP 连接的背景： 负载均衡技术通过设置虚拟服务器IP（VIP），将后端多台真实服务器的应用资源虚拟成一台高性能的应用服务器，通过负载均衡算法，将大量来自客户端的应用请求分配到后端的服务器进行处理。负载均衡设备持续的对服务器上的应用状态进行检查，并自动对无效的应用服务器进行隔离，实现了一个简单、扩展性强、可靠性高的应用解决方案。解决了单台服务器处理性能不足，扩展性不够，可靠性较低的问题
>
> 2 、TCP 复用：TCP 连接复用技术通过将前端 多个客户端的 HTTP 请求复用到后端与服务器建立的一个 TCP 连接上，这种技术大大减小服务器的性能负载，较少与服务器之间新建立 TCP 连接所带来的延时，并最大限度的降低客户端对后端服务器的并发连接数请求，减少服务器的资源占用；
>
> 3 、http复用： 客户端可以在一个TCP连接中发送多个HTTP请求，这种技术叫做HTTP复用（HTTP Multiplexing），而HTTP复用则是一个客户端的多个HTTP请求通过一个TCP连接进行处理；
>
> 4 、TCP复用和 HTTP 复用区别： TCP是负载均衡设备的独特功能，HTTP 复用是 HTTP1.1 协议所支持的新功能；

#### 10 TCP连接 4 次挥手流程是怎么样的？ 为什么？

#### 11 TCP拥塞控制是什么？怎么工作的？TCP是如何保证传输正确的？

#### 12 TCP 和 UDP 的区别？

> TCP ： 传送控制协议（ Transmission Control Protocol);

> UDP:  用户数据协议 (User Datagram Protocol );
>
> 

#### 13 HTTP 常见的请求头，用途？

#### 14 HTTP报文机构提是什么结构？

#### 15 HTTP 的缓存控制如何实现（以及日常如何使用）？

#### 16 HTTP 200、206、**301、302、304**、307、401、403、404、500状态码分别代表什么？用途？**（302、304重点**）

#### 17 307和302区别是什么？

#### 18 XHR 的各种data类型对应的content-type header是什么？

#### 19 HTTPS加密算法是什么？

#### 20 HTTPS建立连接的过程是什么？**（重点）**

#### 21 中间人攻击是什么？如何避免？数字证书又是什么？作用？如何保证安全性的？**（重点）**

#### 22 HTTP2和HTTP1.1的区别？

#### 23 HTTP2中的流、数据帧、多路复用是什么？

#### 24 手写 XHR 

#### 25 HTTP协议的优化和加速方面：TCP连接复用、内容缓存、TCP缓冲、HTTP压缩、SSL加速