---
title: Git Basic Workflow
linktitle: General
toc: true
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

## Definitions

- **Working tree** &rarr; the area outside the `.git` directory. It contains the current state of the project (including any changes that one has made).
- **Staging area** &rarr; it is a file maintained by git that contains all the changes that have been marked to be included in the next commit. Each file can be in one of three different status: _modified_, _staged_ or _committed_.
- **Commit** &rarr; snapshot of the current status of the project.
- **HEAD** &rarr; it represents tht currently checked-out snapshot of the project. One can think to it as a pointer to the current branch.
- `.gitignore` &rarr; it is a file containing a list of files or filename patterns for Git to skip for the current repo. ([doc](https://git-scm.com/docs/gitignore)).
- **Branch** &rarr; it represents and independent line of development in a project. The default branch that git creates when the project is initialized is called _master._

---
## Configuration

- `git config -l` &rarr; check current git configuration.
- `git config --global user.name “email”` &rarr; configure default email for each commit.
- `git config --global user.email “name”` &rarr; configure default user name ffor each commit.
- `git config --global alias.<alias-name> <git-command>` &rarr; create shortcut for a git command.
  - e.g. `git config --global alias.ciao commit -m` set `git ciao` to `git commit -m`

The `--global` argument states to set this configurations as default for each directory in the systems. One can change those parameters for a single project if needed (just by omitting the `--global` argument).

---
## Basic Commands

- `git init` &rarr; initialize an empty git repository.
- `git status` &rarr; return information about the current working tree and pending changes.

### Exploring Project Hisotry
- `git log` &rarr; return informations on the last few commits.
  - `git log -<limit>` &rarr; limit the number of commits to the specified number.
  - `git log --stat` &rarr; return informations on the last few commits adding more statistics (how many files and lines where changed).
	- `git log -p` &rarr; (`p` stands for patch) return informations on the last few commits and all the changes made in each committed file.
	- `git log -p branch_name` &rarr; return informations on the changes between the current working tree and the last commit of the provided branch.
	- `git log --graph —oneline --all` &rarr; print only the first line of the commit messages and plot the log to be similar to a git graph, for better visual explanation of the git history. Use `--all` if the complete history is needed.
- `git diff` &rarr; show differences between the unstaged files in the current working tree and the HEAD commit. It can be useful to explore when adding big amount of code in a single commit, to remember exactly what was changed.
  - `git diff <file>` &rarr; show the differences concerning only the provided file.
- `git show commit_ID` &rarr; show differences between current working tree and the commit provided.

### Committing

- `git add <file_name>` &rarr; add the file to the staging area.
  - `git add .` &rarr; stage all files
- `git rm file_name` &rarr; the file is removed from the directory and the change is automatically staged for the future commit.
  - **NOTE:** it is best practice explaining the reason why files are removed inside the commit messages.
- `git mv file_name new_file_name` &rarr; rename a file and stage the change automatically. Can also be used for moving files in subdirectories using relative paths.
- `git commit` &rarr; commit the files added in the staging area. It opens an editor in which one can insert a detailed commit message.
	- `git commit -m "message"` &rarr; create a commit with a short message (it doesn't open the deafult git editor).
	- `git commit -a` &rarr; commit all the tracked files without separately adding them to the staging area. **NOTE:** _untracked (new) files are not committed in this way, one needs to add them using the `git add` command._

### Anatomy of a Commit Message

Keep the audience of the message in mind. Moreover, each company/project can have different rules on how to write commit message and one should stick to them.

In general, try to stick to the following

1. 1st line with (generally) 50 characters or less containing a short description of the changes.
2. Empty line.
3. More detailed explanation of the changes divided in paragraph.
  - Try to keep each line under 72 characters in lenght (suggested because the command `git log` does not do any line wrapping).
  - This explanation can reference bugs or issues that were corrected, or include links to more informations when relevant.

---
## Resources

- [Linux rules for commit messages](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/process/submitting-patches.rst?id=HEAD)
- [More advice for commit messages](https://thoughtbot.com/blog/5-useful-tips-for-a-better-commit-message)
- [Setting the commit email address](https://help.github.com/en/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address)
- [Examples](https://gist.github.com/octocat/9257657) of file patterns that should be included in `.gitignore` files.
