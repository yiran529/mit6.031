## Difference between Python and Jave
#### Programs and Their Execution

Python
Programs consist of modules.  Modules in turn can contain statements, function definitions, and/or class definitions.  Each module is associated with a source file (.py extension) and possibly a byte code file (.pyc extension).
An comipler/interpreter called a Python virtual machine (PVM) translates Python source files to byte code before execution.  The PVM may save the corresponding byte code in files for subsequent executions.

Java
Programs consist of interfaces and classes.  Interfaces and classes are contained in source files (.java extension).  A source file compiles into one or more executable byte code files (.class extension).
Classes and interfaces can be part of a package.  A package is a bit like a module, from which any resources can be imported.  The byte code files for a package are usually contained in a directory whose name is the package name. 
Programs must first be compiled before they can be executed.  Programs are either applets or applications.  Applets are programs that run in a Web browser.  Applications are programs that run on a standalone computer.  In either case, Java programs execute in an interpreter called a Java virtual machine (JVM).


#### basic framework
Python


The simplest version of this program consists of a single statement:

print("Hello world!")


The PVM simply executes this statement.

 
Another version embeds this statement in a main function which is called at the end of the module:

 
def main():

    print("Hello world!")

 

main()

 

The PVM executes the function definition and then calls the function.

 

 Java:

The simplest version of this program defines a main method within a class:

 

 

public class HelloWorld{

 

    static public void main(String[] args){

        System.out.println("Hello world!");

    }

}

 

The source code defines but does not call this method.  At program startup, the compiled class HelloWorld is loaded into the JVM.  The JVM then calls main, which is a class method in HelloWorld.

 

A Java application must include at least one class that defines a main method.  The byte code file for this class is the entry point for program execution.



#### categories of data types
Python

All data values, including functions, are objects.  Thus, all data types are reference types.



Java

There are two broad categories of data types: primitive types and reference types. 

 

The primitive types include the numeric types (double, int, char) and boolean.  The values of primitive types are immutable and are not instances of classes.

 

Reference types are classes.  Thus, any object or instance of a class is of a reference type.  These include strings, arrays, lists, maps, and so forth.


#### String and Char

Python

String literals are formed using either the single quotes or the double quotes as delimiters.

 

Strings are instances of the str class.  Strings are immutable objects.

 

Character literals are simply strings that contain a single character.

 

An escape sequence is formed using the '\' character followed by an appropriate letter such as 'n' or 't'.

 


Java
String literals are formed using the double quotes as delimiters. 

 

 

Strings are instances of the String class.  Strings are immutable objects.

 

Character literals are formed using the single quotes as delimiters. 

 

Characters are values of the char data type.  This type uses  2 bytes to represent the Unicode set.

 

An escape sequence, either as a character or as a string, is formed using the '\' character followed by an appropriate letter such as 'n' or 't'. 




#### String Concatention
Python


 

The concatenation operator + joins together two strings to form a third, new string.  Operands of other types must be converted to strings before they can be concatenated.

 

Examples:

 

"35" + " pages long."

str(35) + " pages long."

 

If x and y are any objects, the code
str(x) + str(y)
concatenates their string representations.

 

 
Java

The concatenation operator + joins together two strings to form a third, new string.  If one of the operands is a string, the other operand can be of any type.

 

Examples:

 

"35" + " pages long."

35 + " pages long."

 

Any non-numeric object can also be used in a string concatenation, because all Java objects recognize the toString() method.  This method returns the name of the object’s class by default, but can be overridden to return a more descriptive string.  Thus, if x and y are any objects, the code

 

x.toString() + y.toString()
or
x + y

 

concatenates their string representations.



#### Type Conversion
Python



Numeric types can be converted to other numeric types by using the appropriate type conversion functions.  The function’s name is the name of the destination type.

 

Examples:

 

int(3.14)

 

float(3)

 

The ord and chr functions are used to convert between integers and characters, as follows:


ord('A')

chr(65)

 

The str function converts any Python object to its corresponding string representation.

 
str(45)

str(3.14)

 

 



Java

 
Numeric types can be converted to other numeric types by using the appropriate cast operators.  A cast operator is formed by enclosing the destination type name in parentheses.

 

Examples:

 

(int) 3.14

 

(double) 3

 

(char) 45

 

(int) 'a'

 

When casting from an integer to a character or the other way around, the integer is assumed to be the character’s Unicode value.

 

 

 

The simplest way to convert any value to a string is to concatenate it to an empty string, as follows:

 

"" + 3.14

"" + 45







#### Arithmetic and Math Function
Python


Arithmetic operators include +, -, *, /, %, //, and ** (exponentiation).

 

Two integer operands yield an integer result, usless you use /, which yields a float result.  At least one float operand yields a float result. // yields an integer quotient.

 

max, min, abs, and round are standard functions with the standard effects (round returns a float).

 

The math module includes standard functions for trigonometry, logarithms, square roots, and so forth.

 

Examples:

 

round(3.14)

 

math.sqrt(2)

 

 




Java

Arithmetic operators include +, -, *, /, and %.

 

Two integer operands yield an integer result.  At least one float operand yields a float result.

 

The Math class includes class methods such as round, max, min, abs, and pow (exponentiation), as well as methods for trigonometry, logarithms, square roots, and so forth.

 


Examples:

 

Math.round(3.14)

 

Math.sqrt(2)

 



#### Comparisons and Comparibles
Python



 

The comparison operators are ==, !=, <, >, <=, and >=.

 

All comparison operators return True or False.

 

Only objects that are comparable, such as numbers and sequences of comparable objects (strings and lists of numbers or strings), can be operands for comparisons.

 

 

 

Example:

 

print("AAB" > "AAA")

 

 

 

 

 

 
Java
The comparison operators are ==, !=, <, >, <=, and >=.

 

All comparison operators return True or False.

 

All values of primitive types are comparable.

 

Values of reference types are comparable if and only if they recognize the compareTo method. compareTo returns 0 if the two objects are equal (using the equals method), a negative integer if the receiver object is less than the parameter object, and a positive integer if the receiver object is greater than the parameter object.  Classes of comparable objects generally implement the Comparable interface, which includes the compareTo method.

 

Example:

 

String s = "AAB";

System.out.println(s.compareTo("AAA") > 0);






#### Import Statements
Python

Java

 

A module is imported using the form

 

import <module name>

 

The resources of that module are then referenced using the form

 

<module name>.<resource name>

 

Example:

 

import math

 

print(math.sqrt(2), math.pi)

 

 

Alternatively, an individual resource can be imported using the form

 

from <module name> import <resource name>

 

The resource is then referenced without the module name as a qualifier.

 

Examples:

 

from math import sqrt, pi

 

print(sqrt(2), pi)

 

from random import *

 

# Reference all resources from random
Python
A module is imported using the form

 

import <module name>

 

The resources of that module are then referenced using the form

 

<module name>.<resource name>

 

Example:

 

import math

 

print(math.sqrt(2), math.pi)

 

 

Alternatively, an individual resource can be imported using the form

 

from <module name> import <resource name>

 

The resource is then referenced without the module name as a qualifier.

 

Examples:

 

from math import sqrt, pi

 

print(sqrt(2), pi)

 

from random import *

 

i.e.  Reference all resources from random

 
 

 


Java
A package resource is imported using the form

 

import <package name>.<resource name>;

 

The resource is then referenced without the package name as a qualifier.

 

Example:

 

import javax.swing.JButton;

 

JButton b = new JButton("Reset");

 

Alternatively, all of the package’s resources can be imported using the form

 

import <package name>.*;

 

Occasionally, two resources will have the same name in different packages.  To use both resources, do not import them but just reference them using the package names as qualifiers.

 

Example:

 

java.util.List<String> names = new java.util.ArrayList<String>();

 

java.awt.List namesView = new java.awt.List();





#### Terminal Output
Python


 

The print statement automatically converts data to text, displays it, and moves the cursor to the next line:

 

print("Hello world!")

print(34)

 

To prevent the output of a newline, use the optional end keyword argument with the empty string:

 

print("Hello world!", end = "")

 

 

 


Java

The method println, when run with the class variable System.out, converts data to text, displays it, and moves the cursor to the next line:

 

System.out.println("Hello world!");

System.out.println(34);

 

To prevent the output of a newline, use the method print:

 

System.out.print("Hello world!");







#### Terminal Input
Python


 

The input function displays its argument as a prompt and waits for input.  When the user presses the Enter or Return key, the function returns a string representing the input text.  The programmer then either leaves the string alone or converts it to the type of data that it represents (such as an integer).

 

 

Input a line of text as a string:

 

name = input("Enter your name: ")

 

Input an integer:

 

age = int(input("Enter your age: "))

 

 



Java

The Scanner class is used for the input of text and numeric data from the keyboard.  The programmer instantiates a Scanner and uses the appropriate methods for each type of data being input.

 

Create a Scanner object attached to the keyboard:

 

Scanner keyboard = new Scanner(System.in);

 

Input a line of text as a string:

 

String s = keyboard.nextLine("Enter your name: ");

 

Input an integer:

 

int i = keyboard.nextInt("Enter your age: ");

 

Input a double:

 

double d = keyboard.nextDouble("Enter your wage: ");

 

Caution: using the same scanner to input strings after numbers can result in logic errors.  Thus, it is best to use separate scanner objects for numbers and text.




#### Loops
Python


 

There is just one type of for loop, which visits each element in an iterable object, such as a string or list.

 

Form:

 

for <variable> in <iterable>:

    <statement>

    …

    <statement>

 

Example:

 

for s in aListOfStrings:

    print(s)

 

 

The variable picks up the value of each element in the iterable object and is visible in the loop body.

 

The statements in the loop body are marked by indentation.

 

Simple count-controlled loops that iterate through a range of integers have three forms:

 

for <variable> in range(<upper bound>):

    <statement>

    …

    <statement>


for <variable> in range(<lower bound>, <upper bound>):

    <statement>

    …

    <statement>

 

for <variable> in range(<lower bound>, <upper bound>, <step value>):

    <statement>

    …

    <statement>


 





Java

There are two types, a loop for visiting each element in an iterable object, and a loop with the same behavior as a while loop.

 

Form of the first type (also called an enhanced for loop):

 

for (<type> <variable> : <iterable>){

    <statement>

    …

    <statement>

}

 

Example:

 

for (String s : aListOfStrings){

    System.out.println(s);

}

 

The variable picks up the value of each element in the iterable object and is visible in the loop body.

 

The statements in the loop body are marked by curly braces ({}).  When there is just one statement in the loop body, the braces may be omitted.

 

Form of the second type:

 

for (<initializer>; <continuation>; <update>){

    <statement>

    …

    <statement>

}

 

Example:

 

for (int i = 1; i <= 10; i++){

    System.out.println(i);

}

 

This loop has the same behavior as the following while loop:

 

int i = 1;

while (i <= 10){

    System.out.println(i);

    i++;

}








#### Object Instantion
Python



 

The form for object instantiation is

 

<class name>(<arguments>)

 

Example:

 

account = CheckingAccount("Ken",

                          "1111",

                          500.00)

 

 

 


Java 

The form for object instantiation is

 

new <class name>(<arguments>)

 

Example:

 

CheckingAccount account = new CheckingAccount("Ken",

                                              "1111",

                                              500.00)

 

Generic collections require one or more type parameters for the element types as well, of the form

 

new <class name><element types>(<arguments>)

 

Example:

 

List<String> names = new ArrayList<String>();

 

#### Interfaces
Python


 

An interface is not a distinct program component.  However, the set of methods belonging to a data type can be viewed by running either the dir or the help functions with that data type as an argument.

 

Examples:

 

print(dir(int))

 

print(help(list))

 

dir returns a list of the names of the typeճ attributes, including method names.

 

help returns a string consisting of the docstrings of the type and all of its methods.

 






Java

An interface consists of a name and a set method headers.  It specifies the set of methods that an implementing class must include.  The interfaces of the built-in classes can be viewed in the JavaDoc. 

 

A single class can implement several different interfaces.

 

An interface guarantees a common, abstract behavior of all implementing classes.  For example, the java.util.List interface specifies methods for all classes of lists, including java.util.ArrayList and java.util.LinkedList.

 

An interface can extend another, more general interface.  For example, the List interface extends the Collection interface.  This means that all of the methods required by the Collection interface will also be required for all lists.

 

Interfaces are like the interstitial glue that holds program components together.  Whenever possible, use interface names for the types of variables, parameters, and method return types.

 

Uses of interfaces:

 

List<String> aList = new ArrayList<String>();

aList.add("Mary");

aList.add("Bill");

Collections.sort(aList);

 

The ArrayList class implements the List interface.  Note the use of List rather than ArrayList and LinkedList to type the list1 and list2 variables. 

 

The method Collections.sort expects a Collection of Comparables as an argument.  Because the String class implements the Comparable interface and the List interface extends the Collection interface, the compiler does not complain and all goes well. 

 

Finally, note that the LinkedList constructor can accept a Collection as an argument.  This allows a new list to be built from the elements contained in another list, regardless of implementation.

**What the hell is interfaces?** A set of method




#### Collections: String
Python
 

A string is a sequence of 0 or more characters.

 

Strings are instances of the str class.  Strings are immutable objects.

 

The len function returns the number of characters in a string.

 

The subscript operator ([]) accesses a character at a given position.

 

Example:

 

aString = "Hello world!"

 

print(len(aString), aString[2])

 

Strings can be compared using the standard comparison operators ==, <, etc.

 



Java
A string is a sequence of 0 or more characters

 

Strings are instances of the String class.  Strings are immutable objects.

 

The length() method returns the number of characters in a string.

 

The charAt method accesses a character at a given position.

 

Example:

 

String aString = "Hello world!"

 

System.out.println(aString.length());

System.out.println(aString.charAt(2));

 

The String class includes many useful methods for searching, obtaining substrings, and so forth.

 

Strings should be compared using the methods equals and compareTo.






#### Collections: Arrays
Python

 

The array module includes resources for creating and using sequences of basic types such as numbers.  An array is essentially a more efficient version of a list.

 

 


Java
An array is a sequence of elements of the same type.  Each element is accessed in constant time using an index position.  Unlike a list, an array’s length is fixed when it is created and cannot be changed.  Moreover, the only operations on an array are access or replacement of elements via subscripts.

 

Like other data structures, arrays are objects and thus are of a reference type.  The type of an array is determined by its element type, which is specified when the array is instantiated.

 

Examples:

 

int[] ages = new int[10];

String[] names = new String[10];

 

The variables ages and names now refer to arrays capable of holding 10 integers and 10 strings, respectively.  Each array element in ages has a default value of 0.  Each array element in names has a default value of null (as does a new array whose elements are of any reference type).

 

The length of an array can be obtained from the array object’s length variable, as follows:

 

System.out.println(names.length);

 

The elements in the names array can be reset with a for loop, using the subscript and the length variable, as follows:

 

java.util.Scanner input = new java.util.Scanner(System.in);

for (int i = 0; i < names.length; i++)

    names[i] = reader.nextLine("Enter a name: ");

 

The enhanced for loop can be used just to reference the elements in an array:

 

for (String name: names)

    System.out.println(name);

 



 #### Collections: List
 Python

A list is a mutable sequence of 0 or more objects of any type.

 

Lists have a literal representation, of the form

 

[<e0, e1, …, en-1]

 

The len function returns the number of elements in a list.

 

The subscript operator ([]) accesses an element at a given position.

 

Example:

 

aList = [45, 56, 67]

 

print(len(aList), aList[2])

 

Lists of comparable objects can be compared using the standard comparison operators ==, <, etc.

 

The list class includes many useful methods for insertions, removals, searches, and so forth.

 

 


Java
A list is a mutable sequence of 0 or more objects of any type.  A generic list constrains its elements to the same supertype.

 

The List interface includes the methods common to all implementing classes. 

 

The implementing classes include ArrayList and LinkedList.

 

A generic list specifies the element type of the list variable and the instantiated list object, as follows:

 

List<String> names = new ArrayList<String>();

List<Integer> ages = new LinkedList<Integer>();

 

Note first that both list variables are viewed as of type List, although they refer to instances of different list classes.  Both list objects will respond to any method in the List interface.

 

Note second that the first list can contain only strings, whereas the second list can contain only instances of class Integer.

 

Note third that the class Integer is a wrapper class, which allows values of type int to be stored in a list.  When an int is inserted into the second list, the JVM wraps it in an Integer object.  When an Integer object is accessed in this list, the int value contained therein is returned.

 

Example:

 

ages.add(63);

ages.set(0, ages.get(0) + 1);  // Increment age







#### Collections: Sets
Python
 

A set is a mutable collection of 0 or more unique objects of any type.

 

The len function returns the number of elements in a set.

 

The set class includes many useful methods for insertions, removals, searches, and so forth.

 

set1 = set()

for x in range(10):

    set1.add(x)

 

for x in set1: print(x)

 

set2 = set([1,2,3])

 

set3 = set1.intersection(set2)

 

 

 

 

 
Java
A set is a mutable collection of 0 or more unique objects of any type.  A generic set constrains its elements to the same supertype.

 

The Set interface includes the methods common to all implementing classes.  The SortedSet interface extends the Set interface to include methods for sorted sets. 

 

The implementing classes include HashSet and TreeSet.  A TreeSet can return its elements in sorted order.  Thus, it also implements the SortedSet interface.

 

A generic set specifies the element type of the set variable and the instantiated set object, as follows:

 

Set<String> names = new HashSet<String>();

SortedSet<Integer> ints = new TreeSet<Integer>();

 

Example:

 

// Add 10 random ints between 1 and 10

for (int i = 1; i <= 10; i++)

    ints.add((int)(Math.random() * 10 + 1));

 

// Print all elements in sorted order

for (int element : ints)

    System.out.println(element);

 

Note the use of a for loop to access the elements of a set.

 

A set can also be created from a list, as follows:

 

Set<String> names = new HashSet<String>(listOfNames);

 

Java collections typically include a constructor that accepts another collection as an argument and adds its elements to the newly instantiated collection.








#### Collections: Dictionaries and Maps
Python

 

A dictionary is a mutable collection of 0 or more unique key/value pairs.  Put another way, a dictionary contains a set of keys, where each key is associated with a value.

 

The len function returns the number of elements in a dictionary.

 

The subscript operator accesses a value at an existing key.  This operator also can be used to a add a new key or to replace a value at an existing key.

 

The dict class includes many useful methods.

 

 

 

 

 

Example:

 

#Associate 10 random ages between 1 and 10

#with consecutive names

names = {}

for i in range(1, 11):

    name = "Name" + str(i)

    names[name] = random.randint(1, 10)

 

// Print all keys and their values

for key in names.keys():

    print(key, names[key])

 

 

 
Java
A map is a mutable collection of 0 or more unique key/value pairs.  Put another way, a map contains a set of keys, where each key is associated with a value.  A generic map constrains its keys to the same supertype and values to the same supertype.

 

The Map interface includes the methods common to all implementing classes.  The SortedMap interface extends the Map interface to include methods for sorted maps. 

 

The implementing classes include HashMap and TreeMap.  A TreeMap can return its keys in sorted order.  Thus, it also implements the SortedMap interface.

 

A generic map specifies the key/value types of the map variable and the instantiated map object, as follows:

 

Map<String, Integer> names = new HashMap<String, Integer>();

SortedMap<Integer, Integer> ints = new TreeMap<Integer, Integer>();

 

Example:

 

// Associate 10 random ages between 1 and 10

// with consecutive names

for (int i = 1; i <= 10; i++){

    String name = "Name" + i;

    names.put(name, (int)(Math.random() * 10 + 1));

 

// Print all keys and their values

for (String key : names.keySet())

    System.out.println(key + " " + names.get(key));

 

Note the use of a for loop to access the set of keys returned by keySet().






#### Collections: Iterator
Python
An iterator is an object that supports the traversal of a collection.  The PVM automatically uses an iterator whenever it sees a for loop.

 

The iter function expects an iterable collection as an argument and returns an iterator object for that collection.

 

Example:

 

i = iter([1, 2, 3])              

 

An iterator tracks a current position pointer to an element in the collection.  The iterator method next() advances this pointer and returns the most recently visited element.

 

Example:

 

print  i.next(), i.next(), i.next()

 

When next has returned the last element in the collection, any subsequent call of next will raise a StopIteration exception.  When the programmer is finished using an iterator, it should be closed by using the close() method.

 

To traverse all of the elements with an iterator, embed the call of next in a try/except statement, as follows:

 

while True:

    try:

        element = i.next()

        # process element

    except StopIteration:

        i.close()

        break

 

 
Java
An iterator is an object that supports the traversal of a collection.  The compiler generates code that uses an iterator whenever it sees an enhanced for loop.

 

All collections implement the Iterable interface.  This interface includes a single method, iterator(), which returns an iterator object.

 

An iterator object implements the Iterator interface.  This interface includes the methods next(), hastNext(), and remove().

 

Like collections, iterators can be generic.  Thus, the generic collection’s element type must be specified when a variable of type Iterator is declared.

 

Example use:

 

# Create a list of strings

List<String> lyst = new ArrayList<String>();

 

# Add some strings to lyst

 

# Open an iterator on lyst

Iterator<String> i = lyst.iterator();

 

# Print all the strings in lyst using the iterator

while (i.hasNext()){

    String s = i.next();

    System.out.println(s);

}              

 

 
 #### Class Structure
 Python

Java

 

Class definitions have the general form

 

class <name>(<superclass name>):

 

    <class variables>

 

    <class methods>

 

    <instance methods>

 

 

 

 

 

 

The parenthesized superclass name is omitted for basic classes.  Items within the class definition can appear in any order.

 

Example:

 

class Student:

 

    NUM_GRADES = 5

 

    def __init__(self, name):

        self.name = name

        self.grades = []

        for i in range(Student.NUM_GRADES):

            self.grades.append(0)

 

    def getName(self): return self.name

   

    def getGrade(self, i):

        return self.grades[i – 1]

 

    def setGrade(self, i, newGrade):

        self.grades[i - 1] = newGrade

 

    def __str__(self):

        """Format: Name on the first line

        and all grades on the second line,

        separated by spaces.

        """

        result = self.name + '\n'

        result += ''.join(map(str, self.grades))

        return result

 

 

 

 

 

 

 

 

 

 

Usage:

 

s = Student('Mary')

for i in range(1, Student.NUM_GRADES + 1)

    s.setGrade(i, 100)

print s

 

 

Class definitions have the general form

 

<visbility modifier> class <name> extends <superclass name>

                                  implements <list of names>{

 

    <class variables>

 

    <class methods>

 

    <instance variables>

 

    <instance methods>

 

    <inner classes>

}

 

Classes that do not explicitly extend another class extend the Object class by default.  A class may implement zero or more interfaces.

 

Example:

 

public class Student{

 

    public static final int NUM_GRADES = 5;

 

    private String name;

    private int[] grades;

 

    public Student(String name){

        this.name = name;

        this.grades = new int[NUM_GRADES];

    }

 

    public String getName(){

        return this.name;

    }

   

    public int getGrade(int i){

        return this.grades[i – 1];

    }

 

    public void setGrade(int i, int newGrade){

        this.grades[i - 1] = newGrade;

    }

 

    public String toString(){

        /*

        Format: Name on the first line

        and all grades on the second line,

        separated by spaces.

        */

        String result = this.name + '\n';

        for (String grade : this.grades)

            result += grade + ' ';

        return result;

    }

}

 

Usage:

 

Student s = new Student("Mary");

for (int i = 1; i <= Student.NUM_GRADES; i++)

    s.setGrade(i, 100);

System.out.println(s);




#### Visiblity Modifier
Python

Java

 

All items defined within a class (variables or methods) are potentially visible to programmers who use the class.

 

Implementers of a class usually discourage direct access to variables by using the underscore in their names.

 

 

 

There are four levels of access to classes and the items defined within them.  There are three visibility modifiers that specify access: public, private, and protected.

 

Public access allows any program component to reference an item.

 

Example:

 

public static final int NUM_GRADES = 0;

 

Private access allows access to an item only by other items within the enclosing class definition.

 

Example:

 

private int[] grades;

 

Protected access extends access to an item from the defining class to all subclasses.

 

Example:

 

protected String name;

 

When a visibility modifier is omitted, the item has package access.  This means that access is extended from the defining class to all components in the same package.  For most purposes, package access is equivalent to public access.





#### Instances Variables and Constructors
Python

Java

 

Instance variables are always prefixed with the reserved word self.  They are typically introduced and initialized in a constructor method named __init__.

 

 

 

 

 

 

In the following example, the variables self.name and self.grades are instance variables, whereas the variable NUM_GRADES is a class variable:

 

class Student:

 

    NUM_GRADES = 5

 

    def __init__(self, name):

        self.name = name

        self.grades = []

        for i in range(Student.NUM_GRADES):

            self.grades.append(0)

 

 

 

 

The PVM automatically calls the constructor method when the programmer requests a new instance of the class, as follows:

 

s = Student('Mary')

 

The constructor method always expects at least one argument, self.  When the method is called, the object being instantiated is passed here and thus is bound to self throughout the code.  Other arguments may be given to supply initial values for the object’s data.

 

Instance variables are declared at the same level as methods within a class definition.  They are usually given private access to restrict visibility.  They can receive initial values either when they are declared or in a constructor. 

 

Instances variable references may or may not be prefixed with the reserved word this.

 

In the following example, the variables this.name and this.grades are instance variables, whereas the variable NUM_GRADES is a class variable:

 

public class Student{

 

    public static final int NUM_GRADES = 5;

 

    private String name;

    private int[] grades;

 

    public Student(String name){

        this.name = name;

        this.grades = new int[NUM_GRADES];

    }

}

 

The JVM automatically calls the constructor when the programmer requests a new instance of the class, as follows:

 

Student s = new Student("Mary");

 

The constructor may receive one or more arguments to supply initial values for the object’s data.

 


#### Reloading

#### Class(static) Variables and Methods
Python

Java

 

Class variables name data that are shared by all instances of a class.

 

A class variable is introduced by a simple assignment statement within a class. 

 

Class variable references are always prefixed with the name of the class.

 

Class variables are spelled in uppercase by convention.

 

Example:

 

class Student:

 

    NUM_GRADES = 5

 

    def __init__(self, name):

        self.name = name

        self.grades = []

        for i in range(Student.NUM_GRADES):

            self.grades.append(0)

 

 

 

 

Other usage:

 

print(Student.NUM_GRADES)

 

Class methods are methods that know nothing about instances of a class, but can access class variables and call other class methods for various purposes.  For example, a method to convert a numeric grade to a letter grade might be defined as a class method in the Student class.

 

Class method calls are always prefixed with the name of the class.

 

Example:

 

class Student:

 

    # Instance method definitions

 

       @classmethod

    def getLetterGrade(cls, grade):

        if grade > 89:

            return 'A'

        elif grade > 79:

            return 'B'

        else:

            return 'F'

 

Usage:

 

s = Student()

for i in range(1, Student.NUM_GRADES + 1):

    print(Student.getLetterGrade(s.getGrade(i)))

 

Class variables name data that are shared by all instances of a class.

 

A class variable declaration is qualified by the reserved word static. 

 

Outside of the class definition, class variable references are prefixed with the name of the class.

 

Class variables are spelled in uppercase by convention.

 

 

 

Example:

 

public class Student{

 

    public static final int NUM_GRADES = 5;

 

    private String name;

    private int[] grades;

 

    public Student(String name){

        this.name = name;

        this.grades = new int[NUM_GRADES];

    }

}

 

Other usage:

 

System.out.println(Student.NUM_GRADES);

 

Class methods are methods that know nothing about instances of a class, but can access class variables and call other class methods for various purposes.  For example, a method to convert a numeric grade to a letter grade might be defined as a class method in the Student class.

 

A class method header is qualified by the reserved word static.

 

Outside of the class definition, class method calls are prefixed with the name of the class.

 

Example:

 

public class Student{

 

    // Instance method definitions and variable declarations

 

    public static char getLetterGrade(int grade){

        if (grade > 89)

            return 'A';

        else if (grade > 79)

            return 'B';

        else

            return 'F';

 

 

Usage:

 

s = new Student();

for (int i = 1; i <= Student.NUM_GRADES; i++)

    System.out.println(Student.getLetterGrade(s.getGrade(i));

#### Sybolic Constants
Python

Java

 

All variables can be reset at any time.  There are no symbolic constants.  Symbols such as True, False, and None are reserved words.

 

 

Final variables serve as symbolic constants.  A final variable declaration is qualified with the reserved word final.  The variable is set to a value in the declaration and cannot be reset.  Any such attempt is caught at compile time.




#### Define a String Representation
Python

Java

 

The str function converts any object to its string representation. 

 

Example:

 

str(3.14)      # returns '3.14'

 

This function can be customized to return the appropriate string for objects of any programmer-defined class by including an __str__ method.

 

When the __str__ method is available, operations such as print automatically use it to obtain an object’s string representation.

 

Example:

 

class Student:

 

    NUM_GRADES = 5

 

    def __init__(self, name):

        self.name = name

        self.grades = []

        for i in range(Student.NUM_GRADES):

            self.grades.append(0)

 

    def __str__(self):

        """Format: Name on the first line

        and all grades on the second line,

        separated by spaces.

        """

        result = self.name + '\n'

        result += ''.join(map(str, self.grades))

        return result

 

 

 

 

 

 

 

Usage:

 

s1 = Student('Mary')

print(s1)

s2 = Student('Bill')

print(str(s1) + '\n' + str(s2))

 

 

The toString() method returns the string representation of an object.  A default implementation of this method is included in the Object class.  This implementation returns a string containing the name of the object’s class and its hash code.  Thus, if the programmer does not include toString in a given class, the default is used via inheritance (Object is the ancestor class of all objects).

 

Operations such as println and + automatically call toString with objects to obtain their string representations.

 

The header of toString is

 

public String toString()

 

Example:

 

public class Student{

 

    public static final int NUM_GRADES = 5;

 

    private String name;

    private int[] grades;

 

    public Student(String name){

        this.name = name;

        this.grades = new int[NUM_GRADES];

    }

 

    public String toString(){

        /*

        Format: Name on the first line

        and all grades on the second line,

        separated by spaces.

        */

        String result = this.name + '\n';

        for (String grade : this.grades)

            result += grade + ' ';

        return result;

    }

}

 

Usage:

 

s1 = new Student("Mary");

System.out.println(s1);

s2 = new Student("Bill");

System.out.println(s1 + '\n' + s2);







#### Defining Equality
Python

Java

 

The == operator compares two objects for equality.  This operator uses the is operator by default.  The is operator tests two object references for equality: do they refer to the exact same object in memory?  Often, this test is too restrictive.  A more relaxed version would compare one or more of the objects’ attributes for equality.  For example, two Student objects could have the same name and be considered equal, even though they are also distinct objects.  This type of equality is called structural equivalence, as opposed to the more restrictive type of object identity.

 

The programmer can override the default definition of == by including a definition of the method __eq__ in a given class.  There are actually three tests to perform.  The receiver object (self) and the second parameter object (other) are first compared for identity using the is operator.  If this test fails, the types of the two objects are then compared.  If this test succeeds, their relevant attributes are compared using the == operator. 

 

The behavior of != can also be modified by overriding the method __ne__.

 

 

 

 

 

 

Example:

 

class Student:

 

    NUM_GRADES = 5

 

    def __init__(self, name):

        self.name = name

        self.grades = []

        for i in range(Student.NUM_GRADES):

            self.grades.append(0)

 

    def __eq__(self, other):

        """

        Compare the names for equality if the

        objects are not identical.       

        """

        if self is other:

            return True

        elif type(self) != type(other):

            return False

        else:

            return self.name == other.name

 

    def __ne__(self, other):

        """

        Compare the names for inequality.

        """

        return not self == other

 

Usage:

 

s1 = Student('Mary')

s2 = Student('Bill')

s3 = Student('Bill')

print(s1 == s2)           # displays False

print(s2 == s3)           # displays True

print(s2 is s3)           # displays False

print(s2 != s3)           # displays False

 

 

The equals() method compares two objects for equality.  This method uses the == operator by default.  The == operator tests two object references for equality: do they refer to the exact same object in memory?  Often, this test is too restrictive.  A more relaxed version would compare one or more of the objects’ attributes for equality.  For example, two Student objects could have the same name and be considered equal, even though they are also distinct objects.  This type of equality is called structural equivalence, as opposed to the more restrictive type of object identity.

 

The programmer can override the default definition of equals by including a definition of this method in a given class.  The attributes being compared are those of the receiver object (this) and the parameter object (other). 

 

The method header for equals is

 

public boolean equals(Object other)

 

Note that the parameter object’s type is Object.  This allows any object to be compared with the receiver object for equality.  Consequently, there are actually three tests to perform in equals.  The first one compares the two objects for identity using ==.  The second one uses the instanceof operator to determine if the type of the parameter object is the same as that of the receiver object.  The third test compares the relevant attributes of the two objects using the equals method with them.

 

Before accessing the parameter’s attributes, the type of the parameter must be cast down to the receiver object’s type using a cast operator.

 

Example:

 

public class Student{

 

    public static final int NUM_GRADES = 5;

 

    private String name;

    private int[] grades;

 

    public Student(String name){

        this.name = name;

        this.grades = new int[NUM_GRADES];

    }

 

    public boolean equals(Object other){

        /*

        Compare the names for equality if the

        objects are not identical.       

        */

        if (this == other)

            return true;

        else if !(other instanceof Student)

            return false;

        else{

            Student otherStudent = (Student)other;

            return this.name.equals(otherStudent.name);

        }

    }

}

 

Usage:

 

Student s1 = new Student("Mary");

Student s2 = new Student("Bill");

Student s3 = new Student("Bill");

System.out.println(s1.equals(s2));        // displays false

System.out.println(s2.equals(s3));        // displays true

System.out.println(s1 == s3);             // displays false

System.out.println(! s2.equals(s3));      // displays false

 **注意：重载equals后，==还是表示原来的等于的意思。这与Python一样。**






#### Defining Interfaces

Python



No interfaces, only classes!

 

 

 




Java

 
The form of an interface is

 

public interface <name> extends <name>{

 

    <final variables>

 

    <method headers>

}

 

The extension of another interface is optional.  A method header is terminated with a semicolon.

 

In the next example, the interface TrueStack is defined for all implementations that restrict operations to the standard ones on stacks.  Because TrueStack extends Iterable, each implementation must also include an iterator method, and each stack can be traversed with an enhanced for loop.

 

The code for this interface would be placed in its own source file, named TrueStack.java.  This file can be compiled before any implementing classes are defined.

 

Example interface:

 

public interface TrueStack<E> extends Iterable<E>{

 

    public boolean isEmpty();

  

    public E peek();

 

    public E pop();

 

    public void push(E element);

 

    public int size();

}

 

Each implementing class would in turn be placed in its own file.  An interface must be successfully compiled before any of its implementing classes.

 

Example implementation, based on an ArrayList:

 

import java.util.*;

 

public class ArrayStack<E> implements TrueStack<E>{

 

    private List<E> list;

 

    public ArrayStack(){

        list = new ArrayList<E>();

    }

 

    public boolean empty(){

        return list.isEmpty();

    }

  

    public E peek(){

        return list.get(list.size() - 1);

    }

 

    public E pop(){

        return list.remove(list.size() - 1);

    }

 

    public void push(E element){

        list.add(element);

    }

 

    public int size(){

        return list.size();

    }

 

    public Iterator<E> iterator(){

        return null;                 # Deferred to another

    }                                # topic

}

 


 #### Inner Classes
 Python

Java

 

A class might be defined just for use in another class definition.  For example, a LinkedStack class might use a OneWayNode class.  Ideally, the definition of OneWayNode would be nested within LinkedStack, but that is not allowed.  For the sake of comparison, here is an implementation of these two classes:

 

class OneWayNode:

 

    def __init__(self, data, next):

        self.data = data

        self.next = next

 

class LinkedStack:

 

    def __init__(self):

        self.items = None

        self.size = 0

 

    def push(self, element):

        self.items = OneWayNode(element, self.items)

        self.size += 1

 

    def pop(self):

        element = self.items.data

        self.items = self.items.next

        self.size -= 1

        return element

 

    def peek(self):

        return self.items.data

 

    def isEmpty(self):

        return len(self) == 0

 

    def __len__(self):

        return self.size

 

 

 

A class might be defined just for use in another class definition.  For example, a LinkedStack class might use a OneWayNode class.  The definition of OneWayNode can be nested within LinkedStack.  OneWayNode is specified as a private class, sometimes also called an inner class.  Here is an implementation: 

 

public class LinkedStack<E> implements TrueStack<E>{

 

    private OneWayNode<E> items;

    private int size;

 

    public LinkedStack(){

        this.items = null;

        this.size = 0;

    }

 

    public void push(E element){

        this.items = new OneWayNode<E>(element, items);

        this.size += 1;

    }

   

    public E pop(){

        E element = this.items.data;

        this.items = this.items.next;

        this.size -= 1;

        return element;

    }

 

    public E peek(){

        return this.items.data;

    }

 

    public boolean isEmpty(){

        return this.size() == 0;

    }

 

    public int size(){

        return this.size;

    }

 

    public Iterator<E> iterator(){

        return null;                 # Deferred to another

    }                                # topic

 

    private class OneWayNode<E>{

 

        private E data;

        private OneWayNode next;

 

        private OneWayNode(E data, OneWayNode next){

            this.data = data;

            this.next = next;

        }

    }

}

 

#### Iterator
Python

Java

 

By convention, the iter() method returns an iterator on an iterable object.  The user of an iterator can expect the method next() to return the next object in an iteration, until next raises a StopIteration exception.  At that point, one closes the iterator using the close() method.

 

Let us assume that the LinkedStack class now includes an iter method.  Then one can visit the objects in a stack, from top to bottom, in either of the following ways:

 

stack = LinkedStack()

i = stack.iter()

while True:

    try:

        element = i.next()

        # process element

    except StopIteration:

        i.close()

        break

 

for element in stack:

    # process element

 

The iter method actually builds and returns a generator object.  The code for this object executes in a separate process running concurrently with the process that uses the iterator.  A generator object can maintain state, such as a current position pointer to the elements in the collection.  This reference in the current example is initially to the first node in the stack’s linked list.  The generator’s code also executes a while True loop.  If the current position equals None, then the last node has been passed and the generator raises a StopIteration exception.  Otherwise, the generator yields the element at the current node.  The yield statement pauses the execution of the process executing the generator’s code until the method next() is called.  This method returns the element just yielded.  When control returns to the generator object, the current pointer is set to the next field of the current node.  The generator’s process runs forever, unless the user calls its close() method.

 

Example: 

 

class OneWayNode:

 

    def __init__(self, data, next):

        self.data = data

        self.next = next

 

class LinkedStack:

 

    def __init__(self):

        self.items = None

        self.size = 0

 

    def push(self, element):

        self.items = OneWayNode(element, self.items)

        self.size += 1

 

    def pop(self):

        element = self.items.data

        self.items = self.items.next

        self.size -= 1

        return element

 

    def peek(self):

        return self.items.data

 

    def isEmpty(self):

        return len(self) == 0

 

    def __len__(self):

        return self.size

 

    def __iter__(self):

        curPos = self.items

        while True:

            if curPos is None:

                raise StopIteration

            yield curPos.data

            curPos = curPos.next

 

 

 

 

The iterator() method returns an iterator on an iterable object.  The user of an iterator can expect the method next() to return the next object in an iteration, while the method hasNext() returns True. 

 

Let us assume that the LinkedStack class now includes an iterator method.  Then one can visit the objects in a stack, from top to bottom, in either of the following ways:

 

TrueStack<String> stack = new LinedStack<String>();

i = stack.iterator();

while (i.hasNext()){

    String element = i.next()

    // process element

}

 

for (String element : stack)

    # process element

 

The implementing class defines an iterator() method that returns an instance of an inner class.  This class implements the Iterator interface.  Its methods next() and hasNext() track a current position pointer to the elements in the collection.

 

Note that several iterators may be open concurrently on the same collection.  To maintain the consistency of each iterator with the collection’s data, collection-based modifications (push, pop) are not allowed during the operation of any iterator.  The collection now maintains a count of its modifications.  When an iterator is instantiated, it sets its own count of modifications to the collection’s count.  On each call of the next() method, the iterator compares the two counts.  If they are not the same, a collection-based modification has occurred and an exception is thrown.

 

Example:

 

import java.util.iterator;

 

public class LinkedStack<E> implements TrueStack<E>{

 

    private OneWayNode<E> items;

    private int size;

    private int modCount;

 

    public LinkedStack(){

        this.items = null;

        this.size = 0;

        this.modCount = 0;

    }

 

    public void push(E element){

        this.items = new OneWayNode<E>(element, items);

        this.size += 1;

        this.modCount += 1;

    }

   

    public E pop(){

        E element = this.items.data;

        this.items = this.items.next;

        this.size -= 1;

        this.modCount += 1;

        return element;

    }

 

    public E peek(){

        return this.items.data;

    }

 

    public boolean isEmpty(){

        return this.size() == 0;

    }

 

    public int size(){

        return this.size;

    }

 

    public Iterator<E> iterator(){

        return new StackIterator<E>();                

    }

 

    private class StackIterator<E> implements Iterator<E>{

 

        private OneWayNode curPos;

        private int curModCount;

 

        private StackIterator(){

            this.curPos = items;

            this.curModCount = modCount;

        }

 

        public boolean hasNext(){

            return curPos != null;

        }

 

        public E next(){

            if (! this.hasNext()

                throw new IllegalStateException();

            if (this.curModCount != modCount)

                throw new ConcurrentModificationException()

            E data = this.curPos.data;

            this.curPos = this.curPos.next();

            return data;

        }

 

        public void remove(){

            throw new UnsupportedOperationException();

        }

    }                                

 

    private class OneWayNode<E>{

 

        private E data;

        private OneWayNode next;

 

        private OneWayNode(E data, OneWayNode next){

            this.data = data;

            This.next = next;

        }

    }

}







#### Inheritance and Defining Subclass
Python

Java

 

In an object-oriented programming language, one can define a new class that reuses code in another class.  The new class becomes a sublclass of the existing class, which is also called its parent class.  The subclass inherits all of the attributes, including methods and variables, from its parent class and from any ancestor classes in the hierarchy.

 

Subclassing is a convenient way to provide extra or more specialized functionality to an existing resource.  For example, a queue guarantees access to its elements in first-in, first-out order.  A priority queue behaves in most respects just like a queue, except that the higher priority elements are removed first.  If elements have equal priority, they are removed in standard FIFO order.  A priority queue is thus a subclass of queue, with a specialized method for insertions that guarantees the appropriate ordering of elements.  The priority queue obtains all of its other methods from the queue class for free.

 

 

Here is the definition of a LinkedQueue class for ordinary queues: 

 

 

class OneWayNode:

 

    def __init__(self, data, next):

        self.data = data

        self.next = next

 

class LinkedQueue:

 

    def __init__(self):

        self.front = self.rear = None

        self.size = 0

 

    def enqueue(self, element):

        n = OneWayNode(element, None)

        if self.isEmpty():

            self.rear = self.front = n

        else:

            self.rear.next = n

            self.rear = n

        self.size += 1

 

    def dequeue(self):

        element = self.front.data

        self.front = self.front.next

        self.size -= 1

        if self.isEmpty():

            self.rear = None

        return element

 

    def peek(self):

        return self.front.data

 

    def isEmpty(self):

        return len(self) == 0

 

    def __len__(self):

        return self.size

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

The form for defining a subclass is

 

class <subclass name>(<parent class name>):

 

    <variables and methods>

 

The LinkedPriorityQueue class is a subclass of LinkedQueue.  The new class includes just two methods, __init__ and enqueue.  The __init__ method in LinkedPriorityQueue simply calls the same method in its parent class, using the form

 

<parent class name>.__init__(self)

 

This causes the parent class to initialize its instance variables.

 

The new enqueue method first checks for an empty queue and if that is true, calls the enqueue method in the parent class using the form

 

<parent class name>.<method name>(<args>)

 

Otherwise, the method searches for the appropriate position of the new element and inserts it there.  Because the front of the queue is at the head of the linked structure, the search stops when the incoming element is strictly less than element in the queue.  Thus, an incoming element is placed behind all elements of equal priority.

 

 

 

 

 

Here is the definition of LinkedPriorityQueue:  

 

class LinkedPriorityQueue(LinkedQueue):

 

    def __init__(self):

        LinkedQueue.__init__(self)

 

    def enqueue(self, element):

        if self.isEmpty():

            LinkedQueue.enqueue(self, element)

        else:

            probe = self.front

            while probe != None and element >= probe.data:

                trailer = probe

                probe = probe.next

            if probe == None:            # At rear of queue

                self.rear.next = OneWayNode(element, None)

                self.rear = self.rear.next

            elif probe == self.front:    # At front of queue

                self.front = Node(element, self.front.next)

            else:                        # Betwixt two nodes

                trailer.next = Node(element, probe)

            self.size += 1

 

 

In an object-oriented programming language, one can define a new class that reuses code in another class.  The new class becomes a sublclass of the existing class, which is also called its parent class.  The subclass inherits all of the attributes, including methods and variables, provided they are specified as public or protected, from its parent class and from any ancestor classes in the hierarchy.

 

Subclassing is a convenient way to provide extra or more specialized functionality to an existing resource.  For example, a queue guarantees access to its elements in first-in, first-out order.  A priority queue behaves in most respects just like a queue, except that the higher priority elements are removed first.  If elements have equal priority, they are removed in standard FIFO order.  A priority queue is thus a subclass of queue, with a specialized method for insertions that guarantees the appropriate ordering of elements.  The priority queue obtains all of its other methods from the queue class for free.

 

The LinkedQueue class makes its instance variables visible to subclasses but not to others by declaring them as protected.  Likewise, the inner OneWayNode class and its attributes are also defined as protected.

 

Here is the definition of a LinkedQueue class for ordinary queues:

 

import java.util.*;

 

public class LinkedQueue<E> implements TrueQueue<E>{

 

    protected OneWayNode<E> front, rear;

    Protected int size;

 

    public LinkedQueue(){

      this.front = this.rear = null;

      this.size = 0;

    }

 

    public void enqueue(E element){

        OneWayNode<E> n = OneWayNode<E>(element, null);

        if (this.isEmpty())

            this.rear = this.front = n;

        else{

            this.rear.next = n;

            this.rear = n;

        }

        this.size += 1;

    }

 

    public E dequeue(){

        E element = this.front.data;

        this.front = this.front.next;

        this.size -= 1;

        if (this.isEmpty())

            this.rear = null;

        return element;

    }

 

    public E peek(){;

      return this.front.data;

    }

 

    public boolean isEmpty(){

      return this.size() == 0;

    }

 

    public int size(){

      return this.size == 0;

    }

 

    protected class OneWayNode<E>{

 

        protected E data;

        protected OneWayNode<E> next;

 

        protected OneWayNode(E data, OneWayNode next){

            this.data = data;

            this.next = next;

        }

    }

}

 

 

The form for defining a subclass is

 

public class <subclass name> extends <parent class name>{

 

    <variables and methods>

}

 

The LinkedPriorityQueue class is a subclass of LinkedQueue.  The new class includes a constructor and the enqueue method.  The constructor in LinkedPriorityQueue is optional, but explicitly calls the constructor in its parent class, using the statement

 

super();

 

This causes the parent class to initialize its instance variables.

 

The new enqueue method first checks for an empty queue and if that is true, calls the enqueue method in the parent class using the form

 

super.<method name>(<args>)

 

Otherwise, the method searches for the appropriate position of the new element and inserts it there.  Because the front of the queue is at the head of the linked structure, the search stops only when the incoming element is strictly less than element in the queue.  Thus, an incoming element is placed behind all elements of equal priority. 

 

Elements in a priority queue must be comparable.  Therefore, the element type of the LinkedPriorityQueue class is specified as Comparable.  The syntax for relating the element types of LinkedPriorityQueue and LinkedQueue in the class header is a bit tricky, but gets the job done.

 

Here is the definition of LinkedPriorityQueue:  

 

import java.util.*;

 

public class LinkedPriorityQueue<E extends Comparable>

                                extends LinkedQueue<E>{

 

    public LinkedPriorityQueue(){

       super();

    }

 

    public void enqueue(E element){

       if (this.isEmpty())

           super.enqueue(element);

        else{

            OneWayNode<E> probe = this.front;

            OneWayNode<E> trailer = null;

            while (true){

                if (probe == null ||

                    element.compareTo(probe.data) < 0)

                    break;

                trailer = probe;

                probe = probe.next;

            }

            if (probe == null)                   # At rear of queue

                this.rear.next = new OneWayNode<E>(element, null);

                this.rear = this.rear.next;

            else if (probe == this.front)        # At front of queue

                this.front = new OneWayNode<E>(element, this.front.next);

            else                                 # Betwixt two nodes

                trailer.next = new OneWayNode<E>(element, probe);

            this.size += 1;

        }

    }

}

 



#### Abstract classes and methods
Python

Java

 

When two or more classes contain redundant code, it can be factored into a common parent class.  This class is typically called an abstract class, because it is not instantiated.

 

Consider two implementations of stacks, ArrayStack and LinkedStack.  Their internal containers of elements are different, as are their methods for adding, removing, and iterating through these structures.  However, if we assume that each stack tracks its size in the same manner, the methods len and isEmpty will be the same for any implementation.  And if we assume that each implementation includes an iter method, then we can implement several other methods, such as str and addAll, just once for all stacks. 

 

The next example defines an AbstractStack class that includes the information common to all stack implementations.  To access these resources, a stack implementation, such as ArrayStack or LinkedStack, simply extends AbstractStack.   

 

class AbstractStack:

 

    def __init__(self):

        self.size = 0

 

    def isEmpty(self):

        return len(self) == 0

 

    def __len__(self):

        return self.size

   

    def __str__(self):

        result = ''

        for element in self:

            result += str(element) + '\n'

        return result

 

    def addAll(self, otherIterable):

        for element in otherIterable:

            self.push(element)              

 

AbstractStack is responsible for initializing just one instance variable, size.  Note that the str method uses a for loop over self, which implies that the stack object is iterable.  Consequently, each stack implementation must include its own definition of the iter method.  Finally, note that the addAll method copies elements from another interable object to the stack, using the push method.  Thus, the push method also must be included in each stack implementation.

 

The first implementation, ArrayStack, includes an init method that allows the programmer to specify an optional iterable object as an argument.  If this argument is not None, it is passed to the addAll method to copy its elements to the new stack.  addAll can also be called to add several elements at any subsequent point in time.  ArrayStack is also responsible for maintaining the sequence of elements.  Thus, it includes the definitions of peek, pop, push, and iter.

 

Example:

 

class ArrayStack(AbstractStack):

 

    def __init__(self, otherIterable = None):

        AbstractStack.__init__(self)

        self.items = []

        if otherIterable != None:

            self.addAll(otherIterable)

 

    def push(self, element):

        self.items.append(element)

        self.size += 1

 

    def pop(self):

        self.size -= 1

        return self.items.pop()

 

    def peek(self):

        return self.items[-1]

 

    def __iter__(self):

        probe = len(self.items) - 1

        while True:

            if probe < 0:

                raise StopIteration

            yield self.items[probe]

            probe -= 1

 

The same methods are defined in LinkedStack, but of course they access elements in a very different type of internal structure.

 

class OneWayNode:

 

    def __init__(self, data, next):

        self.data = data

        self.next = next

 

class LinkedStack(AbstractStack):

 

    def __init__(self, otherIterable = None):

        AbstractStack.__init__(self)

        self.items = None

        if otherIterable != None:

            self.addAll(otherIterable)

 

    def push(self, element):

        self.items = OneWayNode(element, self.items)

        self.size += 1

 

    def pop(self):

        element = self.items.data

        self.items = self.items.next

        self.size -= 1

        return element

 

    def peek(self):

        return self.items.data

 

    def __iter__(self):

        probe = self.items

        while True:

            if probe is None:

                raise StopIteration

            yield probe.data

            probe = probe.next

 

 

 

 

When two or more classes contain redundant code, it can be factored into a common parent class.  This class is typically called an abstract class, because it is not instantiated.

 

Consider two implementations of stacks, ArrayStack and LinkedStack.  Their internal containers of elements are different, as are their methods for adding, removing, and iterating through these structures.  However, if we assume that each stack tracks its size in the same manner, the methods len and isEmpty will be the same for any implementation.  And if we assume that each implementation includes an iter method, then we can implement several other methods, such as str and addAll, just once for all stacks. 

 

The next example defines an AbstractStack class that includes the information common to all stack implementations. To access these resources, a stack implementation, such as ArrayStack or LinkedStack, simply extends AbstractStack.  This class is specified as abstract with the reserved word abstract.  The class must also include all of the methods in the TrueStack interface.  The methods that must be defined in the subclasses  are specified as abstract methods, again with the reserved word abstract.

 

import java.util.*;

 

abstract public AbstractStack<E> implements TrueStack<E>{

 

    protected int size, modCount;

 

    public AbstractStack(){

        this.size = 0;

        this.modCount = 0;

    }

 

    public boolean empty(){

        return this.size() == 0;

    }

 

    public int size(){

        return size;

    }

 

    public String toString(){

        String result = '';

        for (E element : this)

            result += str(element) + '\n';

        return result;

    }

 

    public void addAll(Collection<E> col){

        for (E element : col)

            this.push(element);

    }

 

    abstract public E peek();

 

    abstract public E pop();

 

    abstract public void push(E element){

 

    abstract public Iterator<E> iterator();

}

 

AbstractStack is responsible for initializing two instance variables, size and modCount.  Note that the toString method uses a for loop over this, which implies that the stack object is iterable.  Consequently, each stack implementation must include its own definition of the iterator method.  Finally, note that the addAll method copies elements from any Collection object to the stack.  Any Collection object recognizes the iterator method, and thus can be used with a for loop.

 

The first implementation, ArrayStack, includes two constructors.  One allows the programmer to specify a Collection object as an argument.  This object is passed to the addAll method to copy its elements to the new stack.  addAll can also be called to add several elements at any subsequent point in time.  ArrayStack is also responsible for maintaining the sequence of elements.  Thus, it includes the definitions of peek, pop, push, and iterator.

 

import java.util.*;

 

public class ArrayStack<E> extends AbstractStack<E>{

 

    private List<E> list;

 

    public ArrayStack(){

        super();

        this.list = new ArrayList<E>();

    }

 

    public ArrayStack(Collection<E> col){

        this();

        this.addAll(col);

    }

 

    public E peek(){

        if (this.isEmpty())

            throw new EmptyStackException();

        return list.get(this.size() - 1);

    }

 

    public E pop(){

        if (this.isEmpty())

            throw new EmptyStackException();

        this.size -= 1;

        this.modCount += 1;

        return this.list.remove(this.size());

    }

 

    public void push(E element){

        this.list.add(element);

        this.size += 1;

        this.modCount += 1;

    }

 

    public Iterator<E> iterator(){

        return new StackIterator<E>();                

    }

 

    private class StackIterator<E> implements Iterator<E>{

 

        private int curPos;

        private int curModCount;

 

        private StackIterator(){

            this.curPos = this.size() - 1;

            this.curModCount = modCount;

        }

 

        public boolean hasNext(){

            return curPos >= 0;

        }

 

        public E next(){

            if (! this.hasNext()

                throw new IllegalStateException();

            if (this.curModCount != modCount)

                throw new ConcurrentModificationException()

            E data = list.get(this.curPos);

            this.curPos -= 1;

            return data;

        }

 

        public void remove(){

            throw new UnsupportedOperationException();

        }

    }                                

}

 

The same methods are defined in LinkedStack, but of course they access elements in a very different type of internal structure.

 

import java.util.iterator;

 

public class LinkedStack<E> extends AbstractStack<E>{

 

    private OneWayNode<E> items;

 

    public LinkedStack(){

        super();

        this.items = null;

    }

 

    public LinkedStack(Collection<E> col){

        this();

        this.addAll(col);

    }

 

    public void push(E element){

        this.items = new OneWayNode<E>(element, items);

        this.size += 1;

        this.modCount += 1;

    }

   

    public E pop(){

        E element = this.items.data;

        this.items = this.items.next;

        this.size -= 1;

        this.modCount += 1;

        return element;

    }

 

    public E peek(){

        return this.items.data;

    }

 

    public Iterator<E> iterator(){

        return new StackIterator<E>();                

    }

 

    private class StackIterator<E> implements Iterator<E>{

 

        private OneWayNode curPos;

        private int curModCount;

 

        private StackIterator(){

            this.curPos = items;

            this.curModCount = modCount;

        }

 

        public boolean hasNext(){

            return curPos != null;

        }

 

        public E next(){

            if (! this.hasNext()

                throw new IllegalStateException();

            if (this.curModCount != modCount)

                throw new ConcurrentModificationException()

            E data = this.curPos.data;

            this.curPos = this.curPos.next();

            return data;

        }

 

        public void remove(){

            throw new UnsupportedOperationException();

        }

    }                               

 

    private class OneWayNode<E>{

 

        private E data;

        private OneWayNode next;

 

        private OneWayNode(E data, OneWayNode next){

            this.data = data;

            This.next = next;

        }

    }

}



#### == vs. equals()