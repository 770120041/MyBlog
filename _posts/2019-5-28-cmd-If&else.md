---
layout: post
title:  "cmd107 If and else"
categories: cmd
---
## Checking that a File or Folder Exists
```
IF EXIST "temp.txt" ECHO found
```
Both the true condition and the false condition:
```
IF EXIST "temp.txt" (
    ECHO found
) ELSE (
    ECHO not found
)
```
NOTE: It’s a good idea to always quote both operands (sides) of any IF check. This avoids nasty bugs when a variable doesn’t exist, which causes the the operand to effectively disappear and cause a syntax error.

<hr>

## Checking If A Variable Is Not Set
```
IF "%var%"=="" (SET var=default value)
```
Or
```
IF NOT DEFINED var (SET var=default value)
```
<hr>

## Checking If a Variable Matches a Text String
```
SET var=Hello, World!

IF "%var%"=="Hello, World!" (
    ECHO found
)
```
Or with a case insensitive comparison
```
IF /I "%var%"=="hello, world!" (
    ECHO found
)
```
<hr>

## Artimetic Comparisons
```
SET /A var=1

IF /I "%var%" EQU "1" ECHO equality with 1

IF /I "%var%" NEQ "0" ECHO inequality with 0

IF /I "%var%" GEQ "1" ECHO greater than or equal to 1

IF /I "%var%" LEQ "1" ECHO less than or equal to 1
```

## Checking a Return Code
```
IF /I "%ERRORLEVEL%" NEQ "0" (
    ECHO execution failed
)
```




