---
description: Anonymous functions that can capture their environment.
coverY: 0
---

# 💌 Closures

`Closures` are anonymous functions you can save in a variable or pass as arguments to other functions.&#x20;

`Closures` are identified with a pair of vertical pipes `|numb|`or `|param1, param2|` etc.

`Closures` are usually short and relevant only within a narrow context rather than in any arbitrary scenario.

{% hint style="success" %}
Unlike functions, closures can capture values from the scope in which they're defined. More on that later in this section...
{% endhint %}

## Closure as a Cache Mechanism

For demonstration we start with a function simulated to have high overhead:

```rust
use std::thread;
use std::time::Duration;

/// A function to simulate some calculation taking 
/// two seconds to complete.
fn simulated_expensive_calculation(intensity: u32) -> u32 {
    println!("calculating slowly...");
    thread::sleep(Duration::from_secs(2));
    intensity
}

/// Main core business logic for the application.
fn generate_workout(intensity: u32, random_number: u32) {
    if intensity < 25 {
        println!(
            "Today, do {} pushups!",
            simulated_expensive_calculation(intensity)
        );
        println!(
            "Next, do {} situps!",
            simulated_expensive_calculation(intensity)
        );
    } else {
        if random_number == 3 {
            println!("Take a break today! Remember to stay hydrated!");
        } else {
            println!(
                "Today, run for {} minutes!",
                simulated_expensive_calculation(intensity)
            );
        }
    }
}

fn main() {
    /// Sample values for simulating functionality
    let simulated_user_specified_value = 10;
    let simulated_random_number = 7;

    generate_workout(simulated_user_specified_value, simulated_random_number);
}

```

Ok so we have some fake values, a fake 2-second function to do imaginary work on a back end and the core business logic function called `generate_workout`.

The problem is that in its current form the code is calling the expensive function multiple times, increasing the user experience exposure to the slow backend work.

One way to refactor is to use functions:

```rust
fn generate_workout(intensity: u32, random_number: u32) {
    let expensive_result = simulated_expensive_calculation(intensity);

    if intensity < 25 {
        println!("Today, do {} pushups!", expensive_result);
        println!("Next, do {} situps!", expensive_result);
    } else {
        if random_number == 3 {
            println!("Take a break today! Remember to stay hydrated!");
        } else {
            println!("Today, run for {} minutes!", expensive_result);
        }
    }
}
```

The issue above is the long-time function occurs first, and blocks the remaining code. This is where `closures` come in.

We can store the `closure` in a variable rather than the result of a function call.

{% hint style="info" %}
The type annotations of parameter and return values are options and not necessarily practiced in the world of one-off `closure` implementation.
{% endhint %}

```rust
    let expensive_closure = |num: u32| -> u32 {
        println!("calculating slowly...");
        thread::sleep(Duration::from_secs(2));
        num
    };
```

Above can be translated as "the `expensive_closure` contains the _**definition**_ of an anonymous function, not the _**resulting value**_ of calling the function.

We call closures like we call functions.

```rust
use std::thread;
use std::time::Duration;

fn generate_workout(intensity: u32, random_number: u32) {
    let expensive_closure = |num| {
        println!("calculating slowly...");
        thread::sleep(Duration::from_secs(2));
        num
    };

    if intensity < 25 {
        println!("Today, do {} pushups!", expensive_closure(intensity));
        println!("Next, do {} situps!", expensive_closure(intensity));
    } else {
        if random_number == 3 {
            println!("Take a break today! Remember to stay hydrated!");
        } else {
            println!(
                "Today, run for {} minutes!",
                expensive_closure(intensity)
            );
        }
    }
}

fn main() {
    let simulated_user_specified_value = 10;
    let simulated_random_number = 7;

    generate_workout(simulated_user_specified_value, simulated_random_number);
}

```

So the problem still isn't solved because this slow function is still being called twice in the first `if` statement.

Next, we use a `Struct` to hold the `closure` and store the value in the cache to implement a _lazy evaluation_ pattern.

```rust
// this struct will hold the closure in `calculation`
struct Cacher<T>
where
    T: Fn(u32) -> u32, // Trait bounds on T specify a closure using the `Fn` trait.
{
    calculation: T,
    value: Option<u32>,
}

fn main() {}
```

This will execute the business logic one time and store value in the cache. Thereafter it will recall the cache, bypass the backend.

```rust
struct Cacher<T>
where
    T: Fn(u32) -> u32,
{
    calculation: T,
    value: Option<u32>,
}

impl<T> Cacher<T>
where
    T: Fn(u32) -> u32,
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        // if anything is cached, use it...otherwise make the call
        match self.value {
            Some(v) => v,
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            }
        }
    }
}

fn main() {}
rust
```

So now in the runtime we have:

```rust
use std::thread;
use std::time::Duration;

struct Cacher<T>
where
    T: Fn(u32) -> u32,
{
    calculation: T,
    value: Option<u32>,
}

impl<T> Cacher<T>
where
    T: Fn(u32) -> u32,
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        match self.value {
            Some(v) => v,
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            }
        }
    }
}

fn generate_workout(intensity: u32, random_number: u32) {
    // define the `closure function` as `Cacher` type
    let mut expensive_result = Cacher::new(|num| {
        println!("calculating slowly...");
        thread::sleep(Duration::from_secs(2));
        num
    });

    if intensity < 25 {
        println!("Today, do {} pushups!", expensive_result.value(intensity));
        println!("Next, do {} situps!", expensive_result.value(intensity));
    } else {
        if random_number == 3 {
            println!("Take a break today! Remember to stay hydrated!");
        } else {
            println!(
                "Today, run for {} minutes!",
                expensive_result.value(intensity)
            );
        }
    }
}

fn main() {
    let simulated_user_specified_value = 10;
    let simulated_random_number = 7;

    generate_workout(simulated_user_specified_value, simulated_random_number);
}

```

## Limitations of the Cacher Implementation

This assumes the `Cacher` instance assumes it will always get the same value for the parameter `arg` to the `value` method. This could be solved by storing the values into a `HashMap` and performing a lookup against the map to reduce the quantity of repeated identical queries.

The current implementation is also limited to the `u32` type of storage. A production implementation would require move flexibility of the `Cacher` functionality.

## Capturing the Environment with Closures

One major feature `closures` have over `functions` is their ability to capture their environment and access variables from the scope in which they're defined.

```rust
fn main() {
    let x = 4;

    // the equal_to_x closure is allowed to use the
    // x variable that's defined in the same scope
    // that equal_to_x is defined in!
    let equal_to_x = |z| z == x;

    let y = 4;

    assert!(equal_to_x(y));
}
```

The code above would not compile if implemented with a function due to the scope conflicts.

`Closures` capture values from their environment in three ways and parallel how a function takes a parameter:

* taking ownership
* borrowing mutably
* borrowing immutably

These are encoded in thee `Fn` traits as follows:

* `FnOnce` consumes the variable it captures from its enclosing scope (its environment). To consume the captured variables, the closure must take ownership of the variables and move them into the closure when it is defined. The `Once` part of the name represents the fact that the closure can't take ownership of the same variables more than once, so it can be called only once.
* `FnMut` can change the environment because it mutably borrows values
* `Fn` borrows values from the environment immutably

`Closures` can user `Iterators` to capture their environment.

