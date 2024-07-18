#### Exception
After a method throws an exception, the runtime system attempts to find something to handle it. The set of possible "somethings" to handle the exception is the ordered list of methods that had been called to get to the method where the error occurred. The list of methods is known as the call stack.

The runtime system searches the call stack for a method that contains a block of code that can handle the exception. This block of code is called an exception handler. The search begins with the method in which the error occurred and proceeds through the call stack in the reverse order in which the methods were called. When an appropriate handler is found, the runtime system passes the exception to the handler. An exception handler is considered appropriate if the type of the exception object thrown matches the type that can be handled by the handler.

The exception handler chosen is said to catch the exception. If the runtime system exhaustively searches all the methods on the call stack without finding an appropriate exception handler, as shown in the next figure, the runtime system (and, consequently, the program) terminates.

总之，exception跟普通的编译器报错的不同是，它不一定会让程序停止执行（在一些大型项目中让程序停止成本很大），而是可能会根据程序员的设计去处理这些错误

#### Three kinds of exceptions
* checked exception: These are exceptional conditions that a well-written application should anticipate and recover from. 如除零
* error: These are exceptional conditions that are external to the application, and that the application usually cannot anticipate or recover from. 比如硬件问题，内存不足，overflow
* runtime exception(运行时错误) ： These are exceptional conditions that are internal to the application, and that the application usually cannot anticipate or recover from. 比如错误地使用API，空指针错误，数组越界错误

If there is an unchecked exception, it propagates up to the top level of the Java program, which ends the program and displays a stack trace showing where the exception occurred.  

 The last two ones are Unchecked Exception

#### To handle exceptions: try + catch/finally syntax
* go to learn the syntax!
* The pattern of chained exceptions is good for debugging

#### difference between try-catch and throws
try-catch 在当前位置处理异常
throws向上抛出异常，可以无限向上抛，直到到达main函数，此时异常交由JVM虚拟机处理
如果上述的处理都没有，程序就直接停止运行（？）


检查型异常是让编译器检查的异常，一般是special result，比如没找到指定元素，一个method如果要用检查型异常，就必须在signature里面写，如果调用了含检查型异常的方法，也必须有对相关异常的处理
非检查型异常 are not expected to be handled by the code except perhaps at the top level.它们一般是bug（错误）。 We wouldn’t want every method up the call chain to have to declare that it (might) throw all the kinds of bug-related exceptions that can happen at lower call levels: index out of bounds, null values, illegal arguments, assertion failures, etc.

注（GPT回答）：
编译时处理检查型异常的意思是：在编写代码时，编译器会检查所有可能抛出检查型异常的代码路径。如果某个方法可能抛出检查型异常，而代码中没有显式处理（使用try-catch块）或在方法签名中声明（使用throws关键字），编译器将会报错，不允许代码通过编译。这种机制确保了程序员在编写代码时就考虑到并处理了这些可能的异常情况。