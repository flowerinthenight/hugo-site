---
title: "A little something I've been building"
description: "2026-07-12"
categories: ["jennah", "ai", "agent", "memory", "context"]
date: "2026-07-12"
tags: [jennah, ai, agent, memory, context]
weight: 1
---

<br>

I've been spending a lot of my nights and weekends on [Jennah](https://jennah.nightblue.io/). It kind of nagged its way into existence: AI agents are getting genuinely good, but the memory underneath them is still held together with tape. Everyone builds it the same way: a vector database for recall, a separate graph database for reasoning, an execution log off in the corner, and a pile of app-side glue trying to keep the three in sync. That's the thing I wanted to address. Jennah's whole idea is one store; execution logs, vector recall, and reasoning graphs living together in a single store per agent, instead of a vector DB bolted onto a graph DB bolted onto a log.

And because it's all in one place, I can commit a whole reasoning step atomically, then query across all of an agent's memory in a single request - semantic recall, graph reasoning, and recent history fused into one ranked, consistent answer, instead of me stitching three systems together and hoping they agree. And I wanted memory data to be consistent regardless of whether it's single region, multi-region (within a geography), or globally distributed. That's the part I keep geeking out over. The hard problem isn't storing any one kind of memory, it's making them behave as one. It's exactly the kind of thing I love as a CTO — unglamorous, foundational, the sort of problem that, if designed right, nobody ever has to think about again. Which is a weird thing to be excited about at 1am, but here we are.

More to come soon.

<br>

(If you're an investor in the space, you can contact me anytime. And yes, Jennah will be competing with [Mem0](https://mem0.ai/), [Zep](https://www.getzep.com/).)

<br>
