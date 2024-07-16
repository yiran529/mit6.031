## Testing
#### Validation
* 正式推导
* Code Review
* Testing
#### Why software testing is hard
test cases must be chosen carefully and systematically
#### Test-first programming
terms:
* module
* specification
* implementation
* test case & test suite
In test-first programming, you write the spec and the tests before you even write any code. The development of a single function proceeds in this order:

1. Spec: Write a specification for the function.
2. Test: Write tests that exercise the specification.
3. Implement: Write the implementation.

#### Systematic testing
a good test suite must be:
* Correct
* Thorough
* Small
Here is an important principle:
Normally when you’re coding, your goal is to make the program work. But as a test suite designer, you want to make it fail.

#### Choosing test cases by partioning
* disjoint 
* complete
* correct input
* include boundaries:0, emptiness for colletion types...
  one paritition vs. multiple partitions
  eg. BigInteger*BigInteger

#### JUnit 
eg.
~~~
public class MaxTest {
  ...

  @Test
  public void testALessThanB() {
      assertEquals(2, Math.max(1, 2));
  }

  @Test
  public void testBothEqual() {
      assertEquals(9, Math.max(9, 9));
  }

  @Test
  public void testAGreaterThanB() {
      assertEquals(10, Math.max(10, -9));
  }
}
~~~

#### Documenting testing strategy
eg.
~~~
public class MaxTest {
  /*
   * Testing strategy
   *
   * partition:
   *    a < b
   *    a > b
   *    a = b
   */
}
~~~


#### Black Box and White Box


#### Coverage
One way to judge a test suite is to ask how thoroughly it exercises the program. This notion is called coverage. Here are three common kinds of coverage:

* Statement coverage: is every statement run by some test case?
* Branch coverage: for every if or while statement in the program, are both the true and the false direction taken by some test case?
* Path coverage: is every possible combination of branches — every path through the program — taken by some test case?

Infeasible to cverage all.

A good code coverage tool for Eclipse is EclEmma


#### Unit and integration tesing
* unit test: test a single module in isolation
* integration test:tests a combination of modules, or even the entire program
* note: when doing unit test, don't test in a way that the test cases depend on the correctness of other modules. One way to achieve this is to write stub version of the modules it calls.


#### Automated regression test
*  Running all your tests after every change is called regression testing.
*  Whenever you find and fix a bug, take the input that elicited the bug and add it to your automated test suite as a test case. This kind of test case is called a regression test. 
  


#### Iterative test-first programming
* The specification can be incorrect, incomplete, ambiguous, or missing corner cases. Trying to write tests can uncover these problems early, before you’ve wasted time working on an implementation of a buggy spec. Similarly, writing the implementation may help you discover missing or incorrect tests, or prompt you to revisit and revise the specification.
* it doesn’t make sense to devote enormous amounts of time making one step perfect before moving on to the next step.
   Plan for iteration:
    For a large specification, start by writing only one part of the spec, proceed to test and implement that part, then iterate with a more complete spec.

    For a complex test suite, start by choosing a few important partitions, and create a small test suite for them. Proceed with a simple implementation that passes those tests, and then iterate on the test suite with more partitions.

    For a tricky implementation, first write a simple brute-force implementation that tests your spec and validates your test suite. Then move on to the harder implementation with confidence that your spec is good and your tests are correct.

* AN IMPORTANT IDEA ABOUT ITERATION:Iteration is a feature of every modern software engineering process (such as Agile and Scrum), with good empirical support for its effectiveness. Iteration requires a different mindset than you may be used to as a student solving homework and exam problems. Instead of trying to solve a problem perfectly from start to finish, iteration means reaching a rough solution as soon as possible, and then steadily refining and improving it, so that you have time to discard and rework if necessary. Iteration makes the best use of your time when a problem is difficult and the solution space is unknown.

需要注意：编写测试集的书写规范，包括需要写partition的划分原则，以及每一种情况都要写一个单独的method
Document the partitions and subdomains in a comment at the top of the JUnit test class. For example, to document our strategy for testing max, we would write this in MaxTest.java:

public class MaxTest {
  /*
   * Testing strategy
   *
   * partition:
   *    a < b
   *    a > b
   *    a = b
   */
Each test case should have a comment above it saying which subdomain(s) it covers, e.g.:
// covers a < b
  @Test
  public void testALessThanB() {
      assertEquals(2, Math.max(1, 2));
  }
