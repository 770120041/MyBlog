---
layout: post
title:  "cmd106 STD I/O"
categories: cmd
---
## File Numbers
Each of these three standard files, otherwise known as the standard streams, are referernced using the numbers 0, 1, and 2. Stdin is file 0, stdout is file 1, and stderr is file 2.

## Redirection
A very common task in batch files is sending the output of a program to a log file. The > operator sends, or redirects, stdout or stderr to another file. For example, you can write a listing of the current directory to a text file:
```
DIR > temp.txt
DIR SomeFile.txt  2>> error.txt
```

By default, the > and >> operators redirect stdout. You can redirect stderr by using the file number 2 in front of the operator:
```
DIR SomeFile.txt 2>&1
```


### Special usage
You can even combine the stdout and stderr streams using the file number and the & prefix:
```
DIR SomeFile.txt > output.txt 2>&1
```

Understand this
2 refers to the second file descriptor of the process, i.e. stderr.

> means redirection.

&1 means the target of the redirection should be the same location as the first file descriptor, i.e. stdout.

So `> output.txt 2>&1` means first redirects stdout to output.txt and then redirects stderr there as well. This effectively silences all output (regular or error) from the dir command

## Suppressing Program Output
The pseudofile NUL is used to discard any output from a program. Here is an example of emulating the Unix command sleep by calling ping against the loopback address. We redirect stdout to the NUL device to avoid printing the output on the command prompt screen.


```
PING 127.0.0.1 > NUL
```

## A Cool Party Trick
You can quickly create a new text file, say maybe a batch script, from just the command line by redirecting the command prompt’s own stdin, called `CON`, to a text file. When you are done typing, hit `CTRL+Z`, which sends the end-of-file (EOF) character.

```
TYPE CON > output.txt
```

