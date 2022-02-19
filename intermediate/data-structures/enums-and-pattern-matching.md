---
description: >-
  Enums allow you to define a type by enumerating its possible variants. Pattern
  matching expressions make it easy to run different code for different values
  of an enum.
coverY: 0
---

# 🎰 Enums and Pattern Matching

`Enums` can create custom types that can be one of a set of enumerated values. Using `Option<T>` helps use the type system to prevent errors. When `enum` values have data inside them, you can use `match` or `if let` to extract and use those values. See next section for `Match`.

The `_` pattern tells Rust to ignore all other conditions in a `match` statement.

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

Adding `else` with `if let` would operate similarly to the `_` pattern shown above in the first exampe.
