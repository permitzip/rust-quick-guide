---
coverY: 0
description: A few important data structure types and points about data with Rust.
---

# 🎋 Data Structures

## Ownership

Rust is known for a feature called ownership.&#x20;

Ownership offers memory-safe guarantees without needing a garbage collector.

Ownership rules:

* Each value in Rust has a variable that’s called its _owner_.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

In the context of [Rust functions](../functions-and-extensible-features/#ownership), Ownership is easier to contextualize.

[Learn more](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)

