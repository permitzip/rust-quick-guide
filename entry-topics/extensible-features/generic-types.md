---
coverY: 0
---

# 🧱 Generic Types

Generics are abstract stand-ins for concrete types or other properties. This allows us to express the behavior of generics or how they relate to other generics without knowing what will be in their place when compiling and running the code.

Note: short type for `Type` is `T`.

## In Function Definitions

Place `generics` in the signature of the `function` where we would usually specify a data type.

To define a `generic function` place type name declarations inside angle brackets `<>` between the name of the function and the parameters list, like this:

```rust
fn largest<T>(list: &[T]) -> T {

```

This definition translates to

* the function `largest` is generic over some type `T`.&#x20;
* the function has one parameter named `list`, which is a slice of values of type `T`.
* The `largest` function will return a value of the same type `T`

## In Struct Definitions

The same syntax is used for `generic` `structs`.

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}

```

Any `type T` will work for `Point` ; however, they must both be of type `T` or it will not compile.

## In Method Definitions

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}

```

The example above implements a method named `x` on the `Point<T>` `struct` that will return a reference to the `x` field of type `T`.

Declaring `T` as a generic type after `impl` allows `Rust` to identify the type in the angle brackets in `Point` is generic rather than concrete.

note: generic types does not slow down performance
