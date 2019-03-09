---
layout: post
title:  "C# Invoke methods"
categories: C#
---

## Delegate
A funciton pointer
```r
public delegate int Manipulate(int a);
var faManipulate = new Manipulate(fa);

public int fa(int a){
    return a*2;
}
```


## Anonymous Delegate
Exaclty the same as example Above, just make it anonymous
```r
public delegate int Manipulate(int a);
Manipulate anonymousDelegate = delegate(int a){ return a*2; };
```

## Lambda Expression
Lambda expression are anything starts with left/right value<br>
They can return a value(that can be invoked), or it can return an expression of a delegate(Then it can be compiled and executed)

#### Syntax
left of => left value <br>
right of => right value <br>

(param1,param2) => {} <br>

If many lines:<br>
```r
(param1,param2) => {
    line1
    line2
    return 
}
```



#### Example
```r
Manipulate lambdaDelegate = a => a*2;
```
There is still type check as the definition of Manipulate
If we chagne the paramters of Manipulte to Long, then a must be a long
