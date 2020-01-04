---
title: Functions and Pointers
linktitle: Functions and Pointers
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Basic Concepts
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## Pointers

```c
int* pointer_name; // pointer declaration

// return the value pointed by that pointer
var = *pointer_name;

// return the address of the variable
pointer = &var_name;
```

- **DEF**: memory address of a variable of a certain type.
- Pointers are 64 bits in length.

---
## Functions

```c
// before main
int function_name (parameters); // PROTOTYPE

// after main
int function_name (parameters) { // DEFINITION
    // code
}
```

- _Prototypes_ are usually placed in a **header file** `file.h`.