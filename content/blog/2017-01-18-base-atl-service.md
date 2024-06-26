---
title: "ATL service base code"
description: "2017-01-18"
date: "2017-01-18"
paige:
  feed:
    hide_page: true
tags: [c++, windows, atl, service]
weight: 1
---

Yet another service code for Windows. This time, it's an ATL service. ATL services are basically the same as [traditional Windows services](https://github.com/flowerinthenight/win32-base-service) but with some advantages.

* Can be started on-demand automatically by the first client call (via COM).
* Clients can call service functions with parameters and return values using COM. In traditional services, clients normally communicate using service control codes and you need some kind of IPC (named pipes, shared memory, etc.) for bi-directional data exchange.

A client console app is provided to demonstrate service function call with return value and service-to-client notification via IDispatch.

Check out the source code [here](https://github.com/flowerinthenight/base-atlcom-svc).

<br>
