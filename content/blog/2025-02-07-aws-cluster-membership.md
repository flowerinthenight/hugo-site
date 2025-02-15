---
title: "Cluster membership management on AWS"
description: "2025-02-07"
categories: ["tech", "programming", "software", "systems"]
date: "2025-02-07"
tags: [hedge, cluster, memberlist, distributed-systems, leader-election, aws, timesync, golang, go, true-time, clockbound, cgo, ffi]
weight: 1
---

In continuation with my previous [post](/blog/2025-02-02-aws-dist-locking/), I have now finished porting [hedge](https://github.com/flowerinthenight/hedge) to AWS. It's a trimmed-down version for now; only the features directly related to cluster membership are ported. I decided to make a separate repo, called [hedge-cb](https://github.com/flowerinthenight/hedge-cb) (in keeping with the `-cb` theme), instead of updating hedge directly. And it's mainly due to CGO. I didn't really fancy the idea of introducing CGO to hedge as it could break a lot of the CI builds at work. I had to extract the shared protobuf definitions to a separate [repo](https://github.com/flowerinthenight/hedge-proto) however, which is a breaking change to hedge. But at least it's only a version change (v2) as opposed to adding CGO.

What remains now in the short term is figuring out how to make this thing work on EKS. Since the ClockBound daemon is a requirement, EKS pods need to be able to read its shared memory segment backing file which is `/var/run/clockbound/shm`. I have to mount that file (or maybe the whole directory) to the pod. I'm not sure how it will behave though; it's not just some regular file.

Anyway, as far as testing goes, I've tried it with both single- and multi-zone AutoScaling Groups and it works as expected. Time will tell how stable it's going to be once deployed to some production environment with larger cluster sizes.

Feature-wise, my initial intention was to make the cluster membership management part working first. That includes the ability to track member nodes dynamically, member-list, election of a leader node, the ability for any node to send both streaming and one-shot message(s) to the leader node, and broadcast-to-all mechanisms, both one-shot and streaming. And these are all working now. At the moment, I don't have any plans to support the remaining features like distributed semaphores, and storage (both K/V and spill-over). I can use other libraries for those should the need arises.

Finally, I'm thinking of porting hedge to both Rust and Zig as well. Looking forward to that.

<br>

Related blogs:

1) [AWS ClockBound client for Go](/blog/2025-01-22-clockbound-client-go/)
2) [AWS ClockBound client for Go (update)](/blog/2025-01-27-clockbound-client-go-update/)
3) [Distributed locking on AWS (ClockBound)](/blog/2025-02-02-aws-dist-locking/)
4) This blog
5) [Static-linked CGO binaries using musl and Zig](/blog/2025-02-15-cgo-static-linked-bin-musl/)

<br>
