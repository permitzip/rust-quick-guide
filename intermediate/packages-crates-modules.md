---
description: Notes on structuring larger projects with packages, crates, and modules.
coverY: 0
---

# 📦 Packages, Crates, Modules

## Packages

Packages are a `Cargo` feature that lets you build, test, and share crates.&#x20;

* The `bin` folder will be named after the folder where the _`src/main.rs`_ file is locate. This defines the `package`.
* If a package contains both _`ssrc/main.rs`_ and _`src/lib.rs`_ then the `package` has two crates (a library and a binary)

## Crates

A `crate` is a tree of modules that produces a library or executable.

## **Modules**

**Modules** and **use:** Let you control the organization, scope, and privacy of paths.

* `Modules` organize code within a crate into groups for readability. `Modules` also control the _privacy_ of items (if they can be used outside of the code (_public)._

{% code title="scr/lib.rs" %}
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}

```
{% endcode %}



```
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment

```

## **Paths**

**Paths** are a way of naming an item, such as a struct, function, or module

* absolute path starts from a crate root by using crate name or literal crate
* relative path starts from the current module and uses `self`, `super`, or an identifier in the current module

### Starting Relative Paths with Super

Using `super` is like starting a filesystem path with `..` syntax. This helps prevent the number of places to update code if changes are made later.

{% code title="src/lib.rs" %}
```rust
fn serve_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order(); // function in crate, but not the module (the parent)
    }

    fn cook_order() {}
}

```
{% endcode %}

Making `struct` public does not make each field also public.&#x20;

Making `enum` public _does_ make all of the variants public.

### Bringing Paths into Scope with the use Keyword

The `use` keyword helps bring moduls into scope to simplify the path when calling functions or objects.

Adding `use` and a path in a scope is similar to creating a symbolic link in the filesystem.

Below is an example with a relative path:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use self::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}

```

You can create an alias on the import. For example:

```rust
use std::fmt::Result;
use std::io::Result as IoResult; // alias

fn function1() -> Result {
    // --snip--
    Ok(())
}

fn function2() -> IoResult<()> {
    // --snip--
    Ok(())
}

```

A name import with the `use` keyword creates a name which is scoped to private. To enable the code that calls our code to refer to that name as if it had been defined in that code's scope, we combine `pub` and `use`.

The `*` is the glob operator and allows bulk import or all public items defined in a path into scope (often useful for writing tests).
