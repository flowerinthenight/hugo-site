---
title: "Support the 'Any' protobuf type from an external grpc-gateway process"
description: "2021-12-31"
date: "2021-12-31"
paige:
  feed:
    hide_page: true
tags: [protobuf, grpc-gateway, any]
weight: 1
---

This is just a quick one.

I had some trouble making the [`Any`](https://developers.google.com/protocol-buffers/docs/proto3#any) protobuf type work when running [`grpc-gateway`](https://github.com/grpc-ecosystem/grpc-gateway) from a separate process with the gateway throwing an unknown message type error. A quick fix for this is to import the generated `pb.go` file to your proxy source. Something like:

```go
package main

import (
    ...

    _ "github.com/username/pkgwithpbgo"
)
```

<br>
