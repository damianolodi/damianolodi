---
title: Libraries for Various Things
linktitle: for Miscellaneous
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Libraries...
    weight: 7

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

## The `stdio.h` Library
```c
#include <stdio.h>
```

- `printf()` &rarr; Possible placeholders: 
  - `%c` (single char), `%s` (string);
  - `%i` (integer), `%f` (float), `%u` (unsigned decimal integer);
  - `%p` (pointer).
- `scanf("%i", &varName)` &rarr; **wait user input**. Various placeholders are available (number in different bases, strings,...). Return the number of characters written, or a negative number if some errors occured. **WARNING:** the input should be checked for correctness.

---
## The `unistd.h` Library

```c
#include <unistd.h>
```

- `sleep(i)` &rarr; **suspend code execution** for at least _i seconds._