---
description: How does Rust work with Strings.
coverY: 0
---

# 🧵 Strings

A `String` is a collection of characters.

`Strings` are more complicated than many developers give them credit for, and these are UTF-8 making it more prone to confusion.

Rust has only one string type in the core lanugage, which is the string slice `str` that is usually seen in its borrowed form `&str`.

The `String` type in the std library, rather than coded into the core, is a growable, mutable, owned, UTF-i encoded string type.

Quick reference:

* You can concatenate strings using the `+` operator.
* Append a string using `push_str` method (for mutable `String`)
* Use the `format!` macro to format multiple strings together.
* Rust Looks at strings as either `Bytes`, `Scalar Values`, or `Grapheme Cluster`.
* Slicing a `String` at an invalid `Byte` reference will cuase `Rust` to panic.&#x20;
  * note: remember that valid Unicode scalar values may be made up of more than 1 `byte` which could lead to panic.







## Hash Map

Associate a value with a particular key. It's an implementation of the more general data structure called a `map`.

The type `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V`.

A new Hash Map:

```rust
 fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
}

```

`Hash Maps` like `Vectors` store their data on the heap.

The `zip` method creates an `iterator` of `tuples` from two different `vectors`.

The `collect` method then can turn the interator of tuples into a ``HashMap`.``&#x20;

```rust
fn main() {
    use std::collections::HashMap;

    let teams = vec![String::from("Blue"), String::from("Yellow")];
    let initial_scores = vec![10, 50];

    let mut scores: HashMap<_, _> =
        teams.into_iter().zip(initial_scores.into_iter()).collect();
}
t
```

The type annotation `HashMap<_,_>` is needed because it's possible to `collect` into many different data structures and Rust doesn'tk now which you want unless you specify.

**Ownership:** for values that implement the `Copy` trait, values are copied into the `HashMap`. For owned values, the values will be moved and the `HashMap` will be the owner of those values.

Use the `get` method to avoid panic reactions from the application.

Iterating `HashMap` is similar to iterating `Vector` and is somewhat similar to `Python` syntax also.

```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    for (key, value) in &scores {
        println!("{}: {}", key, value);
    }
}

```

Use `insert` method to update the value.

Use the `entry` method will insert only if the key does not already have a value. Otherwise, it returns the value.

