## Equality
#### Equivalence realation
离散数学中,E是一个等价关系，当且仅当它是自反，对称且传递的
And similarly for the method.equals()
An equality operation should always be an equivalence relation, or surprising behavior and bugs will result.
#### Equallity of immutabel types
* How to define?
    1. Using the abstraction function. we would say that a equals b if and only if AF(a) = AF(b).
    2. Using observation. two objects are equal if and only if they cannot be distinguished by calling **any** operations(namely defined observers) of the abstract data type
#### == vs. equals()
* 1.  The == operator compares references.  More precisely, it tests reference equality. Two references are == if they point to the same storage in memory.
  2. The equals() operation compares object contents – in other words, object equality, in the sense that we’ve been talking about in this reading. The equals operation has to be defined appropriately for every abstract data type.
* Note that == unfortunately flips its meaning between Java and Python
#### Implementing equals()
* the default meaning of equals() is the same as reference equality. For immutable data types, this is almost always wrong. So you have to override the equals() method.
##### The wrong way to implement equals()

e.g.
```java
public class Duration {
    ...   
    // Problematic definition of equals()
    public boolean equals(Duration that) {
        return this.getLength() == that.getLength();        
    }
}
...
Duration d1 = new Duration (1, 2);
Duration d2 = new Duration (1, 2);
Object o2 = d2;
d1.equals(d2) → true
d1.equals(o2) → false
```
为什么会这样？因为我们本来想override，但结果却overload了
内部代码大概是这样的：
```java
public class Duration extends Object {
    // explicit method that we declared:
    public boolean equals(Duration that) {
        return this.getLength() == that.getLength();
    }
    // implicit method inherited from Object:
    public boolean equals(Object that) {
        return this == that;
    }
}
``` 
If we pass a Duration reference, as in d1.equals(d2), we end up calling the equals(Duration) version. This happens even though o2 and d2 both point to the same object at runtime! Equality has become inconsistent.
How to avoid?     This is such a common error that Java has a language feature, the annotation `@Override`, which you should use **whenever your intention is to override a method in your superclass**. With this annotation, the Java compiler will check that a method with the same signature actually exists in the superclass, and give you a compiler error if you’ve made a mistake in the signature.



##### A better way to implement equals()
```java
@Override
public boolean equals(Object that) {
    return that instanceof Duration && this.sameValue((Duration)that);
}

// returns true iff this and that represent the same abstract value
private boolean sameValue(Duration that) {
    return this.getLength() == that.getLength();
}
```
(In general, using instanceof in object-oriented programming is a bad smell. In good object-oriented programming, instanceof is disallowed anywhere except for implementing equals. This prohibition also includes other ways of inspecting objects’ runtime types. )

#### The object contract
The specification of the Object class is so important that it is often referred to as the Object Contract. The contract can be found in the method specifications for the Object class. Here we will focus on the contract for equals.
1. equals must define an equivalence relation
    反例：ADT为时间，用observer，如果两个时间相差五秒以内则认为相等。事实上，这样不满足对称性。
2. equals must be consistent.现在是equal的必须保证以后都是equal 
3. x.equals(null) should return false
4. 对于两个equals()判定为相等的对象，hasCode()结果应该一样
    hashCode的默认实现是这样的：
    ```java
    public class Object {
        ...
        public boolean equals(Object that) { return this == that; }
        public int hashCode() { return /* the memory address of this */; }
    }
    ```
    此时如果equals的实现不变，那么Object contract is satisfied。否则，就不满足。Immutable objects need a different implementation of hashCode(). For Duration, since we haven’t overridden the default hashCode() yet, we’re currently breaking the Object contract:
    ```java
    Duration d1 = new Duration(1, 2);//表示1min2seconds
    Duration d2 = new Duration(1, 2);
    d1.equals(d2) → true
    d1.hashCode() → 2392
    d2.hashCode() → 4823
    ```

    You can, for example:
    ```java
    @Override
    public int hashCode() {
        return (int) getLength();
    }
    ```
    Here is a rule:
        **Always override hashCode when you override equals.**
    And, remember to use **Override**, 让编译器帮你检查， 避免你出现一些奇怪的错误。比如将hashCode()写成hashcode()。

#### Equality of mutable types
首先需要保证是等价关系
 let’s refine our definition and allow for two notions of equality based on observation:
 1. **observational equality** means that two references cannot be distinguished now, in the current state of the program. A client can try to distinguish them only by calling operations that don’t change the state of either object (i.e. only observers and producers, *not mutators*) and comparing the results of those operations.This tests whether the two references “look” the same for the current state of the object.
 2. **behavioral equality** means that two references cannot be distinguished now or in the future, *even if a mutator is called* to change the state of one object but not the other. This tests whether the two references will “behave” the same, in this and all future states.

 Obviouly, behavioral equality is a stronger notion.

 #### Breaking a HashSet's rep invariant
 看起来对于mutable types应该用observational equality，但实际上这样会产生一些问题，以下是例子：
 ```java
List<String> list = new ArrayList<>();
list.add("a");

Set<List<String>> set = new HashSet<List<String>>();
set.add(list);

set.contains(list) → true

list.add("goodbye");

set.contains(list) → false!
for (List<String> l : set) { 
    set.contains(l) → false! 
}
 ```
 What's going on? List<String> is a mutable object. In the standard Java implementation of collection classes like List, mutations affect the result of equals() and hashCode().  当list加了一个"goodbye"后，list的哈希值会发生改变，与原来的哈希值不同，这样就导致了问题。（可能也因为不equals从而导致一些其它问题？）

 The Java library is unfortunately inconsistent about its interpretation of equals() for mutable classes. Collections like List, Set, and Map use observational equality, but other mutable classes like StringBuilder, and arrays, use behavioral equality.
                  ||
                  ||
                  ||
                  ||
                __||__
                \    /
                 \  / 
                  \/
#### The final rule for equals() and hashCode()
The lesson we should draw from this example is that **equals() should implement behavioral equality**. Otherwise instances of the type cannot be safely stored in a data structure that depends on hashing, like HashMap and HashSet.

For immutable types:
* Behavioral equality is the same as observational equality.
* equals() must be overridden to compare abstract values.
* hashCode() must be overriden to map the abstract value to an integer.
  
For mutable types:
* Behavioral equality is different from observational equality.
* equals() should generally not be overriden, but inherit the implementation from Object that compares references, just like ==.
* hashCode() should likewise not be overridden, but inherit Object’s implementation that maps the reference into an integer.

#### Autoboxing and equality


#### Sum
* Equality should be an equivalence relation (reflexive, symmetric, transitive).
* Equality and hash code must be consistent with each other, so that data structures that use hash tables (like HashSet and HashMap) work properly.
* The abstraction function is the basis for equality in immutable data types (which means immutable types must override equals(), and therefore hashCode()).
* Reference equality is the basis for equality in mutable data types; this is the only way to ensure consistency over time and avoid breaking rep invariants of hash tables.
