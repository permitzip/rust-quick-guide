---
description: Processing a series of items.
coverY: 0
---

# 📡 Iterators

The iterator pattern allows you to perform some tasks on a sequence of items in turn. It is responsible for the logic of iterating over each item and determining when the sequence has finished.

{% hint style="warning" %}
In Rust, iterators are lazy and have no effect until you call methods that consume the iterator to use it up. The following code statement by itself does nothing useful!

`let v1 = vec![1, 2, 3];`\
`let v1_iter = v1.iter();`

In order to do something with the iterator, you have to implement a loop. For example:

`for val in v1_iter {`

&#x20;         `println!("Got: {}", val);`&#x20;

`}`
{% endhint %}

## The **Iterator** Trait and the **next** Method

All `Iterators` implement a trait named `Iterator` which includes a `next` method and and `Item` type.

Using the `next` method of an `Iterator` changes the internal state of the `iterator` since that is what is used to keep track of where it is in sequence. The code _consumes_ the `iterator`. Each call to `next` eats up an item from the iterator. If the data is not given to another vector, it's gone. For example, using the `sum()` method would return the sum of the elements but `consumes` the `iterator` to do so.&#x20;

{% hint style="info" %}
If you need to create an `iterator` that takes ownership and returns owned values, call `into_iter` instead of `iter`.
{% endhint %}

## Methods that Consume the Iterator

{% hint style="info" %}
Methods that call `next()` are called _**consuming**_**  **_**adaptors**_ because calling them uses up the iterator.
{% endhint %}

## Methods that Produce Other Iterators

Other methods defined on the `Iterator` trait, known as _**iterator adaptors**_, allow you to change `iterators` into different kinds of iterators.&#x20;

Remember, `iterators` are lazy and you have to call one of the `consuming adaptor` methods to get results from calls.

The `collect` method is a `consuming adaptor`. Example:

```rust
fn main() {
    let v1: Vec<i32> = vec![1, 2, 3];
    // without calling `.collect()` this 
    // code would do nothing.
    let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

    assert_eq!(v2, vec![2, 3, 4]);
}
```

Calling the `map` method creates a new iterator and the `collect` method consumes the new `iterator` and creates a new `vector`.
