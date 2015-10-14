Running picoc from the command line is possible under linux, UNIX and cygwin environments. This page describes the options available.

# Command line parameters #

## Running standard C programs ##

You can run standard C programs straight from the command line:
```
> picoc myprogram.c
```

If your program is split into multiple files you can list them all on the command line.
```
> picoc myprog1.c myprog2.c myprog3.c
```

If your program takes arguments you add them after a '-' character.
```
> picoc myprogram.c - arg1 arg2
```

## Running script files ##

Scripts are slightly simpler than standard C programs - all the system headers are included automatically for you so you don't need to include them yourself. Also, scripts don't have a main() function - they just have statements which are run directly.

```
> picoc -s myprogram.c
```

Here's an example script:

```
printf("Starting my script\n");

int total = 0;
int i;
for (i = 0; i < 10; i++)
{
    printf("i = %d\n", i);
    total += i;
}

printf("The total is %d\n", total);
```

Here's the output from this script under cygwin:
```
$ ./picoc -s myscript.c
Starting my script
i = 0
i = 1
i = 2
i = 3
i = 4
i = 5
i = 6
i = 7
i = 8
i = 9
The total is 45
```

## Interactive mode ##

If you want to type C statements directly into picoc you can use the interactive mode:
```
> picoc -i
```

Here's an example session:
```
$ ./picoc -i
starting picoc v2.1
picoc> char inbuf[80];
picoc> gets(inbuf);
hello!
picoc> printf("I got: %s\n", inbuf);
I got: hello!
```

You can quit picoc's interactive mode using control-D.

### Deleting variables and functions ###

Sometimes in interactive mode you want to change a function or redeclare a variable. You can do this using the "delete" statement:
```
$ ./picoc -i
starting picoc v2.1
picoc> int fred = 1234;
picoc> printf("fred = %d\n", fred);
fred = 1234
picoc> delete fred;
picoc> char *fred = "hello";
picoc> printf("fred = '%s'\n", fred);
fred = 'hello'
```

# Environment variables #

In some cases you may want to change the picoc stack space. The default stack size is 128KB which should be large enough for most programs.

To change the stack size you can set the **STACKSIZE** environment variable to a different value. The value is in bytes.