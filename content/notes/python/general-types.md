---
title: Data Types
linktitle: Data Types
toc: true
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Basic Concepts
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## Lists

```py
l = [100, 21, 88, 3]
l = list() # empty list

# List Comprehension -> quick list creation
evens_to_50 = [i for i in range(51) if i % 2 == 0]
other_list = [[i,j] for i in range(x) for j in range(y) if (x+y)!=n]

# Element selection
l[:2]   # first two items (0 and 1)
l[3:]   # third through last items
l[::2]  # first through end with a step of 2
## in general [start(inclusive):end(exclusive):stride]
## negative stride progress the list in reverse order

# Multi-index lists
l = [0, 1, 2, 3;
     4, 5, 6, 7]
l[1][2] # 6
```

- `l1 + l2` &rarr; **concatenate** lists.
- `len(l)` &rarr; **length** of the list.
- `min(l)`, `max(l)` &rarr; return **min** or **max** value of the list.
- `l.append(item)` &rarr; **add** _item_ at the end of the list.
- `l.insert(i, value)` &rarr; **insert** _new value_ in position _i_.
- `l.pop(index)` &rarr; **remove** item at _index_ and return it.
- `l.sort()` &rarr; **sort** in ascending order.
- `l.reverse()` &rarr; **reverse** the order of the list.
- `l.remove(x)` &rarr; remove the _first occurrence_ of x in the list.
- `item in l` &rarr; return a boolean if _item_ **is found** in _list_.
- `l.index(value)` &rarr; return **first index** in which _value_ is found.
- `l.clear()` &rarr; remove all the items of the list.
- `l.copy()` &rarr; create a copy of the list.
- `l.extend(l2)` &rarr; append all the elements of _l2_ at the end of _l_.

---
## Strings

```py
s = "Ciao!"

# Print a variable directly in a string through formatting
print(f'This {var_name} is formatted')

# Operations with string
l = "ciao"
l1 = l + l # is "ciaociao"
l2 = l * 2 # is again "ciaociao"
```

Strings are **immutable:** the value of a single character cannot be changed like in lists.

- `len(string)` &rarr; **lenght** of the string.
- `"substring" in s` &rarr; return _True_ if the substring is contained in the string.
- `s.lower()` &rarr; return the string in lowercase.
- `s.strip()` &rarr; remove white spaces both on the left and on the right of the string. Similar behavior for `rstrip()` and `lstrip()`.
- `" ".join(["A","B","C","D"])` &rarr; **combine list elements** into the string "A B C D".
- `s.find("substring", start, stop)` &rarr; **search** substring in string and return the lowest index where it is found. Return -1 instead.
- `s.startswith("something")` &rarr; check if _string_ **starts** with "something".
- `s.endswith("something")`&rarr; check if _string_ **ends** with "something".
- `s.split('delimiter')` &rarr; **split** a string using _delimiter_. Return a list.
- `s.replace('e','z')` &rarr; **replace** all "e" with "z"
- `'string {0} something {1}'.format(34,'something_else')` &rarr; **format the string** replacing `{ }` with arguments ([doc](https://docs.python.org/3.1/library/stdtypes.html?highlight=format#str.format)).
  - One can also use formatting expression to print and align variables
- `zip(a,b,...)` &rarr; make an **iterator** that aggregates elements from each of the iterables ([doc](https://docs.python.org/3.3/library/functions.html?highlight=zip#zip)).
- `'string'.encode('utf-8')` &rarr; return a bytearray **encoded as ASCII** characters.
- `b'bytearray'.decode('utf-8')` &rarr; translate the bytearray interpreting its bytes as ASCII characters. Return a string.
- `s.isnumeric()` &rarr; return _True_ if the string is made only by numbers.
- `"a".isalpha()` &rarr; return _True_ if the character is a letter.

---
## Dictionaries

```py
d = { "CA":"Canada",
      "GB":"Great Britain",
      "IN":"India",
      "key": value}

# Merge two dicts
z = {**x, **y}
```

- `d["key"]` &rarr; return the value associated with the key "key"
- `len(d)` &rarr; return the number of items in the dictionary.
- `d.keys()` &rarr; return a _dict\_keys_ object containing all the keys of the dict.
  - `sorted(d.keys())` &rarr; return the keys of the dict sorted alphabetically
- `d.values()` &rarr; return a sequence containing the values.
- `d.items()` &rarr; return a sequence of _(key, value)_ pairs
- `d.get("AU", "Sorry")` &rarr; return the value from the dictionary d that has the key "AU", or the string "Sorry" if the key "AU" is not found in d
- `"key" in d` &rarr; checks if _key_ exists in _d_
- `d.update(d2)` &rarr; update the dictionary with the items coming from _d2_. Existing entries will be replaced; new entries will be added.
- `d.clear()` &rarr; remove all the items of the dictionary.

---
## Tuples (sistema)

([doc](https://docs.python.org/3.3/tutorial/datastructures.html#tuples-and-sequences)) Values divided by commas, like

```py
t = 12345, 54321, 'hello!'
u = t, (1, 2, 3, 4, 5)      # nested tuples
empty = ()                  # empty tuple
single = ('hello',)         # 1-element tuple

x, y, z = t                     # unpacking tuples
a, b, *c = (1, 2, 3, 4, 5)      # a=1, b=2, c=[3,4,5]
a, b, *c, d = (1, 2, 3, 4, 5)   # a=2, b=2, c=[3,4], d=5
```

Tuples are **immutable** and they can contain an heterogeneous sequence of elements that are accessed via _unpacking_ or _indexing_.

They are usually adopted when the position in the sequence has a very precise meaning (like name, email address, ...).

---
## Other Types

### Sets
A set is an **unordered collection with no duplicate elements.** More on them is available in the [documentation](https://docs.python.org/3/tutorial/datastructures.html#sets).

---
## Resources
- Data types python [documentation](https://docs.python.org/3/tutorial/datastructures.html)
