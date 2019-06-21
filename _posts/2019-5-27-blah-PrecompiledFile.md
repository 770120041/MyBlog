---
layout: post
title:  "Precompiled File Stdafx.h"
categories: Blah
---
## what's stdafx.h
When you create a new project in Visual Studio, a precompiled header file named `"pch.h"` is added to the project. 
(In earlier versions of Visual Studio, the file was called `"stdafx.h"`.) 
The purpose of the file is to speed up the build process. 
Any stable header files, for example Standard Library headers such as <vector>, s
hould be included here. The precompiled header is compiled only when it, 
or any files it includes, are modified. If you only make changes in your project source code, 
the build will skip compilation for the precompiled header.

The compiler options for precompiled headers are /Y. In the project propery pages, 
the options are located under Configuration Properties > C/C++ > Precompiled Headers. 
You can choose to not use precompiled headers, and you can specify the header file name and the name and path of the output file.

## When to Precompile Source Code
Precompiled code is useful during the development cycle to reduce compilation time, especially if:

* You always use a large body of code that changes infrequently.

* Your program comprises multiple modules, all of which use a standard set of include files and the same compilation options. In this case, all include files can be precompiled into one precompiled header.

The first compilation — the one that creates the precompiled header (PCH) file — takes a bit longer than subsequent compilations. Subsequent compilations can proceed more quickly by including the precompiled code.

You can precompile both C and C++ programs. In C++ programming, it is common practice to separate class interface information into header files. These header files can later be included in programs that use the class. By precompiling these headers, you can reduce the time a program takes to compile.
