## Regular Expression
terminals(终结符): they can’t be expanded any further
nonterminals(非终结符): can
A production in a grammar has the form:

    nonterminal ::= expression of terminals, nonterminals, and operators 
    
    e.g. url ::= 'http://mit.edu/'
#### Grammar operators
* Repetition, represented by :*

x ::= y*        // x matches zero or more y

* Concatenation, represented not by a symbol, but just a space:

x ::= y z       // x matches y followed by z

* Union, also called alternation, represented by :|

x ::= y | z     // x matches either y or z 

* 一般 | 的优先级最低
m ::= a (b|c) d      // m matches a, followed by either b or c, followed by d
x ::= (y z | a b)*   // x matches zero or more yz or ab pairs

* 0 or 1 occurrence is represented by :?

x ::= y?       // an x is a y or is the empty string

* 1 or more occurrences is represented by :+

x ::= y+       // an x is one or more y
               //    equivalent to  x ::= y y*

* An exact number of occurrences is represented by , and a range of occurences by , , or :{n}{n,m}{n,}{,m}

x ::= y{3}     // an x is three y
               // equivalent to x ::= y y y 

x ::= y{1,3}   // an x is between one and three y
               // equivalent to x ::= y | y y | y y y

x ::= y{,4}    // an x is at most four y
               // equivalent to x ::=   | y | y y | y y y | y y y y
               //                     ^--- note the empty string here, so this can match zero y's

x ::= y{2,}    // an x is two or more y
               // equivalent to x ::= y y y*

* A character class represents the set of single-character strings matching any of the characters listed in the square brackets:[...]

x ::= [aeiou]  // equivalent to  x ::= 'a' | 'e' | 'i' | 'o' | 'u'

* Ranges of characters can be described compactly using :-

x ::= [a-ckx-z]    // equivalent to  x ::= 'a' | 'b' | 'c' | 'k' | 'x' | 'y' | 'z'

* An inverted character class represents the set of single-character strings matching a character not listed in the brackets:[^...]

x ::= [^a-c]  // equivalent to  x ::= 'd' | 'e' | 'f' | ... | '0' | '1' | '2' | ... | '!' | '@'
              //                          | ... (all other possible characters)

#### Recursion in grammar
http://didit.csail.mit.edu:4949/

To handle this kind of string, the grammar is now:

url ::= 'http://' hostname (':' port)? '/' 
hostname ::= word '.' hostname | word '.' word
port ::= [0-9]+
word ::= [a-z]+

#### Parse trees
The leaves of the parse tree are labeled with terminals, representing the parts of the string that have been parsed. They don’t have any children, and can’t be expanded any further. If we concatenate the leaves together in order, we get back the original string.
例子见讲义

#### Regular expressions
A regular grammar has a special property: by substituting every nonterminal (except the root one) with its righthand side, you can reduce it down to a single production for the root, with only terminals and operators on the right-hand side.

#### Using regular expressions in practice
In Java, you can use regexes for manipulating strings (see String.split, String.matches, java.util.regex.Pattern). They’re built-in as a first-class feature of modern scripting languages like Python, Ruby, and JavaScript, and you can use them in many text editors for find and replace. 