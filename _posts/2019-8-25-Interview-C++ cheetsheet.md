---
layout: post
title:  "C++ CheatSheet"
categories: Interview
---
# Polymorphism
## Compiled-time polymorphism
Compile time polymorphism: This type of polymorphism is achieved by function overloading or operator overloading. 

### Virtual Function
Virtual function is member function declared in base class, and is required to be redefined in derived class. When using a **Pointer or Reference** of the base class, you can call a virtual function for that object and execute the derived class’s version of the function.

* Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for function call.
* They are mainly used to achieve **Runtime polymorphism**
* Functions are declared with a virtual keyword in base class.
* The resolving of function call is done at Run-time.
* They are always defined in base class and overridden in derived class. It is **not mandatory for derived class to override** (or re-define the virtual function), in that case base class version of function is used.

Compile-time(early binding) VS run-time(late binding) behavior of Virtual Functions

### Implementation
[g4g](https://www.geeksforgeeks.org/virtual-functions-and-runtime-polymorphism-in-c-set-1-introduction/)

<hr>

## Pure Virtual Functions and Abstract Classes in C++
A pure virtual function (or abstract function) in C++ is a virtual function for which we don’t have implementation, we **only declare** it. A pure virtual function is declared by assigning 0 in declaration. See the following example.

```
// An abstract class 
class Test 
{    
    // Data members of class 
public: 
    // Pure Virtual Function 
    virtual void show() = 0; 
    
   /* Other members */
}; 
```

* A class is abstract if it has at least one pure virtual function.
* If we do not override the pure virtual function in derived class, then derived class also becomes abstract class.
<hr>

# diff between C and C++
[link](https://www.geeksforgeeks.org/write-c-program-produce-different-result-c/)

# Class access control
## Friend class
Friend Class A friend class can access private and protected members of other class in which it is declared as friend. 

Friend Function Like friend class, a friend function can be given special grant to access private and protected members. A friend function can be:
a) A method of another class
b) A global function

* Friendship is not mutual. If class A is a friend of B, then B doesn’t become a friend of A automatically.

* Friendship is not inherited (See this for more details)

# Static
static members are only declared in class declaration, not defined. They must be explicitly defined outside the class using scope resolution operator.

If we try to access static member ‘a’ without explicit definition of it, we will get compilation error. For example, following program fails in compilation.
```
#include <iostream> 
using namespace std; 

class A 
{ 
	int x; 
public: 
	A() { cout << "A's constructor called " << endl; } 
}; 

class B 
{ 
	static A a; 
public: 
	B() { cout << "B's constructor called " << endl; } 
	static A getA() { return a; } 
}; 

int main() 
{ 
	B b; 
	A a = b.getA(); 
	return 0; 
} 

```

# New and malloc
 Following are the differences between malloc() and operator new.

new is an operator, while malloc() is a function.
new returns exact data type, while malloc() returns void *.
new calls constructors( class instances are initalized and deinitialized automatically), while malloc() does not( classes won’t get initalized or deinitialized automatically
Syntax:
int *n = new int(10); // initialization with new()
str = (char *) malloc(15); //malloc()
free( ) is used on resources allocated by malloc( ), or calloc( ) in C

Delete is used on resources allocated by new in C++

# Copy Constructor
A copy constructor is a member function which initializes an object using another object of the same class. A copy constructor has the following general function prototype: ClassName (const ClassName &old_obj);

Point(int x1, int y1) { x = x1; y = y1; }

// Copy constructor

Point(const Point &p2) {x = p2.x; y = p2.y; }

When is copy constructor called?

In C++, a Copy Constructor may be called in following cases:

When an object of the class is returned by value.
When an object of the class is passed (to a function) by value as an argument.
When an object is constructed based on another object of the same class.
When compiler generates a temporary object.

https://www.geeksforgeeks.org/commonly-asked-oop-interview-questions/