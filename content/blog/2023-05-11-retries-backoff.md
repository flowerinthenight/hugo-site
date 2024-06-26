---
title: "Retries with backoff in distributed systems"
description: "2023-05-11"
categories: ["Tech"]
date: "2023-05-11"
paige:
  feed:
    hide_page: true
tags: [retry, retries, backoff, distributed-systems]
weight: 1
---

In a distributed system, where multiple processes communicate with each other over a network, failures are inevitable. Network partitions, hardware failures, and software bugs can all cause a request to fail. Retries with backoff are a critical technique to help mitigate these failures.

Retries refer to the act of retrying a failed request. When a request fails, the client can retry the request, hoping that it will succeed the next time around. However, simply retrying the request immediately after a failure can be problematic. If the failure was caused by a temporary network issue, for example, retrying immediately will likely result in another failure. This is where backoff comes in.

Backoff refers to the practice of waiting a certain amount of time before retrying a failed request. The idea is to wait long enough for any temporary issues to resolve themselves before retrying. The amount of time to wait is typically increased with each retry, hence the term "backoff." The idea is that if the request fails multiple times, the client will eventually back off enough to give the system a chance to recover.

There are several benefits to using retries with backoff in a distributed system. First, it can help reduce the impact of temporary failures. By waiting before retrying a request, the client can avoid bombarding the target with requests, which can exacerbate the problem. Second, it can help improve overall system availability. By retrying failed requests, the client can work around transient issues that might otherwise cause the entire system to fail.

There are several strategies for implementing retries with backoff. One common approach is exponential backoff, where the client waits an increasing amount of time between each retry. Another approach is jittered backoff, where the client adds a random amount of time to the wait period to avoid the so-called "thundering herd" problem, where multiple clients all retry at the same time.

Example 1:

```go
import (
    ...
    backoffv4 "github.com/cenkalti/backoff/v4"
)

func main() {
    var n int
    operation := func() error {
        n++
        log.Printf("n=%v\n", n)
        if n >= 10 {
            return nil
        }

        return fmt.Errorf("backoff")
    }

    err := backoffv4.Retry(operation, backoffv4.NewExponentialBackOff())
    if err != nil {
        log.Println("final backoff failed")
    }
}
```

Example 2 (my preference):

```go
import (
    ...
    gaxv2 "github.com/googleapis/gax-go/v2"
)

func main() {
    bo := gaxv2.Backoff{
        Initial: time.Second,
        Max:     time.Minute,
    }

    var n int
    operation := func() error {
        n++
        log.Printf("cnt=%v\n", n)
        if n >= 10 {
            return nil
        }

        return fmt.Errorf("backoff")
    }

    for {
        err := operation()
        if err != nil {
            time.Sleep(bo.Pause())
            continue
        }

        break
    }
}
```

In conclusion, retries with backoff are an important technique for improving the robustness and availability of distributed systems. By waiting before retrying failed requests, the client can help reduce the impact of temporary failures and improve overall system availability. There are several strategies for implementing retries with backoff, and choosing the right approach will depend on the specific requirements of the system.

Additional reading: [https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/)

<br>
