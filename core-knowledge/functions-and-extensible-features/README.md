---
description: >-
  This chapter goes over the primary extensible features which make Rust a
  flexible and modular programming language.
coverY: 0
---

# 💪 Functions & Extensible Features

Functions in Rust are made extensible by using generic types and traits.

The features are powerful but the syntax is cumbersome.

The concept of lifetimes is unique to Rust.

Since a reference n Rust only exist for the scope it is defined, then when using generic types, you must also define the lifetime. Rust needs an explicit definition of the lifetime of the reference, or it will not compile.

{% content-ref url="generic-types.md" %}
[generic-types.md](generic-types.md)
{% endcontent-ref %}

{% content-ref url="traits.md" %}
[traits.md](traits.md)
{% endcontent-ref %}

{% content-ref url="lifetime.md" %}
[lifetime.md](lifetime.md)
{% endcontent-ref %}
