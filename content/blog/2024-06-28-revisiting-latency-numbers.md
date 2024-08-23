---
title: "Revisiting latency numbers"
description: "2024-06-28"
categories: ["infra"]
date: "2024-06-28"
tags: [infra, system-design, latency]
weight: 1
---

"Back-of-the-envelop calculations", "napkin-math", "latency numbers every programmer should know" - yes, those numbers that usually come up during system design interview questions. This came into my periphery again while looking at RDMA latency checks and benchmarks with P4d instances in AWS (using SoftRoCE). As an old-timer with (most likely) outdated ideas about system design-related latency numbers, although I'm quite familiar with Jeff Dean's ["Numbers every one should know"](https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf) approximations, I noticed that in a jiffy, I'm still (unconsciously) subscribed to the idea that disk access is most definitely faster than network. Somewhere along the lines of L* cache > memory > disks > network. Come to think of it, not really sure why. It's probably because pre-cloud, I really didn't have access to high tier network bandwidth, so my experience with only crappy networks has been etched in my mind for the longest time. This is usually evident when I do quick, back-of-my-head latency calculations of a potential system to design. Of course in the end, benchmarking will have the last say when it comes to these numbers, but having a rough idea of the performance numbers pre-implementation is always helpful.

{{< paige/figure caption="From Jeff Dean's slides." >}}
{{< paige/image src="../assets/20240628-numbers-everyone-should-know.png" width="70%" >}}
{{< /paige/figure >}}

The idea of 50Gbps, 100Gbps, or even 200Gbps network speeds, and now with P4d's advertised 400Gbps (yes, 400!) somehow didn't translate to my internal insistence that SSDs, especially the NVMEs or even M.2s, should be faster. Now, there might be enterprise/military/space/etc...-grade SSDs that I'm not aware of, or don't have access to, that have stupendous read speeds, but I've never heard of one so let's leave them for now. Some of the faster M.2 SSDs can go up to ~15GB/s read speeds, which is quite impressive, but nowhere near as P4d's. I still can't wrap my head around it. It's the sort of numbers you find listed on supercomputers with their custom Infiniband interconnects.

Anyway, after scouring the rabbit hole that is Reddit regarding discussions, debates, (and fights,) about the correctness of these numbers, I came across sirupsen's [napkin-math](https://github.com/sirupsen/napkin-math) repo, which I think is a fascinating piece of information. If you're a system designer, you really should check it out.

{{< paige/figure caption="From napkin-math repo." >}}
{{< paige/image src="../assets/20240628-numbers-net-ssd.png" width="80%" >}}
{{< /paige/figure >}}

Again, it looks like network can indeed be faster than disk access. I really need to recalibrate my internal latency numbers table to accommodate for these more modern hardware capabilities. The good thing is, at least, these sort of information are now readily available anytime. With that said though, I still believe that system designers should still be able to roughly determine a system's overall performance behavior pre-implementation using napkin-math latency calculations.

<br>
