---
layout: post
title:  "C# nullabls"
categories: C#
---
What's the mearning of C#'s nullables?
For primitive types, like *int*, *bool*, *decimal*, *double* in C#, they cannot be null, while Reference types like *object*, *dynamic* can point to null(like `string something = null`). So C# have created a wrapper type nullable such that primitive types can be able to point to Nullable values. And this characteristic would be most useful for databases.


## Syntax

```
< data_type> ? <variable_name> = null;
```

## Examples

```
using System;

namespace CalculatorApplication {
   class NullablesAtShow {
      static void Main(string[] args) {
         int? num1 = null;
         int? num2 = 45;
         
         double? num3 = new double?();
         double? num4 = 3.14157;
         
         bool? boolval = new bool?();

         // display the values
         Console.WriteLine("Nullables at Show: {0}, {1}, {2}, {3}", num1, num2, num3, num4);
         Console.WriteLine("A Nullable boolean value: {0}", boolval);
         Console.ReadLine();
      }
   }
}
```
