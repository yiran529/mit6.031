General Rules
* Use == for comparing primitive values, like ints, chars, and doubles.
* Use equals() for comparing object values, like lists, arrays, strings, and other objects.
Some Tricky things
Using equals() on primitive types is fortunately easy to catch. 5.equals(5) produces a static error because Java doesn’t allow calling any method on a primitive type.

But the other mistake, using == to compare object values for equality, is much more painful, because == is overloaded in Java. When used on object types, == tests whether the two expressions refer to the same object in memory. In terms of the snapshot diagrams we’ve been drawing, two references are == if their arrows point to the same object bubble. In Python, this operator is called is.(No Error but bug)



---------------------------------------------------------------------------------------------
#### Explain for an exercice
Suppose you have this code:

String s = "foobar";
String t = "foobar";
if (s == t) { ... }
Which of these statements is true?

s == t might return true, because Java might cleverly reuse the same String object for both s and t

The incorrect answers are explained in the previous section of the reading, so here is an explanation of the correct answer.

Sometimes Java is too clever, at the cost of safety from bugs. Java keeps a hash-indexed pool of string objects, particularly to save memory space for literal strings, like "foobar" here.

Java reuses an existing String object from the pool when it sees the same literal string in your code. As a result, you may find that an expression that simply compares literals, like "foobar" == "foobar", returns true! This doesn’t mean that == is actually comparing the individual characters of the strings. It just means that "foobar" on the righthand side happened to be exacly the same object as "foobar" on the lefthand side, because it was reused from the pool.

Java generally doesn’t consult this pool of objects when manipulating strings at runtime. So "FOOBAR".toLowerCase() == "foobar" returns false – probably. The implementation of toLowerCase() may opt to reuse an existing "foobar" from the pool, rather than creating a fresh string object. Its spec doesn’t say one way or the other.

You can see this behavior in a small example program, or learn more about the string literal pool at String.intern().

In any case, the correctness of code should not depend on whether or not Java uses its internal string-caching pool. Use equals() for strings, not ==.