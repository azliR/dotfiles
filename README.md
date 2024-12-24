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

# VPS Setup

If you want to set up a production ready VPS, there are a few steps you should take.

This document goes through the list of steps that I personally take.


## 1. Create a New User with Sudo Permissions
Log in as root
```
ssh root@your-server-ip
```

(optional) Set the system locale to English
```
update-locale LANG=en_US.UTF-8
dpkg-reconfigure locales
```

Create a new user
```
adduser newuser
```

Add the user to the sudo group
```
usermod -aG sudo newuser
```

Test the new user
```
su - newuser
sudo apt update
```


## 2. Set Up SSH Key Authentication
```
# On your local machine, generate an SSH key pair if you don’t already have one
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy the SSH key to the new user on the server
ssh-copy-id -i ~/.ssh/id_ed25519.pub newuser@your-server-ip

# Test key-based login
ssh newuser@your-server-ip
```

## 3. Install Zsh and Dotfiles

Install zsh
```
sudo apt install git curl zsh stow unzip micro -y
sudo chsh -s $(which zsh)
sudo curl -s https://ohmyposh.dev/install.sh | sudo bash -s -- -d /usr/bin
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
git clone https://github.com/azliR/dotfiles
cd dotfiles
stow --adopt .
exit
```


## 4. Harden SSH

Open SSH configuration file
```
sudo micro /etc/ssh/sshd_config
```
Modify the following in the file:
```
# PermitRootLogin no # Disable root login
# PasswordAuthentication no  # Disable key based auth
```
Restart SSH service
```
sudo systemctl restart ssh
```
Test SSH with new settings before logging out
```
ssh newuser@your-server-ip
```

## 4. Set Up a Firewall (UFW)
```
sudo apt install ufw
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status
```

## 5. (Optional) Install and Configure Fail2Ban

```
sudo apt install fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo micro /etc/fail2ban/jail.local
```
Ensure the following lines are set:
```
[sshd]
enabled = true
port = 22
maxretry = 5
bantime = 3600
``` 
Restart fail2ban
``` 
sudo systemctl restart fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

# Docker
In local machine
```
docker context create customname --docker "host=ssh://uservps@hostvps"
docker context customname
docker swarm init
```
Later
```
docker stack deploy -c ./docker-stack.prod.yaml customname --with-registry-auth
```


# Inspiration

The inspiration for this configuration comes from the [dotfiles by Elliot from Dreams of Autonomy](https://github.com/elliottminns/dotfiles). If you're looking for inspiration to create your own emacs configuration, or improve your system configuration synchronisation, then I'd suggest watching his videos.

