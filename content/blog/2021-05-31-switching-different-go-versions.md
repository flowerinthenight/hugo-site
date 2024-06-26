---
title: "Switching to different Go versions"
description: "2021-05-31"
date: "2021-05-31"
paige:
  feed:
    hide_page: true
tags: [golang, versions, switch]
weight: 1
---

I use this handy little script to switch between different Go (golang) versions:

```sh
#!/bin/bash
$1 version &>/dev/null
if [ $? -eq 0 ]; then
    ln -sf $GOPATH/bin/$1 $HOME/.local/bin/go
    go version
    exit 0
fi

go get golang.org/dl/$1 && $1 download
ln -sf $GOPATH/bin/$1 $HOME/.local/bin/go
go version
```

Saving this as an executable script `goset`, I could now switch to different versions like so:

```sh
$ goset go1.16.4
go version go1.16.4 linux/amd64
```

<br>
