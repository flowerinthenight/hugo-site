---
title: "Setting alarm from commandline"
description: "2021-11-08"
date: "2021-11-08"
paige:
  feed:
    hide_page: true
tags: [alarm, cmdline, commandline, gnome, linux]
weight: 1
---

This is just a quick one.

Although I use [Gnome Clocks](https://wiki.gnome.org/Apps/Clocks) occassionally, most of the time I set an alarm via the commandline.

```sh
# OS is POP_OS!, using VLC:
$ sleep 10m && \
  cvlc ~/alarm.mp3 --play-and-exit && \
  notify-send 'alarm done'
```

<br>

