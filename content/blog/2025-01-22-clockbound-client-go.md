---
title: "AWS ClockBound client for Go"
description: "2025-01-22"
categories: ["tech", "programming", "software", "systems"]
date: "2025-01-22"
tags: [aws, timesync, golang, go, true-time, clockbound]
weight: 1
---

I've written a Go client for [AWS ClockBound](https://github.com/aws/clock-bound) called [clockbound-client-go](https://github.com/flowerinthenight/clockbound-client-go). It uses the newer, shared memory segment protocol instead of the older, socket-based protocol. This is a prerequisite library needed to port [spindle](https://github.com/flowerinthenight/spindle) (and maybe even [hedge](https://github.com/flowerinthenight/hedge)) to AWS (for an upcoming project).

As great a tech Google's [TrueTime](https://cloud.google.com/spanner/docs/true-time-external-consistency) is, there is no available API for it. It is only through Spanner that spindle achieves its locking mechanisms with TrueTime. Currently, it's the cheapest way, surprisingly, to do distributed locking that I've tried so far, compared to the likes of Redis, Zookeeper, etcd, Consul, etc. Yes, I could do VPC peering between GCP and AWS but it is quite costly at the moment.

I've heard of AWS' [TimeSync Service](https://aws.amazon.com/blogs/compute/its-about-time-microsecond-accurate-clocks-on-amazon-ec2-instances/) before but only in passing. Now that they've also released [DSQL](https://aws.amazon.com/blogs/database/introducing-amazon-aurora-dsql/), and having seen the papers and blogs about it, turns out that TimeSync is their equivalent to TrueTime, and that there is an API for it!

<br>

Related blogs:

1) This blog
2) [AWS ClockBound client for Go (update)](/blog/2025-01-27-clockbound-client-go-update/)
3) [Distributed locking on AWS (ClockBound)](/blog/2025-02-02-aws-dist-locking/)
4) [Cluster membership management on AWS](/blog/2025-02-07-aws-cluster-membership/)
5) [Static-linked CGO binaries using musl and Zig](/blog/2025-02-15-cgo-static-linked-bin-musl/)

<br>
