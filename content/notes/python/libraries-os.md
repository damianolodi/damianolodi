---
title: Os and Sys
linktitle: Os and Sys
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Packages
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

## The `os` Package

```py
import os
```

- `os.path.exists(path)` &rarr; return _True_ if the path exist.
- `os.path.abspath(relative_path)` &rarr; return the _absolute path._
- `os.path.join(path1, path2, ...)` &rarr; create a path joining 2 or more path objects.
- `os.path.getsize(path)` &rarr; return the **size** of the path, in bytes.
- `os.path.getmtime(path)` &rarr; return the **timestamp of last modification** of the path.
    - The **timestamp** is the time passed from _Jan 1st, 1970._
    - `datetime.datetime.fromtimestamp(timestamp)` &rarr; return date starting from the timestamp.

### Directories
- `os.getcwd()` &rarr; return the **current working directory.**
- `os.chdir(path)` &rarr; change the cwd to _path_.
- `os.mkdir(path)` &rarr; **create a directory**. It can return _FileExistsError_ if the directory already exists.
- `os.rmdir(path)` &rarr; **remove** the directory _only if empty._
- `os.path.isdir(path)` &rarr; check wether the path is a directory.
- `os.listdir(path)` &rarr; **list all file** in the path. _It is better using `scandir()` for increased performance._
- `os.scandir(path)` &rarr; **scan all files in the path** and create an `os.DirEntry` object containing all files with their information ([doc](https://docs.python.org/3/library/os.html#os.DirEntry)).

### Files
- `os.remove(path)` &rarr; **remove** a file. _Return an error if the file does not exist._
- `os.rename(current-path, destination-path)` &rarr; **move** or rename a file. _Return an error if the file does not exist._
- `os.system("command")` &rarr; execute the provided command in the shell ([doc](https://docs.python.org/3.8/library/os.html#os.system)).

---
## The `sys` Package

- `sys.version` &rarr; contains python version installed on the machine.
- `sys.executable` &rarr; contains information about the interpreter used.
- `sys.platform` &rarr; contains information about the OS that is executing the code ([doc](https://docs.python.org/3.8/library/sys.html#sys.platform)).
- `sys.path.append("path")` &rarr; add this to the paths in which libraries are searched when importing.

---
## Resources

- `os` module [documentation](https://docs.python.org/3/library/os.html)
- `os.path` module [documentation](https://docs.python.org/3/library/os.path.html)

