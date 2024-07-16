## Designing Specifications
#### Determinist vs. underdeterminist spec
* a determinist specs: when presented with a state satisfying the precondition, the outcome is completely determined.
* nondeterminist: behaves one way and sometimes another, even if called in the same program with the same inputs(e.g. random numbers)
* an underdeterminist specs: not determinist(not using random...)  e.g.说返回一个数组中val的位序，但是没说明如果有多个返回哪个，这时不同的implementation的返回结果可能不同
#### Declarative vs. operational specs
* declarative: 只说出outcome,income和二者之间的联系
* operational: 可能设计一些实现的具体细节
#### Stronger vs. weaker specs 
* what does that mean: Making a specification stronger means **shrinking the set of implementations that satisfy it**. It may help to think of “stronger” as more **constrained**, or tighter, while “weaker” is less constrained, looser.
* rules:
    A specification S2 is stronger than or equal to a specification S1 if and only if

    S2’s precondition is weaker than or equal to S1’s,
    and
    S2’s postcondition is stronger than or equal to S1’s, for the states that satisfy S1’s precondition.
    stronger其实意味着在集合上是一个子集。
    如果S2 stronger than S1，那么所有满足S2的实现都能用来实现S1.
一个spec可以change成一个比它强的spec
#### Designing good specifications
##### the spec should be coherent
不要在一个spec里描述两个功能，这时可能意味着你要把这个函数分成两个实现不同功能的函数
##### The result of a call should be informative
不要模糊，产生二义性
##### The spec should be strong enuough 
Of course the spec should give clients a strong enough guarantee in the general case — it needs to satisfy their basic requirements.
可以看讲义里的例子。总之，可能是说，spec给出的函数功能要尽可能好地处理各种特殊情况，从而让调用者使用方便。
##### The spec should be alse week enuough
比如不要说open a file named filename。因为你不能保证一定能打开这个文件（BTW，它也没说清楚打开文件的模式，或这个文件是已经存在还是需要新创建）
##### The spec should use abstract types where possible
比如能让参数为list就没必要限制为ArrayList(比如implemantation does not depend on ArrayList)
#### Precondition or postcondition
*  whether to use a precondition, and if so, whether the method code should attempt to make sure the precondition has been met before proceeding. In fact, the most common use of preconditions is to demand a property precisely because it would be hard or expensive for the method to check it.
*  Users usually don't like precondition. It is implementors' job to make it more convenient for users. (That’s why the Java API classes, for example, tend to specify as a postcondition that they throw unchecked exceptions when arguments are inappropriate. This approach makes it easier to find the bug or incorrect assumption in the caller code that led to passing bad arguments. 即便是API里面已经提过precondition了，在实现里还是要进行相关的异常处理，算是给用户一个保证，方便他们。)
*  better to fail fast: 比如，要求一个参数不为0， 最好就在开始就进行判断， 不要等到后面产生一些垃圾错误信息， 然后再抛出一些误导性的异常
*  异常的检查有时可能降低效率，此时最好还是用precondition。比如二分查找，没可能让程序检查你是否已经排好序（否则直接变成线性时间），此时precondition是必要的。
*  The decision of whether to use a precondition is an engineering judgment. The key factors are the cost of the check (in writing and executing code), and the scope of the method(比如方法是私有的还是公有的). 
#### Access control
不要让helper函数公有，否则程序其它部分可能依赖这个具体实现，从而让程序不ready for changes


重申一件事：Declarative specifications are the most useful in practice. Preconditions (which weaken the specification) make life harder for the client, but applied judiciously they are a vital tool in the software designer’s repertoire, allowing the implementor to make necessary assumptions.