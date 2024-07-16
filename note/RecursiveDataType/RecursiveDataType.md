## Recursive Data Type
Let's start with immutable list.
#### Immutable lists
(Immutability is powerful not just because of its safety, but also because of the potential for sharing. )
Immutable list由表头和表尾构成，表尾也是一个list。（类似于广义表的递归定义）

Implementing immutable lists in Java
详见讲义。
注
1. 这个实现是可以实现list共享的
2. 这个是实现使用了两个不同的类Empty和Cons合作实现接口（为什么要这样？因为空list和非空list的rep和一些操作有所不同，不能完全用Cons替代Empty），这与ArrayList和LinkedList是List接口的两种可相互替代的选择不同.

#### Recursive datatype definitions
* recursive definition of ImList: the set ImList<E> consists of the values represented either by an Empty object, or by a Cons object, where a Cons object has fields consisting of elt of type E and rest of type ImList<E>
* Formally, a datatype definition has:
    （讲义中的描述有点难懂，但大致意思你可以看下面两个例子总结）：
    1. ImList<E> = Empty + Cons(elt:E, rest:ImList<E>)
    2. Tree<E> = Empty + Node(e:E, left:Tree<E>, right:Tree<E>) （意即，要么是Empty要么是Node，Empty是base case，因为Empty是不含递归的）
    3. Geology = Core(a:int, b:int, c:int, d:int)
          + Planet(core:Core, a:int, b:int, c:int, d:int)
          + System(geology:Geology, a:int, b:int)
        (Geology可以是System，可以是Planet，也可以是base case： Core)
    4. T = A(x:int) + B(a1:A, a2:A)  （要么是A，要么是由连个A组成的B）
    5. V = F(z:int, v:V) + G(z:int, v:V)没有base case，错
   When you write a recursive datatype, document the recursive datatype definition as a comment in the interface

#### Functions over recursive datatypes
```java
public interface ImList<E> {
    // ...
    public int size();
}

public class Empty<E> implements ImList<E> {
    // ...
    public int size() { return 0; }
}

public class Cons<E> implements ImList<E> {
    // ...
    public int size() { return 1 + rest.size(); }
}
```

#### Null vs. empty
DON'T get rid of the Empty class and just use null instead.
Using an object, rather than a null reference, to signal the base case or endpoint of a data structure is an example of a design pattern called sentinel(哨兵) objects. 
The enormous advantage that a sentinel object provides is that it acts like an object in the datatype, so you can call methods on it.不必处处判断list != null，增强了代码的可读性

#### Declared type vs. actual type
在interface的dynamic patch有讨论过
Variables have a declared type; objects, created during the actual execution of the program, have an actual type.
请区分variables和object（variable指向obj）

#### Backtracking search with immutablity
看起来不是很重要，我略过了