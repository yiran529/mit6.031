## Debugging
The topic of today’s class is systematic debugging.
#### Reproduce the bug
* Start by finding a **small, repeatable** test case that produces the failure.比如你写了一个程序找莎士比亚文集中出现最多的单词，出现了错误，没可能用传统的打断点等方式对所有文本进行调试，此时可能的调试方法有：对前一半文本运行程序，对某一段对话运行程序
* any effort you put into making the test case small and repeatable will pay off, because you’ll have to run it over and over while you search for the bug and develop a fix for it. Furthermore, after you’ve fixed the bug, you’ll want to keep the test case in your regression test suite, so that the bug never crops up again. Once you have a test case for the bug, making this test work becomes your goal.
#### Find the bug using the scientific method
//Just FIND, not FIX
* four steps:
    1. study the data
    2. hypothesize
    3. experiment
    4. repeat
* 不是每个bug的查找都必须经过上面的过程。如果你的程序是fail fast的，那么you’ll hopefully get an exception very close to the source of the bug, the stack trace will lead you right to it, and the mistake will be obvious from inspecting the code. A good rule of thumb is the 10-minute rule. If you’ve spent 10 minutes hunting for a bug using ad hoc, unsystematic inspection, then it’s time to take a step back and start applying the scientific method instead.
* When using the "scientific method", you should start taking notes, 记录下你的迭代过程

下面将详细地解释各个步骤
##### Study the data
* Look at the test input that causes the bug, and examine the incorrect results, failed assertions, and stack traces that result from it.
* One important form of data is the stack trace from an exception. Practice reading the stack traces that you get, because they will give you enormous amounts of information about where and what the bug might be.注意，在栈底的一些信息可能是Java库自带的，你需要看你自己写的内容。
##### Hypothesize
* It can help to think about your program as a flow of data, or **steps** in an algorithm, and try to rule out whole sections of the program at once. 假设bug出现的位置的可能性：在某个模块？在某两个模块连接处？由于查找bug是一个查找过程，每个模块又都是有序的，我们可以采用二分查找的办法尝试每种可能性
* Slicing: finding the parts of a program that contributed to computing a particular value.
    e.g.
    ```java
    int x = 0;
    final int bonus = getBonus();
    ...
    for (final Sale s : salesList) {
    ...
    if (isWorthABonus(s)) {
        x += bonus;
    }
    ...
    }
    ...
    System.out.println("x=" + x);  // prints a negative number
    ```
    如果x的值有错，那么上面的语句都包含在x的slice里，我们可以根据slice中的语句进行假设。The upshot is that careful slicing – which you can do just by reading the code – can help you systematically identify places where the bug could be, and also where it could not be. We can rule out the ... code in this example, because it doesn’t contribute to the bad value of x.
    提高slicing效率的设计方法：采用局部变量；利用immutability
* Delta debugging: 比如你有两个相近的test case，其中一个成功，另一个fail，我们可以依次提出假设：Which code is executed for the passing test case, but skipped for the failing test case, and vice versa? One hypothesis is that the bug lies in those lines of code, the delta between the passing run and the failing run.     或比较不同版本代码的区别
* Prioritize hypotheses: When making your hypothesis, you may want to keep in mind that different parts of the system have different likelihoods of failure.比如你的老代码可能比新加的代码更可信， Java库的代码比你的代码可信， 编译器和OS最可信

##### Experiment
* Your experiment should be chosen to test the prediction. The best experiment is a **probe**（注意，是找bug的位置，而不是直接改它）, a gentle observation of the system that disturbs it as little as possible.
* One familiar probe is a print statement. 优点：几乎适用于所有编程语言；缺点：更改了程序，容易忘记改回去。另外，注意不要打印一些没有意义的语词比如“hi”，而应该是类似于“calculateTotalBonus”的东西便于你确定bug所在的位置。
* A more elaborate kind of print debugging is logging, which keeps informative print statements permanently in the code and turns them on or off with a global setting, like a boolean DEBUG constant or a log level variable. 有些Logging framwork可以帮你log到server或network中，且让info更可读。在一些很复杂的系统中，没有logging非常难进行开发。
* Another kind of probe is an assertion that tests variable values or other internal state.优点： 不用手动去查看输出，一般也不用去把它删掉  缺点：容易忘记修改默认的assertion关闭
* breakpoint
* Swap components: If you hypothesize that the bug is in a module, and you happen to have a different implementation of it that satisfies the same interface, then one experiment you can do is to try swapping in the alternative.比如你怀疑是binarySearch()的问题，那么你可以用简单版本的linearSearch()来替代试试
* One bug at a time
    1. Keep a bug list
    2. Don’t get distracted from the bug you’re working on.  Keep your code changes focused on careful, controlled probes of one bug at a time.
* It’s tempting to try to do an experiment that seems to fix the hypothesized bug, instead of a mere probe. This is almost always the wrong thing to do. First, it leads to a kind of ad hoc guess-and-test programming, which produces awful, complex, hard-to-understand code. Second, your fix may just mask the true bug without actually removing it — treating the symptom rather than the disease.比如，看到数组越界错误不要直接给越界一个exception就算了，要找到真正的产生这个问题的问题。

##### Repeat
After the experiment, reflect on the results and modify your hypothesis. If what you observed disagreed with what the hypothesis predicted, then reject that hypothesis and make a new one. If the observation agreed with the prediction, then refine the hypothesis to narrow down the location and cause of the bug. Then repeat the process with this fresh hypothesis.
* Keep an audit trail.Keep a log in a text file of what you did, in what order, and what happened as a result, including the hypothesis, the experiment and what you boserve as the result of the experiment
* Check the plug: If you have been iterating on a bug and nothing makes sense, consider "check the plug". An example of “checking the plug” in programming is to make sure your source code and object code are up to date. 
* If YOU didn’t fix it, it isn’t really fixed




#### Fix the bug
Once you find the bug and understand its cause, the third step is to devise a fix for it. Sth you should do:
* Look for related bugs, and newly-created ones.
* Undo debugging probes. 
* Make a regression test.