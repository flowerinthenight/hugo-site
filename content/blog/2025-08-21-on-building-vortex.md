---
title: "On building Vortex"
description: "2025-08-21"
categories: ["updates", "startup"]
date: "2025-08-21"
tags: [updates, startup]
weight: 1
---

<br>

A quick update on what I've been building for the last couple of weeks.

I’ve put together a small team at [Alphaus](https://alphaus.cloud/) to explore [eBPF](https://ebpf.io/) with a specific goal: building a tool for AI security and monitoring. The kernel-level visibility you get with eBPF is perfect for this, but the real work is in making it useful.

Our immediate focus has been on getting visibility into encrypted network traffic. We needed a way to inspect TLS traffic without messing with certificates or acting as a man-in-the-middle. The obvious path was using eBPF uprobes to hook user-space functions directly.

We're calling the tool [Vortex](https://vortex.nightblue.io/) (thought it sounded cool). It looks something like this:

<br>

{{< paige/figure caption="Vortex design" >}}
<a href="https://vortex.nightblue.io/">
{{< paige/image src="../assets/20250821-vortex-design-01.png" width="90%" >}}
</a>
{{< /paige/figure >}}

<br>

And I'm happy to report we've had our first major successes. We can now reliably see plaintext traffic for apps built on Python (and other OpenSSL-based apps). We can now hook into the `SSL_read[_ex]` and `SSL_write[_ex]` functions in the underlying OpenSSL library that Python's `ssl` module (and many other tools) uses. This gives us the unencrypted buffer just before it's handed to the kernel for sending, or just after it's been received and decrypted.

Still in progress but the same fundamental approach works for NodeJS applications, although they have their own TLS stack. Fortunately for us, they also use the same function names so we can attach to those as well, and pull the plaintext data out.

This means we can now monitor the raw traffic of a huge range of services without any code instrumentation or network configuration changes.

Why are we doing this? The goal is to feed this traffic data into our monitoring agent. From there, we can do interesting work like detecting anomalous traffic patterns to (and from) AI models, identifying potential data exfiltration over encrypted channels, getting a clear map of what your AI services are actually talking to, etc.

It’s not trivial, of course. Dealing with function offsets, different library versions, and the nuances between runtimes is a challenge, but the core mechanism is solid.

More to share soon as we build out the next pieces.

> **Sidenote:** This project actually made me excited in programming once again as it involves tinkering with the low-level guts of the Linux kernel, which I love.

<br>
