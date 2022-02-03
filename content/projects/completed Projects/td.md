+++
date = "2022-02-03"
description = "td"
featuredalt = ""
featuredpath = "date"
linktitle = "projects"
title = "td"
slug = "td"
type = "active"
aliases = "/projects/"
+++

[td](https://github.com/nicktehrany/td) minimal cli todo lists manager

I just needed a very simple way to quickly create/edit todo lists from cmd line. There are many other projects like this, e.g. [t](https://github.com/sjl/t) which was the major inspiration for this project, however often these implementations are rather old and therefore require old python libraries for example. Since I don't want to install an old python library just for this one application, I decided to implement a simple todo list manager in C since that will be on most systems by default.

## Installation

Installation uses cmake, I typically install into my users __home__ directory and not globally, therefore I add a flag to cmake to change the `PREFIX`. 

```bash
git clone https://github.com/nicktehrany/td.git
cd td && mkdir build && cd build

# Install it to prefix DIR, leave the flag out if installing globally
cmake -DCMAKE_INSTALL_PREFIX:PATH=$HOME/local/ ..
make install
```

## Usage

Usage is very simple!!

### Adding tasks

Simply call `td [task description]`, which will create a __.todos__ file in the current directory (if none exists) and add the item.

```bash
$ td Finish kernel debugging
$ td Finish td README
```

### Listing tasks

Listing tasks is just calling `td`

```bash
$ td
0 - Finish kernel debugging
1 - Finish td README
```

### Finishing tasks

Finishing a task will remove it from the todo list and is done with `td -f [task_ID]`

```bash
$ td -f 1
$ td
0 - Finish kernel debugging
```

### Editing tasks

Editing is similar to finishing and is done with `td -e [task_ID] [task description]`

```bash
$ td -e 0 Attempt to finish the kernel debugging
$ td
0 - Attempt to finish the kernel debugging
```

### Manually changing tasks

Since the todo list is kept in a __.todos__ file, you can simply open it in a text editor and modify it (change tasks/remove tasks/add tasks) and save that. The tasks just need to follow the format of having `[task_ID] - [description]`.

### Using todo lists

Output can also be easily piped to other applications (e.g. `wc`) to see just how much you still have to do.

```bash
$ td | wc -l
1
```

## Bugs and Contributing

For any bugs feel free to put in an Issue or a PR and I'll be happy to merge it. Contributing is also very welcome, just keep in mind this is supposed to be minimal, so features should be more general and not super niche items. However, even if they are niche feel free to put in an Issue for improvements and we can see if it is something that might actually be useful.
