## Avoid Debugging
### First Defense: Make bugs impossible
* stactic checking
* dynamic checking
* immutability
_____________________________________________________________________________________________________-
### Second defense: Locolize bugs
If we can’t prevent bugs, we can try to localize them to a small part of the program, so that we don’t have to look too hard to find the cause of a bug. When localized to a single method or small module, bugs may be found simply by studying the program text.
One way: fail fast(by checking precondition)
Checking preconditions is an example of defensive programming. Real programs are rarely bug-free. Defensive programming offers a way to mitigate the effects of bugs even if you don’t know where they are.
Another way: Assertion (introduced below)
The third way: Incremental development增量开发
#### Assertions
* In Java, assert is not a method but a built-in statement in the language. The simplest form of assertion takes a boolean expression and throws AssertionError if the boolean expression evaluates to false
* assert x >= 0 : "x is " + x;//冒号及后面的语句是提示性语句，不是必须的，但如果有就会很好
* A serious problem with Java assertions is that assertions are off by default.因为asserions可能降低程序效率，我们需要手动打开它
* 代码实现里的assert 和JUnit里的assertEquals等是不一样的，后者不用手动开启
#### What to assert
* Method arguments requirements: assert x >= 0;
* Method return value requirements
     This kind of assertion is sometimes called a self check. For example, the sqrt method might square its result to check whether it is reasonably close to x: assert Math.abs(r*r - x) < .0001;
* When should you write runtime assertions? As you write the code, not after the fact.否则你可能会忘记一些重要的不变量
#### What not to assert
Runtime assertions are not free. They can clutter the code, so they must be used judiciously. 
* avoid trival（没有价值的）assertions: 
    x = y + 1; assert x == y+1;
* Never use assertions to test **conditions that are external to your program**, such as the existence of files, the availability of the network, or the correctness of input typed by a human user. Assertion failures indicate bugs. External failures are not bugs, and no change you can make to your program in advance that will prevent them. External failures should be handled using exceptions instead, like FileNotFoundException or NoRouteToHostException.
* Java's assertion mechanisms are designed so that assertions are executed only during testing and debugging, and **turned off** when the program is released to users. Thus, the correctness of your program should never depend on whether or not the assertion expressions are executed. In particular, asserted expressions should not have side-effects. 
    e.g. assert list.remove(x);//Do not do this!
    Instead: boolean found = list.remove(x); assert found;
#### Incremental development
Build only a bit of your program at a time, and test that bit thoroughly before you move on.
Two ways:
* Unit testing
* regression testing
_________________________________________________________________________________________________________ 
### Modularity & encapsulation 模块化与封装
* Modularity means dividing up a system into components, or modules, each of which can be designed, implemented, tested, reasoned about, and reused separately from the rest of the system. 
* Encapsulation means building walls around a module so that the module is responsible for its own internal behavior, and bugs in other parts of the system can’t damage its integrity.
    1. One kind of encapsulation is access control, using public and private to control the visibility and accessibility of your variables and methods.
    2. Another kind of encapsulation comes from variable scope.Keeping variable scopes as small as possible makes it much easier to reason about where a bug might be in the program. 
        比如你写了一个for循环死循环了，如果i是全局变量，那么i可能是受到在循环里的一个函数的作用，但如果只是一个局部变量那就不用管循环里的函数了
        Some rules
        * Always declare a loop variable in the for-loop initializer. 
        * Declare a variable only when you first need it, and in the innermost curly-brace block that you can.
        * Avoid global variables. Using global variables is a very bad idea, especially as programs get large. 

  