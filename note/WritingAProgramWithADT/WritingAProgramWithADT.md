## Writing a program with ADT
Bringing together what we’ve learned about abstract data types and recursive data types, this reading demonstrates the development of a recursive ADT to solve a simple problem.
#### Recipe for programming
Writing a program (consisting of ADTs and static methods):

1. Choose datatypes. Decide which ones will be mutable and which immutable.
2. Choose static methods. Write your top-level main method and break it down into smaller steps.
3. Spec. Spec out the ADTs and methods. Keep the ADT operations simple and few at first. Only add complex operations as you need them.
4. Test. Write test cases for each unit (ADT or method).
5. Implement simply first. Choose simple, brute-force representations. The point here is to **put pressure on the specs and the tests**(Also, writing tests put pressure on spec), and try to pull your whole program together as soon as possible. Make the whole program work correctly first. Skip the advanced features for now. Skip performance optimization. Skip corner cases. Keep a to-do list of what you have to revisit.
6. Iterate. Now that it’s all working, make it work better. Reimplement, optimize, redesign if necessary.

#### A big example
矩阵乘法
...