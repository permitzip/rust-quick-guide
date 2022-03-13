---
description: The scope in which the references must be valid.
coverY: 0
---

# 👾 Lifetime

A `lifetime` is the scope for which a reference is valid. Rust requires us to annotate the relationship using generic `lifetime` parameters to ensure the actual references used at runtime will definitely be valid.

The concept `lifetime` is unique to Rust.

## Preventing Dangling References with Lifetimes

By default, a value only has a `lifetime` defined by the scope in which they are defined.

This can lead to dangling references that 'expire' if part of an `inner scope`.

The Rust compiler has a `borrow checker` that compares scopes to determine whether all borrows are valid.

How do we fix it then?

## Lifetime Annotation Syntax

`Lifetime` `annotations` describe the relationships of the `lifetimes` of multiple references to each other without affecting the `lifetimes`.

Names of `lifetime` parameters start with `'` and are very short, like `generic` types.

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

## Lifetime Annotations in Function Signatures

Similar to `type` parameters, we need to declare `generic` `lifetime` parameters inside angle brackets between the function name and the parameter list.

```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

// The longest function definition below is specifying that all
// the references in the signature must have the same lifetime 'a
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

The code above specifies that the `longest` function references in the signature must all have the same lifetime `'a`.

This `function` signature now tells Rust that for some `lifetime` `'a`, the function takes two parameters, both of which are string slices that live at least as long as `lifetime` `'a`. It also tells Rust that the `string slice` returned from the `function` will live at least as long as `lifetime` `'a`.

```rust
fn main() {
    // outer scope: lifetime 'b
    // string1 or string2 or result NOT permitted to mix
    // lifetimes (ie they cannot be defined in this outer scope
    // unless they all are defined in the outer scope.
    {
        // inner scope: lifetime 'a
        let string1 = String::from("long string is long");
        let string2 = String::from("xyz");
        // string1 and string2 both share the same lifetime
        // result also shares the same lifetime as string1 and string2
        // this is valid
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {}", result);
    }
    
    // outer scope: lifetime 'b
}

// longest function signature defined with 'a lifetime
// attributed to inputs and the return value
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

## Lifetime Annotations in Struct Definitions

Previous examples of `structs` only contained `owned` types but a `struct` can hold references as well. In that case, a`lifetime` definition is required.

```rust
// struct must have a lifetime annotation
// because it holds a reference 
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        // reference to first_sentence is within scope
        // and therefor the same lifetime, and so as such
        // is a valid isntance
        part: first_sentence,
    };
}
```

The `struct` above holds a `string slice` reference. Like declaring `genereics`, the `lifetime` parameters are defined using the angle brackets `<>`.&#x20;

To read the above definition, you would say "any instance of `ImportExcerpt` must not outlive the reference it holds in its `part` field.

## Lifetime Elision

There are a few patterns programmed into Rust's analysis of references that may be exempt from `lifetimes` explicitly.&#x20;

1. **First Rule** - Each parameter that is a reference gets its own `lifetime` parameter.\
   _example_:`fn foo<'a, 'b>(x: &'a i32, y: &'b i32);`
2. **Second Rule** - If there is exactly one input `lifetime` parameter, that `lifetime` is assigned to all output `lifetime` parameters.\
   _example:_ `fn foo<'a>(x: &'a i32) -> &'a i32`
3. **Third Rule** - If there are multiple input `lifetime` parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output `lifetime` parameters. This makes methods much nicer to read and write because fewer symbols are necessary.

## Lifetime Annotations in Method Definitions

Implementing methods on a struct wi lifetimes is the same syntax as that of `generic` type parameters.&#x20;

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

// The lifetime parameter declaration after `impl` and its use
// after the type name are required, but we're not required t 
// annotate the lifetime of the reference to `self` because of the
// first elision rule.
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}

// There are two input lifetimes, so Rust applies the first lifetime
// elision rule and gives both `&self` and `announcement` their own
// lifetimes. Then, because one of the parameters is `&self`, the 
// return type gets the lifetime of &self, and all lifetimes have been 
// accounted for.
impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}

```

## The Static Lifetime

`'static` lifetime is a reference that _can_ live for the entire duration of the program. All string literals have the `'static` lifetime, which we can annotate as:

```rust
let s: &'static str = "I have a static lifetime.";
```

The text of the `str` above is stored directly in the program's binary, which is always available.

Probably not best practice to intentionally specify the `'static` `lifetime` since it could create issues unless you have a specific reason to use it.
