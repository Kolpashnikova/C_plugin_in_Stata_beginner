## This is how to compile the C plugin for Stata and run it in September 2019

This is a modification from [Stata Blog](https://blog.stata.com/2018/02/20/programming-an-estimation-command-in-stata-writing-a-c-plugin/) because their code doesn't run in September 2019

First, create a C file (here is an example from the Stata blog), called here hello.c

	// version 1.0.0 14Feb2018
	#include "stplugin.h"
	#include <stdio.h>
	#include <string.h>
	STDLL stata_call(int argc, char *argv[])
	{
	    char  msg[81] ;
	    strcpy(msg, "Hello from C\n");
	    SF_display(msg) ;
	    return((ST_retcode) 0) ;
	}

Then, compile the file. 
You need to copy the following two files into your folder:
- [stplugin.c](https://www.stata.com/plugins/stplugin.c)
- [stplugin.h](https://www.stata.com/plugins/stplugin.h)
These are needed for compilation.

You will also need to install [MinGW](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/rubenvb/gcc-4.8-release/).

After installation, run the following command in MinGW (don't forget to cd to the folder where hello.c file is):
	
	x86_64-w64-mingw32-gcc -shared stplugin.c hello.c -o hello.plugin

The line above will compile a hello.plugin file, which will now be in the same directory.

You also need to create the myhello.ado file with the following contents:

	*! version 1.0.0  13Feb2018
	program define myhello
	    version 15.1
	    plugin call hello
	end
	program hello, plugin

Because the .ado and plugin files are in the directory where you created them, move the myhello.ado file to C:\\ado\\plus\\m folder.
Also, create a C:\\ado\\plugins folder, and move hello.plugin file into that folder.

Let your Stata know the path to your plugin files by running the following code in your Stata:

	adopath + C:\ado\plugins

Now you can use the hello.c in Stata. For that, run the following command in your Stata:

	myhello

You will receive 'Hello from C' on your screen.

Kamila Kolpashnikova | Камила Колпашникова 2019
