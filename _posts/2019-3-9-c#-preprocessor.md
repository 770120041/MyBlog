---
layout: post
title:  "C# Preprocessor Directives"
categories: C#
---
## Conditional preprocessor directives
Similiar to C++'s preprocessor directives<br>
example:
```r
#if DEBUG
    Console.WriteLine("Debug version");
#endif
```
We can use conditional preprocessor directives like c++ by combining
```r
 #else, #elif, #endif, #define, and #undef directives 
```

```
#define MYTEST
using System;
public class MyClass
{
    static void Main()
    {
#if (DEBUG && !MYTEST)
        Console.WriteLine("DEBUG is defined");
#elif (!DEBUG && MYTEST)
        Console.WriteLine("MYTEST is defined");
#elif (DEBUG && MYTEST)
        Console.WriteLine("DEBUG and MYTEST are defined");  
#else
        Console.WriteLine("DEBUG and MYTEST are not defined");
#endif
    }
}
```
## Warning and error
Warning lets you generate a CS1030 level one compiler warning from a specific location in your code. For example
```r
#warning Deprecated code in this method.
```
Error lets you generate a CS1029 user-defined error from a specific location in your code. For example:
```r
// preprocessor_error.cs  
// CS1029 expected  
#define DEBUG  
class MainClass   
{  
    static void Main()   
    {  
#if DEBUG  
#error DEBUG is defined  
#endif  
    }  
}
```

# Region
Eegion lets you specify a block of code that you can **expand or collapse** when using the outlining feature of the Visual Studio Code Editor. In longer code files, it is convenient to be able to collapse or hide one or more regions so that you can focus on the part of the file that you are currently working on. The following example shows how to define a region:
```r
#region MyClass definition  
public class MyClass   
{  
    static void Main()   
    {  
    }  
}  
#endregion
```
A #region block cannot overlap with a #if block. However, a #region block can be nested in a #if block, and a #if block can be nested in a #region block.<br>

We can use and ```#endregion``` to indicate the end of a region.
And after the ```#region``` symbol, we can append commented <Summary>Description</Summary> to describe the region

```r
#region MainWindow
    /// <summary>
    /// MainWindow
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
    #endregion
```