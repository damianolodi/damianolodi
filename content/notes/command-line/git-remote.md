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

- `git clone url` &rarr; copy a remote repository locally. Git automatically assigns the `origin` name to the remote repository when `git clone` is called.
- `git remote -v` &rarr; look at the configuration of the remotes.
	- `git remote show origin` &rarr; print much more information about the provided remote.
  - `git remote update` &rarr; update the content of the remote branches without merging any content into the local branches ([doc](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-emupdateem)).
  - `git remote add <name> <url>` &rarr; create a new connection to a remote repository. _After adding a remote, you can use `<name>` as a shortcut for `<url>` in other commands._
- `git branch -r` &rarr; print information on **remote branches.** _Remote branches_ are used by git to track changes made by other people on the remote directory. Those branches are _read-only_ and are used (updated) when fetching and merging before pushing the local changes to the remote.
- `git fetch` &rarr; update _remote branches_ on the local machine with the changes made on the remote directory ([doc](https://git-scm.com/docs/git-fetch)).
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

When a user is making changes to a project to which he has no access to, GitHub will create a fork. This is done because only a few people have commit rights to the repo, but everyone can suggest patches, bug fixes or even feature requests. The people with commit right can then accept or reject the pull request.

{{% alert note %}}
A **fork** is a way to create a copy of the given repository so that it belongs to another user. 

A **pull request** is a commit (or a series of commits) that is sent to the owner of the original repository so that he can incorporate them into the original tree.
{{% /alert %}}

- Fork the project on GitHub. The forked repo is assigned to the user, but GitHub keeps track of the fact that it was forked.
- Clone the forked project locally.
- Explore the log history.
- Create a new branch to add the new feature and apply changes.
- When ready, push the new branch to the remote (forked) repo, using the `-u` label.
- Check that everything is ok.
- Create a pull request on GitHub clicking on the "_Pull Request_" link.
	- If the branch can be automatically merged, GitHub will tell us that. Otherwise, one needs to rebase the changes against the current branch of the original repo, so that it can be merged.
- Complete the pull request adding information on testing and other relevant things.

### Best Practices for Collaboration

- Always synchronise branches before start working on new code, to minimize conflict possibilities.
- Try to avoid having very large changes that modify lots of different things.
- When working on a big change, it makes sense to have a separate feature branch.
- Regularly merge changes made on the master branch back onto the feature branch.
- **Make good commit messages.**
- If more than a version of the project is maintained:
  - keep the latest version of the project in the master branch, and the stable version of the project on a separate branch.
  - **Do not rebase changes that have been pushed to remote repos!**

### Other Notes on Collaboration

#### Updating an Existing Pull Request
It is common to receive comments from the maintainer of the project requesting more documentation, tests or information.

At this point, one should go back to the local repo and apply changes. Then commit and push the changes. The pull request will be automatically updated with the new commit because one is applying new changes to the same branch.

#### Squashing Changes
Sometimes it is required to submit only one commit for every pull request. To modify and rename all the commits in the branch use `git rebase -i branch_name`, where `-I` stands for “interactive”.

{{% alert note %}}
Even though a general rule is not changing history of public repositories, this time one can do it because he is the only person with access to the forked project.
{{% /alert %}}

When everything is done, use `git push -f` to force the push of the local repo.

#### Code Reviews
When comments about the code are made, they will appear in the pull request. To address them, apply changes locally. _Then use `git commit -a --ammend` to apply changes to the latest commit instead of creating a new one._ This will change the history (because the commit ID will be changed), so use the `-f` label when pushing.

From now previous comments will be tagged as outdate because the commit is technically different, but one can still close the conversation if the comments were resolved.

{{% alert note %}}
Comments that are just suggestions are marked as _nit_, but they should be taken into consideration too.
{{% /alert %}}

#### Tracking Issues
When projects are big, coordination in the collaboration is necessary.
The _Issue Tracker_ is a tool that allows monitoring the state of the projects and the bugs that must be solved, along who is working on them. GitHub has this tool built-in.

_Issues and bugs can also be added by external users._

Each issue has an ID that can be referenced using the `#` in comments and pull requests. It is also possible to automatically close the issue when pull requests are made. The issue must be referenced in the pull request or in the commit message.

---
## Resources

- [Caching the GitHub password in git](https://help.github.com/en/github/using-git/caching-your-github-password-in-git)
- [Connecting to GitHub with ssh](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
- [About merge conflicts](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-merge-conflicts)
- [Resolving a merge conflict using the command line](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-using-the-command-line)
- [Rebase explained on git website](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [About pull request merges](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-request-merges)
- [Google Style Guide](http://google.github.io/styleguide/)
- [About pull requests review](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-request-reviews)
- [The perfect code review process](https://medium.com/osedea/the-perfect-code-review-process-845e6ba5c31)
- [Link a pull request to an issue](https://help.github.com/en/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue)
- [Setting guidelines for repository contributors](https://help.github.com/en/github/building-a-strong-community/setting-guidelines-for-repository-contributors)
- [CI/CD explained](https://www.infoworld.com/article/3271126/what-is-cicd-continuous-integration-and-continuous-delivery-explained.html)
- [Travis CI tutorial](https://docs.travis-ci.com/user/tutorial/)
- [Build stages](https://docs.travis-ci.com/user/build-stages/)