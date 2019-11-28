---
title: Custom Symbols and Footprints
linktitle: Data Structures
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Basic Concepts
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

```c
// structure definition
typedef struct {
    int var1;
    char var2;
} struct_name;

// structure initialization
struct_name var_name;
struct_name var_name[n]; // array of structure

// access with dot notation
var_name.var1 = value;
```
Save in `file.h` and import with `#include`.

---
## Recursive Structures

```c
typedef struct struct_name {
    int n;
    struct struct_name *next; // pointer to my structure
} struct_name;
```

`struct_name` is called also before definition so that the compiler knows that a structure called `struct_name` exists.

This sort of structure can be usefull to create a "linked list" in which I store a value and the pointer to the next value in the list:

- the last element of the list will point to NULL;
- this solve the fact that arrays are fixed size in C, but it has the downside that when parsing the list I always have to begin from the start and I cannot go backward;
- if I don't care about sorting, this is an O(1) in terms of adding elements to the list (against O(n) when using `malloc`) but I am "wasting" memory to store all those pointers.

{{< figure src="/notes/c/img/linked-list.jpg" title="Example of recursive structure" lightbox="true" >}}