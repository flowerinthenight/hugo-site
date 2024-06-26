---
title: "Restore default branch from tag while preserving history"
description: "2023-04-06"
categories: ["Tech"]
date: "2023-04-06"
paige:
  feed:
    hide_page: true
tags: [git, history]
weight: 1
---

Fore self reference:

To restore default branch from a tag while preserving history, do:

```sh
$ git checkout tags/v1.2.3 -b v1.2.3
$ git diff main > /tmp/diff.patch
$ git checkout main
$ cat /tmp/diff.patch | git apply
$ git commit -am "Rolled back to v1.2.3"
$ git push origin main
```

<br>
