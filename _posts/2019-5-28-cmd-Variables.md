---
layout: post
title:  "cmd104 Variables"
categories: cmd
---
## Declaration
DOS does not require declaration of variables. The value of undeclared/uninitialized variables is an empty string, or "". Most people like this, as it reduces the amount of code to write. Personally, I’d like the option to require a variable is declared before it’s used, as this catches silly bugs like typos in variable names.

But if we echo a not defined variable, like `echo %helo%`, it will print `%helo%`
## Variable Assignment
The SET command assigns a value to a variable.
*Do not use whitespace between the name and value; SET foo = bar will not work but SET foo=bar will work.*

The /A switch supports arthimetic operations during assigments. This is a useful tool if you need to validated that user input is a numerical value.
If we use `SET /A four=2+2` we got `four=4`

A common convention is to use lowercase names for your script’s variables. System-wide variables, known as environmental variables, use uppercase names. 

**DOS is case insensitive**, so this convention isn’t enforced but it’s a good idea to make your script’s easier to read and troubleshoot.

**WARNING: SET will always overwrite (clobber) any existing variables.** It’s a good idea to verify you aren’t overwriting a system-wide variable when writing a script. A quick ECHO %foo% will confirm that the variable foo isn’t an existing variable. Or we can use `if not defined Variable` to judge it

## Scope
By default, variables are global to your entire command prompt session. Call the SETLOCAL command to make variables local to the scope of your script. After calling SETLOCAL, any variable assignments revert upon calling ENDLOCAL, calling EXIT, or **when execution reaches the end of file (EOF) in your script**.


### Delayed explansion
There is also a variable syntax using !, like !var!. This is a special type of situation called delayed expansion. It need to be enabled by `SETLOCAL EnableDelayedExpansion`

## CMD argument for Script
Command Line Arguments to Your Script

You can read the command line arguments passed to your script using a special syntax. The syntax is a single *% character followed by the ordinal position of the argument from 0 – 9*. The zero ordinal argument is the name of the batch file itself. So the variable %0 in our script HelloWorld.cmd will be “HelloWorld.cmd”.

The command line argument variables are * *%0: the name of the script/program as called on the command line; always a non-empty value* * %1: the first command line argument; empty if no arguments were provided * %2: the second command line argument; empty if a second argument wasn’t provided * …: * %9: the ninth command line argument

NOTE: DOS does support more than 9 command line arguments, however, you cannot directly read the 10th argument of higher. 

Reference [here](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/shift) for document of shifting variabels.



## Tricks with Command Line Arguments
Command Line Arguments also support some really useful optional syntax to run quasi-macros on command line arguments that are file paths. These macros are called variable substitution support and can resolve the path, timestamp, or size of file that is a command line argument. The documentation for this super useful feature is a bit hard to find – run ‘FOR /?’ and page to the end of the output.
* `%~I` removes quotes from the first command line argument, which is super useful when working with arguments to file paths. You will need to quote any file paths, but, quoting a file path twice will cause a file not found error.
* `%~fI` is the full path to the folder of the first command line argument
* `%~dpI` is the full path to the parent folder of the first command line argument. I use this trick in nearly every batch file I write to determine where the script file itself lives. The syntax SET parent=%~dp0 will put the path of the folder for the script file in the variable %parent%.
* `%~nxI` is just the file name and file extension of the first command line argument. I also use this trick frequently to determine the name of the script at runtime. If I need to print messages to the user, I like to prefix the message with the script’s name, like ECHO %~n0: some message instead of ECHO some message . The prefixing helps the end user by knowing the output is from the script and not another program being called by the script. It may sound silly until you spend hours trying to track down an obtuse error message generated by a script. This is a nice piece of polish I picked up from the Unix/Linux world.

