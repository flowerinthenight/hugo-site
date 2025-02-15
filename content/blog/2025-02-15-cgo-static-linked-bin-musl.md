---
title: "Static-linked CGO binaries using musl and Zig"
description: "2025-02-15"
categories: ["tech", "programming", "software", "systems"]
date: "2025-02-15"
tags: [cgo, golang, musl, zig, libc]
weight: 1
---

The ability to produce statically-linked binaries by default in Go is one of the many good reasons why I appreciate and use it. Being able to copy or move only a single file around across all sorts of locations without worrying too much about missing libraries or runtimes has definitely saved me a lot of annoyances many times over. However, working on [hedge-cb](https://github.com/flowerinthenight/hedge-cb), and therefore, CGO, for the past few days, reminded me once again how much of a good thing static binaries are. So I tried checking out whether I could still do static linking with CGO.

As I mentioned, even though Go produces static binaries by default, introducing CGO into the mix makes your binary dynamically-linked instead, as it will now require a C runtime, usually `glibc`, to be present on the target environment. That in itself is not really such a bad thing as `glibc` is almost always present in most Linux distributions used in production. The annoyance, however, starts when you introduce a dependency through CGO, in which case, you will need to make sure that that dependency's `lib***.so` is also present in your target environment, alongside the C runtime.

For a binary that depends on [hedge-cb](https://github.com/flowerinthenight/hedge-cb), and therefore, [ClockBound](https://github.com/aws/clock-bound), deploying it to, say, EC2 (it's AWS-native after all), requires the following to be present in the VM:

* the [ClockBound daemon](https://github.com/aws/clock-bound/tree/main/clock-bound-d) running - our source for "true time";
* `libclockbound.so` copied to `/usr/lib/` or `/usr/lib64/` - output when compiling [ClockBound FFI](https://github.com/aws/clock-bound/tree/main/clock-bound-ffi);
* a C runtime - Amazon Linux comes with `glibc` preinstalled.

There's no getting around the ClockBound daemon as it's functionally required. But I can do away with `libclockbound.so` and `glibc` altogether if the binary is statically built. I will share what I did in this blog, although this is not the only way how. Now, there's a fair bit of discussions on the interwebs about the pros and cons of static binaries when it comes to the C runtime; I'm not going to rehash them here. For example purposes only, I went with [musl](https://musl.libc.org/), a lightweight alternative to `glibc`, and the [Zig](https://ziglang.org/) toolchain. Zig here is probably optional as `musl` comes with `musl-gcc`, which can serve as my CGO compiler, but I was struggling to make it work. Zig (`zig cc` to be specific), on the other hand, was a breeze.

First, let's build ClockBound's FFI. Rust environment is required here.

```sh
$ cd /tmp/ && git clone https://github.com/aws/clock-bound
$ cd clock-bound/clock-bound-ffi/
$ rustup target add x86_64-unknown-linux-musl
$ cargo build --release --target=x86_64-unknown-linux-musl
```

The build outputs of note here are `libclockbound.so`, which we don't need, and `libclockbound.a`, which we do. Now, let's build `musl`.

```sh
$ cd /tmp/ && wget https://musl.libc.org/releases/musl-1.2.5.tar.gz
$ tar xvzf musl-1.2.5.tar.gz && cd musl-1.2.5/ && ./configure && make && sudo make install
```

Next, copy the FFI archive library to `musl`'s install location.

```sh
$ sudo cp -v /tmp/clock-bound/target/x86_64-unknown-linux-musl/release/libclockbound.a /usr/local/musl/lib/
```

Finally, build the binary using `zig cc` as our CGO compiler. Here, I'm building `hedge-cb`'s sample code.

```sh
$ cd /tmp/ && git clone https://github.com/flowerinthenight/hedge-cb && cd hedge-cb/example/
$ cp /tmp/clock-bound/clock-bound-ffi/include/clockbound.h .
$ CC="zig cc -target x86_64-linux-musl -I. -L/usr/local/musl/lib -lunwind" \
  GOOS=linux GOARCH=amd64 go build -v --ldflags '-linkmode=external -extldflags=-static'

# Output should be a static binary.
$ ldd ./example
      not a dynamic executable
```

So, yes, we can build static binaries even with CGO; but now, I'm stuck with `musl`. I'm sure `musl` is a fine piece of software, but I'm not really that familiar with it. I'm aware we use it at work since many of our containers in production use Alpine Linux as base. But would I exchange `glibc` for `musl` just for static binaries? I'm not so sure.

As always, tradeoffs.

<br>

Related blogs:

1) [AWS ClockBound client for Go](/blog/2025-01-22-clockbound-client-go/)
2) [AWS ClockBound client for Go (update)](/blog/2025-01-27-clockbound-client-go-update/)
3) [Distributed locking on AWS (ClockBound)](/blog/2025-02-02-aws-dist-locking/)
4) [Cluster membership management on AWS](/blog/2025-02-07-aws-cluster-membership/)
5) This blog

<br>
