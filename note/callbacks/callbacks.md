## callbacks
In this reading we talk about callbacks, in which an implementer **calls a function provided by the client**.We discuss this idea in two contexts, graphical user interfaces and web servers.
But callbacks are an example of a bigger idea, first-class functions, or treating functions just like data: passing them as parameters, returning them as results, and storing them in variables and data structures.
#### Input handling in a graphical user interface(GUI)
e.g.
```java
JButton playButton = new JButton("Play");

playButton.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent event) {
        playSound();
    } 
});
```
This code creates an instance of an anonymous class implementing the ActionListener interface, which has exactly one method, actionPerformed. When this ActionListener instance is given to the button with addActionListener(), the button promises to call its actionPerformed method every time the user presses the button.

GUI event handling is an instance of the **Listener pattern**, also known as Publish-Subscribe. In the Listener pattern:
* An event source generates (or publishes) a stream of discrete events, which correspond to state transitions in the source.
* One or more listeners register interest (subscribe) to the stream of events, providing a function to be called when a new event occurs.
In this example:

* the JButton is the event source;
its events are button presses;
* the listener is the anonymous ActionListener instance
* the function called when the event happens is actionPerformed
  
#### callbacks
* def: A callback is a function that a client provides to a module for the module to call.
* analogy(类比): 客户端给服务端提供一个电话号码，然后服务端在将来“回拨”给客户端

#### Route handling in a web server 

#### First-class func
* def: Using callbacks requires a programming language in which functions are first-class, which means they can be treated like any other value in the language: passed as parameters, returned as return values, and stored in variables and data structures.
* what is not first-class: access control is not first-class – you can’t pass public or private as a parameter into a function; a while loop or if statement is not first-class. 
* what is first-class: In old programming languages, only data was first-class: built-in types (like numbers) and user-defined types. But in modern programming languages, like Python and JavaScript, both data and functions are first-class. First-class functions are a very powerful programming idea.
* in an OOP language like Java that doesn’t support first-class functions directly, how to implement first-class func: use an object with a method representing the function/ lambda expr.

#### Concurrency in event processing systems
Using callbacks in a system **inevitably forces the programmer to think about concurrency**, because control flow is no longer under their control. A callback might be called at any time, and in some systems, it might be called from a different thread than the client originally came from.
比如GUI库在创建GUI时会自动开启一个新线程；HttpServer会开启新线程监听新的链接。

#### Implementing an event source
All our examples of callbacks so far have been from the client’s point of view. What about the implementer’s side?