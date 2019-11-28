---
title: Libraries for Working on Variables
linktitle: for Variables
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Libraries...
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## The `string.h` library

```c
#include <string.h>
```
_In this context, `s` usually represent the pointer to the string._

- `strlen(s)` &rarr; return **lenght** of the string
- `strcmp(s1, s2)` &rarr; **compare** the two strings. Return 0 if the two strings are equal
- `strcpy(s1, s2)` &rarr; **copy** `s2` into `s1`

---
## The `ctype.h` library

```c
#include <ctype.h>
```

- `isdigit(int)` &rarr; return non-zero if the provided value **is a digit**
- `isalnum(int)` &rarr; return non-zero if the provided value **is a digit or a letter**
