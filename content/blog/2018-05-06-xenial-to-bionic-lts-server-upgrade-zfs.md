---
title: "Ubuntu 16.04 LTS to Ubuntu 18.04 LTS"
description: "2018-05-06"
date: "2018-05-06"
paige:
  feed:
    hide_page: true
tags: [ubuntu, upgrade, xenial, bionic]
weight: 1
---

I just upgraded my home server from Ubuntu 16.04 LTS to 18.04 LTS without any problems so far. For context, this server runs a Samba file server using ZFS. Although after update, I had to run the following command to my pool:

```sh
# pool name is 'm0'
$ sudo zpool upgrade m0
```

By the way, at the time of this writing, Ubuntu Server update is not yet available (I think you have to wait for 18.04.1). I used the following command to update mine:

```sh
$ do-release-upgrade -m server -d
```

<br>
