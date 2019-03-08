---
layout: post
title:  "C# Attributes and Properities"
---

# Properties
 
 In Java, the best practice for data members is a "bean property", which is a **getter/setter** pair.<br> This is the only way to create a "rich" domain model that self-validates or calculates related fields based on input. The only other basic standard is a "struct", using fields as data members, which in Java leads to an anemic domain model. <br>

 C# by contrast has first-class syntax support for a true "property"; a member whose usage is identical to a field, but that defines a getter/setter code block pair. This makes for a clear differentiation between the definition and usage of **"data members"** versus **"function members"**; anything the object has in terms of containing and storing data is modeled using either a public field or property, which have identical usage syntax (best practice is to use a property, especially since the introduction of the "auto-property" which is syntactical shorthand for a property that acts on a backing field). Anything the object does in the sense of producing or processing data is a method. The line blurs when you consider using read-only (or write-only) properties, but overall this feature of the language makes the intent of objects and their members much easier to understand.<br>
 
 There are also abstract properities for derived class:
 ```
 public abstract string Name {
         get;
         set;
      }
 ```

 In a word, properties decouples data members and function and made class declaration clearly.




# Attributes
C# attribtues are used as declaring tags, kind of similiar to python Decorators. But it can be used for more kinds of ways <br>
examples:
```
[Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
```
## Attribute parameters
```
[Conditional("DEBUG"), Conditional("TEST1")]
void TraceMethod()
{
    // ...
}
```

## Attribute targets
[target : attribute-list]
example
```
using System;
using System.Reflection;
[assembly: AssemblyTitleAttribute("Production assembly 4")]
[module: CLSCompliant(true)]
```
