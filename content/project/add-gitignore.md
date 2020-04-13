---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Add Gitignore"
subtitle: "Automating _.gitignore_ file creation using Python."
summary: "The creation of _.gitignore_ file is quite boring and error-prone. I tried to automate the creation process making a simple Python script that can be run from the command line."
authors: []
tags: ["Automation"]
categories: ["Automation", "Python"]
date: 2020-04-14T12:00:00+02:00
lastmod: 2020-04-14T12:00:00+02:00

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
links:
- name: Github
  url: https://github.com/damianolodi/add-gitignore
  icon_pack: fab
  icon: github

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

## Introduction
Creating _.gitignore_ files is for sure one of the most boring things of starting a new project. Even though using version control is very important, the types of file that one should remember to place inside is huge. In fact, they can depend from lots of different variables, like:

- your specific development  environment;
- the OS on which you are working on;
- the OS used by other project collaborators (if any);
- the technology or software you are working with.

This task seems perfect to be automated so I made a simple command-line script. To be fair, a very mature project of this kind already exists. In fact, with [gitignore.io](http://gitignore.io/) you can create _.gitignore_ files from a web interface or even using the command-line. Unfortunately, even when used locally it requires an internet connection to work.
### Goals
The script that I created meet the following characteristics:

- it works locally even without an internet connection;
- it creates or update the _.gitignore_ file in the path provided;
- the created _.gitignore_ file is customisable based on the technologies used in the project;
- the user can add support or customise the output of the program. 

### Prerequisites
To understand the basic working principle of this project you should have a basic knowledge of [Python](https://www.python.org/). You should also know something about [Git](https://git-scm.com/), but I suppose you do (otherwise why the hell you know what a _.gitignore_ file is??)

---
## Design Rationale
The structure of the project is basic:

- one file containing the main program and all the necessary functions;
- one file containing the various user templates.

### Structure of the Template File
I could go with two different approaches while designing the working principle:

1. copy-pasting pre-made files from a directory to another;
2. creating the file based on the value of some pre-loaded variables.

In principle, the user will install the program wherever he wants on his system. Since the program can be called from the command line in whichever path, I decided to go with the second option. The main downside, in this case, is that every time the program is called it will load the `templates.py` file and, if the number of templates is large, this is for sure slower than copy-pasting files. Luckily, the number of templates used from a single user is probably limited and, due to the power of a modern computer, the difference in performance isn’t noticeable.

The structure of the `templates.py` file is very simple: it is a _list of multi-line strings,_ each containing a single template. At the end of the file, each string is placed inside a dictionary for easy accessibility from the main program.

### Structure of the Main File
The main file is `add-gitignore.py`, since this is the name that I want to type on the command line to run it.

The main program is called under the `if __name__ == “__main__”:` condition. In this way, the code run only when the script is called directly and not when the module is imported in other programs. This script is of course not designed for that but, since this is a common best practice, I decided to stick with it.

The script expects some arguments when called:

- the path in which the _.gitignore_ file should be created [**mandatory**]
- a list of all the templates (besides the main one) that should be added to the file [**optional**]

I access to the arguments passed using `sys.argv` and I start to parse the list.

#### Check the Path
First I check if the _project path_ was passed. Since it is logical to create the _.gitignore_ file right after creating a _Git_ project, I provided the possibility to pass the flag `-h` to tell the program that the file should be created in the directory from which the script was called. If instead a _path_ is passed, I check if it exists before proceeding.

#### Check _.gitignore_ Existence 
If the _path_ exists, I check if a _.gitignore_ file already exists inside it. In that case, I print a warning telling the user that the content will be appended to the existing file.

{{% alert note %}}
A way to improve the project is by asking the user if he still wants to continue.
{{% /alert %}}

Then, I create/update the _.gitignore_ file with the content of the _main_ template.

#### Add Further Templates
Finally, for each parameter provided beside the path, the program searches the imported `programs` dictionary for any keys matching the value passed and append the associated multi-line string to the file recently created. If a matching key is not found, a warning message is printed to the user.

During this step is important to find a way to avoid Python to raise errors if a key is not found in the dictionary. This will prevent the program from stopping if many templates are passed.

{{% alert note %}}
Looking back to the way I implemented it, it is not very “pythonic”. It is more “c-like”, if you know the C language. This implementation can be improved.
{{% /alert %}}

---
## Conclusion
All-in-all, automating the _.gitignore_ creation process was a fun way to improve Python skill. In fact, this forced me to apply common actions like reading/writing from a file, searching in lists or dictionaries and managing errors. Also, **writing about the design rationale highlighted some features that can be better implemented.**

If you want a way to improve your Python skills, I would suggest finding a way to replicate the work of this script. Then, after completing it, compare the code of the two solutions to see differences in the implementations (and learn from them).

If instead, you just want to download and use the script, [the code is available on Github](https://github.com/damianolodi/add-gitignore) and is published under the MIT license. You can find detailed instructions for usage and installation on the attached README.
