---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "How to Create and Execute a Command Line Script with Python on macOS (and Linux)"
subtitle: "Making automation even better."
summary: "Python can be a very powerful replacement of Bash to write scripts that can used inside the command-line. Unfortunately, for the Python beginner, some non-obvious steps are required to transform a “normal” program into an executable script. This post will explain exactly how to execute a Python script from the command line explaining each necessary step."
authors: []
tags: []
categories: ["Python", "Automation"]
date: 2020-05-09T12:00:00+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Introduction

Let’s imagine that you just completed your first script to automate a super boring piece of your workflow. Now it’s time to use it! Very simple: call `python3 relative/path/to/my/scrpt.py` and you are done... but there are some downsides with this:

1. the command to type is very long;
2. also, based on where you are calling your script and where your script is saved, the relative path you have to type can change every time. When you have 1 script it is workable to remember it, but what about when you will have 100?

I bet that with such downsides you will use your script (at most) 2 times and then you will fall back to do everything manually.

Luckily for you, following this guide you will understand how to set up your script so that you can call it from your terminal typing just `my_script.py`. This will be independent of where you are on your system (exactly how you call all the other bash program that you already use!).

## Prerequisites

Before you begin this guide you’ll need the following:

- you need **Python 3** installed. In principle, the concepts explained can be extended to different programming languages. Nonetheless, I will use Python as a demonstration during the article.
- **You need to have a working script** on which you can work along with the tutorial. If you don’t have one, you can create a file called `example_script.py` and type the following code in it:

  ```py
  print(“Hello world!”)
  ```

- You need **macOS,** but the same commands _can be used also on a Linux system_ since they are both based on Unix.
- You need to have some confidence with the terminal and the command line. You should understand the basic usage of the `cd` and `ls` commands and understand what is a _path._

---

## Step 1 - Saving the File

Save your script somewhere on your system and remember where you placed it. To generalise, I will suppose that your file is named `example_script.py` and the path to this file is `path/to/my/example_scritp.py`. Of course, you can substitute those value with whatever you want.

In principle, whichever place on your system is good but, thinking long-term when you will have lots of automation script, maybe its better if you dedicate a single place to collect all your script. Personally, I keep all my scripts in `~/Documents/projects/Automation/`, but you can decide the place that best suite your needs.

---

## Step 2 - Adding the Shebang

When you call your script using `python3 example_script.py` your are just telling your system the following:

> Take my file `example_script.py` and translate what I wrote inside it using the program called `python3`.

Then, your OS starts looking into all its system directories for a file called `python3` and, if he can find it, it will do what you have asked. Otherwise, an error is printed on your terminal.

The `python3` executable was placed on your system while you installed Python. You want to tell your system to use the `python3` file without explicitly writing it in the command line. To do this, **you can use a special character sequence that must be placed in the first line of the script.** So, in practice, our new file will look like this:

```py
#! path/to/the/python3/executable

print(“Hello world!”)
```

> The sequence `#!` is called ***shebang,*** and it is used by the system basically to understand in which language the program is written.

The problem now is that you need to know where the Python executable is on your system, but this can change based on how you installed it and which system you are using. Luckily for you, the command `which` can search it for you! Type

```bash
$ which python3
```

on your terminal and you will see where the Python executable is located on your system. In my case the terminal returns `/usr/local/anaconda3/bin/python3`. I am using macOS 10.15 and I installed Python through **Homebrew** and the **Anaconda distribution,** so keep in mind that this value can change for you if you are using a different system.

You have to place this value after the shebang in your script. In my case, I will have a file that will look like

```py
#! /usr/local/anaconda3/bin/python3

print(“Hello world!”)
```

---

## Step 3 - Adding Execution Permission to the Script

You are not done yet. _To execute a file, you need permission to do it!_ Using the terminal, navigate to where you saved your script and type `ls -l`. This command will print the list of all files inside the directory alogn with some details. Among all the lines, you should see something like this:

```bash
-rw-r--r--@ 1 user staff 58 Apr 26 16:01 example_script.py
```

where `user` is replaced by the name of the user on your computer. You are interested in the first 10 characters. Each character is telling you something about the file.

1. The first character is telling you if the element is a file (`-`), a directory (`d`) or a link (`l`).
2. Characters 2 through 4 are telling you the _file owner’s permissions._ In the order: read (`r`), write (`w`) and execute (`x`).
3. Characters 5 through 10 are telling the same thing, but for the group to which the files belong (characters 5 to 7) and for all the other users (characters 8 to 10). We are not interested in those in this tutorial.

As you can see, the 4th character is `-`, so it means that you cannot execute the file. You need to grant yourself permission to execute it. To do that you can type

```bash
$ chmod +x example_script.py
```

If you do this and type `ls -l` again you should find an output that looks like

```bash
-rwxr-xr-x@ 1 user staff 58 Apr 26 16:01 example_script.py
```

As you see, you now have the permission to execute the file!

To check that this is true, try to run the program typing

```bash
$ ./example_script.py
```

If you see the expected output (in the case of the example program, the phrase `Hello world!`) it means that everything worked correctly.

---

## Step 4 - Modify the PATH Variable

At this point, you need to solve one final problem: in fact, you don’t need to type `python3` anymore but you still need to type the relative path to the script. _To solve this problem you need to modify the `PATH` variable._

{{% callout note %}}
The `PATH` variable is a special variable used by your system to know in which directories it should search the programs that you want to execute.
{{% /callout %}}

In particular, you need to add the directory in which `example_script.py` is saved. So first of all open again your terminal and type `ls -al`. You should find two files called `.bash-profile` and `.bashrc`.

{{% callout warning %}}
If you are on **macOS Catalina** (or newer) the default shell that you are using is **Zsh**, so you will probably see `.zshrc` and `.zprofile` instead.
{{% /callout %}}

If you don’t see it, don’t worry, you will create it. In fact, in both cases, you should type

```bash
$ nano .bashrc
```

(or `nano .zshrc`). The `nano` text editor will open in your terminal, and you are ready to modify your `PATH` variable. Now type

```bash
export PATH=“$PATH:path/to/my”
```

(This means that you should type the path up until the directory that contains your script.) At this point close and save the file using `ctrl + X` and `Y`. Then, close and reopen the terminal.

{{% callout note %}}
The file `.bashrc` is called and executed from the terminal every time a new session is open.
{{% /callout %}}

The code that you wrote inside it is telling your terminal to update the `PATH` variable adding, at the end, a new directory. So, from now on, when you type a command on the terminal your system will search also inside that directory to find the program that you requested.

To test that everything is working, you can write

```bash
$ example_script.py
```

from wherever on your system and check if the output is the one that you expect.

{{% callout note %}}
Once you completed the script if you want to call the program without typing the `.py` file extension every time, rename the file and remove the extension.
{{% /callout %}}

## Conclusion

In this article you understood how to:

1. find where an executable is located on your system using `which`;
2. add a proper shebang to your script to suggest how it should be interpreted by your system;
3. add a directory to the `PATH` variable to tell your system where to search for your script.

Now you can call your automation scripts from every path on your system, great! I am sure that, from now on, you will write much more automation scripts!
