---
layout: post
title:  "cmd110 Parsing Input"
categories: cmd
---
## Position variable
Percent Variables [Docs](https://ss64.com/nt/syntax-percent.html)

Escape Character [Docs](https://www.robvanderwoude.com/escapechars.php)

<hr>

## Comman shortcuts
[docs](https://ss64.com/nt/syntax-args.html)


here is a list of common shortcuts:

`%~f1` - expands %1 to a Fully qualified path name - C:\utils\MyFile.txt

`%~d1` - expands %1 to a Drive letter only - C:

`%~p1` - expands %1 to a Path only - \utils\

`%~n1` - expands %1 to a file Name, or if only a path is present (with no trailing backslash) - the last folder in that path

`%~x1` - expands %1 to a file eXtension only - .txt

`%~s1` - changes the meaning of f, n and x to reference the Short name (see note below)

`%~1` - expand %1 removing any surrounding quotes (")

`%~a1` - display the file attributes of %1

`%~t1` - display the date/time of %1

`%~z1` - display the file size of %1

`%~$PATH:1` - search the PATH environment variable and expand %1 to the fully qualified name of the first match found.

The modifiers above can be combined:

%~dp1 - expands %1 to a drive letter and path only

%~nx2 - expands %2 to a file name and extension only

<hr>

### How the CMD shell command line parser evaluates variables:
The CMD shell reads through each line of a batch file once from left to right, when it finds a percent sign it checks the next character and proceeds as follows:

If the next character is another percent sign (%), CMD replaces the two with a single percent sign and then reads the third character evaluating it either as a FOR parameter or, if not in a FOR loop, just treating it as the plain character '%'.
%%G => somevalue
%%Hello => %Hello
 
If the next character is a digit (0-9) CMD replaces the percent sign and digit with the value of the corresponding command line parameter. Also if the character is a tilde (~) then Parameter Extensions will be applied.
%1 => somevalue
 
If the next character is anything else (so not a number or a % symbol) CMD treats everything up to the next percent sign as the name of a variable and replaces it with the value of that variable.
%_somevariable% => somevalue
When working at the command line (not in a batch script) there is no possibility of any batch file parameters %1, %2 etc so the logic above is not followed and hence FOR parameters on the command line only need a single %.

One other place where a percent character is used is as the modulus operator in the SET /A command, in a batch file this should always be passed as a pair of double %%'s.

It is worth also considering how this affects normal variables - this does not make for readable code, but just to prove a point, lets try to create a standard variable with a variable name that is purely numeric:

SET 1=Hello

You might expect that to create the variable %1%, but in a batch file it will Fail.

Even if the script was called without passing any parameter %1, the parameters take precedence, so %1% is evaluated as %1 plus an extra % at the end. Remember the command line is parsed left to right only once, so the %1 is evaluated before the second % is even read. 
You can of course do this at the command line and it will work!

<hr> 

### Example
Here we get the full path to a local file passed as the first argument. We write an error message to stderr if the file does not exist and exit our script:

```
SET filepath=%~f1

IF NOT EXIST "%filepath%" (
    ECHO %~n0: file not found - %filepath% >&2
    EXIT /B 1
)
```
