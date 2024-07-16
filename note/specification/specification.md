## Specifications
#### Behavioral equivalence
* spec does not need to descrbe when the requirement is not satisfied
#### Why specifications
* It is some kinds of contract between implementations and clients.
*  Precise specifications in the code let you apportion blame (to code fragments, not people!), and can spare you the agony of puzzling over where a bug fix should go.
*  It is easier for clients to read specifications than codes.
*  Specifications let the implementer to change the code without telling the clients.
*  Specifications make code faster. Because implementers could skip some check with the restriction on inputs.
*  allowing the code of the module and the code of a client to be changed independently, so long as the changes respect the specification.
  
#### Specifications structure
Abstractly speaking, a specification of a method has several parts:
* a method signature, giving the name, parameter types, return type, and exceptions thrown
* a requires clause, describing additional restrictions on the parameters
* an effects clause, describing the return value, exceptions, and other effects of the method

These parts form the precondition and the postcondition
* The precondition is an obligation on the client.
* The postcondition is an obligation on the implementer of the method. 
* The overall structure is a logical implication: if the precondition holds when the method is called, then the postcondition must hold when the method completes; otherwise, the method could be free to do anything.(When the precondition is violated, we can make it easier to fix the bugs by fail test criteria though the implementers are not obliged to do it.)

#### Specifications in JAVA
examples(Java doc 写法):

/**
 * Find a value in an array.
 * @param arr array to search, requires that val occurs exactly once
 *            in arr
 * @param val value to search for
 * @return index i such that arr[i] = val
 */
static int find(int[] arr, int val)

#### Don't talk about local variables of the method or private fields of the method’s class in specifications

#### Do not allow null references
null values are disallowed in parameters and return values unless the spec explicitly says otherwise.
But generally speaking, avoid null(it might cause "a staggering variety of bugs")
But emptiness is OK.

#### Specifications and testing
Even glass box testing should follow specifications, which means the test cases should not count on the bahaviours that specifiations do not define
So what does glass box testing mean, if it can’t go beyond the spec? It means you are trying to find new test cases that exercise different parts of the implementation, but still checking those test cases in an implementation-independent way, following the spec.
 A good unit test is focused on just a single specification.
 Good integration tests, tests that use a combination of modules, will make sure that our different methods have compatible specifications

 #### Specifications for mutating methods
 * we should describe side-effects — changes to mutable objects — in the postcondition.
 * Just as null is implicitly disallowed unless stated otherwise, programmers also assume that mutation is disallowed unless stated otherwise.

#### Exceptions
* Exceptions for signaling bugs  这种一般是非检查型异常
* Exceptions for special results  这种一般是检查型异常
  We don't need to return special results for special cases, instead we can throw exceptions.

#### Checked and unchecked exception
看官方文档或本文件夹下的另一个md

#### Declare exceptions in Java
* Since an exception is a possible output from a method, it may have to be described in the postcondition for the method.
The Java way of documenting an exception as a postcondition is a @throws clause in the Javadoc comment. 
* bugs in either the client or the implementation – are not part of the postcondition of a method, so they **should not** appear in either @throws or throws. For example, NullPointerException need never be mentioned in a spec.
