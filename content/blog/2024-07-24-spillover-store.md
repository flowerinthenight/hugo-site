---
title: "How Alphaus saves on costs by 'stitching storage'"
description: "2024-07-24"
categories: ["infra"]
date: "2024-07-24"
paige:
  feed:
    hide_page: true
tags: [infra, system-design]
weight: 1
---

One of [Alphaus](https://www.alphaus.cloud/)' data processing pipelines ingests around 10TB of client financial data per day. The processing engine is running on [GKE](https://cloud.google.com/kubernetes-engine) with around 80-100 (depending on what week in the month) pods sharing the total workload. Each pod has around 10GB of memory and around 30GB of attached storage. The consistency of this load allowed us to purchase enough [Committed Use Discounts (CUDs)](https://cloud.google.com/docs/cuds) for the underlying VMs to save on compute costs.

These pod resource limits are usually enough about 80% of the time. However, since late last year, some of the accounts have datasets that are way, way beyond these limits causing persistent [**OOMKilled**](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/#exceed-a-container-s-memory-limit) events.

Our first stop-gap solution was to increase the memory limit. The trouble was, even with 20GB+ of memory wasn't really enough for some of the input datasets. And on top of this, GKE's cluster autoscaler also started to increase the VM sizes to those which we don't have CUDs for. Suffice it to say, it increased our monthly cloud spend to about **+20%** while delaying the overall processing time due to pods crashing (and restarting).

We tried several solutions. One was using local files which required increasing the size of the attached storage. While cost-effective, the performance drop was significant. We also tried using the database we are currently using which turned out worse in terms of perfomance and costs. We also tried to use our cache layer (named [Jupiter](https://github.com/alphauslabs/jupiter)) which was very performant but prohibitively expensive.

Enter **Spill-over Store (SoS)**, our current solution. Inspired by [Apache Ignite](https://ignite.apache.org/)'s design, the idea is to stitch together the already available memory and storage in the running pods, providing an on-demand storage for really big datasets.

{{< paige/figure caption="Illustration of hedge's Spill-over Store." >}}
{{< paige/image src="../assets/sos.png" width="90%" >}}
{{< /paige/figure >}}

From the image above, the pod that is trying to load a huge dataset will exhaust its local memory first, then "spill over" to its local disk, then another pod's memory (using gRPC streaming), then disk, and so on. Thus, our example 100GB dataset will utilize around 4 pods in total within the cluster.

This solution allowed us to actually go back to our original pod resource limits. Both disk and network performance is acceptable (we don't use GCP's Tier 1 network) and still within our SLA as the solution only applies to about 20% of the ingestion pipeline. The majority still uses local in-memory processing.

As Alphaus grows (and therefore ingests more and more data) and serve more clients, maybe we will end up using **Apache Ignite** or some other off-the-shelf distributed solutions, but as of now, **SoS** works. With that said, if you have a cost-effective (and better) solution in mind, feel free to contact us. We'd love to talk.

Finally, you can find **SoS**'s implementation [here](https://github.com/flowerinthenight/hedge/blob/main/sos.go) (if you're interested).

<br>
