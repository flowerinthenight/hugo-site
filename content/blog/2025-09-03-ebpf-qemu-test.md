---
title: "Using QEMU for eBPF testing"
description: "2025-09-03"
categories: ["updates", "startup"]
date: "2025-09-03"
tags: [updates, startup]
weight: 1
---

<br>

When building, and especially testing, [Vortex](https://vortex.nightblue.io/), we use several cloud VMs with different kernel versions (usually LTS ones), and a dedicated GKE test cluster. The trouble with eBPF though, is that a lot of the features depend on what kernel version is running on the target system. And maintaining multiple cloud VMs with different kernel versions is a tad expensive, and really not that straightforward.

And so, [QEMU](https://www.qemu.org/) to the rescue. When it comes to emulators, QEMU doesn't really need introductions. This blog is a guide (mostly to myself) on how to build a Debian-based image with a specific kernel version to be used as an eBPF testbed.

This guide uses an Ubuntu-based system. This also works with a cloud VM with KVM enabled. First, let's install our dependencies.

> ```sh
> $ sudo apt update
> $ sudo apt git install make gcc flex bison libncurses-dev libelf-dev libssl-dev \
>   debootstrap dwarves qemu-system -y
> ```

Next, we clone the stable version of the Linux kernel. I'll be using `workdir` as the working directory.

> ```sh
> $ mkdir -p workdir/
> $ cd workdir/
> $ git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git
>
> # Checkout desired version (tag):
> $ cd linux-stable/
> $ git checkout -b v6.6.102 v6.6.102
> ```

After several trial and error, the following config seems to work for eBPF testing.

> ```sh
> $ make defconfig
> $ make kvm_guest.config
>
> # Required for Debian Stretch and later:
> $ ./scripts/config --set-val CONFIG_CONFIGFS_FS y
> $ ./scripts/config --set-val CONFIG_SECURITYFS y
>
> # BPF-related configs:
> $ ./scripts/config --set-val CONFIG_BPF y
> $ ./scripts/config --set-val CONFIG_BPF_SYSCALL y
> $ ./scripts/config --set-val CONFIG_MODULES y
> $ ./scripts/config --set-val CONFIG_BPF_EVENTS y
> $ ./scripts/config --set-val CONFIG_PERF_EVENTS y
> $ ./scripts/config --set-val CONFIG_HAVE_PERF_EVENTS y
> $ ./scripts/config --set-val CONFIG_FUNCTION_TRACER y
> $ ./scripts/config --set-val CONFIG_DEBUG_INFO_DWARF_TOOLCHAIN_DEFAULT y
> $ ./scripts/config --set-val CONFIG_DEBUG_INFO y
> $ ./scripts/config --set-val CONFIG_DEBUG_INFO_BTF y
> $ ./scripts/config --set-val CONFIG_NET_CLS_BPF y
> $ ./scripts/config --set-val CONFIG_NET_ACT_BPF y
> $ ./scripts/config --set-val CONFIG_NET_SCH_INGRESS y
> $ ./scripts/config --set-val CONFIG_BPF_JIT y
> $ ./scripts/config --set-val CONFIG_HAVE_BPF_JIT y
> $ ./scripts/config --set-val CONFIG_CGROUP_BPF y
> $ ./scripts/config --set-val CONFIG_KPROBES y
> $ ./scripts/config --set-val CONFIG_HAVE_KPROBES y
> $ ./scripts/config --set-val CONFIG_KPROBE_EVENTS y
> $ ./scripts/config --set-val CONFIG_KPROBES_ON_FTRACE y
> $ ./scripts/config --set-val CONFIG_UPROBES y
> $ ./scripts/config --set-val CONFIG_UPROBE_EVENTS y
> $ ./scripts/config --set-val CONFIG_ARCH_SUPPORTS_UPROBES y
> $ ./scripts/config --set-val CONFIG_MMU y
> $ ./scripts/config --set-val CONFIG_TRACEPOINTS y
> $ ./scripts/config --set-val CONFIG_HAVE_SYSCALL_TRACEPOINTS y
> $ ./scripts/config --set-val CONFIG_FTRACE y
> $ ./scripts/config --set-val CONFIG_FTRACE_SYSCALLS y
>
> $ ./scripts/config --set-val CONFIG_CMDLINE_BOOL y
> $ echo 'CONFIG_CMDLINE="net.ifnames=0"' >> .config
>
> $ make olddefconfig
> ```

After setting up the build config, we can now build the kernel.

> ```sh
> $ make -j$(nproc)
> ```

`arch/x86/boot/bzImage` is now our newly-built kernel. Next, let's build a [Debian-based (bullseye)](https://www.debian.org/releases/bullseye/) image for QEMU to boot with our custom kernel.

> ```sh
> $ cd ../
> $ mkdir -p debian-bullseye/
> $ cd debian-bullseye/
> $ wget https://raw.githubusercontent.com/google/syzkaller/master/tools/create-image.sh
> $ chmod +x create-image.sh
> $ ./create-image.sh --feature full
> ```

`bullseye.img` is now our new Linux image. Let's boot it using QEMU, mapping our local 10021 port to the VM's SSH (22) port.


> ```sh
> $ cd ../
> $ qemu-system-x86_64 \
>   -m 2G \
>   -smp 2 \
>   -kernel linux-stable/arch/x86/boot/bzImage \
>   -append "console=ttyS0 root=/dev/sda earlyprintk=serial net.ifnames=0" \
>   -drive file=debian-bullseye/bullseye.img,format=raw \
>   -net user,host=10.0.2.10,hostfwd=tcp:127.0.0.1:10021-:22 \
>   -net nic,model=e1000 \
>   -enable-kvm \
>   -nographic \
>   -pidfile vm.pid \
>   2>&1 | tee vm.log
> ```

Using a separate terminal, we can now use `ssh` and `scp` to access the VM and copy files to it. Let's copy the `vortex-agent` binary to the home directory.

> ```sh
> $ scp -i debian-bullseye/bullseye.id_rsa -P 10021 \
>   -o "StrictHostKeyChecking no" \
>   $VORTEX_AGENT_ROOT/bin/vortex-agent \
>   root@localhost:~/
> ```

Finally, we can `ssh` to the VM.

> ```sh
> $ ssh -i debian-bullseye/bullseye.id_rsa -p 10021 \
>   -o "StrictHostKeyChecking no" root@localhost
>
> # Then run the binary:
> $ ./vortex-agent run --logtostderr
> ```

To close the VM, we can either do:

> ```sh
> $ poweroff
> ```

from within the VM, or kill the process from outside:

> ```sh
> $ kill [-9] $(cat vm.pid)
> ```

<br>

Related blogs:

1) [On building Vortex](/blog/2025-08-21-on-building-vortex/)
2) This blog

<br>
