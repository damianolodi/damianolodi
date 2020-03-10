---
title: Working with Remotes and Collaboration
linktitle: Remotes
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

## Remote Repositories

- `git clone url` &rarr; copy a remote repository locally. Git automatically assign the `origin` name to the remote repositoy when `git clone` is called.
- `git remote -v` &rarr; look at the configuration of the remotes.
	- `git remote show origin` &rarr; print much more information about the provided remote.
  - `git remote update` &rarr; update the content of the remote branches without merging any content into the local branches ([doc](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-emupdateem)).
  - `git remote add <name> <url>` &rarr; create a new connection to a remote repository. _After adding a remote, you can use `<name>` as a shortcut for `<url>` in other commands._
- `git branch -r` &rarr; print information on **remote branches.** _Remote branches_ are used by git to track changes made by other people on the remote direcotry. Those branches are _read-only_ and are used (updated) when fetching and merging before pushing the local changes to the remote.
- `git fetch` &rarr; update _remote branches_ on the local machine with the changes made on the remote direcotry ([doc](https://git-scm.com/docs/git-fetch)).
	- **NOTE:** if new branches are created in the remote, they are fetched in remote branches but not added to the local branches. They are automatically added when `git checkout branch-name` is called for the first time.
- `git log origin/branch-name` &rarr; return the commit history of the _remote branch_ provided.
- `git merge origin/branch-name` &rarr; try to merge the _remote branch_ on the active branch.
- `git pull` &rarr; git fetches the changes made on the remote and automatically tries to merge them on the current local branch.
  - `git pull remote-name` &rarr; git fetches the changes from a particular remote (if more than one remote is present.
- `git push -u origin branch_name` &rarr; bush the branch to the remote directory. Add `-u` the first time the branch is pushed to create the branch upstream (i.e. to create the remote repository).

### Git workflows when remotes are involved

Change &rarr; stage &rarr; commit &rarr; fetch (changes made by other on the remote) &rarr; merge &rarr; resolve conflicts (if necessary) &rarr; push

---
## Collaboration

When a user is making changes to a project to which he has no access to, GitHub will create a fork. 

{{% alert note %}}
A **fork** is a way to create a copy of the given repository so that it belongs to another user. 

A **pull request** is a commit (or a series of commits) that is sent to the owner of the original repository so that he can incorporate them into the original tree.
{{% /alert %}}

The common workflow when collaborating in external project is:
- make a fork of the project;
- apply changes locally;
- then make a pull request to incorporate the changes of the fork in the original repo.

---
## Best Practices for Collaboration

- Always synchronise branches before start working on new code, to minimize conflict possibilities.
- Try to avoid having very large changes that modify lot of different things.
- When working on a big change, it makes sense to have a separate feature branch.
- Regularly merge changes made on the master branch back onto the feature branch.
- **Make good commit messages.**
- If more than a versione of the project is mainatained:
  - keep the latest versione of the project in the master branch, and the stable versione of the project on a separate branch.
  - **Do not rebase changes that have been pushed to remote repos!**

---
## Resources

- [Caching the GitHub password in git](https://help.github.com/en/github/using-git/caching-your-github-password-in-git)
- [Connecting to GitHub with ssh](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
- [About merge conflicts](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-merge-conflicts)
- [Resolving a merge conflict using the command line](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-using-the-command-line)
- [Rebase explained on git website](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
