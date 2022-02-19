---
description: What are vectors in Rust?
coverY: 0
---

# 🔌 Vectors

Vectors allow you to store a variable number of values next to each other.

Vectors are implemented using generics.

```rust
let v: Vec<i32> = Vec::new();

// or using a macro:
let v = vec![1, 2, 3];
v.push(4);
v.push(5); // add more values in vector
```

Access vectors with bracket notation, index starts at 0.

Requestion an index to a nonexistent element causes the program to panic.

To avoid panic use the `get` method which returnes `None` without panicking if index is outside of the vector.

If the vector (or a reference to vector value(s)) is borrowed, the vector cannot be modified.

### Using Vectors as Enums for MultiType Storage

`Vectors` can only store values that are the same type. However, if you defined an `enum` and create a vector of `enum` elements, you technically are making a list of `enums` but each is defined within the `enum` spec differently. See example:

```rust
fn main() {
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
}

```

An advantage to this method is to explicitly state what types are allowed in the vector. This is by design to avoid errors with operations performed on the wrong type of elements in a vector.

