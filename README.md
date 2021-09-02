# My website

This is the source code for my website where I keep info on all projects and notes for my uni courses.
The website is built and deployed on every commit to this [remote repo](https://github.com/nicktehrany/nicktehrany.github.io)
and the website is available at [https://nicktehrany.github.io](https://nicktehrany.github.io)

The website is powered by [Hugo](https://gohugo.io/) and uses the [hello-friend-ng](https://github.com/rhazdon/hugo-theme-hello-friend-ng) theme.

For math functions use `$ latex code $` for inline math and `$$ latex code $$` for blocks, but if blocks contain newline (`\\`)
the entire latex math code needs to be encapsulated in a `$$\displaylines{latex \\ code}$$`

## Setup

```bash
git clone https://github.com/nicktehrany/website
git submodule init
git submodule update --remote --merge

# Need to specify certain commit of theme since it causes issues
cd themes/hello-friend-ng
git checkout 4c3a69eaa6b3747fc5344fa4e1311459d6e0b88f
```
## Contributing

If you have changes/improvements/spelling fixes or just additions to my Uni notes, feel free to put in a pull request! Also add yourself with your github link as an author to the info.md file of the respective notes folder, where you contributed changes.

## Commands

### Generate new course folder for notes

```bash
hugo new -k course notes/new-course
```

Note that naming needs to be done with `-` to parse titles correctly, when the folder/file contains multiple words.

### Generate new note file for course notes

```bash
hugo new -k note notes/[course]/content/[new-note].md
```
