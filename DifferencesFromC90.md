# How picoc differs from C90 #

picoc is a tiny C language, not a complete implementation of C90. It doesn't aim to implement every single feature of C90 but it does aim to be close enough that most programs will run without modification.

picoc also has scripting abilities which enhance it beyond what C90 offers.

## C preprocessor ##

There is no true preprocessor in picoc. The most popular preprocessor features are implemented in a slightly limited way.

## #define ##

#define macros are implemented but have some limitations. They can only be used as part of expressions and operate a bit like functions. Since they're used in expressions they must result in a value.

## #if / #ifdef / #else / #endif ##

The conditional compilation operators are implemented, but have some limitations. The operator "defined()" is not implemented. These operators can only be used at statement boundaries.

## #include ##

#include is supported however the level of support depends on the specific port of picoc on your platform. Linux/UNIX and cygwin support #include fully.

# Function declarations #

This style of function declaration is supported:

```
int my_function(char param1, int param2, char *param3)
{
   ...
}
```

The old "K&R" form of function declaration is not supported.

# Predefined macros #

A few macros are pre-defined:

  * **PICOC\_VERSION** - gives the picoc version as a string eg. `"v2.1 beta r524"`
  * **LITTLE\_ENDIAN** - is 1 on little-endian architectures or 0 on big-endian architectures
  * **BIG\_ENDIAN** - is 1 on big-endian architectures or 0 on little-endian architectures

# Function pointers #

Pointers to functions are currently not supported.

# Storage classes #

Many of the storage classes in C90 really only have meaning in a compiler so they're not implemented in picoc. This includes: static, extern, volatile, register and auto. They're recognised but currently ignored.

# struct and unions #

Structs and unions can only be defined globally. It's not possible to define them within the scope of a function.

Bitfields in structs are not supported.

# Linking with libraries #

Because picoc is an interpreter (and not a compiler) libraries must be linked with picoc itself. Also a glue module must be written to interface to picoc. This is the same as other interpreters like python.

If you're looking for an example check the interface to the C standard library time functions in cstdlib/time.c.

# goto #

The goto statement is implemented, but only supports forward gotos, not backward. The rationale for this is that backward gotos are not necessary for any "legitimate" use of goto.

Some discussion on this topic:
  * http://www.cprogramming.com/tutorial/goto.html
  * http://kerneltrap.org/node/553/2131

# Scripting enhancements #

## Interactive mode ##

picoc can be used interactively, from a command line. In Linux/UNIX/cygwin run it as:

```
picoc <module1.c> <module2.c>...
```

You'll be presented with a picoc command prompt. You can type programs directly into the picoc command line and call functions directly as well.

Unlike standard C you can write C commands directly on the command line and they'll be executed immediately. eg.

```
int i;
for (i = 1; i <= 10; i++)
    printf("Hello world: %d\n", i);
```

Use your operating system's end of file character to quit the interactive mode. (Control-D is end of file in Linux/UNIX)

## Deleting functions or variables ##

You may want to delete a function or variable declaration in interactive mode so you can re-declare it. Use "delete whatever;" to do this.

## Running programs from the command line ##

If you're running a picoc program from the command line on Linux/UNIX/cygwin you can invoke it like:

```
picoc <module1.c> <module2.c>...
```

The main() function will be run once the program is loaded. To use command line arguments with it do:

```
picoc <module1.c> <module2.c>... - arg1 arg2...
```

If you're running a script which has no main() function run:

```
picoc -s <module1.c> <module2.c>... - arg1 arg2...
```