---
title: "Jennah on LongMemEval: 95.6%"
description: "2026-07-29"
categories: ["jennah", "ai", "agent", "memory", "benchmark"]
date: "2026-07-29"
tags: [jennah, ai, agent, memory, benchmark]
weight: 1
---

<br>

&nbsp;&nbsp;I finished the bi-temporal part of [Jennah](https://jennah.nightblue.io/)'s episodic memory over the weekend; basically time-travel queries. I needed it so I could run [LongMemEval](https://github.com/xiaowu0162/LongMemEval) honestly against it. Asking "what did the user believe back in March?" is very different from "what is true now?", and if the memory system can't distinguish between the two, it just ends up guessing.

&nbsp;&nbsp;For the benchmark setup, I used Claude Opus 4.8 as both the judge and answer generator, with `gemini-embedding-001` for vector embeddings (Jennah supports managed embedding as well as BYO-vectors). I ran all 500 questions from `longmemeval_s_cleaned.json`.

Here are the results:

```
single-session-preference    100.0%
single-session-assistant     100.0%
single-session-user           93.3%
knowledge-update             100.0%
temporal-reasoning            93.3%
multi-session                 86.7%
-----------------------------------
OVERALL                       95.6%
```

&nbsp;&nbsp;I'm pretty happy with 95.6%. Of course, benchmarks only tell part of the story, but I mainly wanted to verify if the underlying mechanics (retrieval intelligence, graph updates, and time metadata) were actually working together. A high knowledge-update score alongside a weak temporal-reasoning score would've meant time metadata was just decorative, but thankfully that wasn't the case.

&nbsp;&nbsp;What I enjoyed most about this run was that it executed multi-region against a global control plane. Scoring well on a single local box is one thing, but keeping data consistent across regions during a benchmark run is where things get interesting.

&nbsp;&nbsp;Latency is still an issue though. Caching will help, but there are a few other bottlenecks to tune before it feels right. Correctness first, speed later.

<br>
