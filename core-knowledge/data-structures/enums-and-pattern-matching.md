---
description: >-
  Enums allow you to define a type by enumerating its possible variants. Pattern
  matching expressions make it easy to run different code for different values
  of an enum.
coverY: 0
---

# 🎰 Enums and Pattern Matching

## Enums

`Enums` can create custom types that can be one of a set of enumerated values. Using `Option<T>` helps use the type system to prevent errors. When `enum` values have data inside them, you can use `match` or `if let` to extract and use those values. See next section for `Match`.

The `_` pattern tells Rust to ignore all other conditions in a `match` statement.

Think of `enums` as a lines of names which can optionally carry data with them.

```rust
fn main() {
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
}
```

## The Option Enum

The Rust library comes out of the box with a save `enum` called `Options<T>`.&#x20;

{% hint style="info" %}
A problem in other languages is the concept of a null value. If you try to use a null as a not-null value, you will get an error of some kind. This error is extremely easy to make, so Rust does not have a true `null`.
{% endhint %}

Rust solves this by creating an `enum` list including the names `None` and `Some<T>`. The `T` parameters is s `Generic Type Parameter` talked about later.

Remember, `None` is just a name in the `enum`...it's not really 'nothing'...it's an `enum` name. But now you can use it to create placeholder parameters that have no value, or a default empty value.

```rust
fn main() {
    let some_number = Some(5);
    let some_string = Some("a string");

    let absent_number: Option<i32> = None;
}
```

Since this is so pervasive in its use in `Rust` that you don't need to bring into scope directly (just like it's shown above).

Thinks of the code above saying "I need something to store 5, and anotherthing to store the words "a string" and something for a future number I'll use but not sure what it's value is yet.

## Pattern Matching

`Match` statements must meet the exhaustiveness requirements.

```rust
match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(), // ignore all other conditions
}
```

Using `if let` simplifies the match statement that allows one pattern matched while ignoring the rest.&#x20;

#### Long Way (without if let)

```rust
fn main() {
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (), // do nothing if no max is set
    }
}

```

#### Shorter Way (using if let)

The code below works the same way as `match` , where the patters is `Some(max)` , and the `max` binds to the value inside the `Some`. The code isn't executed if the value doesn't match the pattern.

```rust
fn main() {
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }
}

```

Adding `else` with `if let` would operate similarly to the `_` pattern shown above in the first example.
