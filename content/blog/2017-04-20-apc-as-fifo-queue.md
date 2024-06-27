---
title: "Using APC as FIFO queue in Windows"
description: "2017-04-20"
date: "2017-04-20"
paige:
  feed:
    hide_page: true
tags: [c++, fifo, queue]
weight: 1
---

In this way, we can implement a FIFO queue without using explicit locking/synchronization for enqueueing/dequeueing.

For self reference:

{{< gist flowerinthenight 48d81f079d64dabbad7fa80928159461 >}}

<br>
