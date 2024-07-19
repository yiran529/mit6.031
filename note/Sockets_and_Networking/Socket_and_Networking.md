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
socket直译为插座
* def： A socket represents one end of the connection between client and server.
* 监听套接字用于服务器进程等待来自远程客户端的连接。在 Java 中，使用 ServerSocket 创建监听套接字，并使用其 accept 方法对其进行监听。请记住，一个端口只能有一个监听器，因此如果另一个线程或进程正在监听同一端口，监听就会失败。已连接的套接字可以向连接另一端的进程发送和接收信息。在 Java 中，客户端使用 Socket constructor 建立与服务器的套接字连接。服务器通过 ServerSocket.accept 返回的 Socket 对象获得已连接的套接字。
* 容易误解的一个地方：In the real world, a USB socket allows only **one** cable to be connected to it, so a “listening” USB socket becomes a “connected” USB socket when a cable is physically plugged into it, and stops being available for new connections. But that’s not how network sockets work. Instead, when a new client arrives at a listening socket, a fresh connected socket is created on the server to manage the new connection. The listening socket continues to exist, bound to the same port number and ready to accept another arriving client when the server calls accept on it again. This allows a server to have **multiple** active client connections that originally targeted the same port number.
##### buffers
* The data that clients and servers exchange over the network is sent in chunks(块). 
* 网络将这一大块数据切成数据包，每个数据包在网络上分别路由。在另一端，接收器将数据包重新组合成字节流。这样就形成了一种突发的数据传输--当你想读取数据时，数据可能已经在那里了，也可能需要等待数据到达并重新组合。
* When data arrive, they go into a **buffer**, an array in memory that holds the data until you read it.
##### Bytes streams
The data going into or coming out of a socket is a stream of bytes.In Java, InputStream objects represent sources of data flowing into your program. 
##### Character streams
* The stream of bytes provided by InputStream and OutputStream is often too low-level to be useful. We may need to interpret the stream of bytes as a stream of Unicode characters, because Unicode can represent a wide variety of human languages 
* In Java, Reader and Writer represent incoming and outgoing streams of Unicode characters. 
* 封装器 InputStreamReader 和 OutputStreamWriter 将字节流调整为字符流
I/O 的陷阱之一是Usually, when you create a Reader or Writer, Java will default to UTF-8 encoding. But problems occur when other programs on your computer use a different character encoding to read and write files, which means your Java program can’t interoperate with them.
##### Blocking
Input/output streams exhibit blocking behavior. For example, for socket streams:
When an incoming socket’s buffer is empty, calling read blocks until data are available.
When the destination socket’s buffer is full, calling write blocks until space is available.


#### Using network sockets in Java
一个大例子
要点
* To send a request to the server, we write the request to the socket output stream, just as we would use BlockingQueue.put() in the queue paradigm. To receive a reply, we read it from the input stream, where we would have used BlockingQueue.take(). 
* println() initially puts the message into a buffer inside the PrintWriter object, on the client’s side of the connection. The message content isn’t sent to the server until the buffer fills up or the client closes its side of the connection. So it’s critically important to flush the buffers after writing, forcing all buffer contents to be sent. Anyway ,**always remember to flush.**

##### Multithreaded server code
If we want to handle multiple clients at once, using blocking I/O, then the server needs a new thread to handle I/O with each new client. While each client-specific thread is working with its own client, another thread (perhaps the main thread) stands ready to accept a new connection.
##### Closing streams and sockets with try-with-resources
One new bit of Java syntax is particularly useful for working with streams and sockets: the try-with-resources statement. This statement automatically calls close() on variables declared in its parenthesized preamble

necessary for a client to know in order to connect to and communicate with a server: server IP address(Usually, we have a hostname instead, and the networking API will automatically look up the IP address of the hostname for us.), server pot number, wire protocol


#### Wire protocol
现在，我们用套接字连接了客户端和服务器，那么它们在套接字上来回传递什么呢？与消息传递中使用同步队列发送和接收内存对象不同，这里我们发送和接收的是字节流。我们将选择或设计一种协议，而不是为信息选择或设计一种抽象的数据类型。
协议是通信双方可以交换的一组消息。特别是有线协议，它是一组以字节序列表示的信息，如 hello world 和 bye（假设我们已经商定了将这些字符编码成字节的方法）。
许多互联网应用程序都使用简单的基于 ASCII 的有线协议。你可以使用一个名为 Telnet 的程序来查看它们。

##### Telnet client
telnet 是一种实用工具，可让你直接通过网络连接到监听服务器，并通过终端接口与之通信。
##### designing a wire protocol
When designing a wire protocol, apply the same rules of thumb you use for designing the operations of an abstract data type:
1. 减少不同信息的数量。最好是有几个可以组合的命令和响应，而不是许多复杂的信息。
2. 每条信息都应有明确的目的和一致的行为。
3. 消息集必须足以让客户端提出所需的请求，并让服务器交付结果。
Just as we demand **representation independence** from our types, we should aim for platform-independence in our protocols. 
##### 课程big idea在wire protocol的应用
##### Specifying a wire protocol

#### Testing client/server code
##### Separate network code from data structures and algorithms
对于不依赖网络的ADT，先单独测试好。
##### Separate socket code from stream code
A function or module that needs to read from and write to a socket may only need access to the input/output streams, not to the socket itself. This design allows you to test the module by connecting it to streams that don’t come from a socket.