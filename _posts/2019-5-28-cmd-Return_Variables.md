---
layout: post
title:  "cmd105 Return Variables"
categories: cmd
---
## Return Code Conventions
By convention, command line execution should return *zero when execution succeeds* and non-zero when execution fails. Warning messages typically don’t effect the return code. What matters is did the script work or not?

## Checking Return Codes In Your Script Commands
The environmental variable `%ERRORLEVEL%` contains the return code of the last executed program or script. A very helpful feature is the built-in DOS commands like ECHO, IF, and SET will preserve the existing value of %ERRORLEVEL%.

The conventional technique to check for a non-zero return code using the `NEQ (Not-Equal-To)` operator of the IF command:

```
IF %ERRORLEVEL% NEQ 0 (
  REM do something here to address the error
)
```

Another common technique is:
```
IF ERRORLEVEL 1 (
  REM do something here to address the error
)
```

The ERRORLEVEL 1 statement is true when the return code is any number equal to or greater than 1. However, I don’t use this technique because programs can return negative numbers as well as positive numbers. Most programs rarely document every possible return code, so I’d rather explicity check for non-zero with the NEQ 0 style than assuming return codes will be 1 or greater on error.

### Check for a specific value
You may also want to check for specific error codes. For example, you can test that an executable program or script is in your PATH by simply calling the program and checking for return code 9009.

```
SomeFile.exe
IF %ERRORLEVEL% EQU 9009 (
  ECHO error - SomeFile.exe not found in your PATH
)
```
## Conditional Execution Using the Return Code

There’s a super cool shorthand you can use to execute a second command based on the success or failure of a command. The first program/script must conform to the convention of returning 0 on success and non-0 on failure for this to work.

To execute a follow-on command after sucess, we use the `&&` operator:
```
SomeCommand.exe && ECHO SomeCommand.exe succeeded!
```
To execute a follow-on command after failure, we use the `||` operator:
```
SomeCommand.exe || ECHO SomeCommand.exe failed with return code %ERRORLEVEL%
```

## Tips and Tricks for Return Codes
I recommend sticking to zero for success and return codes that are positive values for DOS batch files. The positive values are a good idea because other callers may use the IF ERRORLEVEL 1 syntax to check your script.

I also recommend documenting your possible return codes with easy to read SET statements at the top of your script file, like this:
```
SET /A ERROR_HELP_SCREEN=1
SET /A ERROR_FILE_NOT_FOUND=2
```
