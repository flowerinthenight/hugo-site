---
title: "Output glog from go test"
description: "2020-03-18"
date: "2020-03-18"
paige:
  feed:
    hide_page: true
tags: [golang, glog, test]
weight: 1
---

If you're using [glog](https://github.com/golang/glog) in your Go codes, you can output those when running `go test ...` by using the `--args` parameter:

```sh
$ go test -v ./... -count=1 -cover -race -mod=vendor --args --logtostderr --v=1
```

<br>
