---
title: Built-in Functions
linktitle: Built-in
toc: false
type: docs
date: "2020-01-18T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Packages
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

- `locals()` &rarr; it returns a _dict_ with all local variables.
- `globals()` &rarr; it return a _dict_ with all global variables.
    - It returns also the values of `__builtins__`, `__doc__`, `__file__`, `__name__`, `__package__`, `__cached__`, `__loader__`, `__spec__`.
- `vars()` &rarr; it has the same output of `locals()` and `globals()` depending if called in a function or in the main program.
    - `vars(object)` &rarr; it returns all the attributes defined on the object. 
