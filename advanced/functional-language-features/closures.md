---
description: A closure combined a function pointer (fn) with the environment (context)
coverY: 0
---

# 💌 Closures

{% hint style="success" %}
Unlike functions, closures can capture values from their context (ie the scope in which they're defined).
{% endhint %}

Closures have a deeper vision than a standard function. Closures...

* are identified with a pair of vertical pipes `|numb|`or `|param1, param2|` etc.
* are anonymous functions&#x20;
* can be saved in variables or passed as arguments to other functions.&#x20;
* can use `Iterators` to capture their environment.
* are usually short and relevant only within a narrow context rather than in any arbitrary scenario.

## Capturing the Environment with Closures

One major feature `closures` have over `functions` is their ability to capture their environment and access variables from the scope in which they're defined.

```rust
fn main() {
    let x = 4;

    // the equal_to_x closure is allowed to use the
    // x variable that's defined in the same scope
    // that equal_to_x is defined in!
    let equal_to_x = |z| z == x;

    let y = 4;

    assert!(equal_to_x(y));
}
```

The code above would not compile if implemented with a function due to the scope conflicts.

`Closures` capture values from their environment in three ways and parallel how a function takes a parameter. Closures implement the function traits `Fn()`, `FnMut()`, or `FnOnce()`

* `Fn()` a closure that immutably borrows from the context
* `FnMut()` a closure that mutably borrows from the context
* `FnOnce()` takes ownership of the context. **These can only be called once.**

[A great explainer of closures and their function traits from Medium](https://medium.com/swlh/understanding-closures-in-rust-21f286ed1759).
