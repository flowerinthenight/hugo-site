---
title: "Thoughts on the newer systems programming languages"
description: "2024-08-20"
categories: ["programming"]
date: "2024-08-20"
paige:
  feed:
    hide_page: true
tags: [zig, systems, programming]
weight: 1
---

I'm talking about the new crop of systems programming languages that advertise themselves as better replacements for C and/or C++: [Rust](https://www.rust-lang.org/), [Zig](https://ziglang.org/), [D](https://dlang.org/), [Odin](https://odin-lang.org/), [ Nim ](https://nim-lang.org/), etc. I'm using the word "new" loosely here as Rust and Zig, for example, are almost a decade old now. The topic of systems programming has been on my radar (again) recently at work due to our attempts at improving the performance of some of the more critical parts of our stack. At [Alphaus](https://www.alphaus.cloud/), we use Go as our main programming language and as much as I like Go, there are still areas in our infrastructure that could be served better with non-GC languages.

This post will not be an X vs Y review, or at least, not intentionally. Programming languages, I think, are one of those things that developers tend to get attached to, alongside editors, OS'es (or distros in the Linux world). "Here be dragons.", as they say. But I want to share my thoughts from a perspective of someone who can introduce a new programming language to teams, instead of someone who wants to make a case and convince somebody who can do so.

Before I joined, Alphaus was a PHP shop. Around early 2018, after about half a year, I introduced Go as our main programming language but only for new services; no rewrites. I didn't know Go at that time, nobody in the company did. But why Go though? I could have chosen Java, or C#. Seven years on, now, I consider that a good decision. Did I know it would be? Of course not. Java probably would have been fine. But thinking about it now, I would attribute it mainly down to Go's simplicity (or size?). Or at least at that time.

<br>

{{< paige/figure caption="From Go's homepage." >}}
<a href="https://go.dev/">
{{< paige/image src="../assets/20240820-golang.png" width="90%" >}}
</a>
{{< /paige/figure >}}

<br>

Before Alphaus, my background was embedded, low-level systems. Over the years of working with several programming languages, I've come to appreciate the simplicity of C. Even though experience-wise, I probably have written more C++ than C, there's something in C's simplicity that I yearn when working with more complex languages. And I think that translated to my choice of Go, really. It was the C equivalent in the high-level language world for me (apart from the fact that it's created by C people). So you can say there's definitely a bias there on my part. And I think it's true. We always say (in software engineering circles) "use the right tool for the job", but the "right" part there is the tricky bit. In my case, "right" was ultimately based on my own biases, my own preferences. Although in hindsight, it wasn't really purely selfish. There was a big push for Go when going cloud native at that time as well. And Docker and Kubernetes were also written in Go, which I think also had an influence, as we based our stack on these tools too. And our product was in infrastructure (we pivoted later) so there's definitely an alignment there.

Go's simplicity, in my mind, also meant easy recruitment. I said nobody in the company knew Go at that time but it wasn't really a big deal to me either as I was quite sure the engineering team would pick it up and be productive with it in no time. The same with recruitment; I can hire non-Go engineers knowing that they can learn Go in about a week and then start contributing. With that said though, simplicity being translated to quick productivity is not really a deal-breaker to me. It only applies when you don't have people who know the language. When you have a team who are already proficient with, say, C++, they will be productive with it.

You see, I could go on enumerating about the pros of Go by mentioning its merits as a language but what I'm really trying to say here is that the "objective" (or "right") reasons why I should choose Go only had a slight influence on me. Even without all these, I'm sure I'd still end up with Go mainly because of my bias in C. I'm sure there are a lot of engineering leaders out there who are more objective or data-driven than me but again, programming languages are one of those things we get emotional about. And I'm sure I'm not alone in thinking this way.

Now back to the present. At the moment, I'm mulling about introducing a systems language to the company. Not a replacement of Go, but a compliment to Go. Of all the choices listed above, and based on the thought process I just laid out, it's obvious that my choice would be Zig. It's advertised as "the better C", while Rust is "the better C++". But I'm not quite sure yet.

You could argue that it's impractical, maybe borderline irresponsible, of me to not choose Rust. It should be the "right" choice. It's memory-safe, more mature, already included in the Linux kernel, proven in production by some of the big names in tech, etc. And more importantly, some of the people I follow (and look up to) are also advocating for it.

<br>

{{< paige/figure caption="From Rust's homepage." >}}
<a href="https://www.rust-lang.org/">
{{< paige/image src="../assets/20240820-rust.png" width="90%" >}}
</a>
{{< /paige/figure >}}

<br>

I know Rust a bit. I've dabbled with it outside work although I'm not really proficient with it since I haven't written any production code with it. It is a complex language. Not C++-level kind of complex but it is still very complex. I believe it is "the better C++" and if I'm still working in C++ now, I'd choose it as well. But I'm not. And my bias is stronger now than ever before due to C, and now, Go.

I'm actually looking at Zig now. It resonates with me as it still feels "C"-ish but a lot better. I like the choice of control over memory allocations. It's not as safe as Rust but it provides mechanisms to make it much safer than C. And it's still a simple language overall. Having said that, no decisions yet. It's only been 2-3 months. And I'm not in a hurry. I think the only real blocker at the moment is it not being tagged 1.x yet.

<br>

{{< paige/figure caption="From Zig's homepage." >}}
<a href="https://ziglang.org/">
{{< paige/image src="../assets/20240820-zig.png" width="90%" >}}
</a>
{{< /paige/figure >}}

<br>

This post has been all over the place. In the end, you might not care whatever my choice will be and the justifications I have for it but another way of looking at it is that if you happen to be in the position of trying to convince your leader(s) to switch to your preferred programming language, basing your arguments only from an objective point of view might not hold much water. You're probably going to fight an already losing battle from the start.

Or it could still work though, who knows.

<br>
