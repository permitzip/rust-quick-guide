---
description: HashMaps in Rust
coverY: 0
---

# 🗺 HashMaps

A `HasMap` associates a value with a particular key. It's an implementation of the more general data structure called a `map`.

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

