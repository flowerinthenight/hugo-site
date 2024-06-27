---
title: "How I set up my Samba file server"
description: "2016-03-28"
date: "2016-03-28"
paige:
  feed:
    hide_page: true
categories: ["Tech"]
weight: 1
---

I installed Samba to my Ubuntu Server (which is an old ThinkPad laptop that has been gathering dust in my closet for ages):

```sh
sudo apt-get install samba
```

I have three 3-TB HDDs that I planned to use as my main server storage; one main server, and the other two for backup. All of these HDDs are in ext4 format.

Then I created a new directory under `/media` where I will mount my main server storage:

```sh
mkdir /media/myserver
```

Then I edited the samba config file `/etc/samba/smb.conf`. I just scrolled to the very bottom and added these lines (after [print$] section):

```conf
[MYSERVER]
    path = /media/myserver
    browsable = yes
    writable = yes
    guest ok = yes
    read only = no
    ```

Then I tested the config file for syntax errors using the command:

```sh
testparm
```

Before I added the mount entry for my server to `/etc/fstab`, I took note of my HDD's UUID from the following command:

```sh
sudo blkid
```

in which the output looked something like

```sh
/dev/sdb1: UUID="996a1b79-bf22-49fd-a3d1-33eab2708cfb" TYPE="ext4"
```

Then I edited `/etc/fstab` and added the line below:

```sh
UUID=996a1b79-bf22-49fd-a3d1-33eab2708cfb /media/myserver ext4 defaults 0 0
```

Lastly, I then rebooted my server.

To access my server, I opened File Explorer and browsed to `\\my_server_ip\myserver`.

<br>
