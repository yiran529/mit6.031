## Sockets & Networking
#### Client/server design pattern
我们将探讨使用消息传递进行通信的客户机/服务器设计模式。在这种模式中，有两种进程：客户端和服务器。客户端通过连接服务器来启动通信。客户端向服务器发送请求，而服务器则发回回复。最后，客户端断开连接。服务器可以同时处理多个客户端的连接，客户端也可以连接多个服务器。

#### Sockets and streams
输入/输出（I/O）是指与进程之间的通信--也许是通过网络，也许是与文件之间的通信，也许是通过命令行或图形用户界面与用户之间的通信。
##### IP地址
A network interface is identified by an IP address. IP version 4 addresses are 32-bit numbers written in four 8-bit parts.
##### Hostnames
Hostnames are names that can be translated into IP addresses. A single hostname can map to different IP addresses at different times; and multiple hostnames can map to the same IP address. 
##### Port numbers(端口号)
一台机器上可能有多个客户希望连接的服务器应用程序，因此我们需要一种方法，将同一网络接口上的流量导向不同的进程。
网络接口有多个端口，由 16 位数字标识。端口 0 是保留端口，因此端口号实际上从 1 到 65535 不等。
##### Network sockets
* def： A socket represents one end of the connection between client and server.
* 监听套接字用于服务器进程等待来自远程客户端的连接。在 Java 中，使用 ServerSocket 创建监听套接字，并使用其 accept 方法对其进行监听。请记住，一个端口只能有一个监听器，因此如果另一个线程或进程正在监听同一端口，监听就会失败。已连接的套接字可以向连接另一端的进程发送和接收信息。在 Java 中，客户端使用 Socket constructor 建立与服务器的套接字连接。服务器通过 ServerSocket.accept 返回的 Socket 对象获得已连接的套接字。
* In the real world, a USB socket allows only **one** cable to be connected to it, so a “listening” USB socket becomes a “connected” USB socket when a cable is physically plugged into it, and stops being available for new connections. But that’s not how network sockets work. Instead, when a new client arrives at a listening socket, a fresh connected socket is created on the server to manage the new connection. The listening socket continues to exist, bound to the same port number and ready to accept another arriving client when the server calls accept on it again. This allows a server to have **multiple** active client connections that originally targeted the same port number.
##### buffers
* The data that clients and servers exchange over the network is sent in chunks(块). 
* 网络将这一大块数据切成数据包，每个数据包在网络上分别路由。在另一端，接收器将数据包重新组合成字节流。这样就形成了一种突发的数据传输--当你想读取数据时，数据可能已经在那里了，也可能需要等待数据到达并重新组合。
* When data arrive, they go into a **buffer**, an array in memory that holds the data until you read it.
##### Bytes streams
The data going into or coming out of a socket is a stream of bytes.In Java, InputStream objects represent sources of data flowing into your program. 
##### Character streams
* The stream of bytes provided by InputStream and OutputStream is often too low-level to be useful. We may need to interpret the stream of bytes as a stream of Unicode characters, because Unicode can represent a wide variety of human languages 
* 