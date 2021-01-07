+++
date = "2020-01-11"
description = "Dotfiles"
featuredalt = ""
featuredpath = "date"
linktitle = "projects"
title = "Dotfiles"
slug = "Dotfiles"
type = "active"
aliases = "/projects/"
+++

[![Build Status](https://travis-ci.com/nicktehrany/dotfiles.svg?branch=master)](https://travis-ci.com/nicktehrany/dotfiles)

My [dotfiles](https://github.com/nicktehrany/dotfiles) repository, which is more of a continuous project likely to never be done,
for managing all my config files for vim, zsh, tmux, git, other dotfiles and utilities I use.

## Installation

```shell
git clone https://github.com/nicktehrany/dotfiles
cd dotfiles
sudo ./install
```

Check the [.travis.yml](https://github.com/nicktehrany/dotfiles/blob/master/.travis.yml) for packages that will be installed,
if not already installed. **All existing links or config files will be overwritten!** (Check [.install.conf.yaml](https://github.com/nicktehrany/dotfiles/blob/master/install.conf.yaml) for all links)

Then just open a new konsole. The updating and installing of submodules and vscode plugins will take a few minutes.

Next, to set the default konsole theme, open the konsole menu (`Ctrl` + `Shift` + `m`),
under settings select manage profiles, then select `blue_default` and set it as default.
Lastly, to have the status bar of tmux work correctly run inside tmux `prefix` + `I` to install required plugins via tpm, followed by
reloading tmux with `prefix` + `R`.

## Commands

In order to update submodules if their remote repository changes.

```shell
git submodule update --remote --merge
```

If you only want to relink config files or run all commands except shell (in the [.install.conf.yaml](https://github.com/nicktehrany/dotfiles/blob/master/install.conf.yaml) file)

```shell
./install --only link # or
./install --except shell
```

## Visuals

The current overall look for my shell and other setup (shell theme and so forth can also all be seen in the neofetch output).  
![Visuals](/images/dotfiles/visuals.png)

### VS Code

My vscode is still a work in progress as far as the right color scheme goes. Thus far it looks as follows,

![VSCode](/images/dotfiles/vscode.png)

If you want to play around with different colors, adjust workbench.colorCustomizations in the [settings.json](https://github.com/nicktehrany/dotfiles/blob/master/vscode/settings.json) file, and possibly add more of the vscode color variables you can set. A nicely compiled list of
these can be found [here](https://gist.github.com/lol-russo/1c7a0b958be4b9434c5a120f24d5e7c3).
