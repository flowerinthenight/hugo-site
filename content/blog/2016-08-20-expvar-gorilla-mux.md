---
title: "How to serve expvar when using gorilla/mux"
description: "2016-08-20"
date: "2016-08-20"
paige:
  feed:
    hide_page: true
categories: ["Code", "golang"]
weight: 1
---

For personal reference:

{{< gist flowerinthenight b6e639978dc2c21042ccea526700f214 >}}

### Access root

```sh
http://localhost:8000
```


### Access expvar information

```sh
http://localhost:8000/debug/vars
```

<br>
