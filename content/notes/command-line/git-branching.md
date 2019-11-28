---
title: Rewriting History
linktitle: Branching
toc: false
type: docs
date: "2019-11-24T00:00:00+01:00"
draft: false
menu:
  command-line:
    parent: Git
    weight: 3

diagram: true

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Branching

```mermaid
graph LR
    subgraph Master
    id1(Master 1) --> id2(Master 2) --> id3(Master 3) --> id4(Master 4) --> id5(Master 5)
    end
    id3(Master 3) --> id6(New Branch 1)
    subgraph New Branch
    id6(New Branch 1) --> id7(New Branch 2)
    end
```

## Branching

`git branch` &rarr; **list** all the branches in the repo

`git branch <name>` &rarr; **create** a new branch

`git checkout <branch>` &rarr; **switch** to an existing branch

- `git checkout -b <branch>` &rarr; _create and switch_ to a new branch

`git merge <branch>` &rarr; **merge** the provided branch into the _current branch_

`git branch -d <branch>` &rarr; **delete** the selected branch

---
## Rewriting History

`git commit —amend` &rarr; replace the last commit with the staged changes and last commit combined
  -   Use with nothing staged to edit the last commit’s message

`git rebase <base>` …