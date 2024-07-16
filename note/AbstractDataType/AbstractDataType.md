## Abstraction Data Type(ADT)
#### What abstraction means
abstraction, modularity, encapsulation, info hiding, seperation of concerns
We have in fact already encountered some of these ideas in previous classesWe
#### User-defined types
The key idea of data abstraction is that a type is characterized by the **operations** 
you can perform on it. 
Explanation:
Users could already define their own types in early programming languages: you could create a record type date, for example, with integer fields for day, month, and year. But what made abstract types new and different was the focus on **operations**: the user of the type would not need to worry about how its values were actually stored, in the same way that a programmer can ignore how the compiler actually stores integers. All that matters is the operations.
#### Classifying types and operations
* Types, whether built-in or user-defined, can be classified as mutable or immutable.
* Operations of an abstract type are classified as followed:
    Creators Producers Observers Mutators
    具体含义请看讲义
#### An abstract type is defined by its operations
So, for example, when we talk about the List type, what we mean is not a linked list or an array or any other specific data structure for representing a list.

Instead, the List type is **a set of opaque values** — the possible objects that can have List type — that satisfy the specifications of all the operations of List: get(), size(), etc. The values of an abstract type are opaque in the sense that a client can’t examine the data stored inside them, except as permitted by operations.

Expanding our metaphor of a **specification firewall**, you might picture values of an abstract type as **hard shells**, hiding not just the implementation of an individual function, but of a set of related functions (the operations of the type) and the data they share (the private fields stored inside values of the type).

The operations of the type constitute its **abstraction**. This is the public part, visible to clients who use the type.

The fields of the class that implements the type, as well as related classes that help implement a complex data structure, constitute a particular **representation**. This part is private, visible only to the implementer of the type.

#### Designing an abstract types
Designing an abstract type involves **choosing good operations** and **determining how they should behave**.
rules(经验法则):
1. It’s better to have a few, simple operations that can be combined in powerful ways, rather than lots of complex operations.
2. Each operation should have a well-defined purpose, and should have a coherent behavior rather than a multitude of special cases. 比如我们不会想给list搞一个sum函数，否则你就要对存储元素类型不同的list定义不同的sum，这样很难使用
3. The set of operations should be adequate in the sense that there must be enough to do the kinds of computations clients are likely to want to do.(A good test is to check that every property of an object of the type can be extracted.)
4. it should not mix generic and domain-specific features.(设计通用还是特定的方法取决于类是通用的还是特定的)

#### Representation independence
* This means that the use of an abstract type is independent of its representation, so that changes in representation have no effect on code outside the abstract type itself. (比如list，无论是ArrayList还是LinkedList都是一样的)
* As an implementer, you will only be able to safely change the representation of an ADT if its operations are fully specified with **preconditions and postconditions**, so that clients know what to depend on, and you know what you can safely change.

#### Difference between specifications, representation, and implementation
The specification of an ADT includes the name of the class (2), the Javadoc comment just before the class (1), and the specifications of its public methods and fields (Javadoc in 5, method signature in 6). These parts are the contract that is visible to a client of the class.

The representation of an ADT consists of its fields (4) and any assumptions or requirements about those fields (3).

The implementation of an ADT consists of the method implementations that manipulate its rep (7).
简而言之，specification就是给用户看的，representation是The private fields of the class that implements the type，implemetation是方法的具体实现

#### Sum
看讲义，总结了lec10的重要概念

#### Testing an ADT
The only way to test creators, producers, and mutators is by calling observers on the objects that result, and likewise, the only way to test observers is by creating objects for them to observe.
e.g.
```java
// testing strategy for each operation of MyString:
//
// valueOf():
//    partiton on return value: true, false
// length(), charAt(), substring():
//    partition on string length: 0, 1, >1
//    partition on this: produced by valueOf(), produced by substring()
// charAt(): 
//    partition on i=0, 0<i<len-1, i=len-1
// substring():
//    partition on start=0, 0<start<len, start=len
//    partition on end=0, 0<end<len, end=len
//    partition on end-start: 0, >0
```