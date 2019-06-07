---
layout: post
title:  "cmd111 More Tricks"
categories: cmd
---
## Comment
I like to include a header on all of scripts that documents the who/what/when/why/how. You can use the :: comment trick to make this header info more readable:
```
:: Name:     MyScript.cmd
:: Purpose:  Configures the FooBar engine to run from a source control tree path
:: Author:   stevejansen_github@icloud.com
:: Revision: March 2013 - initial version
::           April 2013 - added support for FooBar v2 switches

@ECHO OFF
SETLOCAL ENABLEEXTENSIONS ENABLEDELAYEDEXPANSION

:: variables
SET me=%~n0


:END
ENDLOCAL
ECHO ON
@EXIT /B 0
```

### Difference with `REM`
:: is a label (also, inaccurately, known in the wild by comment label) can be, in practice, considered a comment just as REM is, as it is an "un-goto-able" label.

There are some differences between REM and ::, though. The main ones are:
*With `ECHO ON` a REM line is shown but not a line commented with ::
*A `::` can execute a line end caret (that is, a ^ at the end of a line starting with :: makes the next line also a comment):
```
:: This is a comment^
echo but watch out because this line is a comment too
```

A line that start in double colon represent an invalid label that is ignored by the command processor, so it may be used to insert a comment. For reasons that can't be traced, many people use :: to insert comments in Batch files, but you must be aware that there are several pitfalls in its use that are described in the link given in Koterpillar's answer. It seems that the first use of :: instead of REM command was with the purpose to speed up the execution of Batch files in slow machines (ie: floppy disks), but that reason is not a valid justification for the use of double colon since many years ago.
<hr>

## Conditional commands based on success/failure

The conditional operators `||` and `&&` provide a convenient shorthand method to run a 2nd command based on the succes or failure of a 1st command.

The && syntax is the AND opeartor to invoke a 2nd command when the first command returns a zero (success) exit code.

`DIR myfile.txt >NUL 2>&1 && TYPE myfile.txt`

The || syntax is an OR operator to invoke a 2nd command when the first command returns a non-zero (failure) exit code.

`DIR myfile.txt >NUL 2>&1 || CALL :WARNING file not found - myfile.txt`

We can even combined the techniques. Notice how we use the () grouping construct with && to run 2 commands together should the 1st fail.

`DIR myfile.txt >NUL 2>&1 || (ECHO %me%: WARNING - file not found - myfile.txt >2 && EXIT /B 1)`

## Getting the full path to the parent directory of the script
```
:: variables
PUSHD "%~dp0" >NUL && SET root=%CD% && POPD >NUL
```



