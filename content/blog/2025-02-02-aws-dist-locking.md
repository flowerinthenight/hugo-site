---
title: "Distributed locking on AWS (ClockBound)"
description: "2025-02-02"
categories: ["tech", "programming", "software", "systems"]
date: "2025-02-02"
tags: [spindle, locking, distributed-locking, aws, timesync, golang, go, true-time, clockbound, cgo, ffi]
weight: 1
---

After some testing time, I now have a working port of [spindle](https://github.com/flowerinthenight/spindle) in AWS. There were some slight changes from the original library to account for some of the differences between Cloud Spanner and PostgreSQL, but not really by much. It's called [spindle-cb](https://github.com/flowerinthenight/spindle-cb), if you're interested. It's still half the battle though; I still have to port [hedge](https://github.com/flowerinthenight/hedge) as well before I could really use it for some of the planned projects in my pipeline.

As far as lock services are concerned, spindle-cb (and spindle for that matter) is what you call a coarse-grained lock (as opposed to fine-grained). Most of my use cases of distributed locks are really around application-level orchestrations across multiple nodes. That means using a single lock (or two) within the duration of the application, instead of multiple locks for multiple objects/data, although that's not a limitation of the library by any means. With that said, I don't usually use spindle[-cb] directly, but through a cluster-aware wrapper, like, say, hedge.

At this point, I couldn't really vouch for spindle-cb's reliability, at least not yet. But since it's really just spindle with a different "true time" source, I have high hopes. And having used spindle in production for many years without any issues, I'm optimistic.

<br>

Related blogs:

1) [AWS ClockBound client for Go](/blog/2025-01-22-clockbound-client-go/)
2) [AWS ClockBound client for Go (update)](/blog/2025-01-27-clockbound-client-go-update/)
3) This blog post

<br>
