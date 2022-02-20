---
description: Allocating values on the heap.
---

# 🏟 Box\<T>

Boxes allow you to store data on the heap rather than the stack.

Common applications

* When you have a type whose size can't be known at compile-time and you want to use a value of that type in a context that requires an exact size.
* When you have a large amount of data and you want to transfer ownership but ensure the data won't be copied when you do so.
* When you want to own a value and you care only that it's a type that implements a particular trait rather than being of a specific type.

## Using a Box\<T> to Store Data to the Heap

```rust
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```

Not a very realistic example above, but demonstrates that `b` points to the value `5` somewhere on the `heap`.&#x20;

## Enable Recursive Types with Boxes

`Recursive` `type` is a good example of a `type` whose size can't be known at compile time. The nesting nature could technically continue infinitely, Rust doesn't know how much space a value of a recursive type needs.

A `Box<T>` has a known size, so by inserting a box in a recursive `type` definition, you can have `recursive types`.

Pointers cannot be compared to the original value they reference without using `*` operator.

```rust
fn main() {
    let x = 5;
    let y = &x;

    assert_eq!(5, x);
    assert_eq!(5, *y); // dereference using * to follow the pointer to the value
}

```

### Using Box\<T> Like a Reference

Let's try this with Box\<T>:

```rust
fn main() {
    let x = 5;
    let y = Box::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```

The only difference between the examples is `y` is set as an instance of a box pointing to a copied value of `x` rather than reference pointing to the value of `x`.

### Defining Our Own Smart Point

Let's take a closer look at how this works.

```rust
use std::ops::Deref;

struct MyBox<T>(T); // tuple struct with one element

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

fn main() {
    let x = 5;
    let y = MyBox::new(x);
    
    assert_eq!(5, x);
    assert_eq!(5, *y); // dereference, only possible when implementing `Deref`
}
```

`Box<T>` is essentially defined as a tuple struct with one element, so we define ours the same way above. Also included is the `new` function as a construction.

```rust
fn main() {
    let x = 5;
    let y = MyBox::new(x);
    
    assert_eq!(5, x);
    assert_eq!(5, *y); // dereference, only possible when implementing `Deref`
}
```

We cannot `dereference` with `*` until we implement the `Deref` `Trait`.

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

The `type Target = T` defines an associated type for the `Deref` `trait` to use.&#x20;

The body of the `deref` method is filled with `&self.0` so `deref` returns a reference to the value we want to access with the `*` operator.

When we entered `*y` , behind the scenes, Rust actually ran:

`*(y.deref())`

### Implicit Deref Coercions with Functions and Methods

`Deref coercion` is a convenience that Rust performs on arguments to functions and methods. It happens automatically when we pass a reference to a particular type's value as an argument to a function or method that doesn't match the parameter type in the function or method definition (eg `&String` to &`str` because `String` implements the `Deref` trait such that it returns `&str`.&#x20;

Its purpose is to aid in limiting the explicit references and dereferences with `&` and `*`.

```rust
fn hello(name: &str) {
    println!("Hello, {}!", name);
}

fn main() {
    let m = MyBox::new(String::from("Rust")); 
    hello(&m);    // works because deref coorcion which is possible
                  // because MyBox<String> implements the `Deref`.
}
```

`Deref` uses the `*` operator on immutable instances, but the `DerefMut` trait could be implemented to use `*` as a mutable reference.

## Running Code on Cleanup with the Drop Trait

work in progress...
