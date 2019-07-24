---
title: Setting Up Windows Subsystem Linux (WSL) for Modern Web Development on Windows
date: 2018-09-13
slug: "web-development-on-windows"
featured_image: /uploads/webdev-win.png
categories:
  - dev
---

**UPDATE:** I've since stopped using WSL altogether since most of my work now is React, Gatsby or Hugo based. Node (and thus create-react-app and gatsby) all work fine (and seemingly faster) with the Windows native Node installation. Hugo is available via [Chocolatey](https://chocolatey.org/) natively for Windows. If I need to do WordPress or PHP stuff, there's a Docker-based setup called [Devilbox](http://devilbox.org/) which I started using on my Mac as well which makes that pretty darn neat, easy and efficient. But that's just me - the rest of the article might prove useful to someone trying to get WSL set up. :)

Onward...

Web development on Windows can be quite pleasant now thanks to Windows Subsystem Linux, which gives us typical Mac users a super familiar command line for our work. However, it takes some work to make the development experience actually _nice_.

Here's how I went about it.

## Install Windows Subsystem Linux (WSL)

Install Windows Subsystem Linux by following the directions here: [Install the Linux Subsystem on Windows 10 | Microsoft Docs](https://msdn.microsoft.com/en-au/commandline/wsl/install_guide)

Make sure to launch your distro's Windows app before proceeding as there's some setup it has to do on first run.

## Fix the Godawful Terminal

The Windows terminal is absolutely hideous. Let’s fix that shit.

### Install Hyper.js

First, let’s install [Hyper.js](https://hyper.is/). It is a much better looking modern terminal emulator.

Once Hyper is installed, let’s edit the config to make Bash its default.

Under Hyper preferences `(CTRL + ,)`, set shell to:

```json
{
  "shell": "C:\\Windows\\System32\\bash.exe"
}
```

### Install ZSH

[ZSH](http://www.zsh.org/) is a more modern shell and I prefer it over Bash. ZSH can be installed via `apt-get`.

```sh
sudo apt-get install zsh
```

Add the following to `~/.bashrc` to run ZSH when Bash starts:

```sh
if [ -t 1 ]; then
exec zsh
fi
```

Relaunch Hyper and choose option 2.

### Install Oh My ZSH

We already have a big improvment over the default shell, but if we install [Oh My ZSH](https://github.com/robbyrussell/oh-my-zsh), we get additional niceties to make the development experience that much better. Run the following:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Get a Decent Font

Now that we have a familiar shell, I hate the font. I like [Fira Code](https://github.com/tonsky/FiraCode). Download Fira Code and install it into Windows and then set the default font in Hyper `(CTRL + ,)`.

Finally, I like dark colors for my terminal and development. [Hyper Material Theme](https://hyper.is/plugins/hyper-material-theme) does the trick for me.

Lastly, the only hideous thing is the directory listings. Add the following shit to the bottom of your `.zshrc` file.

```sh
# Change ls colors
LS_COLORS="ow=01;36;40" && export LS_COLORS

# Make cd use the ls colors
zstyle ':completion:*' list-colors "${(@s.:.)LS_COLORS}"
autoload -Uz compinit
compinit
```

Relaunch Hyper. Directories are now pretty.

The couple things I wish Hyper did (but will do soon hopefully) are ~~allowing us to adjust the line height (it's too squished)~~ (**update:** Hyper v2.1.1 allows us to adjust the line height) and making color themes work correctly for VIM. VIM works great - but the colors are nowhere near right.

## Development Setup

### Git

```sh
sudo apt-get install git
```

### Node & NPM

At this point, unless you absolutely need NVM, I'd recommend not installing it in WSL. It. Is. Ridicuously. Slow. Instead, install Node per the [installation instructions](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions-enterprise-linux-fedora-and-snap-packages) on their site.

### Editor

For code editing, you can't go wrong with [Visual Studio Code](https://code.visualstudio.com/). I set Fira Code as my font of choice and we set VS code's integrated terminal to use WSL bash instead of PowerShell. In `settings.json`, that looks like this:

```json
"terminal.integrated.shell.windows": "C:\\Windows\\System32\\bash.exe",
```

## A Note on Directories

The directories in WSL initially confused the shit out of me, but here's the skinny:

### For your code (and anything else you want to work on with a Windows Application like VS Code)

When working in the Linux shell, save your projects in `/mnt/c/Users/<username>`. This directory is mounted to your Windows user directory so that applications like VS Code for Windows can work with the files.

### For all your Linux-related stuff (like dotfiles)

All your dotfiles like `.zshrc`, etc will be in your `~` (home) directory in Linux, which is NOT something you should try to edit in Windows. Use `vim` or `nano` within the shell to edit these things.

## The end?

This has made for a pleasant development experience for me. Except when working with WordPress, I don't really miss my Mac so much. I'll update as time goes on.
