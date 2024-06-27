---
title: "Connecting to Torguard VPN using Wireguard from Ubuntu"
description: "2020-03-25"
date: "2020-03-25"
paige:
  feed:
    hide_page: true
tags: [torguard, wireguard, ubuntu, linux]
weight: 1
---

As of this writing, Torguard's desktop app still doesn't support Wireguard out of the box. I'm using Ubuntu 19.10 for testing.

If you haven't installed Wireguard yet, you can refer to the installation instructions [here](https://www.wireguard.com/install/). In my case, it was:

```sh
$ sudo apt install wireguard
```

Next, you need to login to your Torguard client area, go to Tools --> Enable Wireguard Access. Select a location (in my case, I chose Asia-Singapore2) then click 'Enable Wireguard'. You will be presented with an option to download the generated config file (in my case Asia-Singapore2.conf). Download it and save it to /etc/wireguard/.

To enable the VPN connection, run:

```sh
# The last bit should correspond to the filename of your config file without the '.conf',
# in my case 'Asia-Singapore2'
$ sudo wg-quick up Asia-Singapore2

# Confirm with:
$ sudo wg

# If no issues, you can check your public IP with:
$ curl ipecho.net/plain; echo 
```

Finally, to disconnect, run:

```sh
$ sudo wg-quick down Asia-Singapore2
```

That's it.

<br>
