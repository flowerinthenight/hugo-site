---
title: "Update Spacemacs from command line"
description: "2021-02-21"
date: "2021-02-21"
paige:
  feed:
    hide_page: true
tags: [emacs, spacemacs]
weight: 1
---

Reference:

Ever since I've started using Spacemacs on Linux, I've been using this simple script as a command alias to update Spacemacs using the command line.

```sh
alias upe='git -C ~/.emacs.d/ pull && \
  emacs --batch -l ~/.emacs.d/init.el \
  --eval="(configuration-layer/update-packages t)" 2>&1 | \
  tee /tmp/emacs-update && \
  grep -i -E "Found.*to.*update.*" /tmp/emacs-update && \
  emacs'
```

I use the develop branch of Spacemacs, thus the update using git. Then it will check if there are package updates and if so, it will relaunch Spacemacs to actually install the updates.

<br>
