---
title: "A hacky way to update problematic Go modules"
description: "2021-06-30"
date: "2021-06-30"
paige:
  feed:
    hide_page: true
tags: [golang, modules, update]
weight: 1
---

The only options for updating modules that I'm aware so far are a) `go get -u all`, and b) specific modules, i.e. `go get -u domain.com/module[@v1.2.3]`. For problematic ones, my only option is b), which is a bit time consuming. There must be some other way out there that I'm not aware of but at the moment, I use this simple command:

```sh
$ cat go.mod | grep -i 'github' | grep -i -v 'indirect' | awk '{print $1}' > update; \
  while read -r v; do go get -u $v; done < update; rm update
```

<br>
