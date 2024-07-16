## recursion
#### Recursion
A recursive func is defined in terms of base cases and recursive steps.
#### Choosing the right decomposition for a problem
* Finding the right way to decompose a problem, such as a method implementation, is important. Good decompositions are simple, short, easy to understand, safe from bugs, and ready for change.    **Recrusion** is an elegant and simple decomposition for some problems.
#### Helper method
#### Choosing the right recursive subproblem
#### Recursive problems vs. recursive data
#### Mutual recursion
#### Reentrant(可重入) code
* Recursion – a method calling itself – is a special case of a general phenomenon in programming called **reentrancy**. Reentrant code can be safely re-entered, meaning that it can be called again even while a call to it is underway. Reentrant code keeps its state entirely in parameters and local variables, and doesn’t use static variables or global variables, and doesn’t share aliases to mutable objects with other parts of the program, or other calls to itself. 
* It’s good to design your code to be reentrant as much as possible. Reentrant code is safer from bugs and can be used in more situations, like concurrency, callbacks, or mutual recursion.
#### When to use recursion rather than iteration
Some reasons to use recursion
* The problem is naturally recursive or the data are naturally recursive
* Another reason to use recursion is to take more advantage of immutability. In an ideal recursive implementation, all variables are final, all data are **immutable**, and the recursive methods are all pure functions in the sense that they do not mutate anything. The behavior of a method can be understood simply as a relationship between its parameters and its return value, with no side effects on any other part of the program. This kind of paradigm is called **functional programming**, and it is far easier to reason about than **imperative programming** with loops and variables.
#### Common mistakes in recursive implementations
* The base case is missing entirely or not all the base cases are covered
* The recursive step does not reduced to a smaller subproblem
* Aliases to a mutable data structure are inadvertently shared, and mutated, among the recursive calls.
