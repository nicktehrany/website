# My website

This is the source code for my website where I keep info on all projects and notes for my uni courses.
I build the website when I have updates and deploy it to the [remote repo](https://github.com/nicktehrany/nicktehrany.github.io)
and the website is available at [https://nicktehrany.github.io](https://nicktehrany.github.io)

The website is powered by [Hugo](https://gohugo.io/) and uses the [hello-friend-ng](https://github.com/rhazdon/hugo-theme-hello-friend-ng) theme.

For math functions use `$ latex code $` for inline math and `$$ latex code $$` for blocks, but if blocks contain newline (`\\`)
the entire latex math code needs to be encapsulated in a `$$\displaylines{latex \\ code}$$`

## Commands

In the website dir run,

### Generate new course folder for notes

```bash
hugo new -k course notes/new_course
```

### Generate new note file for course notes

```bash
hugo new -k note notes/IN4331-Web-Scale-Data-Management/new_note.md
```
