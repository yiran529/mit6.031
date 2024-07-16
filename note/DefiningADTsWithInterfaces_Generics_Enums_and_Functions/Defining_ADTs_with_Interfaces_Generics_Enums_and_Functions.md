## Defining ADTS with Interfaces, Generics(泛型), Ecums, and Functions
#### Interfaces
* An interface in Java is a list of method signatures without method bodies. 
* A class implements an interface if it declares the interface in its implements clause, and provides method bodies for all of the interface’s methods. So one way to define an abstract data type in Java is as an interface. The type’s implementation is then a class implementing that interface.
* Advantage of defining ADTs with interfaces: 
  1. The client can’t create inadvertent(无意的) dependencies on the ADT’s rep, because fields can’t be put in an interface at all. The implementation is kept well and truly separated, in a different class altogether.
  2. Another advantage is that multiple different representations of the abstract data type can coexist in the same program, as different classes implementing the interface. 
  3. Java’s static type checking allows the compiler to catch many mistakes in implementing an ADT’s contract. 比如它会检查你有没有漏了某个方法没有实现
* About interfaces in Java: Syntactically, a Java interface contains the spec of an ADT, namely its public method signatures. Something to note:
   1. Interfaces不包含关于rep的信息，所以不能用new对interfaces声明任何实例变量，而且interfaces也不能声明constructor
* Java8的新特性
    1. interfaces可以有default instance method with method bodies.这是为了解决这样的问题而设计的：如果一个interface需要新增一个方法，那么就需要在所有实现了这个interface的class中增加这个抽象方法的实现，非常麻烦。具体使用方法如下：
    ```java
    // A simple program to Test Interface default 
    // methods in java 
    interface TestInterface 
    { 
        // abstract method 
        public void square(int a); 
  
        // default method 
        default void show() 
        { 
          System.out.println("Default Method Executed"); 
        } 
    } 
  
    class TestClass implements TestInterface 
    { 
        // implementation of square abstract method 
        public void square(int a) 
        { 
            System.out.println(a*a); 
        } 
  
        public static void main(String args[]) 
        { 
            TestClass d = new TestClass(); 
            d.square(4); 

            // default method executed 
            d.show(); 
        } 
    } 
    ```
     2. interfaces can have stactic methods, which works just like stactic methods in classes
    ```java

    // A simple Java program to TestClassnstrate static 
    // methods in java 
    interface TestInterface 
    { 
        // abstract method 
        public void square (int a); 

        // static method 
        static void show() 
        { 
            System.out.println("Static Method Executed"); 
        } 
    } 
  
    class TestClass implements TestInterface 
    { 
        // Implementation of square abstract method 
        public void square (int a) 
        { 
            System.out.println(a*a); 
       } 
  
        public static void main(String args[]) 
        { 
            TestClass d = new TestClass(); 
            d.square(4); 
  
            // Static method executed 
            TestInterface.show(); 
        } 
    } 
    ```
* More to note
    1. 我们希望interfaces是independent of rep的
    2. 可以在class中增加一些它所实现的interfaces中没有的方法
    3. Circular references: 意思就是比如一个interface的一个抽象方法以实现这个interface的class的类型为返回值，这在Java中是允许的，而且有时候是必要的

#### Subtypes
* type is a set of val
* What is subtypes? 比如List，没有哪个object的type是List，他们都是ArrayList或LinkedList（实现List的具体的类）A subtype is simply a **subset** of the supertype: ArrayList and LinkedList are subtypes of List.
* “B is a subtype of A” means “every B is an A.” In terms of specifications: “every B satisfies the specification for A.”即B的spec要**stronger** than A
* Java编译器只能帮你检查部分关于subtype的错误，比如class没按照interface进行实现，但是它不能检查你的subtype的spec是否强于supertype
* 定义方法：
    1. class `implements` interface
    2. interface `extends` interface(即增强spec)，比如`interface SortedList<E> extends Set<E>`
* Sth to note:
  1. So a List might be mutable, or it might be immutable, the spec doesn’t guarantee either way.
  2. Immutability is not the safest assumption. 最好的方法是涉及mutate的地方都用new来copy一份
  3. Mutation of a parameter is indeed disallowed by default（！！！！！！）, because it surprises clients otherwise. But no such restriction applies to the return value 
  4. Java8中List有一种静态方法是List.of(...)，如`list = List.of(a);`，执行结果是The List.of() method returns object that belongs to some hidden class. We can’t say for sure whether it’s a ArrayList (and most likely it isn’t). All we know is that it implements List.所以更specific地说，list指向一个实现List接口的对象，而不仅仅是一个Collection对象
  5. 用来实现接口的抽象方法的具体实现可以比抽象方法更强（即更强的spec），但是不能弱（否则会违背抽象方法的spec），也不能额外地添加throw或增删参数，否则会incomparable
  6. mutable square不是mutable rectangle的subtype（最后一个reading exercice的例子），有点反直觉
* Example: MyString 看讲义 sth to note
    1. 采用了constructor
    2. 在具体方法前加一个`Override`，是比较有必要的(因为编译器可以帮你检查这是不是正确实现了抽象方法)
    3. 加了一个private的constructor用来实现substring的功能，具体原因看讲义
    4. 在具体实现时没必要再写一遍spec，否则不DRY
    5. 你会发现如果用类似`MyString s = new FastMyString(true);`的语句会打破abstraction barrier，Clients must know the name of the concrete representation class，毕竟interfaces中根本就不可能包含constructor方法。在Java8中，提供了一种方法解决这个问题，即允许interfaces包含stactic methods(namely factory method),e.g.:
        ```java
        public interface MyString { 

        /**
        * @param b a boolean value
        * @return string representation of b, either "true" or "false"
        */
         public static MyString valueOf(boolean b) {
             return new FastMyString(b);
        }

        // ...

        }




        MyString s = MyString.valueOf(true);
        System.out.println("The first character is: " + s.charAt(0));
        ```
    但是，Hiding the implementation completely is a tradeoff, because sometimes the client wants a **choice of implementations** with different characteristics.这就是为什么ArrayList and LinkedList are exposed by the Java library, because they vary in performance of operations like get() and insert().

#### Subclassing
* Another way to make a subtype of another type: subclassing
* Subclas can inherit method bodies and the fields, which is different from interfaces
* Just like implementing interfaces, subclassing also imply subtype(stronger spec)
* Subclassing inherits spec. 似乎令程序更DRY and more ready for change(you can override)， 但实际上subclass会导致父类与子类之间的rep exposure和dependence
* **Dynamic dispatch**
    Java中任何类都是Object类的子类，它们都继承了toString(),Equals()等方法
    下面我们假设在FastMyString中重写了toString方法
    ```java
    FastMyString fms = new FastMyString(true);
    fms.toString() → "true"
    ```
    ```java
    Object obj = new FastMyString(true);
    obj.toString() → ?
    ```
    首先需要明确的是，一个变量的类型可以和它所指的对象的类型不一致（比如上面第二个例子）。比如上面第二个例子，obj的static type is Object, while its dynamic type is FastmMyString。编译的时候检查的只能是静态类型。
    所谓Dynamic dispatch就是指，如果子类中重写了方法，如果我们需要调用obj的toString,怎么知道是用Object类的还是用FastMyString重写过的？关键就是看它所指的对象的类型（正因为这只有在runtime才知道，所以叫dynamic dispatch）。所以问号处应该是"true".

#### Generic types
*  generic type: a type whose specification is in terms of a placeholder type to be filled in later
*  注意，静态方法不能直接使用类的泛型参数，因为泛型是与类实例相关的，而静态方法不依赖于类的实例。但是，我们可以在静态方法上使用自己的泛型参数，使得该静态方法可以使用泛型。
    e.g. 
    ```java

    /**
    * A mutable set.
    * @param <E> type of elements in the set
    */
    public interface Set<E> {
        // example creator operation
        /**
        * Make an empty set.
        * @param <F> type of elements in the set
        * @return a new set instance, initially empty
        */
        public static <F> Set<F> make() { ... }
        //*******此时如果Set<String> strings = Set.make();编译器可以理解你是想要搞一个String类型的Set*******

        //下面是observer和mutator的例子，它们的定义就比较正常
        // example observer operations

        /**
        * Get size of the set.
        * @return the number of elements in this set
        */
        public int size();

        /**
        * Test for membership.
        * @param e an element
        * @return true iff this set contains e
        */
        public boolean contains(E e);

        // example mutator operations

        /**
         * Modifies this set by adding e to the set.
        * @param e element to add
        */
        public void add(E e);

        /**
        * Modifies this set by removing e, if found.
        * If e is not found in the set, has no effect.
        * @param e element to remove
        */
        public void remove(E e);
        //...
    }

    ```
* Implementing generic interfaces:
    * We can either write a non-generic implementation that replaces E with a specific type, or a generic implementation that keeps the placeholder.
    * 如果是用第二种方法，注意A generic implementation can only rely on details of a placeholder type E that are explicitly included in the generic interface specification.比如你就不能用只有String才有的method实现

    e.g.
    ```java
    public class CharSet implements Set<Character> {}
    //or
    public class HashSet<E> implements Set<E> {}
    ```

#### Enumerations
* Sometimes an ADT has a small, finite set of immutable values
* When the set of values is small and finite, it makes sense to define all the values as named constants, called an enumeration. Java has the enum construct to make this convenient:
    `public enum Month { JANUARY, FEBRUARY, MARCH, ..., DECEMBER };`
    This enum defines a new type name, Month, in the same way that class and interface define new type names. It also defines a set of named values, which we write in all-caps because they are effectively public static final constants. So you can now write:
    `Month thisMonth = MARCH;`
*  one of the advantages of using an enumeration type is that   there is only ever one object in memory representing each value of the enumeration, and there is no way for a client to create more (no constructors!) So == is just as good as equals() for an enumeration. In fact == is more fail-fast, because it does a static check that both sides have the same enum type, whereas equals() delays that check until runtime.
* In that sense, using an enumeration can feel like you’re using primitive int constants. Java even supports using them in switch statements.But unlike int values, enumerations have more static checking
* An enum declaration can contain all the usual fields and methods that a class can.(例子见讲义)

#### ADTs in non-OOP language
the notion of an abstract data type does not depend on language features like classes, or interfaces, or public/private access control. Data abstraction is a powerful design pattern that is ubiquitous in software engineering.比如C语言中的FILE类型