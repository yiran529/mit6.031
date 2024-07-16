## Concurrency(并发)
#### Overview
* Concurrency means multiple computations are happening at the same time.
* Concurrency is everywhere in modern programming, whether we like it or not
    讲义有例子
* In fact, concurrency is essential in modern programming
    讲义有例子
#### Two models for concurrent programming
shared memory and message passing
* Shared memory:  In the shared memory model of concurrency, concurrent modules interact by reading and writing shared objects in memory.
    讲义上有图示和例子
* Message passing: In the message-passing model, concurrent modules interact by sending messages to each other through a communication channel. Modules send off messages, and incoming messages to each module are queued up for handling.
    讲义有图示和例子

#### Processes, threads and time-slicing
The message-passing and shared-memory models are about how concurrent modules communicate. **The concurrent modules themselves** come in two different kinds: processes and threads.
* process: 
    **A process is an instance of a running program that is isolated from other processes on the same machine.** In particular, it has its own private section of the machine’s memory.

    The process abstraction is a **virtual computer**. It makes the program feel like it has the entire machine to itself – like a fresh computer has been created, with fresh memory, just to run that program.

    Just like computers connected across a network, processes normally **share no memory between them**. A process can’t access another process’s memory or objects at all. Sharing memory between processes is possible on most operating systems, but it needs special effort. By contrast, a new process is automatically **ready for message passing**, because it is created with standard input & output streams, which are the System.out and System.in streams you’ve used in Java.

    Whenever you start a Java program – indeed, whenever you start any program on your computer – it starts a fresh process to contain the running program.
* Thread: 
    **A thread is a locus of control inside a running program.** Think of it as **a place in the program that is being run, plus the stack of method calls that led to that place** (so the thread can go back up the stack when it reaches return statements).

    Just as a process represents a virtual computer, the thread abstraction represents a **virtual processor**. Making a new thread simulates making a fresh processor inside the virtual computer represented by the process. This new virtual processor runs the same program and shares the same memory as other threads in the process.

    Threads are **automatically ready for shared memory**, because threads share all the memory in the process. It takes special effort to get “thread-local” memory that’s private to a single thread. It’s also necessary to set up **message-passing** explicitly, by creating and using queue data structures. We’ll talk about how to do that in a future reading.

    Whenever you run a Java program, the program starts with one thread, which calls main() as its first step. This thread is referred to as the main thread.
进程和线程有什么区别(CSDN)
1. 根本区别：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位

2. 资源开销：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

3. 包含关系：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

4. 内存分配：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的

5. 影响关系：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。

6. 执行过程：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行
   
* Time slicing: 
    How can you have many concurrent threads with only one or two processors in your computer? When there are more threads than processors, concurrency is simulated by time slicing, which means that the processor switches between threads. 
    On most systems, time slicing happens unpredictably and nondeterministically, meaning that a thread may be paused or resumed at any time.
    图示可以看本文件夹下的一张图片，或讲义

#### Starting thread in Java
You can start new threads by making an instance of Thread and telling it to start(). You provide code for the new thread to run by creating a class implementing Runnable. The first thing the new thread will do is call the run() method in this class. For example:
```java
// ... in the main method:
new Thread(new HelloRunnable()).start();

// elsewhere in the code
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }
}
```
But a very common idiom is starting a thread with an anonymous Runnable, which eliminates the need to name the HelloRunnable class at all:
```java
new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello from a thread!");
    }
}).start();
```
The next two sections discuss the idea of anonymous classes, because they’re widely used in Java beyond just threads.
#### Anonymous classes  
An anonymous class declares an unnamed class that implements an interface and immediately creates the one and only instance of that class. Compare to the code above:
```java
// no StringLengthComparator class!
SortedSet<String> strings = new TreeSet<>(new Comparator<String>() {
    @Override public int compare(String s1, String s2) {
        if (s1.length() == s2.length()) {
            return s1.compareTo(s2);
        }
        return s1.length() - s2.length();
    }
});
strings.addAll(List.of("yolanda", "zach", "alice", "bob"));
// strings is { "bob", "zach", "alice", "yolanda" }
```
(否则，需要写一个类去 implement Comparator接口)
优点 缩小Comparator的scope；不用reader到处去找Comparator的实现
缺点 不能重复使用，不DRY；如果匿名部分很长，容易破坏程序的结构

#### Using an anonymous Runnable to start a thread
```java
new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello from a thread!");
    }
}).start();
```
Using lambda expression
```java
new Thread(() -> System.out.println("Hello from a thread!")).start();
```

Note for exercice:
Now suppose we run this test on JUnits:
```java
@Test public void testThreadOops() {
  new Thread(() -> { throw new Error("thread oops"); }).start();
}
```
The Error exception is thrown on the newly-created thread. This exception terminates that thread with a stack trace printed to the console.

But the test method testThreadOops returns normally – there is no exception thrown on the thread that called it. So JUnit does not notice the thrown exception, and the test passes. It does not detect the oops.

#### Interleaving
讲义都有具体例子
* A share memory example
* interleaving: When A and B are running concurrently, these low-level instructions can interleave with each other, meaning that the actual sequence of execution can intersperse A’s operations and B’s operations arbitrarily. 
* race condition:  A race condition means that the correctness of the program (the satisfaction of postconditions and invariants) depends on the relative timing of events in concurrent computations A and B. When this happens, we say “A is in a race with B. (Or you may say "thread interference")
各个线程获取变量的值，改变变量的值，存储变量的值穿插进行
#### Reordering
when you’re using multiple variables and multiple processors, you can’t even count on changes to those variables appearing in the same order.
...例子
原因在于，编译器和处理器做了很多优化让程序跑得更快，比如对一些变量进行临时备份，存储在一些存取速度很快的区域，and working with them temporarily before eventually storing them back to their official location in memory.
(在条件允许的情况下，直接运行当前有能力立即执行的后续指令，避开获取下一条指令所需数据时造成的等待?)
#### Message-passing example
#### Concurrency is hard to test and debug 
It’s very hard to discover race conditions using testing. And even once a test has found a bug, it may be very hard to localize it to the part of the program causing it.
Concurrency bugs exhibit very poor reproducibility. It’s hard to make them happen the same way twice.