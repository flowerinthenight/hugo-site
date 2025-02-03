---
title: "AWS ClockBound client for Go (update)"
description: "2025-01-27"
categories: ["tech", "programming", "software", "systems"]
date: "2025-01-27"
tags: [aws, timesync, golang, go, true-time, clockbound, cgo, ffi]
weight: 1
---

A week ago I published a short [blog](/blog/2025-01-22-clockbound-client-go/) about [clockbound-client-go](https://github.com/flowerinthenight/clockbound-client-go). After some testing, turns out there's an issue in reading the actual time from ClockBound's shared memory segment; it seems to provide only the elapsed time since boot. However, using the Rust client and the FFI bindings produce the correct results. Either there is a problem in the code that reads the shared memory segment, or the SHM contents are wrong. Or, the contents are actually correct, but the expectation is for the implementing client to figure out the bounded time from the available values.

Either way, this is unusable for me at the moment. Instead of spending time debugging this, I wrote another client, called [clockbound-ffi-go](https://github.com/flowerinthenight/clockbound-ffi-go), utilizing the provided [FFI](https://github.com/aws/clock-bound/tree/main/clock-bound-ffi) (meaning, requiring CGO). So far, it works well, as expected. I would have preferred to not use CGO but I need a working version as soon as possible. I'll come back on this when I have the time.

<br>

Related blogs:

1) [AWS ClockBound client for Go](/blog/2025-01-22-clockbound-client-go/)
2) This blog post
3) [Distributed locking on AWS (ClockBound)](/blog/2025-02-02-aws-dist-locking/)

<br>
