---
description: Combine the power of environment data access with features of iteration,.
---

# 😃 Combining Iterators and Closures

The `filter` method on an `iterator` takes a `closure` that takes each `item` from the `iterator` and returns a `Boolean`.

* `true` from the `closure` means the value will be included in the `iterator`
* `false` from the `closure` means the value will be excluded from the `iterator`

Let's go finding shoe sizes in an `environment`:

```rust
#[derive(PartialEq, Debug)]
struct Shoe {
    size: u32,
    style: String,
}

// combine closure (|s|) and iterator (the filter) to get all shoes and filter 
// by a specific shoes size
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn filters_by_size() {
        let shoes = vec![
            Shoe {
                size: 10,
                style: String::from("sneaker"),
            },
            Shoe {
                size: 13,
                style: String::from("sandal"),
            },
            Shoe {
                size: 10,
                style: String::from("boot"),
            },
        ];

        let in_my_size = shoes_in_size(shoes, 10);

        assert_eq!(
            in_my_size,
            vec![
                Shoe {
                    size: 10,
                    style: String::from("sneaker")
                },
                Shoe {
                    size: 10,
                    style: String::from("boot")
                },
            ]
        );
    }
}

```

{% hint style="info" %}
^ Note that `shoe_in_size` calls the method `into_iter` to create an `iterator` that _**takes ownership of the vector**_.&#x20;

Then the call to `filter` adapts that `iterator` into a _**new**_ `iterator` that only contains elements for which the `closure` returns `true`.
{% endhint %}

## Creating Our Own Iterators with the Iterator Trait

Recall that creating an iterator requires a call for `iter`, `into_iter`, or `iter_mut` on a vector. This can be extended to other collection types as well as into `struct` which implements the `Iterator` `trait`.

```rust
struct Counter {
    count: u32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}
```

Once the `next` method and `Item` attributes have been configured, the `Iterator` `trait` interface is complete and so the other methods will now be available by default (if you do not override them).

Take it for a spin:

```rust
struct Counter {
    count: u32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn calling_next_directly() {
        let mut counter = Counter::new();

        assert_eq!(counter.next(), Some(1));
        assert_eq!(counter.next(), Some(2));
        assert_eq!(counter.next(), Some(3));
        assert_eq!(counter.next(), Some(4));
        assert_eq!(counter.next(), Some(5));
        assert_eq!(counter.next(), None);
    }

    #[test]
    fn using_other_iterator_trait_methods() {
        let sum: u32 = Counter::new()
            .zip(Counter::new().skip(1))
            .map(|(a, b)| a * b)
            .filter(|x| x % 3 == 0)
            .sum();
        assert_eq!(18, sum);
    }
}
```

{% hint style="danger" %}
Important takeaway on `iterator adapter` methods is that it helps us avoid having an intermediate mutable vector during the computation and thus adds more security to the application.
{% endhint %}

{% hint style="info" %}
Fun fact: the benchmark for `Iterators` compared to `loops` show that iterators are faster, too!

This would be a function of what is being looped or iterated. Important to understand is that `Iterators` are `zero cost abstractions` (the abstraction imposes no additional overhead on the runtime).
{% endhint %}
