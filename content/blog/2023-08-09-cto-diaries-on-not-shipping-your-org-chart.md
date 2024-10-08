---
title: "CTO Diaries #4: On not shipping your org chart"
description: "2023-08-09"
categories: ["Tech"]
date: "2023-08-09"
tags: [cto-diaries, cto, diaries, conways-law]
weight: 1
---

{{< paige/figure >}}
{{< paige/image src="../assets/20230809-ship-orgchart.png" >}}
{{< /paige/figure >}}

<br>

You've probably heard of the warning "Don't ship the org chart" common among product development circles. I always thought of this as synonymous to [Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law) which states that organizations design systems that mirror their own communication structure. In my experience, I've come to believe that this is true. Whether you like it or not, it is an eventuality. At some point I thought that to be an effective solutions architect, you have to be an "org architect". Or in order to achieve a certain system architecture, you're better off rearranging your org structure than influence cross-functional architects or senior engineering leadership to agree with you. It may work for a time, especially during the start, but eventually, as engineers come and go, the org-mirrored design will out. But I think there's more nuance to this than what's obvious.

There are several types of org structures but I will cover only what's relevant to my situation which is startups. This might be anecdotal but between peers, the most common types fall under these two broad classifications: less boundaries or bigger teams, and smaller, often siloed teams. Teams with less boundaries tend to move slower because more people need to communicate for alignment but will produce a more coherent product. Smaller teams on the other hand, move faster but their outputs are more difficult to integrate together, resulting in an inconsistent product experience. Personally, I don't really have a strong opinion on which is better. I think there are phases in a startup where one works better than the other. But this requires you to understand what you want to build in the first place and then build your org chart around that.

There's also the case of building your org structure based on product lines if you have multiple products as opposed to functional teams covering multiple products. Product-based org structures might promote autonomy and quicker response to industry changes but could easily lead to system duplication (wasted resources). Functional structure however, encourages specialization and focus but inhibits multi-boundary communications.

You may argue that, "Does it really matter? Customers don't really care about how your org is structured as long as your product is usable and has great UX." I would actually agree that this is a good baseline for structuring your org. You will have several vertical teams per product but an overlapping horizontal team that ensures UX coherence, usability, and branding. I think this is what the [Mirroring Hypothesis](https://www.hbs.edu/ris/Publication%20Files/16-124_7ae90679-0ce6-4d72-9e9d-828872c7af49.pdf) study (confirms Conway's Law) refers to as partial mirroring; in which technological knowledge are invested, shared, and acquired beyond operational boundaries. "API-first" companies come to mind as in partial mirroring, building contractual relationships (APIs) that support technical interdependency across boundaries seems to work. And more often than not, your org structure will undergo changes multiple times during your product's lifetime, which will result into subsequent changes to your system's design that will then mirror the new org, and so on and so forth, so you might as well embrace this dynamism instead of fighting against it.

Finally, the study also highlights collaboration patterns in the open source sphere that do not support the mirroring hypothesis (and thus, Conway's Law). My experience with OSS collaboration is mostly outside work so I'm interested to know if there is a case for adopting OSS-style structure within the organization. I think that's a good topic for another day.

To conclude, instead of going with the warning "don't ship the org chart", since you will ship your org chart anyway, be aware of it, understand it, and make sure it works on your favor.

<br>
