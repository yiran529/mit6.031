## Thread Safety
There are basically four ways to make variable access safe in shared-memory concurrency:

* Confinement. Don’t share variables or data between threads.
* Immutability. Make the shared variables unreassignable or the shared data immutable. We’ve talked a lot about immutability already, but there are some additional requirements for concurrent programming that we’ll talk about in this reading.  
* Threadsafe data type. Encapsulate the shared data in an existing threadsafe data type that does the coordination for you. 
* Synchronization. Use synchronization to keep the threads from accessing shared variables or data at the same time. Synchronization is what you need to build your own threadsafe data type.

需要注意的一点是：代码逻辑能保证语句的顺序。即在开启新线程的语句之前的语句在新线程开启时一定是已经执行过的了。
#### What threadsafety means
A data type or static method is **threadsafe** if it 1.behaves correctly when used from multiple threads, 2. regardless of how those threads are executed, and 3.without demanding additional coordination from the calling code.讲义中有就这段话含义的阐释和一个小例子

#### Strategy 1: Confinement
* Definition: Thread confinement is a simple idea: you avoid races on reassignable references and mutable data by keeping the references or data confined to a single thread. (Don’t share variables or data between threads.)
* Local variables are always thread confined.因为local var存储在栈中，而每个thread各自有一个栈
* 即便var is confined，也要注意它所指的Obj.If the object is mutable, then we want to check that the object is confined as well – there can’t be references to it that are reachable from any other thread.
* avoid global var/static var 否则容易被多个线程共享，即使一个方法中避免使用多线程操纵同一个变量，不同的方法也可能在多线程中操纵这个global/static var
* 即便是private final的field也有可能不threadsafe
* Confinement and anonymous classes: In a system where you create and start new threads, sharing fields and local variables with inner Runnable instances is common. You will need to use one of the upcoming strategies to ensure that sharing those non-confined references and objects is safe.讲义中有具体例子
  
#### Strategy 2: Immutability
* Our second way of achieving thread safety is by using **unreassignable**(如用`final`) references and **immutable** data types. 
* 对于并行来说，有些限制太宽松的Immutability(如 beneficent mutation)不能保证安全
    针对threadsafe的更强的Immutability： 
        no mutator methods
        all fields declared private and final
        no representation exposure
        no mutation whatsoever of mutable objects in the rep – not even beneficent mutation

#### Strategy 3: using threadsafe data types
* immutable 的类型天然就是threadsafe的
* Our third major strategy for achieving thread safety is to store shared mutable data in existing threadsafe data types. For instance, `StringBuffer` is threadsafe, while `StringBuilder` is not.我们可能经常需要在Java的API中找两个功能一样的data type，一个threadsafe，一个不。因为threadsafe往往是以性能为代价的
* threadsafe collection: Java Collection API provides another set of wrapper methods to make collections threadsafe, while still mutable.These wrappers effectively make each method of the collection **atomic** with respect to the other methods.
    e.g.
    ```java
    private static Map<Integer,Boolean> cache =
                Collections.synchronizedMap(new HashMap<>());
    ```
    A few point here:
    1. Don’t circumvent the wrapper.即不要保留内层的non-threadsafe的map的引用
    2. 由cache生成的Iterator不threadsafe
    3. atomic operations aren’t enough to prevent races. threadsafe只保证单个操作如put，get是atomic的，但不能保证各个操作之间没有interleave
unreassignable, immutable的type才完全不可能参与race condition（如final String）

#### How to make a safety argument
to convince yourself and others that your concurrent program is correct
* General rule: A safety argument needs to catalog all the threads that exist in your module or program, and the data that they use, and argue which of the four techniques you are using to protect against races for each data object or variable.如果是用了threadsafe data type还要说明各原子操作之间不会interleave
##### Thread safety arguments for data type
如果在这个datatype中开了新的线程，那就针对于这个线程有关的变量写有什么confinement；否则，confinement只跟这个datatype怎么被使用有关，在这里（for data type）一般不写
讲义中有好几个例子
good e.g.
```java
/** MyString is an immutable data type representing a string of characters. */
public class MyString {
    private final char[] a;
    private final int start;
    private final int len;
    // Rep invariant:
    //    0 <= start <= a.length
    //    0 <= len <= a.length-start
    // Abstraction function:
    //    AF(a,start,len) = the string of characters a[start],...,a[start+length-1]
    // Thread safety argument:
    //    This class is threadsafe immutable:
    //    - a, start, and len are final
    //    - a points to a mutable char array, which may be shared with other
    //      MyString objects, but they never mutate it
    //    - the array is never exposed to a client
```
bad:
```java
public class Graph {
    private final Set<Node> nodes =
                   Collections.synchronizedSet(new HashSet<>());
    private final Map<Node,Set<Node>> edges =
                   Collections.synchronizedMap(new HashMap<>());
    // Rep invariant:
    //    for all x, y such that y is a member of edges.get(x),
    //        x, y are both members of nodes
    // Abstraction function:
    //    represents a directed graph whose nodes are the set of nodes
    //        and whose edges are the set (x,y) such that
    //                         y is a member of edges.get(x)
    // Thread safety argument:
    //    - nodes and edges are final, so those variables are immutable
    //      and threadsafe
    //    - nodes and edges point to threadsafe set and map data types
```
#### Strategy 4: Synchronization(同步)
注：在讲义中，这一部分在下一节
针对有shared memory obj使用
* def: prevent threads from accessing the shared data at the same time.
##### Lock
 **Locks** are one synchronization technique. A lock is an abstraction that allows at most one thread to own it at a time. 
  Two operations:
  * `acquire` allows a thread to take ownership of a lock. If a thread tries to acquire a lock currently owned by another thread, it blocks until the other thread releases the lock.
  * `release` relinquishes ownership of the lock, allowing another thread to take ownership of it.
  
Using a lock also tells the compiler and processor that you’re using shared memory concurrently, so that registers and caches will be written back correctly to shared storage. This avoids the problem of reordering

**blocking**: a thread waits (without doing further work) until an event occurs. e.g.: An acquire(l) on thread 1 will block if another thread 2 is holding lock l. The event it waits for is thread 2 performing release(l).

##### Deadlock
* def: 是指两个或多个线程相互等待对方持有的锁，导致这些线程都无法继续执行。
* Deadlock occurs when concurrent modules are stuck waiting for each other to do something. 一个死锁可能有不止两个线程参与。You can also have deadlock without using any locks.

##### Developing a threadsafe abstract data type
除了之前讨论过的spec,test,rep，现在需要加入第四步，即Synchronize.( Make an argument that your rep is threadsafe. Write it down explicitly as a comment in your class, right by the rep invariant, so that a maintainer knows how you designed thread safety into the class.)

##### Locking
* In Java, every object has a lock implicitly associated with it — a String, an array, an ArrayList, and every class you create, all of their object instances have a lock.
```java
Object lock = new Object();
synchronized (lock) { // thread blocks here until lock is free
    // now this thread has the lock
    balance = balance + 1;
} // exiting the block releases the lock
```
Synchronized regions like this provide **mutual exclusion**(互斥锁)确保同一时刻只有一个线程能够持有该锁

* Locks guard access to data.Locks only provide mutual exclusion with other threads that acquire the same lock. All accesses to a data variable must be guarded by the same lock.

When a thread t acquires an object’s lock using synchronized (obj) { ... }, it does**one thing and one thing only**: **it prevents other threads from entering their own synchronized(expression) block**, where expression refers to the same object as obj, until thread t finishes its synchronized block.synchronized (obj) 只对这段代码中的操作进行同步控制，确保在同步块内对 obj 的操作是线程安全的。但是，这并不阻止其它线程通过其他方法或代码块对 obj 进行修改

##### Monitor pattern
```java
/** SimpleBuffer is a threadsafe EditBuffer with a simple rep. */
public class SimpleBuffer implements EditBuffer {
    private String text;
    ...
    public SimpleBuffer() {
        synchronized (this) {
            text = "";
            checkRep();
        }
    }
    public void insert(int position, String insertion) {
        synchronized (this) {
            text = text.substring(0, position) + insertion + text.substring(position);
            checkRep();
        }
    }
    public void delete(int position, int len) {
        synchronized (this) {
            text = text.substring(0, position) + text.substring(position+len);
            checkRep();
        }
    }
    public int length() {
        synchronized (this) {
            return text.length();
        }
    }
    public String toString() {
        synchronized (this) {
            return text;
        }
    }
}
```

This approach is called the **monitor pattern**. A monitor is a class whose methods are mutually exclusive, so that only one thread can be inside an instance of the class at a time.

语法糖，见下（注意constructor不能这样用）：
```java
/** SimpleBuffer is a threadsafe EditBuffer with a simple rep. */
public class SimpleBuffer implements EditBuffer {
    private String text;
    ...
    public SimpleBuffer() {
        text = "";
        checkRep();
    }
    public synchronized void insert(int position, String insertion) {
        text = text.substring(0, position) + insertion + text.substring(position);
        checkRep();
    }
    public synchronized void delete(int position, int len) {
        text = text.substring(0, position) + text.substring(position+len);
        checkRep();
    }
    public synchronized int length() {
        return text.length();
    }
    public synchronized String toString() {
        return text;
    }
}
```
* Some more to note in exercice:
  * Suppose sharedList is a List returned by Collections.synchronizedList.此时sharedList单个操作是threadsafe的，但设计多个操作的组合时则不能保证。
  * reemtrant locks(可重入互斥锁，互斥锁的一种):如果对已经上锁的普通互斥锁进行“加锁”操作，其结果要么失败，要么会阻塞至解锁。而如果换作可重入互斥锁，当且仅当尝试加锁的线程就是持有该锁的线程时，类似的加锁操作就会成功。可重入互斥锁一般都会记录被加锁的次数，只有执行相同次数的解锁操作才会真正解锁。这样的设计的原因是为了给递归的method加锁。有些语言不支持这种。(from wikipedia) 比如
    ```java
    synchronized (obj) {
        // ...
        synchronized (obj) { // <-- uh oh, deadlock?
            // ...
        }
        // <-- do we own the lock on obj?
    }
    ```
* Locking discipline:两条，见讲义

##### Atomic operations
...例子例子
The synchronized keyword is not a panacea（灵丹妙药）. Thread safety requires a discipline — using confinement, immutability, or locks to protect shared data. And that discipline needs to be written down, or maintainers won’t know what it is.

##### Designing a datatype for concurrency

##### Deadlock rears its ugly head
1. Deadlock solution 1: lock ordering
   * def: put an ordering on the locks that need to be acquired simultaneously, and ensuring that all code acquires the locks in that order
    e.g.
    ```java
     public void friend(Wizard that) {
        Wizard first, second;
        if (this.name.compareTo(that.name) < 0) {
            first = this; second = that;
        } else {
            first = that; second = this;
        }
        synchronized (first) {
            synchronized (second) {
                if (friends.add(that)) {
                    that.friend(this);
                } 
            }
        }
    }
    ```
    原理: **The ordering on the locks forces an ordering on the threads acquiring them, so there’s no way to produce a cycle in the waiting-for graph.** 讲义有详尽解释。

2. Deadlock solution 2: coarse-grained locking
    * def: use a single lock to guard many object instances, or even a whole subsystem of a program.
    e.g.
    In the code below, all Wizards belong to a Castle(注意，必须要所有Wizzard对象的castle是同一个，不然无法防止死锁), and we just use that Castle object’s lock to synchronize:
    ```java
    public class Wizard {
        private final Castle castle;
        private final String name;
        private final Set<Wizard> friends;
        ...
        public void friend(Wizard that) {
            synchronized (castle) {
                if (this.friends.add(that)) {
                    that.friend(this);
                 }
            }
        }
    }
    ```
    * 这个方法可能丧失程序的并行性

**一个误解**：如果多个线程使用相同的锁对象进入同步块，*即使同步块内的内容不同，这些线程也不能并行执行这些同步块中的代码*。synchronized 关键字确保在同一时间只有一个线程能够持有锁对象，并执行该锁对象保护的同步代码块。其他试图进入该同步块的线程必须等待，直到持有锁的线程退出同步块并释放锁。**因为synchronized的机制就是依赖于对象自身带的锁的**
 
#### Sum
所有并行程序都需要考虑两个问题：
1. safety:  Does the concurrent program satisfy its invariants and its specifications? Races in accessing mutable data threaten safety. Safety asks the question: can you prove that some bad thing never happens? 即没有Race condition
2. Liveness. Does the program keep running and eventually do what you want, or does it get stuck somewhere waiting forever for events that will never happen? Can you prove that some good thing eventually happens?  没有死锁是其中一种

在不同应用场景中的不同解决策略