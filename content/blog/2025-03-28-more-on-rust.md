---
title: "Fighting Rust's borrow checker"
description: "2025-03-28"
categories: ["rust", "programming"]
date: "2025-03-28"
tags: [rust, programming, rustlang]
weight: 1
---

This is a common phrase I come across in Rust forums and IRC rooms. And I've got the taste of what it is after I finished porting [hedge](https://github.com/flowerinthenight/hedge/) to Rust (called [hedge-rs](https://github.com/flowerinthenight/hedge-rs/), obviously). Coming from C/C++/Go/Zig, working with Rust's borrow checker needs some getting used to. But just to get the thing working, at least initially, I had to do some bits of circumvention, which feels like "dirty" tricks to me at the moment. I'm sure I'll learn some of the more idiomatic ways of doing these in the (near) future as I write more Rust code. Here are some of those observations.

For context, [hedge-rs](https://github.com/flowerinthenight/hedge-rs/) is a non-async, multithreaded code that runs on multiple nodes communicating with each other using an internal protocol on top of TCP.

#### Lots of `.clone()`ing

In Rust, accessing a variable from another thread isn't as straightforward. This doesn't compile.

```rust
fn main() {
    let args: Vec<String> = env::args().collect();

    thread::spawn(|| {
        println!("args1: {}", args[1]);
    });
}
```

The new thread is "borrowing" `args` from the calling function which it could outlive. When `args` goes out of scope, the thread could be accessing `args` that is no longer there.

We could do a "move" by doing this.

```rust
fn main() {
    let args: Vec<String> = env::args().collect();

    // Move args to the new thread.
    thread::spawn(move || {
        println!("args1: {}", args[1]);
    });
}
```

This will now compile (`args` is now moved to the new thread), but then I couldn't do this anymore.

```rust
fn main() {
    let args: Vec<String> = env::args().collect();

    // Move args to the new thread.
    thread::spawn(move || {
        println!("args1: {}", args[1]);
    });

    // args was now moved to the other thread.
    println!("args2: {}", args[2]);
}
```

Even if we argue that this code is correct as we are not changing any values; just reading it, this won't compile. We can clone (or copy) instead. This will work.

```rust
fn main() {
    let args: Vec<String> = env::args().collect();

    // Create a clone of args; move to new thread.
    let args_clone = args.clone();
    thread::spawn(move || {
        println!("args1: {}", args_clone[1]);
    });

    // args is still available to us here.
    println!("args2: {}", args[2]);
}
```

There are several of these in [hedge-rs](https://github.com/flowerinthenight/hedge-rs/). I don't really see this as a big problem for small, stack-allocated variables, but it might be for big-ish structs or arrays/vectors.

#### `Arc<Mutex<T>>` for mutable objects

To pass around mutable objects across threads, Rust provides `Arc<Mutex<T>>`, which is a mutex inside an "Atomically Reference Counted" pointer. This allows us to mutate variables across threads in a thread-safe manner.

```rust
#[derive(Debug)]
struct X {
    val: i32,
}

impl X {
    fn inc(&mut self) -> i32 {
        self.val += 1;
        self.val
    }
}

fn main() {
    let x = Arc::new(Mutex::new(X { val: 0 }));

    let x_clone = x.clone();
    thread::spawn(move || {
        println!("{:?}", x_clone.lock().unwrap().inc());
        // mutex unlocks when out of scope
    });

    println!("{:?}", x.lock().unwrap().inc());
    // mutex unlocks when out of scope
}
```

In this code, `x_clone` here looks like our previous example but it isn't. For `Arc`s, you clone the pointer to increase its reference count, not clone the inner object itself. `x_clone` and `x` here still points to the same `X` object. `Arc` will only cleanup its inner object once the reference count goes to zero.

This is actually alright for me, albeit the weird ergonomics of `.clone()`, and `.lock()` with its implicit unlock pair when going out of scope. What is slightly unexpected is if it's for atomics; I still have to wrap an atomic with an `Arc` as well. For example,

```rust
fn main() {
    let a = Arc::new(AtomicU32::new(0));

    let a_clone = a.clone();
    thread::spawn(move || {
        a_clone.store(1, Ordering::Relaxed);
        println!("{}", a_clone.load(Ordering::Acquire));
    });

    a.store(2, Ordering::Relaxed);
    println!("{}", a.load(Ordering::Acquire));
}
```

I feel like there's not really a need for `Arc` here since it's, you know, atomic.

#### Using `Vec<T>` for late initialization of objects

This one for me definitely feels like a cheat. Say you have a struct field in your other struct that you want to initialize later. Like this `Lock` inside `Op`.

```rust
pub struct Op {
    ...
    lock: Arc<Mutex<Lock>>,
    tx_worker: Sender<WorkerCtrl>,
    ...
}
```

Instantiating `Op` without setting up both `lock` and `tx_worker` is not that straightforward, like:

```rust
let o = Op { lock: ???, tx_worker: ??? };
...
// Initialize o.lock here.
...
// Initialize o.tx_worker here.
...
```

What did I do? Use vectors instead.

```rust
pub struct Op {
    ...
    lock: Vec<Arc<Mutex<Lock>>>,
    tx_worker: Vec<Sender<WorkerCtrl>>,
    ...
}

fn main() {
    // So I can do this:
    let mut o = Op { lock: vec![], tx_worker: vec![] };
    ...
    // Late initialization of lock.
    o.lock = vec![Arc::new(Mutex::new(
        LockBuilder::new()
            .db(self.db.clone())
            .table(self.table.clone())
            .name(lock_name)
            .id(self.id.clone())
            .lease_ms(lease_ms)
            .leader_tx(Some(tx_ldr))
            .build(),
    ))];
    ...
    // And late initialization of o.tx_worker.
    let (tx, rx): (Sender<WorkerCtrl>, Receiver<WorkerCtrl>) = channel();
    o.tx_worker = vec![tx.clone()];

    // I can then use lock like so:
    if o.lock.len() > 0 {
        // use o.lock[0]
    }

    // Same with tx_worker.
    if o.tx_worker.len() > 0 {
        // use o.tx_worker[0]
    }
}
```

It doesn't feel right, but it works.

<br>
