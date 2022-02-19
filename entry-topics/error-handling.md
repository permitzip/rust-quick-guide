---
description: Rust features for handling situations in which something goes wrong.
coverY: 0
---

# 🔀 Error Handling

In many cases, Rust requires you to acknowledge the possibility of an error and take some action before code will compile.

`Recoverable` errors, it is reasonable to report the problem to the user and retry the operation.

`Unrecoverable` errors are always symptoms of bugs, like trying to access a location beyond the end of an array.

Rust does not have exceptions. Instead, it has the type `Result<T, E>` for recoverable errors and `panic!` macro that stops execution when the program encounters an `unrecoverable` error.

When a `panic` occurs, rust will start _unwinding_ the stack, cleaning up the data from each function. Panic can be set to `abort` mode to save time and keep binary files low (but only do that if you know what's going on).

Recoverable errors are built as follows

```rust

#![allow(unused)]
fn main() {
enum Result<T, E> {
    Ok(T),
    Err(E),
}
}

```

Where `T` and `E` are generic type parameters. `T` is the type returned with the `Ok(T)` variant, and `E` is the type returned in a failure case within the `Err(E)` variant.

Example of handling errors:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {:?}", error);
            })
        } else {
            panic!("Problem opening the file: {:?}", error);
        }
    });
}

```

One way to handle the above is to use `match` expressions, but that is not very Rustacean.&#x20;

Reach Chapter 13 in the rust-lang references for information on the `unwrap_or_else` method.

You can also simplify using the `expect` method.

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Failed to open hello.txt");
}

```

The code above will either return the file handle or call the `panic!` macro and passes the message indicated in the `expect` method.

### Propagating Errors

A function whose implementation calls something that might fail, instead of handling the error within the function, is knowns as _propagating._

```rust

#![allow(unused)]
fn main() {
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
}

```

Above can be written much simpler using the `?` operator:

```rust

#![allow(unused)]
fn main() {
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?; // passes errors if they occur
    let mut s = String::new();
    f.read_to_string(&mut s)?; // passes errors if they occur
    Ok(s) // returns this value if no errors
}
}
rust
```

The `?` placed after a `Result` value is defined to work similarly to a `match` expression. If the value of the `Results` is an `Ok` the value in the `Ok` will be returned.

This can be shortened even further by chaining the methods:

```rust

#![allow(unused)]
fn main() {
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
}

```

Important: you can use the `?` operator on a `Result` in a function that returns `Result`, and you can use `?` operator on an `Option` function that returns `Option` ..._**but you cannot mix and match!**_

### _**Guidelines for Error Handling**_

Code should `panic!` when it could end up in a bad state such as when invalid values, contradictory values, or missing values are passed to the code plus one or more of the following:

* the bad state is something unexpected (as opposed to something like an occasional user data entry error)
* the code after this point needs to rely on not being in this bad state, rather than checking for the problem at every step
* There's no good way to encode the information in the types you use.

If someone calls the code and passes a value that doesn't make sense, then `panic!`.

If the failure is expected and the user just needs feedback, return a `Result`

### Creating Custom Types for Validation

Build validations into a function to create an instance of the type rather than repeating the validations everywhere.

```rust

#![allow(unused)]
fn main() {
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
}

```
