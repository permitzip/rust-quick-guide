---
description: >-
  This chapter goes over the primary extensible features which make Rust a
  flexible and modular programming language.
coverY: 0
---

# 💪 Functions & Extensible Features

Functions in Rust are made extensible by using [generic types ](generic-types.md)and [traits](traits.md).

The syntax is cumbersome.

The concept of lifetimes is unique to Rust, will make your brain hurt, but is one of the features that makes Rust memory safe.

## Referencing a Function

A **function item** uniquely identifies a particular instance of a function.

* has no state
* it does not hold a pointer
* and identifier the compiler uses to identify a specific instance of a function
* `function items` are coercible into  a function pointer
* has zero size because it has no obligation to read the function code yet, so no pointer can yet exist

A **function pointer** is a pointer to a function **with a given signature.**&#x20;

* has note state
* example: `fn(u32) -> u32`
* above is an example of defining a function pointer that is signed by u32 in and u32 out

```rust
fn main() {
    let mut x = bar::<i32>; // function item, zero value because no code is required to be generated
    println!("{}", std::mem::size_of_val(&x)); // result is 0
    baz(bar::<u32>); // function item coerced into function pointer so that the function can be called thereby giving it a pointer
    baz(bar::<i32>); // coercion only happens in these examples because the `bar` matches the required type.
}

// bar is a function that accepts any
fn bar<T>(_: u32) -> u32 {
    0
}

// bar is a function that accepts f, which is a function pointer signed by u32
// in and u32 out.
fn baz(f: fn(u32) -> u32) {
    println!("{}", std::mem::size_of_val(&f)); // result is 8
}

```

## Closures

[A topic for later](../../advanced/functional-language-features/closures.md) but this is the name for the syntax that uses the `|` symbols.&#x20;





{% content-ref url="generic-types.md" %}
[generic-types.md](generic-types.md)
{% endcontent-ref %}

{% content-ref url="traits.md" %}
[traits.md](traits.md)
{% endcontent-ref %}

{% content-ref url="lifetime.md" %}
[lifetime.md](lifetime.md)
{% endcontent-ref %}
