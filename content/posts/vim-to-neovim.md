---
title: "Vim to Neovim"
date: 2019-05-01T18:45:50-07:00
draft: true
---

I've recently decided to switch from vim to Neovim. Here's how I did it.

## Installing Neovim

```
brew install neovim
```

## Installing vim-plug (a plugin manager)
```
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

```

## Installing NerdTree

Add the following to `~/.config/nvim/init.vim`

```
call plug#begin('~/.local/share/nvim/plugged')

Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }

call plug#end()

```


