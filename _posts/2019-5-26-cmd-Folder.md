---
layout: post
title:  "cmd101 Folder"
categories: cmd
---

# Reminder
Following command are not usable in power shell
# useful commands
## dir:  
like `ls` in bash,[docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir)

`dir /a` to list all files including hidden files

`dir /s` Lists every occurrence of the specified file name within the specified directory and all subdirectories. like `tree`
<hr>

## cd
change directory,[docs](
    https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/cd
)


### Each drive have seperate current directory
Frequently used :
1.  change to root dir `cd\`
2.  change driver `d:`
3.  use `cd /d` to change current directory and dirve at the same time.
<hr>

## mkdir&md
create a directory or subdirectory

Syntax `md [<Drive>:]<Path>`
<hr>

## rd & rmdir
remove a dir

Syntax `rd [<Drive>:]<Path> [/s [/q]]`

`/s` delete all subdir and files, like `rm -r`

`/q` quite mode, don't pompt confirmation for deleting directory tree, only works if `/s` specified 


<hr>

## 1 & 2
The digit before a redirection symbol is the stream number to redirect.<br>
The default stream is 1, when no number is present, so 1>... and >... are equivalent.<br>

stream 1 is the standard input/output stream, 2 is the standard error stream.

A command can output to multiple streams and it's allowed to redirect each of them to a different destination.

So `2>nul` and `1>nul` simply said that the error output and the normal output will be redirected to nul. So nothing will be outputted.





