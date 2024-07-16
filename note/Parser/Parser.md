## Parser
#### Parser Generator
A parser generator takes a grammar as input and automatically generates a **parser**, which takes a sequence of characters and tries to match the sequence against the grammar.
In 6.031, we are going to use ParserLib, a parser generator for Java developed by the course staff.
#### A ParserLib grammar
#### Whitespace
ParserLib allows a shorthand to indicate that certain kinds of characters should be skipped.
#### Generating the parser
with ParserLib
#### Calling the parser
The parser has a method called parse that takes in the text to be parsed (in the form of either a String, an InputStream, a File or a Reader) and returns a ParseTree.
#### Traversing the parse tree
we need to do something with this parse tree. We’re going to translate it into a value of a recursive abstract data type.
#### Constructing an abstract syntax tree
We need to convert the parse tree into a recursive data type. 
definition:
IntegerExpression = Number(n:int)
                    + Plus(left:IntegerExpression, right:IntegerExpression)
implementation: ...
