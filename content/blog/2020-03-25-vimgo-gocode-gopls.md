---
title: "Follow-up on autocompletion with `gocode` and `vim-go` in Go1.11, transition to `gopls`"
description: "2020-03-25"
date: "2020-03-25"
paige:
  feed:
    hide_page: true
tags: [vim, vim-go, gocode, autocomplete, gopls]
weight: 1
---

##### Update
[vim-go](https://github.com/fatih/vim-go) has already transitioned to [gopls](https://github.com/golang/tools/blob/master/gopls/doc/user.md) as its default completion engine. I just noticed that my Vim is still using [gocode](https://github.com/stamblerre/gocode) even though I updated to the latest version of vim-go. Commenting/removing the line `Plugin 'stamblerre/gocode', {'rtp': 'vim/'}` from my .vimrc did the trick for me. Hope this helps.

Original post [here](https://flowerinthenight.com/blog/2018/09/20/vimgo-go111-gocode).

<br>
