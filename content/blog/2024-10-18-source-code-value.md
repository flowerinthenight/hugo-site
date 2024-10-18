---
title: "How valuable is your source code?"
description: "2024-10-18"
categories: ["tech", "programming", "software"]
date: "2024-10-18"
tags: [programming, software]
weight: 1
---
{{< paige/figure >}}
{{< paige/image src="../assets/20241018-source-code-value.png" >}}
{{< /paige/figure >}}

<br>

I had a think about this topic when I saw a recent Ars Technica [news article](https://arstechnica.com/gadgets/2024/10/winamp-really-whips-open-source-coders-into-frenzy-with-its-source-release/) about Winamp releasing their source code to GitHub and then deleting it after some backlash from the open source community. And we also hear about instances of leaked source codes as well, such as that of [The New York Times](https://www.securityweek.com/new-york-times-responds-to-source-code-leak/), [Okta](https://thehackernews.com/2022/12/hackers-breach-oktas-github.html), [Intel's UEFI source](https://www.securityweek.com/intel-confirms-uefi-source-code-leak-security-experts-raise-concerns/), [Mercedes Benz](https://www.securityweek.com/leaked-github-token-exposed-mercedes-source-code/), [Microsoft](https://www.bleepingcomputer.com/news/microsoft/lapsus-hackers-leak-37gb-of-microsofts-alleged-source-code/), among others. Until now, I haven't really put too much thought into this. I mean, from a business perspective, especially software companies who earn money through code, how valuable do we think our source code really is?

Before I dig deeper though, I think the value lies within a spectrum. And there are valid arguments from both sides. Forget the threat actors' goal of obtaining sensitive information such as keys, secrets, credit card numbers, etc. from the source code, although one could argue that these information are also part of the source code itself (and you could also argue it's technically not), making it extremely valuable; I'm generally referring to source code as a programming artifact, the text that contains the logic, and secondarily, the documentation that supports it. From this perspective, one expression of value would be does it matter if the source code is public or not. Think of Windows and Linux operating systems. One is closed source but the later is open. Is Linux's source code more "valuable" because it's publicly available? Or is it the other way around?

With that said, I want to dig a little deeper on an argument that leans toward source code, as presented above, not being really that valuable; [Peter Naur](https://en.wikipedia.org/wiki/Peter_Naur)'s ["Programming as Theory Building"](https://pages.cs.wisc.edu/~remzi/Naur.pdf). Although the "Backus-Naur Form (BNF)" author didn't explicitly say it as so, his arguments are quite fascinating, and maybe we could learn a thing or two from them.

Peter suggests that programming should be regarded as an activity by which the programmer form an insight, or theory, of how certain affairs of the world will be handled by, or supported by, code; a kind of mapping of these affairs, both characteristics and details, into program text, and any additional documentation. This is in contrast to the more common notion that programming is the production of programmed solutions, including design and implementation, and certain other texts, such as specifications, and documentation. It emphasizes that the knowledge possessed by the programmer by virtue of his or her having the theory, essentially transcends that which is recorded in the documented products. This transcendence can permeate in at least three different areas:

* The programmer having the theory of the program can explain how the solution relates to the affairs of the world that it helps to handle;
* The programmer having the theory of the program can explain why each part of the program is what it is, or is able to support the actual program text with a justification of some sort;
* The programmer having the theory of the program is able to respond constructively to any demand for a modification of the program so as to support the affairs of the world in a new manner.

For the first point, he posits that during theory building, a large part of the world won't have a direct mapping to the code. That invisible contextual information can only be made relevant by someone who understands the program theory and its relation to the world. What is interesting is that he further argues that the code and its documentation is insufficient as a carrier of the most essential part of any program, its theory. It's something that could not be conceivably expressed, but is inextricably bound to human beings.

The second part talks about the technical decisions and tradeoffs made as to why a specific implementation was chosen. Other implementations would have been correct but the programmer with the theory can justify the decisions being made at that time, which comes from intuition, and experience. Here, major decisions such as architecture can be documented, but not all. It is implied that, as the first point argues, it is impossible to document all these tradeoffs, and probably won't have much value if done anyway.

The final point, I think, is the most interesting as it involves program modification, which I think we can all agree, is expected in software. And software modification is closely related to costs. This is the reason why onboarding of new programmers take time; new programmers need to understand the original theory. It is insufficient to only be familiar with the program text and other documentation. What is beneficial, or even required, is the opportunity to work in close contact with the programmer who had the original theory. This is similar to other educational problems of other activities such as writing, or learning a musical instrument. The student doing related activities under suitable supervision and guidance will be better off compared to the student who's only looking at manuals and written instructions without teacher supervision.

One more point he emphasized is the idea of program life, death, and revival. The initial building of the program is the building of its theory. The program remains alive when the original programmer (or team) remains in charge, and retains control of its modifications. Program death means the programmer (or team) who understands the theory is dissolved, or is not in charge anymore. A dead software can continue to provide value during its operation; the actual state of death only becomes visible when demands for modification cannot be answered and/or addressed anymore. Finally, revival basically means the rebuilding of its theory (not necessarily the original) by new programmers. This is why it is often faster to write new code than modify an existing one since writing new code means rebuilding the whole theory, but now, it will be based on the intuition and perception of the world by the new programmer, without being beholden by the original theory.

Philosophy aside, what can we learn from these arguments? Here are some proposals:

* It is a good idea to retain the original programmers for as long as possible. (Perhaps easier said than done due to a lot of factors.)
* Onboarding is slow and expensive. Try to focus on the possibility of the new members having access to the original programmers during the transition.
* In general, keeping programmers longer is more cost effective than high turnover.
* Documentation, while unable to capture the whole program theory, is still helpful in providing context, however minimal, in theory knowledge transfer.

<br>
