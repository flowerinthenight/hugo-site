---
title: "Using Docker's ETW log driver in Windows"
description: "2017-09-20"
date: "2017-09-20"
paige:
  feed:
    hide_page: true
tags: [docker, go, windows, etw]
weight: 1
---

In Docker's [ETW logging driver doc](https://docs.docker.com/engine/admin/logging/etwlogs/), it uses the tool `logman` to view the logs. In this article, I will show you how to use [mftrace](https://msdn.microsoft.com/en-us/library/windows/desktop/ff685116(v=vs.85).aspx) to view Docker ETW logs in real-time.

First, here's a simple application written in Go that logs to STDERR every second.

{{< gist flowerinthenight 5ebeaaa38a59f7ce314a0b96b4357d4a >}}

Next, let's create a Docker image (Windows) using the Dockerfile below.

{{< gist flowerinthenight 9935cfaf68d8de5b5da2d5c4b15b8d85 >}}

```sh
# assuming the code above is saved in a directory called 'demoapp'
$ docker build -t demoapp .
```

To use mftrace, we need a config file.

{{< gist flowerinthenight 4bd8968cc08c14cc98e03494624030a5 >}}

Open a command prompt (or Powershell) and run the following command.

```sh
$ mftrace.exe -c config.xml
```

Then open another command prompt (or Powershell) window and run the Docker image.

```sh
$ docker run -d --log-driver=etwlogs --name demoapp demoapp:latest
```

You should be able to view the application logs in the mftrace window.

You can use this [repo](https://github.com/flowerinthenight/20170914-tokyo-mastercloud-presentation) instead of creating your own folder structure. Instructions are provided in the README as well as an x86 version of mftrace.

<br>
