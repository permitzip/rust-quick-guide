---
description: A hyper-fast overview of important Rust features
coverY: 0
---

# ⛵ Introduction

## What makes Rust so cool?

Rust introduces the idea of ownership. It's Rust's most unique feature and it enables Rust to make memory safety guarantees without needing a garbage collectors.

{% hint style="warning" %}
Don't skip the time to understand this idea deeply. It is specifically the primary feature of this language, skipping to standard paradigm stuff or data structures without internalizing Ownership will lead to disasters at compile time!
{% endhint %}

Basically, when you work in Rust, only one thing can hold store (or own) your value...the value can be borrowed, lent, transferred, but only one variable can own it. You can copy it, but it's not the true value in memory.

In python or JS, your values live multiple lives in memory (global scope, function scope, state, etc). In Rust, your value teleports into every scope during the [lifetime](core-knowledge/functions-and-extensible-features/lifetime.md) of the scope.

## How to Use This Guide

This book is intended to be a trimmed-down version of the official documentation that would allow curious minds to review the examples and core concepts quickly without getting bogged down too much in any one topic.&#x20;

{% hint style="warning" %}
This book is should not be used as a tutorial! Just a quick review of major topics. [The original content](https://doc.rust-lang.org/book/title-page.html) this summary and examples are based on has great step-by-step tutorials.
{% endhint %}

Rust is such a comprehensive shift in programming though and so this summary hopes to serve as a fast way to grasp core concepts and fundamentals.

Each chapter includes the examples that were most impactful for me in understanding the concepts. More time is given to Rust-specific concepts like ownership, generic types, traits, lifetime, etc.&#x20;

This book is broken into the following groups

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

{% hint style="info" %}
Introductory information is not included in this package since it is well documented [elsewhere](https://doc.rust-lang.org/book/ch01-00-getting-started.html) and includes more detail than what is intended for this book.
{% endhint %}

## Acknowledgements

{% hint style="info" %}
This document is no more than a summary of high points from the comprehensive documentation of [The Rust Programming Language 2018 edition.](https://doc.rust-lang.org/book/title-page.html)&#x20;

This is a derivative work. Much of the language is taken word-for-word what is published on the official Rust documentation listed above.&#x20;

I've merely consolidated the data into a quick-reference manual that hopefully will encourage more developers to learn about Rust.

All credit of this work belongs to the authors, Steve Klabnik and Carol Nichols as well as the Rust Community contributions.
{% endhint %}

Happy hacking!

\-Kenny [https://github.com/keneek](https://github.com/keneek)
