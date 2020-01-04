---
title: Git Basic Workflow
linktitle: General
toc: false
type: docs
date: "2019-11-24T00:00:00+01:00"
draft: false
menu:
  command-line:
    parent: Git
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

`git init` &rarr; create an empty git repository in the current directory

`git add <file/dir>` &rarr; stage file or direcotry for the next commit

- `git add .` &rarr; stage all files

`git commit -m “<message>”` commit the staged changes with the message provided. Messages must be:

- written in the present tense;
- be brief (50 characters or less) when using -m

`git status` &rarr; list staged, unstaged and untracked files

`git diff` &rarr; show unstated changes between the index and the working directory

- `git diff <file>` &rarr; preview the differences between the file in the working directory and the one in the staging area

`git log` &rarr; display the entire commit history. _The 40-character code, called **SHA**, uniquely identifies the commit_.

- `git log -<limit>` &rarr; limit the number of commits to the specified number
- `git log --oneline` &rarr; condense each commit to a single line
- `git log --stat` &rarr; returns some statistics about which file has changed in each commit

`git revert <commit>` &rarr; create a new commit that undoes all of the changes made in `<commit>`, then applies it to the current branch

`git reset <file>` &rarr; **remove the file from the staging area**, but leave the working directory unchanged

---
## Git Configuration

`git config --global user.name <name>` &rarr; **define the author name** to be used for all commits

  - `git config user.name <name>` &rarr; define the author name _used in the current repo_

`git config --global user.email <email>` &rarr; **define user email**

`git config --global alias.<alias-name> <git-command>` &rarr; create shortcut for a git command

  - e.g. `git config --global alias.ciao commit -m` set `git ciao` to `git commit -m`

`git config --global color.ui auto` &rarr; activate the colour code while using the diff command
