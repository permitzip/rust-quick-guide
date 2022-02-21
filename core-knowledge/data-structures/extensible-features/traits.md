---
description: >-
  Telling the compiler about functionality of a type which can be shared with
  other types.
coverY: 0
---

# ™ Traits

A `trait` tells the `Rust` compiler about the functionality of a type that can be shared with other types.&#x20;

{% hint style="info" %}
Traits are similar to interfaces
{% endhint %}

## Defining a Trait

Imagine a media aggregator library crate that can display summaries of data.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

```

To translate above, you would say&#x20;

* A `Summary` `trait` consists of the behavior provided by a `summarize` method.

## Implementing a Trait on a Type

Now let's implement the `trait` `Summary` on a `NewsArticle` struct and `Tweet` struct:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

```

## Default Implementations

To set default:

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)") // add a default
    }
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

```

## Traits as Parameters

If you want to accept a parameter that implements a specific trait, add `&impl` to the function definition:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

// Add &impl Summary as a type for item to enforce the implementation of that trait
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

```

The `impl Trait` syntax is actually syntax sugar for a longer form which is called _trait bound_ which looks like:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Above is like saying

* `notify` is a function for an `item` of type `T` which performs actions on type `T` items with a `Summary` trait.

The expanded syntax above is required for enforcing multiple variables with the same traits.

Multiple train bounds are expressed with the `+` operator.

## Clearer Trait Bounds with \`where\` Clauses

When things get complicated, Rust allows the use of `where` clause to improve readability as shown below:

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{

```

It is also possible to return a value of some type that implements a trait using `-> impl Summary` in the return syntax of the function definition.

## Trait Bound _**impl**_ Block to Conditionally Implement Methods

Using a trait bound with an `impl` block that uses generic type parameters allows methods to be conditionally implemented for types with a specific trait(s).

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}


```

The method `cmd_display` above is only implemented if the `Pair` type `T` implements both&#x20;

* `PartialOrd` trait for comparison, and&#x20;
* `Display` trait for printing.

## Blanket Implementation

We can also implement a trait for any type that implements another trait; this is called _blanket implementations_ **and is extensively used in Rust standard library.**

{% hint style="info" %}
example: the standard library implements the `ToString` trait on any type that implements the `Display` trait. The `impl` block in the standard library looks similar to\
\
`impl<T: Display> ToString for T { // --snip-- }`
{% endhint %}

The above effectively states "implement the `ToString` method for any type `T`with the `Display` trait.
