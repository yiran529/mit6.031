## Code review
#### What is code review
Code review is careful, systematic study of source code by people who are not the original author of the code. It’s analogous to proofreading a paper.
#### Some code styles
* Don't repeat yourself(DRY)
  Because it might be dangerous and not easy to dubug.
#### Comments where needed 
* specification
* specify the source of a piece of code
* some comments are bad and unnecessary eg.direct translation of code into English
* obscure code should get a comment eg. to use some math formula
#### Fail faster
Failing fast means that code should reveal its bugs as early as possible. 
Static checking fails faster than dynamic checking, and dynamic checking fails faster than producing a wrong answer that may corrupt subsequent computation.
#### Avoid magic number
Declare constants(sometimes except 0,1,2) as named constants with good clear name. 
When is magic:when their origin is obscure, or when their purpose in the computation is obscure, or when they have hidden dependencies on other constants.
#### (only) One purpose for each variables
* Don’t reuse parameters, and don’t reuse variables. Variables are not a scarce resource in programming. Introduce them freely, give them good names, and just stop using them when you stop needing them.
* Method parameters, in particular, should generally be left unmodified. And use `final` more often.
#### Use good names
* Good method and variable names are **long and self-descriptive**.Comments can often be avoided entirely by making the code itself more readable, with better names that describe the methods and variables.
  
Good names: secondsPerDay

Awful names: tem, data(symptoms of extreme programmer laziness)

* Some rules: classes typically start with a capital letter, and variable names and method names start with a lowercase letter;choose short words and be concise;avoid abbreviations;avoid single-character varable names entirely except where they are easily understood by convention

good names imply sth about their types
#### Use whitespace to help the readers
#### Don't use global variables
#### Methods should return results not print them
In general, only the highest-level parts of a program should interact with the human user or the console. Lower-level parts should take their input as parameters and return their output as results. 
#### Avoid special-case code
Actively resist the temptation to handle special cases separately. If you find yourself writing an if statement for a special case, stop and instead think harder about the general-case code, either to confirm that it can actually already handle the special case you’re worrying about, or put in a little more effort to make it handle the special case.Tackle the general case first.

Writing broader, general-case code pays off. It results in a shorter method, which is easier to understand and has fewer places for bugs to hide. It is likely to be safer from bugs, because it makes fewer assumptions about the values it is working with. And it is more ready for change, because there are fewer places to update when a change to the method’s behavior is made.