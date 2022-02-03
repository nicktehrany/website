+++
date = "2020-01-11"
description = "dotfiles"
featuredalt = ""
featuredpath = "date"
linktitle = "projects"
title = "dotfiles"
slug = "dotfiles"
type = "active"
aliases = "/projects/"
+++

# dotfiles

I hardly use all of these configs at the same time, e.g. love using qutebrowser but the lack of plugins I need prevents me from using it primarily (therefore have tridactyl config for firefox!).

The current overall look for my shell and other setup (shell theme and so forth can also all be seen in the neofetch output).

![Visuals](/images/dotfiles/visual.png)

I use [bspwm](https://github.com/baskerville/bspwm) as a window manager running on PopOS!. I have the following config files:

```bash
.
├── alacritty
│   └── alacritty.yml
├── bash
├── bin
│   ├── cht-sh
│   ├── mdtemp
│   ├── set-brightness
│   ├── set-monitors
│   ├── set-screen-mirror
│   ├── set-volume
│   ├── textemp
│   └── tmux-reattacher
├── bspwn
│   ├── bspwmrc
│   └── sxhkdrc
├── dunst
│   └── dunstrc
├── env
│   ├── custom-shortcuts.conf
│   ├── refind
│   └── userChrome.css
├── fonts
│   ├── Hack Nerd Font.ttf
│   └── Hack Regular Nerd Font Complete.ttf
├── git
├── gnupg
│   └── gpg-agent.conf
├── install-config
├── install-profile
├── lf
│   └── lfrc
├── LICENSE
├── meta
│   ├── base.yaml
│   ├── configs
│   ├── dotbot
│   ├── images
│   └── profiles
├── nvim
│   ├── base.vim
│   ├── commands.vim
│   ├── functions.vim
│   ├── init.vim
│   ├── keymappings.vim
│   ├── lsp-langs.lua
│   ├── lsp.vim
│   ├── plugged
│   ├── plugins
│   ├── plugins.vim
│   └── spell
├── polybar
│   ├── config
│   └── launch.sh
├── qutebrowser
│   ├── config.py
│   └── gruvbox.py
├── README.md
├── tags
├── tmux
├── tridactyl
│   └── tridactylrc
├── vifm
│   ├── colors
│   └── vifmrc
├── vim
├── wallpapers
│   ├── forest_stairs.jpg
│   └── something.jpg
├── zathura
│   └── zathurarc
└── zsh
    ├── ohmyzsh
    ├── themes
    └── zsh-syntax-highlighting
```

## Installation

I use [dotbot](https://github.com/anishathalye/dotbot) for installing and linking of config files. It mainly just links files, since handling install for all kinds of different distros is just too much work. Therefore, check the respective git repo of the projects for install instructions (most of these can typically be installed with a package manager). The configurations are all in the `meta/configs` directory.

```bash
git clone https://github.com/nicktehrany/dotfiles
cd dotfiles

# For installing the tmux config (check meta/profiles/)
./install-config tmux
```

**Note** all previously linked files will be overwritten, check the `meta/configs/` for symlinks that will be created.

### Additional Installations

#### tmux

To have the status bar of tmux work correctly run inside tmux `prefix` + `I` to install required plugins via tpm, followed by reloading tmux with `prefix` + `R`.

#### nvim

The neovim install requires a couple more commands

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
sudo add-apt-repository ppa:neovim-ppa/unstable && apt update && apt install -y neovim ripgrep cargo
npm i -g clangd bash-language-server cmake dockerfile-language-server-nodejs vscode-langservers-extracted pyright vim-language-server yaml-language-server
nvim --headless +PlugInstall +COQdeps +COQnow +qa
nvim --headless +TSInstall cpp c bash python lua json latex regex yaml cmake bibtex dockerfile jsonc +q
cargo install --git https://github.com/latex-lsp/texlab.git --locked
```

#### lf

I don't use stock lf but rather [lfimg](https://github.com/cirala/lfimg) to also have image support. Requirements and install instructions are in their repo.

#### fonts

I use the [Hack Nerd Font](https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/Hack/Regular/complete/Hack%20Regular%20Nerd%20Font%20Complete.ttf) but there are many other fonts available [here](https://github.com/ryanoasis/nerd-fonts). Nvim and others often require a nerd font for rendering icons!

#### bswpm

Automating the install unnecessary, but install instructions are [here](https://github.com/baskerville/bspwm) and it will require the `bspwmrc` and `sxhkdrc` to be linked in `~/.config/{bspwm,sxhkd}`.

#### Wallpapers

I also use wallpapers which I came across somewhere, so if anyone knows who they actually belong to let me know I will credit them.

#### Import custom keyboard shortcuts

Prior to using bspwm I was using gnome and had custom keyboard shortcuts which were exported in `env/custom-shortcuts.conf` using

```bash
dconf dump / | sed -n '/\[org.gnome.settings-daemon.plugins.media-keys/,/^$/p' > env/custom-shortcuts.conf
```

and can be imported again with

```bash
dconf load / < env/custom-shortcuts.conf
```

#### Installing refind boot manager

I use the [refind boot manager](https://www.rodsbooks.com/refind/), with a [theme](https://github.com/EvanPurkhiser/rEFInd-minimal) which I modified, hence it is included in these dotfiles.
Installing it can be done as follows, note this also requires privileges as it is modifying `/boot/EFI`

```bash
# install refind
sudo apt install refind

# create a themes folder for refind (in /boot/EFI/refind)
mkdir themes

# copy the theme in there
cp $HOME/dotfiles/env/refind/refind-minimal /boot/EFI/refind/themes

# Add theme to the refind config at the end of file(in /boot/EFI/refind/refind.conf)
include themes/refind-minimal/theme.conf
```

## Helpful Commands

Since there are a lot of commands for the different applications, I made a handy [gist](https://gist.github.com/nicktehrany/7126ec0ad18f0af050e15596371ceea5) with the most frequently used commands that I use. This does include some applications that are not mentioned in my dotfiles, and overwritten OS shortcuts.
