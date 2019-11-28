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

- `os.getcwd()` &rarr; return the **current working directory.**
- `os.listdir(path)` &rarr; **list all file** in the path. It is better using `scandir()` for more performance.
- `os.scandir(path)` &rarr; **scan all files in the path** and create an `os.DirEntry` object containing all files with their information ([doc](https://docs.python.org/3/library/os.html#os.DirEntry)).
- `os.chdir(path)` &rarr; **change the cwd** to path.
- `os.remove(path)` &rarr; **remove** a file.
- `os.mkdir(path)` &rarr; **create a directory**. To avoid errors if the directory exist, use:
```py
    dirName = 'tempDir'
    try:
        os.mkdir(dirName)   # Create target Directory
        print("Directory " , dirName ,  " Created ")
    except FileExistsError:
        print("Directory " , dirName ,  " already exists")
```

- `os.rename(current-path, destination-path)` &rarr; **move** or rename a file.
- `os.path.exists(path)` &rarr; return _True_ if the path exist.
- `os.system("command")` &rarr; execute the provided command in the shell ([doc](https://docs.python.org/3.8/library/os.html#os.system)).

---
## The `sys` Package

- `sys.version` &rarr; contains python version installed on the machine.
- `sys.executable` &rarr; contains information about the interpreter used.
- `sys.platform` &rarr; contains information about the OS that is executing the code ([doc](https://docs.python.org/3.8/library/sys.html#sys.platform)).
- `sys.path.append("path")` &rarr; add this to the paths in which libraries are searched when importing.
