---
title: "`memx` - Get process' memory usage (Linux)"
description: "2024-05-30"
categories: ["Tech"]
date: "2024-05-30"
paige:
  feed:
    hide_page: true
tags: [process, memory, usage, linux, golang]
weight: 1
---

I shared a simple piece of code for getting a process' memory usage in Linux. It's called [`memx`](https://github.com/flowerinthenight/memx). It's Linux-specific only as it reads the [proportional set size (PSS)](https://en.wikipedia.org/wiki/Proportional_set_size) data from either `/proc/{pid}/smaps_rollup` (if present) or `/proc/{pid}/smaps` file. I've used this piece of code many times at work. We use memory-mapped files extensively in some of our services and this is how we get more accurate results. Very useful in debugging `OOMKilled` events in k8s.

<br>
