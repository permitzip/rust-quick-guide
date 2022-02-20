---
description: Advanced pointer concepts with Rust.
---

# ✨ Smart Pointers

A standard pointer just refers to data.

A smart pointer is a data structure that acts as a pointer but also has additional metadata and capabilities.

A few smart pointers we've already studied include `String` and `Vec<T>`

`String` and `Vec<T>` are smart pointers because they own some memory and allow you to manipulate them.

Generally `smart pointers` are created using `Structs` which implement the `Deref` and `Drop` traits.

* `Deref` `Trait` allows an instance of the smart pointer struct to behave like a reference so you can write code that works with either references or smart pointers.
* `Drop` `Trait` allows you to customize the code that is run when an instance of the smart pointer goes out of scope.
