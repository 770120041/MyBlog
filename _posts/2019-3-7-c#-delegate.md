---
layout: post
title:  "C# Delegate and Events"
---


# Delegate
## Intro
C# delegates are similar to pointers to functions, in C or C++. A delegate is a reference type variable that holds the **reference to a method**, which pointers hold a reference to an object of primitive type.<br>

 The reference can be changed at runtime.

### Syntax

```public delegate int MyDelegate (string s);```<br>
 **MyDelegate** is the name of the declared delegate<br>


Once a delegate type is declared, a delegate object must be created with the **new** keyword and be associated with a **particular method**.<br>

 When creating a delegate, the argument passed to the new expression is written similar to a method call, but without the arguments to the method. <br>

For example:
```
public delegate void printString(string s);
printString ps1 = new printString(WriteToScreen);
printString ps2 = new printString(WriteToFile);
```
### Multicasting of a Delegate
Delegate objects can be composed using the "+" operator. A composed delegate calls the two delegates it was composed from. Only delegates of the same type can be composed. The "-" operator can be used to remove a component delegate from a composed delegate.


# Events
Events are user actions such as key press, clicks, mouse movements, etc., or some occurrence such as system generated notifications.<br>

Applications need to respond to events when they occur. For example, interrupts. Events are used for inter-process communication.

## Using Delegates with Events
The events are declared and raised in a class and associated with the event handlers using delegates within the same class or some other class. *The class containing the event is used to publish the event. This is called the publisher class*. *Some other class that accepts this event is called the subscriber class. Events use the publisher-subscriber model.*

A publisher is an object that contains the definition of the event and the delegate. The event-delegate association is also defined in this object. A publisher class object invokes the event and it is notified to other objects.

A subscriber is an object that accepts the event and provides an event handler. The delegate in the publisher class invokes the method (event handler) of the subscriber class.

## Declaring Events
To declare an event inside a class, first a delegate type for the event must be declared. For example,

```public delegate string MyDel(string str);```
Next, the event itself is declared, using the **event** keyword −

```event MyDel MyEvent;```
The preceding code defines a delegate named BoilerLogHandler and an event named BoilerEventLog, which invokes the delegate when it is raised.

```
using System;

namespace SampleApp {
   public delegate string MyDel(string str);
	
   class EventProgram {
      event MyDel MyEvent;
		
      public EventProgram() {
         this.MyEvent += new MyDel(this.WelcomeUser);
      }
      public string WelcomeUser(string username) {
         return "Welcome " + username;
      }
      static void Main(string[] args) {
         EventProgram obj1 = new EventProgram();
         string result = obj1.MyEvent("Tutorials Point");
         Console.WriteLine(result);
      }
   }
}
```