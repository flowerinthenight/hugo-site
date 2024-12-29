---
title: "Know your target"
description: "2024-12-28"
categories: ["tech", "programming", "software", "systems"]
date: "2024-12-28"
tags: [programming, software]
weight: 1
---
{{< paige/figure >}}
{{< paige/image src="../assets/20241228-know-your-target.png" >}}
{{< /paige/figure >}}

<br>

I remember ages ago, a friend asked me what my tips would be to transition from a typical programmer to a "systems programmer". And I distinctly remember answering him along the lines of "Y'know what, I don't really know, maybe just get more experience?" And now, being an engineering leader to some extent, I get this question once in a while, but even now, I don’t really have a default answer, and a good one at that, to give. And writing this blog is, in part, me trying to remedy that.

Now, a bit of context, as this topic, I think, is a little vague, and you can tackle it from different perspectives. First, what is even a "typical programmer"? And what is a "systems programmer" anyway? I will try to narrow it down to my friend's perspective. He's a web developer, one you would consider as "full-stack". He likes his programming languages, frameworks, design patterns, and we talk a lot about these things, which are all very interesting. We can pin the "typical programmer" definition to that. Does it make sense if we categorize junior, associate, mid-level, senior programmers to this group as well? Maybe. But I also have peers who are seniors for many, many years, and still enjoy what they do (e.g. maintaining legacy systems), and have no intentions/interest in going beyond that; it can be argued that they are also experts in their specific domains.

I also don't think my friend meant those developers who specifically do close-to-hardware systems, like embedded, drivers, OS, robotics, automotive, etc.; I think he refers to the IC path, such as [Senior] Staff+, [Senior] Principal Engineers, etc.

We can also phrase it controversially, like, "How do I transition from a [insert language here|backend|frontend] programmer to an [expert generalist|staff+|principal] level programmer?". There's a lot of nuances to unpack in that line but let's just try to leave it like that and interpret it as you would so we can move on. I will also limit my discussions to the technical side of it; the social/political/soft-skills side of it is another topic I intend to explore in a separate blog post.

Aside from checking the literature regarding the topic, I also tried asking colleagues/peers what they think of this question. In no particular order, some interesting replies I got were:

* Read a lot of papers - I suppose it makes sense to some extent?
* Get a masters or PhD - I'll leave this one for you to interpret. There's a spectrum here but I really admire PhD people with really deep levels of expertise in specific sciences as well as software in general.
* Job hop (specifically between startups and big tech) - You could also do that, I suppose?
* Contribute to OSS - not sure if this works; depends on the OSS in question I suppose?
* Join an early-stage startup - if you're [un]lucky.

Now, to the topic: my one-line answer would be the title of this blog: **know your target**. Let me explain.

**Know your target**

When I say "target", I don't mean customers or users of your software, but your production target platform: mobile, browsers (if doing web development), architectures such as Intel's x86 (x64), RISC architectures such as ARM, MIPS, and PowerPC, the GPU, cloud and its internals, the network your software is running on, storage systems, data centers, mainframes, HPC systems, IoT, etc. With that said though, I don't really expect us to "know" all of these targets; we can go a step higher. Most of these targets are abstracted by the OS/software running on top of them: e.g. Linux, Unix, Windows, OSX, Android, IOS, browsers, the JVM, hypervisors, etc. Knowing these target OS/software is much more important than the language you use to program them. Actually, the language being used will be of lesser consequence to the system artifact itself compared to the target platform it will be running on. Therefore, I can further specify "know your target" to "know your target OS".

You might say, "Of course! Isn't that obvious/expected?" You might be surprised how many programmers have no idea, or don't care, about the target OS their software will be running on. The facilities provided by the language (and the libraries/frameworks around the language ecosystem) are more than enough for them. So when you start talking about processes, threads, concurrency and synchronization at the OS level, or things like IPCs, memory models, I/O, the OS' network stack, filesystems, virtualization, user/kernel mode, protection rings, context switching, etc., you'll get the impression that for them, these things are what solution architects and tech leads are there for.

Take Linux for example. It's only one of the "targets" in our list but Linux is a complex beast in and of itself. A programmer can probably reasonably pick a language up in weeks/months (maybe years for C++, Rust) but he/she might not really "know" Linux even until retirement. There are so many subsystems in Linux that learning all of them is probably an impossibility. One upside though is that your knowledge of Linux will more or less translate to other OSes without the need to start from scratch.

Understanding your target allows you to leverage what it can and can't do. Its capabilities will have a big influence on how you design your system. Of course, the language matters to some extent but you will be better off approaching it in terms of its capabilities and limitations within the confines of your target.

Your knowledge of targets will also help you better understand how complex systems are being designed: even distributed systems, or systems you're probably using now.

So, interested in becoming a systems programmer? First, know your target. Your language of choice is just one of your tools to program your target.

**Closing**

I acknowledge that there are many opinions out there that don't necessarily align with this advice. For instance, a quick check on publicly available literature yields topics about architectural patterns, algorithmic thinking, FIRST and SOLID principles, systems thinking, formal verifications, thinking in terms of tradeoffs (which I agree by the way), thinking in terms of costs, DDD, etc., but I think that by being deliberate about target thinking takes you a long way towards becoming a systems programmer. And personally, I think that "know your target" is a good starting point for future conversations as well.

<br>
