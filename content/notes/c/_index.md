---
# Course title, summary, and position.
linktitle: C and Embedded Systems
summary: Notes on C and embeddend programming
weight: 3

# Page metadata.
title: Overview
date: "2019-11-27T00:00:00Z"
lastmod: "2019-11-27T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  c:
    name: Overview
    weight: 1
---

```c
int var_name = value;           // variable initialization
char str_name[n];               // array initialization
char str_name[] = "Hello World!";
const int VAR_NAME = value;     // constant declaration

int main (void) {               // execution start from here
    // code
    return 0;
}
```

- **Math operators** &rarr; `+`, `-`, `*`, `/`, `%` (mod)
- **Logic operators** &rarr; `&`, `|`, `~`
    - Bitwise operations. See [here](#) for more terminology and applications.
- **Boolean operators** &rarr; `&&`, `||`, `!`
    - Zero is false, non-zero is true.
- **Relational operators** &rarr; `>`, `>=`, `<`, `<=`, `==`, `!=`
- **Shift operators** &rarr; `>>`, `<<`
    - Used to multiply (by 2), divide (by 2) or align bits before logic operations.
- For each string of _n_ charachters, _n+1_ bytes are allocated in memory: the last is added by C as the _null_ character `\0` (all zeros)
- `(type) var_name` &rarr; change variable type
- `man function_name` &rarr; use in terminal to return documentation about the function

### Best Practices

1. Variables are named with capital letters when global

### Tips

- `echo $` &rarr; print what was returned by main. **Useful to debug** complicated programs. Return a non-0 value if anything goes wrong

---
## Variables Scope and Naming Conventions

```c
// the "_t" means "type"

#define CONSTANT value
uint16_t const someConstants[4] = {0, 200, 43, 15};

uint32_t varName; // GLOBAL VARIABLE
external uint32_t varName; // EXTERNAL GLOBAL VARIABLE

uint8_t static anotherVar; // STATIC VARIABLE (PUBLIC SCOPE)

void main(void) {
    uint8_t static anotherOne; // STATIC LOCAL VARIABLE (PRIVATE SCOPE)
    // code
}
```

**Global variables** have a _public scope_, so they can be accessed from every function in the module. They can be also accessed and modified from other modules if they are defined as _external._

{{% alert warning %}}
Using global variables is BAAAAD! They have _permanent memory allocation_, which means that they exist in memory forever.
{{% /alert %}}

**Static variables** also have _permanent memory allocation_, and can be defined globally (outside of all functions in the file) or within a function. In the first case, _only functions in the file can access them_ and this reduces the complexity of the program. 

**Local variables** have _private scope_ and _dynamic allocation_, so they are stored temporarily in a register or in the stack.

{{% alert note %}}
**Restrict scope and allocation as much as possible:** the firmware will be simpler and used memory resources can be reallocated for something else.
{{% /alert %}}

**Constants** are better used when defined using the `#define` preprocessor directive. When lots of constants are necessary, they can be placed in the ROM with the `const` keyword.

---
## Running `main` with External Parameters

```c
int main (int argc, char* argv[]) {}
```
This will enable the program to **accept parameter from the command line** when run.

`argc` &rarr; the number of written words, _including_ the name of the program. This value is auto-generated.

`argv[]` &rarr; array of strings. `argv[1]` is the location of the first parameter inserted by the user.

---
## Compiling the Program

- `clang file.c` &rarr; compile and produce `a.out`
- `clang -0 file file.c` &rarr; compile and produce `file`
- `make file.c` &rarr; call clang to compile (elaborate on that)

### Compiler Workflow

1.  **Preprocessing**
    - `#include` are called _preprocessor directives_
    - `#` means that it should be handled first, before everything else
2.  **Compiling**
    - Code is translated into _assembly language_, understood by CPUs
3.  **Assembling**
    - _Assembly_ is translated into 0s and 1s
4.  **Linking**
    - Also, libraries are compiled in 0s and 1s, and all those files are linked/attached one another

---
## Resources
- The first 4 lessons from MIT's course CS50 - [Lecture 1](https://youtu.be/wEdvGqxafq8), [lecture 2](https://youtu.be/u-kH-5JJSgU), [lecture 3](https://youtu.be/cC9I3XxkZXw), [lecture 4](https://youtu.be/ed2lnJNf7HU) 