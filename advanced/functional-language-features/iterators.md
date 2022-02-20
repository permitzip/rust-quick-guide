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

Using the `next` method of an `Iterator` changes the internal state of the `iterator` since that is what is used to keep track of where it is in sequence. The code _consumes_ the `iterator`. Each call to `next` eats up an item from the iterator.

If you need to create an `iterator` that takes ownership and returns owned values, call `into_iter` instead of `iter`.

## Methods that Consume the Iterator

Methods that call `next` are called _consuming_ _adaptors_ because calling them uses up the iterator.

## Methods that Produce Other Iterators
