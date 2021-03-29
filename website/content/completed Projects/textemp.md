+++
date = "2020-11-08"
description = "textemp"
featuredalt = ""
featuredpath = "date"
linktitle = "projects"
title = "textemp"
slug = "textemp"
type = "completed"
aliases = "/projects/"
+++

I was tired of having to create new folder structure and latex files every time I had to create a new latex project
(which was quite often). Therefore I made a quick and simple command to set up the basic directory structure
and initialize the .tex files with the most basic necessities.

## Setup

Clone the repo anywhere you like,

```bash
git clone https://github.com/nicktehrany/textemp
```

Source the textemp script in your .bashrc, .zshrc or other .*rc file you are using, and add an export of the path to the
textemp folder, like so:

```bash
source ~/PATH/TO/textemp/textemp.sh
export TEXTEMP="$PATH/TO/textemp"
```

where /PATH/TO/textemp is the path to where the textemp repository is saved. (Note the repository folder is called textemp,
as well as the script)

If in doubt, have a look at my [.zshrc](https://github.com/nicktehrany/dotfiles/blob/master/.zsh/.zshrc), which is contained
within my [dotfiles](https://github.com/nicktehrany/dotfiles) repository.

## Execution

Execution is very simple. Templates will initialize the folder with basic latex files and a full Makefile for the project.
There are currently only 5 templates available; article template, article template with code listing style, exercise
report (handy for multiple exercises in an assignments), exercise report with code listing style, and the IEEE trans template.
The command would look as follows:

```bash
textemp [FOLDER] -[FLAG]
```

with possible flags:

```bash
a # Article template
ac # Article with Code template
e # Exercise report template
ec # Exercise report with Code template
IEEE # For IEEE trans template
```

A possible execution would look as follows,

```bash
textemp Report -ac
```

to create a folder called "Report" with the Article Code version template.
