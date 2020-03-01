---
title: Regular Expressions
linktitle: Regular Expressions
toc: true
type: docs
date: "2020-02-15T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Basic Concepts
    weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

```py
import re
```

{{% alert note %}}
**Regular expressions** (also known as _regex_ or _regexp_) are search queries for text that are expressed by string patterns. They allow searching for strings matching a specific pattern.
{{% /alert %}}

```py
# ALWAYS use raw strings for regular expressions in python
regex = r”\[(\d+)\]”
``` 

- `re.search(regex, string)` &rarr; return a _Match_ object with **only the first pattern found.** ([doc](https://docs.python.org/3/library/re.html#re.search))
  - Add the `re.IGNORECASE` parameter to ignore case in characters.
- `re.findall(regex, string)` &rarr; return all found patterns. ([doc](https://docs.python.org/3/library/re.html#re.findall))
- `re.split(regex, string)` take a regex as parameter and uses it to split a string. _Return a list with all the splitted parts._ ([doc](https://docs.python.org/3/library/re.html#re.split))
- `re.sub(regex, replacement, string)` &rarr; return a string in which a part of it (found using regex) is replaced. ([doc](https://docs.python.org/3/library/re.html#re.split))

---
## Special Characters

- `.` &rarr; matches **any character** except a newline.
- `^` &rarr; matches the **start** of the string.
- `&` &rarr; matches the **end** of the string.
- `*` &rarr; (used after a special character or group) tells to search for whichever number of characters (_repeated matches_), e.g.
  - `Py.*n` can match both `Python` and `Pygmalion`.
  - **WARNING:** while searching, it tries to replace as many characters as possible (_greedy_).
- `re_1|re_2` &rarr; matches both defined regex.

Python regex implementation accepts also two more qualifiers:

- `+` &rarr; matches **one or more occurrences** of the character that come before.
- `?` &rarr; matches **0 or 1 occurrence** of the character before it.

---
## Character Classes

_Character classes_ are indicated in parenthesis and tell to **search for all characters included.**

- `re.search(r”[Pp]yhton”, words)` &rarr; search for both uppercase and lowercase `p`.
- `re.search(r”[a-z]iao”, words)` &rarr; search for whatever character between `a` and `z`.
- `[a-zA-Z0-9]` &rarr; combine ranges.
- `[^a-z]` &rarr; search for all characters that **are not** in a group.

---
## Escape Characters

_Escape character_ `\` tells that the following character should be interpreted as is and not as a special character. 

- `\w` &rarr; matches letters, numbers and underscores (not spaces).
- `\s` &rarr; matches white spaces characters (`\t`, `\n` and space).
- `\d` &rarr; matches digits.
- `\b` &rarr; matches word boundaries.
  - It can be used to determine the start and end of the word, e.g. `\b[a-zA-Z]{5}\b` matches word of _exactly_ 5 letters.


{{% alert warning %}}
**WARNING:** escape characters can matches also special string characters like `\n`. Using raw strings, those are not interpreted.
{{% /alert %}}

---
## Repetition Quantifiers

Curly brackets are used to tell how many repetitions there should be when using special characters or groups: e.g. `[a-zA-Z]{5}` matches words of 5 characters.

- `[a-zA-Z]{5,10}` &rarr; mathces word of length from 5 to 10.
  - `{,n}` and `{n,}` are also valid.

---
## Capturing Groups

_Capturing groups_ are portions of the pattern that are enclosed in round parentheses. When found, the words detected with these patterns can be called using `\n`, where `n` is a natural number and it determines the order in which matches where found.

- `Match_obj.groups()` &rarr; see on the documentation.

---
## Resources

- `re` module [documentation](https://docs.python.org/3/library/re.html)
  - More on [special characters](https://docs.python.org/3/library/re.html#regular-expression-syntax)
  - Tutorial on [how to use regular expressions in python](https://docs.python.org/3/howto/regex.html)
- Difference between [greedy and non-greedy](https://docs.python.org/3/howto/regex.html#greedy-versus-non-greedy)
- [Regex 101](https://regex101.com/) - web tool for exploring regex