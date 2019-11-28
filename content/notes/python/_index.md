---
# Course title, summary, and position.
linktitle: Python
summary: Personal python reference notes, OOP concepts and scripts
weight: 1

# Page metadata.
title: Overview
date: "2019-11-27T00:00:00Z"
lastmod: "2018-11-27T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  python:
    name: Overview
    weight: 1
---


## Virtual Environments

The point of virtual environments is to have containers with packages that are useful for a particular project, so that different projects can use different version of the same package.

### `venv` Built-in Module

`python3 -m venv env_name` &rarr; **create** a new environment (it is a directory) in the current path. When run with _-m_, the `python` command search for a package called as the first argument and run it as _main._

- One convention is to _create the project folder_ and create inside a new virtual environment called _venv._
- **Don't commit the _virtual environment_ to source control**, so add it to the _.gitignore_ file.

`source env_name/bin/activate` &rarr; **activate** the environment. If now `which pyhton` is called, it points inside the virtual environment directory.

- To activate on Windows, use `env_name/bin/activate.bat`

- Once the environment is created and activared, packages are installed only for that environment.

`pip freeze > requirements.txt` &rarr; redirect the output of `pip freeze` (list of packages and versions) to a _.txt_ file that can be used to recreate the environment from someone else using `pip install -r requirements.txt`

`deactivate` &rarr; deactivate the virtual environment

- To delete the environment, remove the directory which contains it

_`venv` create the environments with the version of pyhton used to run it and cannot create one with different versions. Use other packages if necessary_

### `virtualenv` Module

1. Install `virtualenv`
- `cd` into the project folder
- `conda create --name project-env-name dependecy1 dependency2 ...` &rarr; **create** the environment
- `source activate project-env-name` &rarr; **activate** the environment
- `source deactivate project-env-name` &rarr; **deactivate** the environment
- `conda env export > environment.yaml` &rarr; **export** the environment
  - keep track of all the dependencies
  - have the possibility to reproduce the environment if something goes wrong
- `conda env create -f environment.yaml` &rarr; create a virtual environment from a _.yaml_ file
- `conda env list` &rarr; list all the environments on the machine with their location

---
## Errors Handling

```py
try:
    <code>
except ErrorName1:      # catch specific error
    <code>
except ErrorName2 as e:
    print(e)            # print the error
except Exception as e:  # catch all the general exception
    <code>
else:
    <code>              # run if no exception is raised
finally:
    <code>              # run no matter what happen
```

Always start with more specific exceptions and finish with the more general excetpions, otherwise specific ones will never be cought

`else` is useful to separate commands so that after `try` there is only the code that can raise the exception one wants to catch

`finally` can be useful for example to free reosurces that were previously used

One can also **raise his own exception** manually

```py
try:
    <code>
    if condition:
        raise ErrorName
except ErrorName:
    <code>
```

---
## Special Variables

When a *file_name.py* file is run, Python assign values to some particular variables. One of this is `__name__` which can have two values:

- `"__main__"` (string) if the file is run directly
- `"file_name"` (string) when it is imported with `import`

This is the reason why one usually structure the file in the following way:

```py
<code that will always be run (classes definition,...)>

def main():
    <code run only when the program is called directly>

if __name__ = "__main__":
    main()
else:
    <code run only when imported>
```

---
## VS Code Setup

### Shortcuts
-   `cmd + shift + P` &rarr; open command palette
-   Python interpreter used (version) is displayed in the botton left corner. Can be changed clicking on that
-   When modify settings in json, on the left of each line there is an icon that suggest all the possible value for each parameter

### Extensions

Python, vscode-icon

### Settings

-   **Color themes** &rarr; default dark theme is ok.
-   **File icons** &rarr; _vscode-icon_ or _material icon theme._
-   Open user settings and place the following code:

```json
"workbench.settings.editor": "json",
"workbench.settings.openDefaultSettings": true,
"workbench.settings.useSplitJSON": true,
"editor.fontSize": 16,
"debug.console.fontSize": 16,
"terminal.integrated.fontSize": 16,
"python.formatting.provider": "black",
"editor.formatOnSave": true
```

- Then re-open the settings so that _global settings_ and _user settings_ are side by side.
- Place `"python.pythonPath": "python_path"` where *python_path* is the path returned by the command `which python3` in Termianl. This will change the defaul python interpreter.
- For each project, one can create a virtual env. and select as the interpreter. VS Code will use it as the default env. for the project when run in the integrated termianl

---
## Resources

- [Virtual environments with venv](https://youtu.be/Kg1Yvry_Ydk) - Corey Shafer.
- [Errors Handling](https://youtu.be/NIWwJbo-9_8) - Corey Shafer.
- [VS Code Python Setup on Mac](https://youtu.be/06I63_p-2A4) - Corey Schafer
- [Jupyter Notebook lessons](https://bitbucket.org/hrojas/learn-pandas) on Pandas.
- [Jupyter notebook tutorial](http://nbviewer.jupyter.org/github/jrjohansson/scientific-python-lectures/blob/master/Lecture-2-Numpy.ipynb) on Numpy.
- [Matplotlib documentation](https://matplotlib.org/).
- [Matplotlib Tutorials](https://www.youtube.com/playlist?list=PL-osiE80TeTvipOqomVEeZ1HRrcEvtZB_) - Corey Shafer.
- [Jupyter Notebook tutorial](http://nbviewer.jupyter.org/github/jrjohansson/scientific-python-lectures/blob/master/Lecture-4-Matplotlib.ipynb) on Matplotlib.