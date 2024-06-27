---
title: "Download the latest Github release using command line"
description: "2022-01-17"
date: "2022-01-17"
paige:
  feed:
    hide_page: true
tags: [cmdline, download, github, release]
weight: 1
---

For personal reference:

Download the latest GitHub Releases asset using common command line tools:

```sh
# Update the url accordingly. The `uname | awk` subcmds will output 'linux'|'darwin'.
$ curl -s https://api.github.com/repos/alphauslabs/bluectl/releases/latest | \
  jq -r ".assets[] | select(.name | contains(\"$(uname -s | awk '{print tolower($0)}')\")) | .browser_download_url" | \
  wget -i -
```

<br>
