---
title: "Using `stern` together with `grep`"
description: "2019-09-30"
date: "2019-09-30"
paige:
  feed:
    hide_page: true
tags: [stern, grep, k8s, logs]
weight: 1
---

I've been using [`stern`](https://github.com/wercker/stern) as my goto log viewer for Kubernetes. It supports multiple pods and other convenient functions on top of `kubectl logs`. And of course, grepping goes hand in hand with log viewing, doesn't it? To combine grep with stern, I use the following commands:

```sh
# For OSX:
# stern <some-prod-prefix> | grep -i --line-buffered -E '<extended-regex>'
$ stern user | grep -i --line-buffered -E 'failed'

# For Linux (Ubuntu specifically):
# stern <some-prod-prefix> | grep -i -E '<extended-regex>'
$ stern user -s 1h | grep -i -E 'error'
```

<br>
