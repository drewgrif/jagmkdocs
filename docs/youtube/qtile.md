---
tags:
    - qtile
    - software
    - youtube
    - window manager
---
![type:video](https://www.youtube.com/embed/fUBjLc5aUH8){:target="_blank"}

## Qtile for Xorg Install

## Introduction
Assuming you 
1. Xorg installed
2. Not using ly console manager

## Installation

```
sudo apt install \
    python3 \
    python3-pip \
    python3-venv \
    python3-v-sim \
    python-dbus-dev \
    libpangocairo-1.0-0 \
    python3-cairocffi \
    python3-xcffib \
    libxkbcommon-dev \
    libxkbcommon-x11-dev \
    git \
    sxhkd

```
Make sure:

1. You have a ~/.local/bin directory
2. You have the following in your .bashrc

```
export PATH="~/.local/bin:$PATH"

```

### Setting Up Virtual Environment

```
python3 -m venv ~/.local/src/venv
```

### Using qtile.git
```
git clone https://github.com/qtile/qtile.git ~/.local/src/venv/qtile

```

### Installing qtile 
```
~/.local/src/venv/bin/pip install ~/.local/src/venv/qtile/.

```
### Installing psutil
```
~/.local/src/venv/bin/pip install psutil

```
### Setting symbolic link
```
ln -sf ~/.local/src/venv/bin/qtile ~/.local/bin/

```

## Usage

```
sudo nano /usr/share/xsessions/qtile.desktop

```

Paste the following.  Change $USER.
```
[Desktop Entry]
Name=Qtile
Comment=Qtile session
Exec=/home/$USER/.local/bin/qtile start
Type=Application
Keywords=wm;tiling

```

## Conclusion

By the end you should be able to log into vanilla qtile.
