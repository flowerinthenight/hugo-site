---
title: "Mount Filen as a systemd service"
description: "2025-12-02"
categories: ["oss", "filen", "storage"]
date: "2025-12-02"
tags: [oss, filen, storage]
weight: 1
---

<br>

This is a guide for my (future) self.

To setup [Filen](https://filen.io/) as a systemd service, see guide below. As an example, the username we will be using is `user1`.

1] Create a unit file under `/usr/lib/systemd/system/` for our service. Take note of the binary location and the mount point.

> ```sh
> cat >/usr/lib/systemd/system/filen-mount.service <<EOL
> [Unit]
> Description=Filen CLI Mount Service
> After=network-online.target
> Wants=network-online.target
>
> [Service]
> Type=simple
> ExecStart=/home/user1/.local/bin/filen mount /home/user1/filen/ --quiet
> Restart=on-failure
> RestartSec=5
> User=user1
> WorkingDirectory=/home/user1/
>
> [Install]
> WantedBy=multi-user.target
> EOL
> ```

2] Set the service to start on boot.

> ```sh
> systemctl daemon-reload
> systemctl enable filen-mount
> systemctl start filen-mount
> ```

(Might need `sudo` for admin permissions.)

<br>
