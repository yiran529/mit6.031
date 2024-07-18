## Queues and Message-Passing
我们之前已经讨论过并行程序的一种模型——共享内存模型，现在我们讨论另一种模型——消息传递模型，这种模型有更高的安全性。另外，消息传递模型一般以immutable type为对象
#### Message passing between threads
* We can use a queue with blocking operations for message passing between threads. Java provides the BlockingQueue interface for queues with blocking operations.`BlockingQueue` extends `Queue`:
    `put(e)` blocks until it can add element e to the end of the queue (if the queue does not have a size bound, put will not block).
    `take()` blocks until it can remove and return the element at the head of the queue, waiting until the queue is non-empty.
* We will implement the **producer-consumer design pattern**(见图) for message passing between threads. Producer threads and consumer threads share a synchronized queue. Producers put data or requests onto the queue, and consumers remove and process them. 
*  we will choose an **immutable** type because our goal in using message passing is to avoid the problems of shared memory. 
*  And just as we designed the operations on a threadsafe ADT to prevent race conditions and enable clients to perform the atomic operations they need, we will design our message objects with those same requirements.
*  一个消息传递的大例子
    如何处理让无线循环中的请求终止：
    1. 设计一个Stop类（正如在递归ADT中设计一个empty类一样）
    2. 用InterruptedException

#### Thread safety arguments with message passing

* **Existing threadsafe data types** for the synchronized queue. This queue is definitely shared and definitely mutable, so we must ensure it is safe for concurrency.
* **Immutability** of messages or data that might be accessible to multiple threads at the same time.
* **Confinement** of data to individual producer/consumer threads. Local variables used by one producer or consumer are not visible to other threads, which only communicate with one another using messages in the queue. 
* **Confinement** of mutable messages or data that are sent over the queue but will only be accessible to one thread at a time.如果第一个线程在将这些数据放入队列并发送给另一个线程时，就像丢掉烫手山芋一样丢掉了所有对这些数据的引用，那么每次就只有一个线程能访问这些数据，从而排除了并发访问。


与同步相比，消息传递可以让并发系统中的每个模块更容易维护自己的线程安全不变式。如果数据是通过线程安全通信通道在模块间传输，我们就不必考虑多个线程访问共享数据的问题。 

#### Race condition
It’s still possible for concurrent message-passing processes to interleave their work in bad ways.
This particularly happens when a client must send multiple messages to the module to do what it needs, because those messages (and the client’s processing of their responses) may interleave with messages sent by other clients. 
在前面的消息传递的大例子中，实际上有经过精心设计以比较好地避免这个问题(见练习)

#### Deadlock
The blocking behavior of blocking queues is very convenient for programming, but blocking also introduces the possibility of deadlock.
when the message-passing queues have limited capacity, and become filled up to that capacity with messages, then even a message-passing system can experience deadlock
讲义举了一种消息传递死锁的情况，简单来说就是：DrinksFridge 在等待客户端读取一些回复并释放回复队列的空间，而客户端则在等待 DrinksFridge 接受一些请求并释放请求队列的空间。

* 避免死锁的方法：
    1. design the system so that there is no possibility of a cycle
    2. 设置时限 一旦超时就throw exception 只不过需要考虑如何处理exception