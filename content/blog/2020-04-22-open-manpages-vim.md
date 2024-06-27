---
title: "Open man pages in Vim"
description: "2020-04-22"
date: "2020-04-22"
paige:
  feed:
    hide_page: true
tags: [man, manual, vim]
weight: 1
---

Most of the time when I open a man page for a certain command or tool, I use Vim:

```sh
# Open man page for the 'dd' command:
$ man dd | col -b | vim -MR -
```

It's actually a handful to type but since I use [zsh](https://www.zsh.org/) + [fzf](https://github.com/junegunn/fzf), it's not really a big deal as recalling the command is pretty straightforward.

But yesterday, I came across this nifty little plugin called [vim-manpager](https://github.com/lambdalisue/vim-manpager). I tried adding it to my vimrc, added the environment variable below to my zshrc:

```sh
export MANPAGER="vim -c MANPAGER -"
```

And now, I can open all man pages by calling the `man` command directly. I can also open man pages directly from within Vim. Brilliant!

<br>
