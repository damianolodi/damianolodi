---
title: Loops and Conditionals
linktitle: Loops and Conditionals
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Basic Concepts
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## If Statement

```python
# if statement
if condition1:
    <code>
elif condition2:
    <code>
else:
    <code>

if any((x, y, z)): # test multiple flags at once
    <code>

# Assign value based on condition
var = value_1 if  condition else value_2
```

- `==`, `!=`, `>`, `<`, `>=`, `<=` &rarr; comparators
- **not**, **and**, **or** &rarr; boolean operators

---
## For Loop

```py
for item in listName:
    <code>

for index, item in enumerate(listName, start=0):
    print index, item

for _ in range(10):
    <code>

list_a = [3, 9, 17, 15, 19]
list_b = [2, 4, 8, 10, 30, 40, 50, 60, 70, 80, 90]
for a, b, ... in zip(list_a , list_b, ...): # loop on multiple lists
    <code>
```

It can loop also on _dictionaries_ and _strings._

- `zip()`&rarr; stops at the end of the shorter list.

{{% alert note %}}
Dictionaries are **unordered**: the loop wil go through _every key_, but **not necessarily in the same order** each time.
{{% /alert %}}

---
## While Loop

```py
while condition:
    <code>
else:
    <code>
```

- `else` &rarr; executed enytime the _condition_ is evaluated as _False._
    - It is **not executed** if the cycle is interrupted by `break`.

