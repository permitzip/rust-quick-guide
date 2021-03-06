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

## Ownership

Recall the rules of ownership from the [Data Structures ](../data-structures/#ownership)introduction.

Functions are the way ownership is more easily conceptualized.

A function can

* take ownership of the value
* borrow the value immutably
* borrow the value mutably

[More information.](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#ownership-and-functions)

## Referencing a Function

A **function item** uniquely identifies a particular instance of a function. A function item

* has no state
* _**does not hold a pointer**_
* is coercible into  a function pointer
* has no size because it has no obligation to read the function code yet, so no pointer can yet exist

A **function pointer** is a pointer to a function **with a given signature.**  A function pointer

* pointer points to the start of the code block that will execute when called
* has note state
* example: `fn(u32) -> u32 (`a function pointer that is signed by u32 in and u32 out)

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

Closures are named closures because they close over their environment. They can capture things from their environment and generate a unique function that specifically references data around their environment at the time they are created.

f: F gives ownership

f: \&F borrows

f: \&mut F mutable borrow



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
