---
title: Homebrew
linktitle: General Concepts
toc: false
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  command-line:
    parent: Homebrew
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

Homebrew is a package manager for Mac.

- Use `xcode-select --install` to install command line tools on macOS
- Copy/paste the first instruction in the official documentation to install
- Test if the installation is successful using `brew help`

There are two kinds of packages: **formulae** usually deal with command line softwares, while **casks** enamble installation of macOS application directly from homebrew.

Homebrew can be unsinstalled pretty simply with the command provided in the FAQ section of the website.

* * *

## Basic Commands

- `brew search` &rarr; **lists** all the packages that can be installed
- `brew search something` &rarr; return packages names which contains _something_ in the name
- `brew list` &rarr; **lists installed** packages
- `brew install package_name` &rarr; **intalls** the package
- `brew uninstall package_name` &rarr; **uninstalls** the package
- `brew info package_name` &rarr; return information about the package (both installed and not installed ones)
- `brew update` &rarr; **updates** the list of available packages and their version (it does not install anything)
- `brew outdated` &rarr; lists all the installed packages that have an updated version
- `brew upgrade` &rarr; **upgrades** all the outdated packages
- `brew cleanup` &rarr; delete old versions of installed packages (when packages are updated, homebrew does not replace the version but just install the new one)

### Adding 3rd Parties Brew Repositories

Sometimes something is not available in the main homebrew repository. Other repositories in which the `brew` command can search can be added. The name of this repositories is provided in documentation of the software one wants to install.

- `brew tap repository_name` &rarr; add the reposistory to homebrew. Now one can search the packages contained in that using `brew search` and install as usual

* * *

## Using Cask

_Basically just add `caks` after `brew` in each command_

- `brew cask install package_name` &rarr; install the cask package
- `brew cask home package_name` &rarr; open the home webpage of the package

* * * 

## Resources

- [Offical Documentation](https://brew.sh/)
- List of all [Homebrew formulae](https://formulae.brew.sh/formula/)
- [Homebrew tutorial](https://www.youtube.com/watch?v=SELYgZvAZbU&list=WL&index=19&t=117s) - Corey Shafer

