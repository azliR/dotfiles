# dotfiles

This repo contains my dotfile configuration, allowing for a consistent computing experience across multiple machines.
These days I primarily use GNU/Linux via the [Arch Linux distribution](https://archlinux.org).

I manage the various configuration files in this repo using [GNU Stow](https://www.gnu.org/software/stow/).  This allows me to set up symlinks for all of my dotfiles using a single command:

```
stow .
```

## Requirements

Ensure you have the following installed on your system

### Git

```
pacman -S git
```

### Stow

```
pacman -S stow
```

## Installation

First, check out the dotfiles repo in your $HOME directory using git

```
$ git clone https://github.com/azliR/dotfiles
$ cd dotfiles
```

then use GNU stow to create symlinks

```
$ stow .
```

# Inspiration

The inspiration for this configuration comes from the [dotfiles by Elliot from Dreams of Autonomy](https://github.com/elliottminns/dotfiles). If you're looking for inspiration to create your own emacs configuration, or improve your system configuration synchronisation, then I'd suggest watching his videos.
