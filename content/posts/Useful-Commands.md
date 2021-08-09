---
title: "Useful Commands"
date: 2021-07-17T14:02:11+02:00
publishdata: 2021-07-17T14:02:11+02:00
type: "blog"
mathjax: true
---

This is not necessarily a blog post but rather a handy spot of where I keep some of the useful commands and shortcuts I
use.

gist with all useful commands that I use or need and tend to forget since it's just too many, a lot of these are custom key mappings so they might not work by default.

### Vim

some vim commands

```bash
Ctrl + c # compile latex file
Ctrl + l # clear output log from latex compile

Ctrl + w arrow # change window to arrow direction

Ctrl + b # open NerdTree
Ctrl + r # refresh NerdTree

:sp # split horizontally
:vsp # split vertically

# Navigation
gg # jump to top of file
G # jump to end of file
ZZ # save and quit
ZQ # quit
\tab # switch to next tab in buffer
\ Shift tab # swithc to previous tab in buffer
/ # search use N and Shift + N to navigate them
w # goto next word
b # goto previous word
{ # goto previous paragraph
} # goto next paragraph
0 # goto beginning of line
$ # goto end of line
Ctrl + ] # goto function def (requires ctags and other plugins installed)
Ctrl + o # go back to previous location
u # or U inside Nerdtree to up a dir

# Editing
Ctrl + w # delete word
a # insert mode after character
o # insert mode after line
dd # delete Line
D # delete part of line after cursor
cw # delete next word and enter insert mode
di( # delete everything inside (), leave the parenthesis
da( # delete everything inside (), including the parenthesis
gq$ # wrap words until end of line (or gq0 for until beginning of line)

:m +1 # move line 1 down (- for up and line numbers) if no plus line number is absolute
```

### Zathura

```bash
K # Zoom in
J # Zoom out
t # goto top
u # go half page up
d # go half page down
i # invert colors
D # change page mode (dual/single)
```

### Shell Window Tiling

```bash
Super + / # go through applications
Super + y # Tile windows
Super + o # change orientation

Super + enter # adjustment mode
  arrow # move window
  Ctrl + arrow # swap windows
  Shift + arrow # resize
  Super + Shift + down # move window down one workspace
  Super + Shift + right # move window to right monitor

Super + s # stacking mode
Super + g # floating mode
```

### Key Remapping

I use these to remap CAPS lock to esc and home to delete.

```bash
xev # gui to find out the keycode of a key
xmodmap -e 'keycode 110=Delete' # remaps the home key to delete
```

### Vifm

```bash
zo # show hidden files
zm # hide hidden files
w # preview
cw # change name (with extensions)
cW # change name (without extensions)
Tab # switch pane
Shift Tab # switch to preview pane
```

### Misc

```bash
grep -rHn 'word' *.md # grep all md files for the word

pandoc file.md -o file.pdf # markdown to pdf conversion
```

#### Docker

```bash
# delete all exited containers
docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs docker

# delete all untagged images
docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")
```
