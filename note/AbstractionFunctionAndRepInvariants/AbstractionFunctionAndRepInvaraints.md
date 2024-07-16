## Abstraction Function & Rep Invariants
#### Invariants
An invariant is a property of a program that is always true, for every possible runtime state of the program. 
比如:
immutability, 一个变量的类型， 在for循环中i的范围。。。

Saying that the ADT preserves its own invariants means that the ADT is responsible for ensuring that its own invariants hold. This is done by hiding or protecting the variables involved in the invariants (e.g., using language features like private), and allowing access only through operations with well-defined contracts.
这会让debug变得方便，比如String是immutable的，这样我们debug时就能排除string改变的可能性

representation exposure:
* meaning that code outside the class can modify the representation directly. Rep exposure threatens not only invariants, but also representation independence
* Java mechanisms to deal with rep exposure: `private`  and `final`
* 但即便用了上面private和final仍然有可能rep exposure，比如某个类属性是可变对象，如果你把他作为函数返回值传出去，它可能会被改变，或者仅仅是在类外面创建了一个同样类型的变量（由于Java的机制，可能这个对象会被重复利用），这样就也会造成rep exposure，具体例子可见讲义。In general, you should carefully inspect the argument types and return types of all your ADT operations. If any of the types are mutable, make sure your implementation doesn’t store those arguments directly in the rep, or return direct references to the rep, because that causes rep exposure.
* Java provide Immutable wrappers around mutable data types.
    e.g. Collections.unmodifiableList() takes a (mutable) List and wraps it with an object that looks like a List.
    (但如果你试图mutate那个被包起来的list，编译器是不会检查的，只有在runtime时会扔出异常)

#### Rep invariant and abstraction function
* The **space of abstract values** consists of the values that the type is designed to support, from the client’s point of view. 比如Java中的BigInteger的abstract value space就是数学意义上的所有整数
* The **space of representation values** (or rep values for short) consists of the Java objects that actually implement the abstract values.比如BigInteger如果是由数组实现的，那么它的rep values space就是所有这样的数组的集合。rep values不一定是单个简单的类，还有可能是复杂的类，比如LinkedList就由多个结点类构成
* 接下来我们从数学的角度谈谈上面二者的关系：
    * 特点：rep val与abs val之间构成映射关系，一定是满射，但不一定是单射，而且rep val中的元素不一定有对应的像
    * **rep invariant**: 在abs val中有对应的像的rep val中元素的集合(严格定义:是rep val到布尔值的函数，如果rep val能表示一个abs val那么布尔值就是真，否则是假。所以用自然语言描述rep invariant就应该是一个判断句)
    * **abs func**：从rep invariant(不是rep val space)到abs val space的映射（实际上也是函数）
    * Both the rep invariant and the abstraction function should be documented in the code, right next to the declaration of the rep itself:（比如用字符串表示字符集合）

    ```java

    public class CharSet {
        private String s;
        // Rep invariant(RI):
        //   s contains no repeated characters
        // Abstraction function(AF):
        //   AF(s) = {s[i] | 0 <= i < s.length()}
        ...
    }
    
    ```

    * confusion: rep val space 和 abs val space就能确定rep invariant和abs function，为什么还要再多此一举写出来？事实上，二者并不能唯一确定它们！反例可以看讲义。The essential point is that implementing an abstract type means not only choosing the two spaces – the abstract value space for the specification and the rep value space for the implementation – but also deciding which rep values are legal, and how to interpret them as abstract values.
    * **Checking** the RI: If your implementation asserts the rep invariant at run time, then you can catch bugs early. 一般来讲，observer不用check，但是it’s good defensive practice to do so anyway，这样你就可以尽早地发现由rep exposure带来的rep invaraint violation(fail fast)
    * 正如specification里隐含了参数和返回值是non-null的，rep invaiant也隐含了这个条件，我们一般不用特别说明。But checkRep() needs to include those null checks, because they are part of the rep invariant, and you want to catch null bugs as early as possible.有时候你可能希望允许rep invariant是null这时候你需要特别说明，并且三思后行

#### Beneficent mutation
The abstract value should never change. But the implementation is free to mutate a rep value as long as it continues to map to the same abstract value, so that the change is invisible to the client. This kind of change is called beneficent mutation.

#### Documenting the AF, RI, and safety from rep exposure
1. It’s good practice to document the abstraction function and rep invariant in the class, using comments right where the private fields of the rep are declared. 

When you describe the rep invariant and abstraction function, you must be **precise**.

It is not enough for the RI to be a generic statement like “all fields are valid.” The job of the rep invariant is to explain precisely what makes the field values valid or not.   It is not enough for the AF to offer a generic explanation like “represents a set of characters.” The job of the abstraction function is to define precisely how the concrete field values are interpreted.

As a function, if we take the documented AF and substitute in actual (legal) field values, we should obtain out a complete description of the single abstract value they represent.比如“代表一个字符的集合”就非常不精确，因为你不知道是怎么个代表法。
2. Another piece of documentation that 6.031 asks you to write is a **rep exposure safety argument**. This is a comment that examines each part of the rep, looks at the code that handles that part of the rep (particularly with respect to parameters and return values from clients, because that is where rep exposure occurs), and presents a reason why the code doesn’t expose the rep.
   e.g

```java
   // Immutable type representing a tweet.
public class Tweet {

    private final String author;
    private final String text;
    private final Date timestamp;

    // Rep invariant:
    //   author is a Twitter username (a nonempty string of letters, digits, underscores)
    //   text.length <= 280
    // Abstraction function:
    //   AF(author, text, timestamp) = a tweet posted by author, with content text, 
    //                                 at time timestamp 
    // Safety from rep exposure:
    //   All fields are private;
    //   author and text are Strings, so are guaranteed immutable;
    //   timestamp is a mutable Date, so Tweet() constructor and getTimestamp() 
    //        make defensive copies to avoid sharing the rep's Date object with clients.

    // Operations (specs and method bodies omitted to save space)
    public Tweet(String author, String text, Date timestamp) { ... }
    public String getAuthor() { ... }
    public String getText() { ... }
    public Date getTimestamp() { ... }
}
```

#### Update out notion of what a spec may talk about
The specification of an abstract type T – which consists of the specs of its operations – should only talk about **things that are visible to the client**. This includes parameters, return values, and exceptions thrown by its operations. Whenever the spec needs to refer to a value of type T, it should describe the value as an abstract value, i.e. mathematical values in the abstract space A.

The spec should not talk about details of the representation, or elements of the rep space R.

#### ADT invariant replace preconditions
 An enormous advantage of a well-designed abstract data type is that it encapsulates and enforces properties that we would otherwise have to stipulate in a precondition.比如我们希望一个method的参数是一个无重的有序的set，我们可以通过写一个这样的ADT——SortedSet来代替spec里的precondition，因为SortedSet本身已经满足了precondition中的要求，我们只要将它作为参数的类型即可。

#### How to establish invariants
To make an invariant hold, we need to:

* make the invariant true in the initial state of the object; and
* ensure that all changes to the object keep the invariant true.

Translating this in terms of the types of ADT operations, this means:

* creators and producers must establish the invariant for new object instances; and
* mutators, observers, and producers must preserve the invariant for existing object instances.

另外，还要保证没有rep exposure