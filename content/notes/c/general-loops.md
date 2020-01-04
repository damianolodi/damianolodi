---
title: Loops and Conditionals
linktitle: Loops and Conditionals
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  c:
    parent: Basic Concepts
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## If Statement

```c
if (condition) {
  <code>
} else {
  <code>
}
```

- **Relational operators** &rarr; `>`, `>=`, `<`, `<=`, `==`, `!=`
- **Boolean operators** &rarr; `&&`, `||`, `!`
    - Zero is false, non-zero is true.

---
## For Loop

```c
for (initialization; condition; last step) {
}
```

---
## While Loop

```c
while (condition) {
}

do {
} while (condition);
```

---
## Logic Operators

The logic operators are `&` (AND), `|` (OR), `^`(EOR) and `~` (NOT). All of these are _bitwise operators._

| A     |     | B     |     | A&B     |     | A\|B      |     | A^B     |     | ~A      |
| ----- | --- | ----- | --- | ------- | --- | --------- | --- | ------- | --- | ------- | 
| 0     |     | 0     |     | 0       |     | 0         |     | 0       |     | 1       |
| 0     |     | 1     |     | 0       |     | 1         |     | 1       |     | 0       |
| 1     |     | 0     |     | 0       |     | 1         |     | 1       |     | -       |
| 1     |     | 1     |     | 1       |     | 1         |     | 0       |     | -       |

- **Mask:** use _AND_ and _1s_ to select only some bits of a number;
  - `a & 0x03` &rarr; select bits 1 and 0.
- **Clear:** use _AND_ and _0s_ to clear unwanted bits;
  - `a & (~0x08)` &rarr; clear bit 3.
- **Set:** use _OR_ and _1s_ to set bits (to 1);
  - `a | 0x08` &rarr; set bit 3 to 1.
- **Toggle:** use _EOR_ and _1s_ to change the value of a bit;
  - `a ^ 0x08` &rarr; change the value of bit 3, independently of its current value.

