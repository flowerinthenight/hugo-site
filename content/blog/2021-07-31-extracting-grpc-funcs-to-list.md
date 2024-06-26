---
title: "Extract gRPC-generated functions to a list"
description: "2021-07-31"
date: "2021-07-31"
paige:
  feed:
    hide_page: true
tags: [golang, grpc, grpc-gateway, cmdline]
weight: 1
---

This might be hacky and there might be a proper way to do this but recently, I needed to generate all the functions' gRPC-generated full names from our protobuf definitions. This is part of our RBAC module that needs to filter gRPC function calls. I got the list from our [generated Go client](https://github.com/alphauslabs/blue-sdk-go) using the following command(s):

```sh
# Main command:
$ grep -o -R -i -E '"/blueapi\..*"' . | awk -F':' '{gsub(/"/, "", $2); print "-", substr($2, 2);}' | sort | uniq

# Actual commands; save as yaml:
$ echo "functions:" > /tmp/funcs.yaml
$ grep -o -R -i -E '"/blueapi\..*"' . | awk -F':' '{gsub(/"/, "", $2); print "-", substr($2, 2);}' | \
  sort | uniq >> /tmp/funcs.yaml
```

The final list is uploaded to this [repository](https://github.com/alphauslabs/blueapi-functions).

<br>

