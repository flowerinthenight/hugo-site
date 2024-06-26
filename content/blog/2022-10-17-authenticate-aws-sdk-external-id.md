---
title: "Authenticating Go AWS SDK using external id"
description: "2022-10-17"
categories: ["Tech"]
date: "2022-10-17"
paige:
  feed:
    hide_page: true
tags: [golang, aws, sdk, assume, roles, external-id]
weight: 1
---

For self reference:

Sample code as to how to authenticate [aws-sdk-go](https://github.com/aws/aws-sdk-go) using [external ids](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html):

{{< gist flowerinthenight 5980424a19f6fe58e613f6c9c9392c3e >}}

<br>
