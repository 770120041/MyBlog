---
layout: post
title:  "cmd108 Loops"
categories: cmd
---
## Old School with GOTO
The old-school way of looping on early versions of DOS was to use labels and GOTO statements. This isn’t used much anymore, though it’s useful for looping through command line arguments.
```
:args
SET arg=%~1
ECHO %arg%
SHIFT
GOTO :args
```

<hr>

## For
The modern way to loop through files or text uses the FOR command. In my opinion, FOR is the single most powerful command in DOS, and one of the least used.

GOTCHA: The FOR command uses a special variable syntax of `%` followed by a single letter, like %I. This syntax is slightly different when FOR is used in a batch file, as it needs an extra percent symbol, or `%%I`. This is a very common source of errors when writing scripts. Should your for loop exit with invalid syntax, be sure to check that you have the %% style variables.

### Looping Through Files
`FOR %I IN (%CD%\*) DO @ECHO %I`

### Looping Through Directories
`FOR /D %I IN (%USERPROFILE%\*) DO @ECHO %I`

Recursively loop through files in all subfolders of the %TEMP% folder

`FOR /R "%TEMP%" %I IN (*) DO @ECHO %I`

Recursively loop through all subfolders in the %TEMP% folder

`FOR /R "%TEMP%" /D %I IN (*) DO @ECHO %I`



