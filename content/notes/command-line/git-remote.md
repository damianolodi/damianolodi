---
title: Remote Repositories
linktitle: Remote
toc: false
type: docs
date: "2019-11-24T00:00:00+01:00"
draft: false
menu:
  command-line:
    parent: Git
    weight: 4

diagram: true

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Linking to Github

1. `git init` &rarr; initialise the project
- `git add .` &rarr; add everything to the staging area
- `git commit -m "First commit"` &rarr; commit the files in the staging area
- Go to [Github](https://github.com/), create a new repository and copy the provided link
- `git remote add origin <link>` &rarr; link the two repositories
- `git push -u origin master` &rarr; push the master branch to the remote repository

---
## Remote Repositories

`git clone <repository_path> <clone_name>` &rarr; **clone** the provided repository in the current path. _Original repo can be located on the local filesystem or on a remote machine via HTTP or SSH_

`git remote add <name> <url>` &rarr; create a new connection to a remote repository. _After adding a remote, you can use `<name>` as a shortcut for `<url>` in other commands._
  
  - `git remote -v` &rarr; lists the name of the remote, origin, as well as its location

`git pull <remote>` &rarr; fetch the specified remotes copy of the current branch and immediately merge it into the local copy

`git push <remote> <branch>` &rarr; push the branch to the remote repo. creates the named branch in the remote repo if it doesn’t exist

  - `git push <remote> --all` &rarr; push all the local branches

`git fetch <remote> <branch>` &rarr; fetches a specific branch from the repo. Leave off `<branch>` to fetch all remote refs.

  - `git fetch` &rarr; This command will not merge changes from the remote into your local repository. It brings those changes onto what’s called a remote branch.
  - To explain this we can say that even though Sally’s new commits have been fetched to your local copy of the Git project, those commits are on the origin/master branch. Your local master branch has not been updated yet, so you can’t view or make changes to any of the work she has added.
  -   One can merge those changes in the local master branch by using the merge command `git merge origin/master`